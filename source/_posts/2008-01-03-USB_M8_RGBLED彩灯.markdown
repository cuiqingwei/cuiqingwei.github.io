---
layout: post
title:  "USB_M8_RGBLED彩灯"
subtitle: "USB控制用Mega8控制RGBLED"
date:    2008-01-03 09:30:15
tags:
  - 折腾
  - DIY
---

用M8控制RGBLED测试了一下，效果不错。并做了一个上位机软件，通过USB控制，现将资料整理上传与君共享！

# 原理图
![原理图](http://cache.amobbs.com/bbs_upload782111/files_9/ourdev_198395.PNG)
<!--more-->
# 源代码
``` c
/******************************************************************************
* rgbCtrl.h
*
* Copyright XiaoCui' Products
*
* DESCRIPTION:
*
* modification history
* --------------------
* 01a, 01jan2008, cuiqingwei written
* --------------------
******************************************************************************/
#include <avr/io.h>
#include <avr/interrupt.h>
#include <avr/pgmspace.h>
#include <avr/wdt.h>
#include "usbdrv.h"
/* Explanation of the HSV color space:
H: Hue        - 色调(如红，蓝，黄，绿...)
S: Saturation - 饱和度
V: Value      - 亮度
Scaling of the HSV values:
H: [0-255] 0 = red, 42 = yellow, 85 = green, 128 = turquoise, 171 = blue, 214 = purple
S: [0-255] 0 = white, 255 = pure colors
V: [0-255] 0 = off,   255 = maximum brightness */
typedef struct {
    unsigned char r;
    unsigned char g;
    unsigned char b;
} RGB;
#define Ledport         PORTD            // RGB Led Port
#define DDR_Ledport     DDRD
#define R_PIN           5                // R Output
#define G_PIN           6                // G
#define B_PIN           7                // B
/* 函数声名 */
void init(void);
void hsv_to_rgb (unsigned char h, unsigned char s, unsigned char v, RGB * out);
/* 全局变量 */
RGB lamp = {0,0,0};
unsigned char type,H,S,V;
//-----------------------------------------------------------------------------
// Init
//-----------------------------------------------------------------------------
void init(void) {
    PORTD = 0;
    PORTB = 0;                  // no pullups on USB and ISP pins
    DDRD = ~(1 << 2);           // all outputs except PD2 = INT0
    DDRB = 0;                   // all USB and ISP pins inputs
    DDR_Ledport = (1<<R_PIN)|(1<<G_PIN)|(1<<B_PIN);
    TCCR0 = 1;
    TIMSK = (1<<TOIE0);        // Overflow Interrupt f/256  
}
uchar usbFunctionSetup(uchar data[8]) {
    usbRequest_t \*rq = (void \*)data;
    if( rq->bRequest ) {
        type = 1;
        H = rq->wValue.bytes[0];
        S = rq->wValue.bytes[1];
        V = rq->wValue.bytes[2];
    }
    else {
        type = 0;
        lamp.r = rq->wValue.bytes[0];
        lamp.g = rq->wValue.bytes[1];
        lamp.b = rq->wValue.bytes[2];
    }
    //computeOutputStatus();
    return 0;
}

//-----------------------------------------------------------------------------
// Main
//-----------------------------------------------------------------------------
int main(void) {        
    cli();
    init();
    usbInit();
    sei();
    H = 180;
    S = 255;
    V = 355;
    type = 1;
    for(;;) {
        usbPoll();
        if ( type ) {
            type = 0;
            hsv_to_rgb(H,S,V,&lamp);
        }
    }
    return 0;
}
//-----------------------------------------------------------------------------
// Converts HSV to RGB
//-----------------------------------------------------------------------------
void hsv_to_rgb (unsigned char h, unsigned char s, unsigned char v, RGB * out)
{
    unsigned char r=0,g=0,b=0, i, f;
    unsigned int p, q, t;
    if( s == 0 ) {        
        r = g = b = v;
    }
    else {        
        i=h/43;
        f=h%43;
        p = (v * (255 - s))/256;
        q = (v * ((10710 - (s * f))/42))/256;
        t = (v * ((10710 - (s * (42 - f)))/42))/256;
        switch( i ) {        
            case 0:
                r = v; g = t; b = p; break;
            case 1:
                r = q; g = v; b = p; break;
            case 2:
                r = p; g = v; b = t; break;
            case 3:
                r = p; g = q; b = v; break;                        
            case 4:
                r = t; g = p; b = v; break;                                
            case 5:
                r = v; g = p; b = q; break;
        }
    }
    out->r=r; out->g=g; out->b=b;
}
//-----------------------------------------------------------------------------
// Timer  0 Interrupt, f/256
//-----------------------------------------------------------------------------
SIGNAL (SIG_OVERFLOW0) {        
    static unsigned char PWM_cnt;
    static unsigned char prescale;
    if(prescale++) {                                
        prescale=0;                // Used to halve the PWM frequency
        return;
    }
    if (PWM_cnt <lamp.r)
        Ledport |= (1 << R_PIN);
    else
        Ledport &=~(1 << R_PIN);
    if (PWM_cnt <lamp.g)
        Ledport |= (1 << G_PIN);
    else
        Ledport &=~(1 << G_PIN);
    if (PWM_cnt <lamp.b)
        Ledport |= (1 << B_PIN);
    else
        Ledport &=~(1 << B_PIN);
    PWM_cnt++;
}
/*-----------------------------------------------------------------------------
                                0ooo
                       ooo0     (   )
                       (   )     ) /
                        \ (     (_/
                         \_)        By:cuiqingwei [gary]
-----------------------------------------------------------------------------*/
```
# PC软件界面
![界面](http://cache.amobbs.com/bbs_upload782111/files_9/ourdev_196733.png)

# 调光效果
![效果](http://cache.amobbs.com/bbs_upload782111/files_9/ourdev_196737.JPG)

# 调试一角
![调试](http://cache.amobbs.com/bbs_upload782111/files_9/ourdev_196739.JPG)

# 资料包
- [Mega8源码(文件大小:1.52M)](http://cache.amobbs.com/bbs_upload782111/files_9/ourdev_196735.rar)
- [上位机源码(文件大小:989K)](http://cache.amobbs.com/bbs_upload782111/files_9/ourdev_198432.rar)
- 所用控件
1. [iocomp.v3.04.SP2.Delphi.BCB(文件大小:336K)](http://cache.amobbs.com/bbs_upload782111/files_9/ourdev_198433.rar)
2. [AAFont 平滑特效字体控件包 V2.63(文件大小:12.28M)](http://cache.amobbs.com/bbs_upload782111/files_9/ourdev_198434.rar)

---

*发表链接*

amoBBS 阿莫电子论坛 [http://www.amobbs.com/thread-883719-1-1.html](http://www.amobbs.com/thread-883719-1-1.html)
