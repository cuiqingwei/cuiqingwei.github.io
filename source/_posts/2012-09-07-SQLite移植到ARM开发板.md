---
layout: post
title: 'SQLite移植到ARM开发板'
date: 2012-09-07 19:50
comments: true
tags:
  - SQLite
  - 嵌入式
---

# 1.下载sqlite3源代码

http://www.sqlite.org/sqlite-autoconf-3071400.tar.gz

# 2.解压源代码：

```bash
tar xvzf sqlite-autoconf-3071400.tar.gz
cd sqlite-autoconf-3071400
```

<!--more-->

# 3. 配置交叉编译到armlinux平台,prefix=/home/at91sam9/sqlite-autoconf-3071400/sqlite-arm为目标目录:

```bash
./configure --host=arm-none-linux-gnueabi --prefix=/home/at91sam9/sqlite-autoconf-3071400/sqlite-arm
./configure --host=arm-linux --prefix=/home/at91sam9/sqlite-autoconf-3071400/sqlite-arm CC=arm-none-linux-gnueabi-gcc
```

# 4.编译:

```bash
make
```

# 5.安装:

```bash
make install
```

# 6.生成文件：

头文件和生成的库文件将安装到/home/at91sam9/sqlite-amalgamation-3.6.1/sqlite-arm目录

```bash
cd /home/at91sam9/sqlite-amalgamation-3.6.1/sqlite-arm/lib
file libsqlite3.so.0.8.6 							// 看库文件是否是针对mipsel版的
arm-none-linux-gnueabi-strip libsqlite3.so.0.8.6 	// 优化，处理掉其中的调试信息
```

用如下命令查看webs执行文件依赖的库文件：

```bash
arm-none-linux-gnueabi-readelf -d libsqlite3.so.0.8.6
```

其中，arm-none-linux-gnueabi-依你用的交叉编译器而定。

查看得到如下信息：

```bash
Dynamic section at offset 0x8ae94 contains 28 entries:
  Tag        Type                         Name/Value
 0x00000001 (NEEDED)                     Shared library: [libdl.so.2]
 0x00000001 (NEEDED)                     Shared library: [libpthread.so.0]
 0x00000001 (NEEDED)                     Shared library: [libgcc_s.so.1]
 0x00000001 (NEEDED)                     Shared library: [libc.so.6]
 0x0000000e (SONAME)                     Library soname: [libsqlite3.so.0]
 0x0000000c (INIT)                       0x51bc
 0x0000000d (FINI)                       0x8186c
 0x00000019 (INIT_ARRAY)                 0x92330
 0x0000001b (INIT_ARRAYSZ)               4 (bytes)
 0x0000001a (FINI_ARRAY)                 0x92334
 0x0000001c (FINI_ARRAYSZ)               4 (bytes)
 0x00000004 (HASH)                       0xd4
 0x00000005 (STRTAB)                     0x1ac4
 0x00000006 (SYMTAB)                     0x954
 0x0000000a (STRSZ)                      4866 (bytes)
 0x0000000b (SYMENT)                     16 (bytes)
 0x00000003 (PLTGOT)                     0x92f94
 0x00000002 (PLTRELSZ)                   1352 (bytes)
 0x00000014 (PLTREL)                     REL
 0x00000017 (JMPREL)                     0x4c74
 0x00000011 (REL)                        0x3074
 0x00000012 (RELSZ)                      7168 (bytes)
 0x00000013 (RELENT)                     8 (bytes)
 0x6ffffffe (VERNEED)                    0x2ff4
 0x6fffffff (VERNEEDNUM)                 4
 0x6ffffff0 (VERSYM)                     0x2dc6
 0x6ffffffa (RELCOUNT)                   704
 0x00000000 (NULL)                       0x0
```

依赖的库如果嵌入式系统lib目录没有，将没有的依赖库文件复制到嵌入式系统的相应lib目录中。

bin文件夹下的sqlite3，是sqlite可执行应用程序，下载到板子Linux系统下的/bin目录或者/usr/bin目录下，并添加文件可执行权限。在板子Linux系统命令行下执行：
libsqlite3.so和libsqlite3.so.0都是libsqlite3.so.0.8.6的软链接文件。真正需要下载到板子目录/lib下的动态libsqlite3.so.0.8.6。

要成功在嵌入式系统运行sqlite3，需要把./lib下的动态链接库下载到板子上。否则会有无法找到动态链接库libsqlite3.so.0的错误提示。

