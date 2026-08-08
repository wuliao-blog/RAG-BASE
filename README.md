# Spring AI Project（RAG-BASE）

一个基于 **Spring AI** 的多模块实战练习项目，覆盖对话、函数调用、本地模型（Ollama）、多模态（文生图 / 语音合成）、以及 **RAG 检索增强生成** 等主流大模型应用开发场景。

> 仓库原名 `RAG-BASE`，核心亮点模块是 `springai_rag`（检索增强生成）与 `springai_all`（对话 + RAG + 函数调用综合示例）。

---

## 🧰 技术栈

| 类别 | 选型 |
|------|------|
| 语言 | Java 17 |
| 框架 | Spring Boot 3.3.8 |
| 大模型 SDK | Spring AI `1.0.0-M5`（Milestone） |
| 国产适配 | `spring-ai-alibaba-starter` `1.0.0-M5.1`（阿里云百炼 / 通义千问） |
| 构建 | Maven（多模块 `pom` 聚合） |
| 模型提供方 | DeepSeek（OpenAI 兼容接口）、Ollama（本地）、阿里云 DashScope（通义千问） |

---

## 📦 模块说明

父工程 `springAiProject`（`packaging=pom`）聚合以下 8 个子模块，每个模块都是独立可运行的 Spring Boot 应用（默认端口 `8899`）：

| 模块 | 功能 | 主要依赖 / 模型 |
|------|------|----------------|
| `springai_hello` | Spring AI 入门 Demo：基于 DeepSeek 的最简对话 | OpenAI Starter → `deepseek-chat` |
| `springai_chat` | 多种对话方式示例（`ChatClient` / `ChatModel` / `ChatController`） | OpenAI Starter → `deepseek-chat` |
| `springai_function` | **Function Calling**：让模型调用本地函数（计算器 `CalculatorService`） | OpenAI Starter → `deepseek-chat` |
| `springai_ollama` | **本地模型推理**：走 Ollama，无需联网 Key | Ollama Starter → `deepseek-r1:1.5b`（`http://localhost:11434`） |
| `springai_alibaba` | 阿里云百炼 / 通义千问 对话示例 | `spring-ai-alibaba-starter` → DashScope |
| `springai_other` | **多模态**：文生图（`ImageModelController`）+ 语音合成 TTS（`AudioModelController`） | `spring-ai-alibaba-starter` → DashScope |
| `springai_rag` | **RAG 检索增强**：`VectorStore` + `QuestionAnswerAdvisor`，`GET /rag?input=` | `spring-ai-alibaba-starter` → DashScope |
| `springai_all` | **综合示例**：对话 + RAG + 函数调用（招聘场景 `RecruitServiceFunction`） | `spring-ai-alibaba-starter` → DashScope |

> 各模块 `src/main/java/com/atguigu/ai/**` 下均有对应 `SpringAi*Application` 启动类与 `*Controller` 接口。

---

## 🚀 快速开始

### 1. 环境要求
- JDK 17+
- Maven 3.8+
- （可选）本地已安装并运行 [Ollama](https://ollama.com/)，用于 `springai_ollama` 模块：`ollama pull deepseek-r1:1.5b`

### 2. 获取代码
```bash
git clone https://github.com/wuliao-blog/RAG-BASE.git
cd RAG-BASE
```

### 3. 配置 API Key
每个模块的 `src/main/resources/application.properties` 中通过 `spring.ai.*` 配置模型与密钥。

> ⚠️ **安全建议**：不要把真实密钥写进 `application.properties` 并提交。推荐用**环境变量**注入（Spring Boot 会自动读取 `SPRING_AI_*` 环境变量覆盖配置）：

```bash
# DeepSeek / OpenAI 兼容（springai_hello / chat / function）
export SPRING_AI_OPENAI_API_KEY=你的_DeepSeek_Key
export SPRING_AI_OPENAI_BASE_URL=https://api.deepseek.com

# 阿里云 DashScope / 通义千问（springai_alibaba / other / rag / all）
export SPRING_AI_DASHSCOPE_API_KEY=你的_DashScope_Key
```

如需本地覆盖，也可在 `application.properties` 中填写（请勿提交到仓库）：
```properties
# DeepSeek 示例
spring.ai.openai.api-key=YOUR_KEY
spring.ai.openai.base-url=https://api.deepseek.com
spring.ai.openai.chat.options.model=deepseek-chat

# Ollama 本地（无需 Key）
spring.ai.ollama.base-url=http://localhost:11434
spring.ai.ollama.chat.options.model=deepseek-r1:1.5b

# 阿里云 DashScope
spring.ai.dashscope.api-key=YOUR_KEY
```

### 4. 构建与运行
```bash
# 构建全部模块
mvn clean package

# 单独运行某个模块（示例：RAG）
cd springai_rag
mvn spring-boot:run
# 浏览器访问：http://localhost:8899/rag?input=你的问题
```

---

## 📁 目录结构

```
RAG-BASE/
├── pom.xml                  # 父工程（聚合 8 个模块）
├── .gitignore               # 已忽略 target/、.idea/ 等
├── springai_hello/          # 入门 Demo
├── springai_chat/          # 对话示例
├── springai_function/       # 函数调用
├── springai_ollama/         # 本地 Ollama
├── springai_alibaba/        # 通义千问
├── springai_other/          # 多模态（图 / 语音）
├── springai_rag/            # RAG 检索增强
└── springai_all/            # 综合示例
```

---

## 🔒 安全须知

- 本项目依赖外部大模型 API，**密钥务必通过环境变量或本地未跟踪配置文件注入，切勿提交真实密钥**。
- 已提交的 `application.properties` 若曾包含密钥，请尽快在对应平台**轮换 / 作废**旧 Key，并清理仓库历史。
- 仓库已通过 `.gitignore` 忽略 `target/`（编译产物）与 `.idea/`（IDE 配置）。

---

## 📄 License

个人学习用途，遵循仓库默认协议。如需商用或转载请先联系作者。

---

> 作者：wuliao-blog ｜ 用途：大模型应用开发学习与实践
