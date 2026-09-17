# RuoYi-AI（若依 AI）· 学习 / 二次开发仓库

> 仓库 `ruoyi-ai2`（GitHub: https://github.com/kll237/ruoyi-ai2）
>
> ⚠️ **来源说明**：本仓库基于开源项目 **RuoYi-AI**（上游：https://github.com/ageerle/ruoyi-ai）进行学习与二次开发，**并非从零原创**。仅用于个人学习、功能验证与本地定制；如需用于生产环境，请遵循上游许可证与官方文档。所有上游代码的著作权归原作者 / 原组织所有。

---

## 一、项目简介

RuoYi-AI 是一套集成了大语言模型（LLM）对话能力的前后端分离后台管理系统：前端基于 Vue3 + Vben Admin 脚手架，后端基于 Spring Boot（Maven）。本仓库在其上游基础上做了本地化定制与学习性修改，作为个人 AI 全栈学习的练习载体。

---

## 二、技术架构

| 层 | 选型 |
| --- | --- |
| 前端管理后台 | Vue 3 + TypeScript + Vite + Ant Design Vue（基于 Vben Admin 脚手架） |
| 前端构建 | pnpm + Turbo（monorepo，workspace） |
| 后端 | Java + Spring Boot（Maven，见 `ruoyi-ai/pom.xml`） |
| AI 能力 | 大模型对话 / 知识库（以 `ruoyi-ai` 后端模块实现为准） |
| 部署 | Docker Compose（`ruoyi-ai/docker-compose.yml`） |

```
浏览器
  │  HTTPS
  ▼
┌─────────────────────┐        ┌─────────────────────────┐
│  Vue3 前端           │◀──────▶│  Spring Boot 后端        │
│ (ruoyi-admin /       │        │  (ruoyi-ai, Maven)      │
│  ruoyi-ai-web)       │        └────────────┬────────────┘
└─────────────────────┘                     │
                                    ┌────────┴────────┐
                                    ▼                 ▼
                                AI 服务          数据库 / 向量库
```

---

## 三、目录结构

```
.
├── ruoyi-admin/              # 前端管理后台（基于 Vben Admin）
│   ├── apps/web-antd/        # Ant Design Vue 版本应用入口
│   ├── package.json          # monorepo 根（pnpm + turbo）
│   ├── .changeset/           # 版本变更集
│   └── README.md             # 前端子项目说明（含技术栈与启动命令）
├── ruoyi-ai/                 # 后端（Spring Boot）+ 部署与文档
│   ├── pom.xml               # Maven 构建
│   ├── ruoyi-admin/          # 后端业务模块（src/main/java/org/...）
│   ├── docker-compose.yml    # 后端容器编排
│   ├── docs/                 # 部署教程 / 工作流模块说明
│   └── README_EN.md          # 后端英文说明（来自上游）
└── .gitignore
```

> 说明：`ruoyi-ai/source-code/ruoyi-ai-web/` 为前端源码的镜像拷贝；其中 `.pnpm-store/` 为 pnpm 依赖缓存，**不应纳入版本控制**，已在 `.gitignore` 中排除。

---

## 四、环境要求

| 依赖 | 版本 / 说明 |
| --- | --- |
| JDK | 17+ |
| Maven | 3.8+ |
| Node.js | ≥ 20.10.0 |
| pnpm | ≥ 9.12.0（项目强制，请勿使用 npm / yarn） |

---

## 五、快速开始

### 后端（Spring Boot）

```bash
cd ruoyi-ai
# 按需要修改应用配置（application-dev.yml / application-prod.yml 已被 .gitignore 忽略）
mvn clean package -DskipTests
# 或直接使用容器编排
docker compose up -d
```

### 前端（Vben Admin）

```bash
cd ruoyi-admin
pnpm install
pnpm run dev:antd       # 启动 Ant Design Vue 版开发服务器
pnpm run build:antd     # 构建生产版本
```

---

## 六、二次开发记录（本仓库相对上游的定制点）

> 本节用于沉淀相对于上游的本地修改，便于复盘与回溯。**请按实际改动补充。**

- 在仓库根目录补充本说明文件，明确项目来源与定位；
- 将误提交的 `ruoyi-ai/source-code/ruoyi-ai-web/.pnpm-store` 依赖缓存移出版本追踪（见 `.gitignore`）；
- （待补充：你实际新增 / 修改的接口、页面、模型接入方式、提示词等）

---

## 七、许可证

遵循上游 RuoYi-AI 的许可证（详见各子目录 `LICENSE` 与上游仓库），本说明文件不额外主张任何上游代码的著作权。