下载到板子后，还需对它建立软链接文件，因为可执行程序寻找的动态链接库的名字为libsqlite3.so.0，在嵌入式系统运行如下指令：

```bash
ln -s /lib/libsqlite3.so.0.8.6  /lib/libsqlite3.so.0
chmod +x sqlite3
./sqlite3
```

打印显示如下信息，代表成功运行！

```bash
# ./sqlite3
SQLite version 3.7.14 2012-09-03 15:42:36
Enter ".help" for instructions
Enter SQL statements terminated with a ";"
```

# 7. 写自己的数据库应用

```c
//error sqlite3_exec( db , sql , 0 , callback , &zErrMsg );

＃include <stdio.h>
＃include <sqlite3.h>

static int callback(void \*NotUsed, int argc, char \*\*argv, char \*\*azColName){
  int i;
  for(i=0; i<argc; i++){
    printf("%s = %s\n", azColName[i], argv[i] ? argv[i] : "NULL");
  }
  printf("\n");
  return 0;
}

int main(int argc, char \*\*argv){
  sqlite3 \*db;
  char \*zErrMsg = 0;
  int rc;

  if( argc!=2 ){
    fprintf(stderr, "Usage: %s DATABASE \n", argv[0]);
    return 0;
  }
  rc = sqlite3_open(argv[1], &db);
  if( rc ){
    fprintf(stderr, "Can't open database: %s\n", sqlite3_errmsg(db));
    sqlite3_close(db);
    return 0;
  }

  char \*sql = " CREATE TABLE SensorData(               \
                        ID       INTEGER PRIMARY KEY,  \
                        SensorID INTEGER,              \
                        SiteNum  INTEGER,              \
                        Time     VARCHAR(12),          \
                        SensorParameter REAL           \
                );";
   //使用sql字符串指定的sql语言 创建数据表SensorData

    sqlite3_exec( db , sql , callback , 0 ,  &zErrMsg );

   //插入数据到数据表

  sql = "INSERT INTO 'SensorData' VALUES( 0 , 1 , 1 ,  '200605011206', 18.9 );" ;

   //error sqlite3_exec( db , sql , 0 , callback , &zErrMsg );
    sqlite3_exec( db , sql , callback , 0 ,  &zErrMsg );

   //插入数据到数据表
  sql = "INSERT INTO  'SensorData'  VALUES(1 , 1 , 1 , '200605011306', 16.4 );" ;

   //error sqlite3_exec( db , sql , 0 , callback , &zErrMsg );
    sqlite3_exec( db , sql , callback , 0 ,  &zErrMsg );



  int nrow = 0, ncolumn = 0;
  char \*\*azResult;
  int i;

  //从数据表查询数据
  sql = "SELECT \* FROM SensorData ";
  sqlite3_get_table( db , sql , &azResult , &nrow , &ncolumn , &zErrMsg );

  printf( "row:%d column=%d" , nrow , ncolumn );
  printf( "The result of querying is :" );
  for( i=0 ; i<( nrow + 1 ) * ncolumn ; i++ ){
     printf( "azResult[%d] = %s \r\n", i , azResult[i] );
  }

  sqlite3_free_table( azResult );

  if( rc!=SQLITE_OK ){
        fprintf(stderr, "SQL error: %s\n", zErrMsg);
        sqlite3_free(zErrMsg);
  }
  sqlite3_close(db);

  return 0;
}
```

# 8. 编写Makefile

```bash
prefix=/home/at91sam9/sqlite-amalgamation-3.6.1/sqlite-arm
exec_prefix=${prefix}
libdir=${exec_prefix}/lib
includedir=${prefix}/include
Libs = -L${libdir} -lsqlite3 -lpthread
Cflags = -I${includedir}
CROSS_COMPILE = arm-none-linux-gnueabi-
CC = $(CROSS_COMPILE)gcc
LD = $(CROSS_COMPILE)ld
radiodb: radiodb.o
        $(CC) $^ $(Libs)  -o $@
radiodb.o:radiodb.c
        $(CC) $(Cflags) -c $^ -o $@
clean:
        rm -rf radiodb  *.o
in:
        cp radiodb /srv/nfs/sqlite
```

注意makefile里的红色字体内容可以在sqlite安装目录的lib下的pkconfig下的文件查得。
