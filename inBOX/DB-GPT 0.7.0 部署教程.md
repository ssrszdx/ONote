---
title: "DB-GPT 0.7.0 部署教程"
source: "https://iyyh.net/archives/c0ababbd-8638-489c-84cd-267dbf886cb8#1.-%E5%89%8D%E7%BD%AE%E7%8E%AF%E5%A2%83"
author:
  - "[[yyhhyy's blog]]"
published:
created: 2025-02-27
description: "DB-GPT 0.7.0 部署教程介绍了如何使用 `uv` 工具管理依赖包，并详细说明了在 macOS、Linux 和 Windows 系统上安装 `uv` 的步骤。教程以 `openai-proxy` 代理模型为例，展示了如何拉取项目、创建 Python 虚拟环境并安装依赖包。配置文件部分讲解了如何修改语言和模型配置，特别是 API 密钥的设置。最后，教程提供了两种启动项目的方式，并指导用户通过访问 `http://localhost:5670/` 来启动项目。"
tags:
  - "clippings"
---
## 1\. 前置环境

因为`DB-GPT` 越来越强大了，只用`pip`来管理依赖包的话不太优雅，因此使用 [**uv**](https://github.com/astral-sh/uv) 来管理。我最近写的项目也再慢慢转 **uv** 了，也推荐大家慢慢转过来。

本教程讲解以 `代理模型` 为例， 也就是 `openai-proxy`

### 1.1 uv 安装

`macOS and Linux`:

```vim
curl -LsSf https://astral.sh/uv/install.sh | sh
```

`windows`(请在`powershell`执行):

```llvm
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

验证下是否成功安装`uv`:

```ebnf
uv -V
```

后续如果`uv`升级可以执行以下命令升级:

```crystal
uv self update
```

### 1.2 环境安装

#### 1.2.1 项目拉取

首先先拉取项目，并进入项目目录

```bash
git clone https://github.com/eosphoros-ai/DB-GPT.git
cd DB-GPT
```

#### 1.2.1 Python虚拟环境创建

`DB-GPT`当前支持的`python`版本为: `py310` 以及 `py311`，这边推荐使用`3.11`

```apache
uv venv --python=3.11
```

执行以上命令后，会在项目目录中生成 `.venv`文件

#### 1.2.2 依赖安装

- **OpenAI Proxy**（只用到代理模型，没有涉及到本地模型）:

`macOS and Linux`:

```dsconfig
uv sync --all-packages --frozen \
--extra "proxy_openai" \
--extra "rag" \
--extra "storage_chromadb" \
--extra "dbgpts"
```

`windows`:

```dsconfig
uv sync --all-packages --frozen --extra "proxy_openai" --extra "rag" --extra "storage_chromadb" --extra "dbgpts"
```

- **Local**（本地模型）:

`macOS and Linux`:

```dsconfig
uv sync --all-packages --frozen \
--extra "rag" \
--extra "storage_chromadb" \
--extra "hf" \
--extra "quant_bnb" \
--extra "dbgpts"
```

`windows`:

```dsconfig
uv sync --all-packages --frozen --extra "rag" --extra "storage_chromadb" --extra "hf" --extra "quant_bnb" --extra "dbgpts" 
```

耐心等待环境安装结束即可。

## 2\. 配置文件

配置文件在 `configs/dbgpt-proxy-openai.toml`

1. 修改语言

```ini
[system]
# 将 -en 改成 -zh 即可
language = "${env:DBGPT_LANG:-zh}"
```

2. 模型配置

```bash
# Model Configurations
[models]
[[models.llms]]
# 模型名称
name = "${env:LLM_MODEL_NAME:-gpt-4o}"
provider = "${env:LLM_MODEL_PROVIDER:-proxy/openai}"
# 代理模型 api地址
api_base = "${env:OPENAI_API_BASE:-https://api.openai.com/v1}"
# 代理模型 api-key 请按照以下格式补充
api_key = "${env:OPENAI_API_KEY:-sk-xxxxx}"
​
# embedding 模型同上  记得最后的api_key需要:填写引入嘛，不然就得写环境变量
[[models.embeddings]]
name = "${env:EMBEDDING_MODEL_NAME:-text-embedding-3-small}"
provider = "${env:EMBEDDING_MODEL_PROVIDER:-proxy/openai}"
api_url = "${env:EMBEDDING_MODEL_API_URL:-https://api.openai.com/v1/embeddings}"
api_key = "${env:OPENAI_API_KEY:-sk-xxxxx}"
```

**PS: 请记得把 models 中的 api\_key 都填写上！**

## 3\. 启动项目

两种方式可以启动

- 方式一：

```applescript
uv run dbgpt start webserver --config configs/dbgpt-proxy-openai.toml
```

- 方式二：

```dockerfile
uv run python packages/dbgpt-app/src/dbgpt_app/dbgpt_server.py --config configs/dbgpt-proxy-openai.toml
```

接下来访问 [http://localhost:5670/](http://localhost:5670/) 即可。