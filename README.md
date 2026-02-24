```markdown
# GoSmart 

[![Go](https://img.shields.io/badge/Go-1.24+-00ADD8?logo=go&logoColor=white)](https://golang.org/)
[![Vue](https://img.shields.io/badge/Vue.js-3.x-4FC08D?logo=vue.js&logoColor=white)](https://vuejs.org/)
[![Gin](https://img.shields.io/badge/Gin-v1.9-00ADD8?logo=go&logoColor=white)](https://github.com/gin-gonic/gin)
[![GORM](https://img.shields.io/badge/GORM-v1.25-00ADD8?logo=go&logoColor=white)](https://gorm.io/)
[![ONNX Runtime](https://img.shields.io/badge/ONNX-Runtime-blue?logo=onnx&logoColor=white)](https://onnxruntime.ai/)

GoSmart 是一个基于 **Go (Gin)** 和 **Vue 3** 构建的现代化 AI 全栈应用平台。它集成了大语言模型（LLM）对话能力与本地图像识别功能，采用高并发、低耦合的架构设计，支持多种 AI 模型后端。

## ✨ 核心特性

### 🤖 智能对话助手 (AI Chat)
- **多模型支持**：
  - **OpenAI Compatible**：支持 GPT-3.5/4 等标准 OpenAI 接口模型。
  - **Ollama 本地模型**：支持接入本地运行的 Llama3, Mistral, DeepSeek 等开源模型，保护数据隐私。
- **流式响应 (Streaming)**：采用 Server-Sent Events (SSE) 技术，实现打字机效果的实时回复体验。
- **会话管理**：支持多会话窗口，自动保存聊天历史。
- **上下文记忆**：智能管理对话上下文，提供连贯的交流体验。

### 👁️ 本地图像识别 (Image Recognition)
- **隐私优先**：基于 **ONNX Runtime** 在服务器本地运行轻量级模型（如 MobileNetV2）。
- **高性能**：无需调用外部云服务 API，识别速度快，无额外 API 费用。
- **即时反馈**：支持上传图片即时分类，返回识别结果与置信度。

### ⚡ 高性能与高可用架构
- **异步消息处理**：集成 **RabbitMQ** 处理聊天消息持久化，实现削峰填谷，提升系统响应速度。
- **多级缓存**：使用 **Redis** 缓存会话状态与热点数据，减轻数据库压力。
- **数据持久化**：使用 **MySQL 8.0** 存储用户数据与完整聊天记录。
- **安全性**：
  - 完整的用户注册/登录流程。
  - **JWT** (JSON Web Token) 身份认证与鉴权。
  - 邮箱验证码验证。

## 🛠️ 技术栈

### 后端 (Backend)
| 组件 | 说明 |
| --- | --- |
| **Language** | Go 1.24+ |
| **Web Framework** | [Gin](https://github.com/gin-gonic/gin) |
| **ORM** | [GORM](https://gorm.io/) (MySQL 8.0) |
| **Cache** | [Redis](https://redis.io/) (go-redis/v8) |
| **Message Queue** | [RabbitMQ](https://www.rabbitmq.com/) (amqp) |
| **AI SDK** | [CloudWeGo/Eino](https://github.com/cloudwego/eino) (LLM 抽象) |
| **Inference** | [onnxruntime_go](https://github.com/yalue/onnxruntime_go) (本地推理) |
| **Auth** | JWT-Go |

### 前端 (Frontend)
| 组件 | 说明 |
| --- | --- |
| **Framework** | Vue 3 (Composition API) |
| **UI Library** | [Element Plus](https://element-plus.org/) |
| **Routing** | Vue Router 4 |
| **HTTP Client** | Axios |
| **Build Tool** | Vue CLI / Webpack |

## 🚀 快速开始

### 前置要求
确保您的环境已安装以下服务：
- **Go** >= 1.24
- **Node.js** >= 16
- **MySQL** >= 8.0
- **Redis** >= 6.0
- **RabbitMQ** >= 3.8
- *(可选)* **Ollama** (如果使用本地 LLM)

### 1. 后端部署

1. **克隆仓库**
   ```bash
   git clone https://github.com/lovits/GoSmart.git
   cd GoSmart
   ```

2. **配置环境**
   修改 `config/config.toml` 文件，配置您的数据库、Redis 和 RabbitMQ 连接信息：
   ```toml
   [mysqlConfig]
   user = "root"
   password = "your_password"
   # ... 其他配置
   ```

3. **设置 AI 模型环境变量**
   ```bash
   # 使用 OpenAI
   export OPENAI_API_KEY="sk-..."
   export OPENAI_MODEL_NAME="gpt-3.5-turbo"
   
   # 或使用本地 Ollama (需在代码/配置中指定 BaseURL)
   ```

4. **运行服务**
   ```bash
   go mod download
   go run main.go
   ```
   后端服务将启动在 `http://localhost:9090` (默认)。

### 2. 前端部署

1. **进入前端目录**
   ```bash
   cd vue-frontend
   ```

2. **安装依赖**
   ```bash
   npm install
   ```

3. **启动开发服务器**
   ```bash
   npm run serve
   ```
   访问 `http://localhost:8080` 即可使用。

## 🔌 API 接口概览

| 模块 | 方法 | 路径 | 描述 |
| --- | --- | --- | --- |
| **用户** | POST | `/api/v1/user/register` | 用户注册 |
| | POST | `/api/v1/user/login` | 用户登录 |
| | POST | `/api/v1/user/captcha` | 获取邮箱验证码 |
| **AI 对话** | GET | `/api/v1/AI/chat/sessions` | 获取当前用户的会话列表 |
| | POST | `/api/v1/AI/chat/send-new-session` | 创建新会话并发送消息 |
| | POST | `/api/v1/AI/chat/send-stream` | **流式**发送消息 (SSE) |
| | POST | `/api/v1/AI/chat/history` | 获取指定会话的历史记录 |
| **图像** | POST | `/api/v1/image/recognize` | 上传图片进行识别 |

> 所有 `/api/v1/AI` 和 `/api/v1/image` 接口均需要通过 Header 携带 `Authorization: Bearer <token>`。

## 📂 目录结构说明

```text
.
├── common/             # 公共模块
│   ├── aihelper/       # LLM 模型接口实现 (OpenAI/Ollama)
│   ├── image/          # 图像识别逻辑 (ONNX Runtime)
│   ├── rabbitmq/       # 消息队列封装
│   └── ...
├── config/             # 配置文件与读取逻辑
├── controller/         # 控制器层 (HTTP 请求处理)
├── service/            # 业务逻辑层 (核心业务处理)
├── dao/                # 数据访问层 (数据库操作)
├── model/              # 数据库模型定义 (User, Message 等)
├── middleware/         # Gin 中间件 (JWT 认证, CORS 等)
├── router/             # 路由定义
├── utils/              # 工具函数
└── vue-frontend/       # Vue 3 前端源代码
    ├── src/
    │   ├── views/      # 页面组件 (Login, AIChat, ImageRecognition)
    │   └── ...
```

## 🤝 贡献 (Contributing)

欢迎提交 Issue 和 Pull Request！

1. Fork 本仓库
2. 创建您的特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交您的更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启一个 Pull Request

## 📄 许可证 (License)

本项目采用 MIT 许可证。
