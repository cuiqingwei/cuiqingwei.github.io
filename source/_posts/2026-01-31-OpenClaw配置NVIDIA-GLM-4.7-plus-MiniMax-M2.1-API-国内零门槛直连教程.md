---
title: OpenClaw配置 NVIDIA GLM-4.7 + MiniMax M2.1 API，国内零门槛直连教程
date: 2026-01-31 16:30:00
tags: [OpenClaw, AI, GLM, MiniMax, NVIDIA]
categories: [技术教程]
---

闲言碎语不多讲，先上成果：
![](/img/20260131/image.png)

<!--more-->

![](/img/20260131/image1.png)

NVIDIA NIM 现在免费提供这两个模型的 API 调用：

| 模型 | 模型名称 | 特点 |
|------|----------|------|
| GLM-4.7 | z-ai/glm4.7 | 智谱最新旗舰，编程能力炸裂，前端审美在线 |
| MiniMax M2.1 | minimaxai/minimax-m2.1 | 多语言编程王者，Agent 任务稳如老狗 |

API 地址统一是：<https://integrate.api.nvidia.com/v1>

## 怎么获取 API Key？

- 打开 [build.nvidia.com](http://build.nvidia.com)
- 注册/登录 NVIDIA 账号
- 进入 Settings → API Keys
- 点击生成新的 API Key

具体细节这里不再赘述。搞定后，日常开发测试绰绰有余。

## 在OpenClaw中配置

修改~/.openclaw(clawdbot)目录下openclaw.json(clawdbot.json)在 `"providers": {` 后插入如下内容：

```json
"nvidia": {
  "baseUrl": "https://integrate.api.nvidia.com/v1",
  "apiKey": "你在build.nvidia.com申请到的API",
  "api": "openai-completions",
  "models": [
    {
      "id": "z-ai/glm4.7",
      "name": "z-ai",
      "reasoning": false,
      "input": ["text"],
      "cost": {
        "input": 0,
        "output": 0,
        "cacheRead": 0,
        "cacheWrite": 0
      },
      "contextWindow": 128000,
      "maxTokens": 8192
    },
    {
      "id": "minimaxai/minimax-m2.1",
      "name": "minimaxai",
      "reasoning": false,
      "input": ["text"],
      "cost": {
        "input": 0,
        "output": 0,
        "cacheRead": 0,
        "cacheWrite": 0
      },
      "contextWindow": 128000,
      "maxTokens": 8192
    }
  ]
}
```

通过 CLI 添加模型：

```bash
openclaw models set nvidia/minimaxai/minimax-m2.1
openclaw models list
```

## 需要✈️吗？

不需要。

NVIDIA NIM 的 API 地址 `integrate.api.nvidia.com` 在国内可以直接访问！

这可能是目前最省心的免费 API 选择了——不用折腾网络环境，不用担心封号，直接干就完了。

## 使用建议

### 适合用 GLM-4.7 的场景：

- 前端开发，尤其是需要好看 UI 的
- 一次性交付的编程任务
- 需要深度思考的复杂问题

### 适合用 MiniMax M2.1 的场景：

- 多语言项目（Java、Go、Rust 这些）
- 需要长时间运行的 Agent 任务
- 对响应速度有要求的场景

### 不太适合的场景：

- 需要图片输入的任务（GLM-4.7 不支持）
- 对延迟极度敏感的实时应用（毕竟是免费的，资源紧张时会变慢）

## 最后

NVIDIA 把国产顶流模型免费开放出来，既是给开发者发福利，也是在推广自家的 NIM 生态。

对于普通用户来说，这就是个体（白嫖）验的好机会。GLM-4.7 和 MiniMax M2.1 都是刚发布的新模型，能力确实不错。趁着现在资源还没被挤爆，赶紧去尝试吧~

---

**原文链接：** [CSDN 博客 - OpenClaw 配置 NVIDIA GLM-4.7 + MiniMax M2.1 API 教程](https://blog.csdn.net/cuiqingwei/article/details/157579351)

**作者：** cuiqingwei
