---
title: 本地安装部署deepseek
date: 2025-09-11 15:16:05
categories: AI
tags:
  - AI
  - devops
---

## 本地安装部署 deepseek

> `DeepSeek`的本地安装可通过`Ollama`框架实现，支持`Windows`、`MacOS`和`Linux`系统，具体步骤包括环境准备、`Ollama`安装、模型下载及运行。

- [DeepSeek 官网](https://www.deepseek.com/)
- [DeepSeek 在线聊天](chat.deepseek.com)

### ‌ 系统要求 ‌

- ‌ 操作系统 ‌：

  - Windows 10 及以上（需启用 WSL 2）。
  - MacOS（Intel/Apple 芯片）。
  - Linux（推荐 Ubuntu 24.04）。‌‌

- ‌ 硬件配置 ‌：

  - 内存：最低 8GB，推荐 16GB 及以上。
  - 存储：至少 10GB 空闲空间。
  - GPU（可选）：NVIDIA 显卡可加速推理。‌‌

- ‌ 软件依赖 ‌：
  - Python 3.10+、Git、终端工具（如 PowerShell 或 Linux 终端）。‌‌

### ‌ 安装步骤 ‌

1. ‌ 安装 Ollama‌：

   - Windows：下载[OllamaSetup.exe](https://ollama.com/download/windows)并运行，建议自定义安装路径（如 `E:\ollama`）。‌‌
   - MacOS/Linux：终端执行命令`curl -fsSL https://ollama.com/install.sh | sh`。‌‌

   校验是否安装成功：

   1. 打开终端：
      - Windows 按 `Win + R` 输入 `cmd`
      - Mac 直接打开 `Terminal`
   2. 输入命令：`ollama list` ，如果终端显示 `llama3` 之类的模型名称，说明已经安装成功！并且桌面会出现一个羊驼图标。

2. ‌ 下载 DeepSeek 模型 ‌：

   - 在[ollama 官网模型](https://ollama.com/search)里寻找`deepseek-r1`,点击进去就可以找到下载安装命令。
   - 运行命令`ollama pull deepseek-r1:7b`下载`7b`版本（`7B` 为常用版本，`1.5B` 适合低配置设备，高性能显卡用户（显存 16GB 以上）可选 `16B` 版本）。‌‌

3. ‌ 运行模型 ‌：

   - 命令行输入`ollama run deepseek-r1:7b`启动交互。‌‌
   - 可选 Web UI：执行`ollama web`后访问[http://localhost:11434]。‌‌

### 图形界面工具

#### AnythingLLM

> [AnythingLLM](https://anythingllm.com/):这是一个全栈应用程序，可以将任何文档、资源（如网址链接、音频、视频）或内容片段转换为上下文，以便任何大语言模型（LLM）在聊天期间作为参考使用。此应用程序允许您选择使用哪个 LLM 或向量数据库，同时支持多用户管理并设置不同权限。

[github 源码](https://github.com/Mintplex-Labs/anything-llm)

#### Chatbox

> [Chatbox](https://chatboxai.app/zh)：Chatbox AI 是一款 AI 客户端应用和智能助手，支持众多先进的 AI 模型和 API，可在 Windows、MacOS、Android、iOS、Linux 和网页版上使用 。

[github 源码](https://github.com/chatboxai/chatbox)

设置 —> 模型提供方（本地提供） 里选择：

○ API 类型：Ollama API

○ 模型名称：deepseek-r1:8b

### 开发集成

> 工作流编排

#### Dify

> Dify 是由腾讯云团队开发的开源 AI 应用开发平台，专注于将大语言模型转化为可落地生产力工具。其核心功能包括可视化工作流编排、多模态交互、知识库管理及灵活部署能力，支持企业快速构建 AI 应用。 ‌

- [官网](https://dify.ai/)
- [github 源码](https://github.com/langgenius/dify/)

核心功能

- ‌ 可视化开发 ‌：通过拖拽式界面设计工作流，无需编写代码即可构建包含 RAG 检索、自动化任务的智能应用。 ‌
- ‌ 多模型支持 ‌：接入 OpenAI、Anthropic、百度、阿里等主流模型，支持本地或云端部署（AWS、阿里云等）。 ‌
- ‌ 低代码集成 ‌：提供 API 管理、权限控制及系统对接能力，支持接入企业自有数据和系统。 ‌

#### n8n

> n8n 是一款开源的工作流自动化平台,它提供了一个低代码平台,允许用户通过拖放操作创建复杂的工作流,无需编写大量代码。

- [官网](https://n8n.io/)
- [github 源码](https://github.com/n8n-io/n8n)

#### Coze 扣子

> 扣子开发平台是一站式 Al Agent 开发工具。提供各类最新大模型和工具、多种开发模式和框架，从开发到部署，为你提供最便捷的 Agent 开发环境。上万家企业、数百万开发者正在使扣子开发平台。

- [官网](https://www.coze.cn)
- [扣子空间](https://space.coze.cn/?category=7524912604796452873)
- [github 源码](https://github.com/coze-dev/coze-studio)

### DeepSeek 版本

> DeepSeek 目前主要分为 `‌V系列`（通用大模型）‌ 和 `‌R系列`（推理优化模型）‌，此外还有一些专项模型。以下是各版本的详细介绍：

#### ‌V 系列（通用大模型）‌

1. ‌DeepSeek-V1‌

   - 发布时间 ‌：2024 年 1 月
   - 特点 ‌：
     - 编码能力强，支持 Python、Java 等编程语言，适合自动化代码生成
     - 长上下文窗口（128K 标记），适合技术文档分析
   - 局限性 ‌：多模态能力弱，复杂推理能力不足
   - **硬件推荐**：
     - GPU: RTX 3090/4090 (24GB 显存)
     - CPU: 8 核以上
     - 内存: 16GB+

2. ‌DeepSeek-V2‌

   - 发布时间 ‌：2024 年上半年
   - 特点 ‌：
     - 2360 亿参数，性能接近 GPT-4 Turbo，训练成本仅为 GPT-4 Turbo 的 1%
     - 完全开源且免费商用，适合科研和商业化应用
   - 局限性 ‌：推理速度较慢，多模态支持有限
   - **硬件推荐**：
     - GPU: A100 40GB 或多卡并行
     - CPU: 16 核服务器级
     - 内存: 64GB+

3. ‌DeepSeek-V2.5‌

   - 发布时间 ‌：2024 年 9 月
   - 特点 ‌：
     - 数学推理能力提升（MATH-500 准确率 82.8%），支持联网搜索
     - 整合对话优化和代码生成模型，通用任务能力增强
   - 局限性 ‌：联网搜索功能未开放 API 接口

4. ‌DeepSeek-V3‌
   - 发布时间 ‌：2024 年 12 月（基础版），2025 年 3 月（升级版 V3-0324）
   - 特点 ‌：
     - 6710 亿参数（MoE 架构），推理速度提升至 60 TPS
     - 支持 FP8 权重开源，可在消费级硬件（如苹果 M3 Ultra）部署
     - 在数学竞赛（AIME 2024）和代码生成任务中表现优异
   - **硬件推荐**：
     - GPU: H100 集群
     - CPU: 32 核以上
     - 内存: 128GB+

#### ‌R 系列（推理优化模型）‌

1. ‌DeepSeek-R1‌

   - ‌ 特点 ‌：
     - 专为推理优化，在数学、编程和自然语言推理任务中表现突出
     - 参数规模包括 7B、14B、32B、70B 和 671B 版本，适用于不同场景
   - ‌ 适用场景 ‌：
     - ‌7B/14B‌：实时对话、简单问答
     - ‌32B/70B‌：复杂推理、多模态数据处理（如医疗诊断）
     - ‌671B（满血版）‌：高性能计算环境，如学术研究、代码生成
   - **硬件推荐**：
     - 7B: RTX 3090 (24GB)
     - 70B: A100 80GB 集群
     - 671B: 32 块 H100

2. ‌DeepSeek-R1-Lite（预览版）‌

   - ‌ 特点 ‌：轻量级推理模型，适合资源受限的场景

#### ‌ 其他专项模型 ‌

1. ‌Coder 系列 ‌：专注于代码生成，如 DeepSeek Coder

2. ‌MoE 系列 ‌：混合专家模型，如 DeepSeek-V3

3. ‌DeepSeek-VL‌：多模态模型，支持图像和文本处理

### 硬件配置要求

1. 小型模型（1B~7B 参数）‌

   - 适用场景 ‌：轻量级推理（如聊天机器人、代码补全）、低资源设备部署（树莓派、旧笔记本）‌
   - 推荐配置 ‌：
     - ‌GPU‌：NVIDIA RTX 3090/4090（24GB 显存）或 Tesla T4（16GB 显存）‌
     - ‌CPU‌：8 核以上（如 Intel i7/i9 或 AMD Ryzen 7/9）‌
     - 内存 ‌：16GB DDR4 及以上 ‌
     - 存储 ‌：500GB NVMe SSD（模型文件约 10~30GB）‌
   - 最低配置 ‌：
     - ‌CPU‌：4 核（纯 CPU 推理）‌
     - 内存 ‌：8GB（1.5B 模型）或 16GB（7B 模型）‌
     - 显卡 ‌：可选 4GB 显存（如 GTX 1650）‌

2. 中型模型（13B~30B 参数）‌

   - 适用场景 ‌：企业级应用（智能客服、合同分析）、复杂 NLP 任务（长文本生成）‌
   - 推荐配置 ‌：
     - ‌GPU‌：NVIDIA A100 40GB 或多张 RTX 3090/4090（通过 NVLink 互联）‌
     - ‌CPU‌：16 核以上（如 Intel Xeon 或 AMD EPYC）‌
     - 内存 ‌：64GB DDR4 及以上 ‌
     - 存储 ‌：1TB NVMe SSD（模型文件约 50~100GB）‌
   - 关键优化 ‌：需多卡并行推理，支持 4-bit 量化降低显存需求 ‌

3. 大型模型（70B+参数）‌

   - 适用场景 ‌：科研计算、金融预测、多模态分析 ‌
   - 推荐配置 ‌：
     - ‌GPU‌：8+块 NVIDIA H100（80GB 显存）或 A100 集群 ‌
     - ‌CPU‌：32 核以上服务器级处理器（如 AMD EPYC 9684X）‌
     - 内存 ‌：128GB DDR5 ECC 及以上 ‌
     - 存储 ‌：4TB NVMe SSD（分布式存储）‌
   - 网络要求 ‌：InfiniBand NDR 400G（低延迟高吞吐）‌

4. 满血版（671B 参数）‌
   - 适用场景 ‌：超大规模训练、高并发服务 ‌
   - 硬件需求 ‌：
     - ‌GPU‌：32 块 H100（80GB 显存）或 A100 集群 ‌
     - ‌CPU‌：256 核以上（如 Intel Xeon Platinum 8490H）‌
     - 内存 ‌：2TB DDR5 ECC‌
     - 存储 ‌：50TB NVMe SSD‌
   - 部署建议 ‌：需多节点分布式训练，显存需求超 1.3TB‌

#### 关键注意事项 ‌

- 显存优先原则 ‌：显存带宽直接影响生成速度，建议选择支持 FP16/INT8 量化的显卡 ‌

- 存储优化 ‌：NVMe SSD 可显著提升模型加载速度，PCIe 4.0/5.0 接口更佳 ‌

- 电源冗余 ‌：高配硬件需搭配冗余电源（如 1200W 以上）‌
