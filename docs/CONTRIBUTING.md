# Chat2Dashboard 开发指南

> 本指南为 GitHub 协作开发者提供快速上手和贡献代码的详细指引

## 📋 目录
- [项目概述](#项目概述)
- [开发环境设置](#开发环境设置)
- [项目架构](#项目架构)
- [开发工作流](#开发工作流)
- [代码规范](#代码规范)
- [测试指南](#测试指南)
- [部署指南](#部署指南)
- [常见问题](#常见问题)
- [贡献指南](#贡献指南)

## 🎯 项目概述

Chat2Dashboard 是一个基于 AI 的数据可视化平台，旨在通过自然语言与数据进行交互，自动生成可视化图表。项目采用前后端分离架构：

- **后端**: FastAPI + Vanna + OpenAI，提供 AI 驱动的 SQL 生成和数据查询
- **前端**: React + Vite，提供现代化的用户界面

### 核心特性
- 🤖 AI 驱动的自然语言到 SQL 转换
- 📊 多种图表类型支持 (ECharts)
- 📁 文件上传和数据库管理
- 🧠 智能训练数据管理
- 📝 请求日志和系统监控

## 🛠 开发环境设置

### 前置要求
- **Python**: 3.11+
- **Node.js**: 18+
- **uv**: Python 包管理器 ([安装指南](https://docs.astral.sh/uv/getting-started/installation/))
- **OpenAI API Key**: 或兼容的 API 服务

### 1. 克隆项目
```bash
git clone https://github.com/your-org/chat2dashboard.git
cd chat2dashboard
```

### 2. 后端设置
```bash
cd backend

# 安装依赖
uv sync

# 创建环境变量文件
cp .env.example .env
# 编辑 .env 文件，添加你的 OpenAI API Key
```

**环境变量配置** (`.env`):
```env
OPENAI_API_KEY=your_openai_api_key_here
OPENAI_API_BASE=https://api.openai.com/v1  # 可选，自定义 API 基地址
```

### 3. 前端设置
```bash
cd frontend

# 安装依赖
npm install

# 或使用 yarn
yarn install
```

### 4. 启动开发服务器

**后端服务器**:
```bash
cd backend
uv run python main.py
```
- API 服务: http://localhost:8000
- Web 界面: http://localhost:8000

**前端开发服务器**:
```bash
cd frontend
npm run dev
```
- 前端界面: http://localhost:5173

## 🏗 项目架构

### 后端架构 (`backend/`)
```
backend/
├── app/                    # 主应用目录
│   ├── main.py            # FastAPI 应用入口
│   ├── config.py          # 应用配置
│   ├── api/v1/            # API 路由
│   │   ├── database.py    # 数据库管理
│   │   ├── visualization.py # 图表生成
│   │   ├── schema.py      # 模式管理
│   │   ├── logs.py        # 日志管理
│   │   └── system.py      # 系统状态
│   ├── core/              # 核心业务逻辑
│   │   ├── agent.py       # DBAgent (Vanna 集成)
│   │   ├── sql_generator.py # AI SQL 生成
│   │   ├── database.py    # 数据库工具
│   │   └── html_generator/ # 图表生成器
│   ├── models/            # Pydantic 模型
│   ├── services/          # 业务服务层
│   └── utils/             # 工具函数
├── databases/             # SQLite 数据库存储
├── logs/                  # 日志文件
└── templates/             # HTML 模板
```

### 前端架构 (`frontend/`)
```
frontend/
├── src/
│   ├── components/        # 可复用组件
│   ├── pages/            # 页面组件
│   ├── context/          # React Context
│   └── assets/           # 静态资源
├── public/               # 公共资源
└── examples/             # 图表示例
```

### 关键技术栈

**后端**:
- **FastAPI**: Web 框架
- **Vanna**: AI SQL 生成框架
- **OpenAI**: 大语言模型 API
- **ChromaDB**: 向量数据库
- **SQLite**: 数据存储
- **Pandas**: 数据处理

**前端**:
- **React**: UI 框架
- **Vite**: 构建工具
- **ECharts**: 图表库

## 🔄 开发工作流

### 1. 分支策略
- `main`: 主分支，生产环境代码
- `develop`: 开发分支，集成最新功能
- `feature/*`: 功能分支
- `bugfix/*`: 修复分支

### 2. 开发流程
1. **创建功能分支**:
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **开发和测试**:
   ```bash
   # 后端测试
   cd backend
   uv run python -m pytest
   
   # 前端测试
   cd frontend
   npm test
   ```

3. **提交代码**:
   ```bash
   git add .
   git commit -m "feat: add your feature description"
   ```

4. **推送和创建 PR**:
   ```bash
   git push origin feature/your-feature-name
   # 在 GitHub 上创建 Pull Request
   ```

### 3. 提交信息规范
使用 [Conventional Commits](https://www.conventionalcommits.org/) 规范:

```
<type>(<scope>): <description>

[optional body]
```

**类型**:
- `feat`: 新功能
- `fix`: 修复 bug
- `docs`: 文档更新
- `style`: 代码格式调整
- `refactor`: 重构
- `test`: 测试相关
- `chore`: 构建/工具相关

**示例**:
```
feat(backend): add SQL generation caching
fix(frontend): resolve chart rendering issue
docs: update API documentation
```

## 📝 代码规范

### Python 代码规范
- 遵循 **PEP 8** 标准
- 使用 **type hints**
- 类和函数需要 **docstring**
- 最大行长度: **88 字符**

**推荐工具**:
```bash
# 安装开发工具
uv add --dev black ruff pytest

# 代码格式化
black app/
ruff check app/
```

### JavaScript/React 规范
- 使用 **ES6+** 语法
- 组件使用 **函数式组件** + Hooks
- 遵循 **React 最佳实践**
- 使用 **ESLint** 检查

### API 设计规范
- 遵循 **RESTful** 设计原则
- 使用合适的 **HTTP 状态码**
- 统一的 **错误响应格式**
- 完整的 **API 文档** (FastAPI 自动生成)

## 🧪 测试指南

### 后端测试
```bash
cd backend

# 运行所有测试
uv run python -m pytest

# 运行特定测试文件
uv run python -m pytest tests/test_agent.py

# 运行覆盖率测试
uv run python -m pytest --cov=app
```

### 前端测试
```bash
cd frontend

# 运行单元测试
npm test

# 运行端到端测试
npm run test:e2e
```

### 测试数据
项目提供示例数据库，位于 `backend/databases/fitbit/`，包含健身数据用于开发测试。

## 🚀 部署指南

### 开发环境
```bash
# 后端
cd backend && uv run python start.py

# 前端
cd frontend && npm run dev
```

### 生产环境
```bash
# 构建前端
cd frontend && npm run build

# 启动后端 (生产模式)
cd backend && uv run uvicorn app.main:app --host 0.0.0.0 --port 8000
```

## ❓ 常见问题

### Q: OpenAI API 调用失败？
**A**: 检查以下几点:
1. API Key 是否正确设置
2. 网络连接是否正常
3. API 配额是否充足
4. 如使用代理 API，检查 `OPENAI_API_BASE` 配置

### Q: 数据库连接失败？
**A**: 确认:
1. 数据库文件路径是否正确
2. 权限是否足够
3. SQLite 是否正确安装

### Q: 前端页面空白？
**A**: 检查:
1. 后端 API 是否正常运行
2. CORS 配置是否正确
3. 浏览器控制台是否有错误信息

### Q: 图表不显示？
**A**: 可能原因:
1. 数据格式不正确
2. ECharts 加载失败
3. 图表类型不支持

## 🤝 贡献指南

### 如何贡献
1. **Fork** 项目到你的 GitHub 账号
2. **Clone** 到本地开发环境
3. **创建分支** 开发新功能或修复 bug
4. **遵循代码规范** 和测试要求
5. **提交 PR** 并描述修改内容
6. **等待 Code Review** 和合并

### PR 检查清单
- [ ] 代码遵循项目规范
- [ ] 添加了必要的测试
- [ ] 更新了相关文档
- [ ] 所有测试通过
- [ ] 提交信息符合规范

### 代码审查标准
- **功能完整性**: 是否实现了预期功能
- **代码质量**: 是否遵循最佳实践
- **测试覆盖**: 是否有足够的测试
- **文档完整**: 是否更新了相关文档
- **向后兼容**: 是否影响现有功能

## 📞 获取帮助

- **Issue 报告**: [GitHub Issues](https://github.com/JasonDZS/chat2dashboard/issues)
- **项目文档**: 查看 `docs/` 目录下的其他文档

## 📄 许可证

本项目采用 MIT 许可证，详见 [LICENSE](../LICENSE) 文件。

---

**Happy Coding! 🎉**

有任何问题或建议，欢迎通过 Issue 或 Discussion 与我们交流！