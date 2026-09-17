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

> 已与上游 `ageerle/ruoyi-ai`（main 分支）做整库比对核实。结论：本仓库**新增了一个独立后端模块 `ruoyi-modules-api`**（约 359 个 Java 文件 / 2.6 万行），在复用上游 `ruoyi-chat` 的基础上扩展了「支付订单、令牌与用量计费、Uniapp 移动端适配」等能力；其他增量还包括课程设计文档、本地部署配置与仓库布局重组。既有上游模块本身未被改动（约 155 个被比对出不同的 `.java` 文件，经验证均为上游版本演进造成的漂移，本仓库副本为较旧快照）。

### 6.1 核心增量：课程设计文档（根目录，上游不含）

作为课程 / 毕业设计载体，在仓库根目录补充了完整的软件工程分析文档：

- `软件设计规格说明书_用例分析与类设计.md`：用例分析（13 个用例 UC1–UC13：用户登录、AI 聊天问答、语音输入、文件管理、数字人交互、用户数据分析等）+ 类设计（用户 / 聊天 / 文件 / 数字人四大模块实体与关系）+ 数据库设计（`sys_user`、`chat_session`、`chat_message`、`chat_model`、`sys_file`、`digital_human` 等核心表）。
- `图2.1_系统用例图.md` / `图2.2_用例关系图.md` / `图3.1_系统类图.md` / `图3.2_类关系类型说明图.md`：对应 UML 图（ASCII）。
- `图表预览.html`：文档图表汇总预览页。

> 上述文档将本仓库定位为一个「基于 RuoYi-AI 的 AI 智能管理系统」：集成 DeepSeek 大模型对话（SSE 流式）、2D 数字人（Live2D）交互、文件管理（MinIO）、用户数据分析等模块，配套 RBAC 权限体系。文档即本仓库的主要分析产出。

### 6.2 部署与配置（本仓库新增）

- `ruoyi-ai/docker-compose.yml`：后端容器编排（上游无，属本地部署补充）。
- `ruoyi-ai/.env`：运行期环境变量配置。
- `ruoyi-ai/README_EN.md`：后端英文说明。

### 6.3 仓库布局重组与新增后端模块

将上游根目录的后端模块（`ruoyi-admin` / `ruoyi-common` / `ruoyi-extend` / `ruoyi-modules` 等）整体归入 `ruoyi-ai/` 子目录，并新增 `source-code/`（含 `ruoyi-ai-web` 前端镜像）、`workspace/`、`script/`。

其中 **`ruoyi-modules-api/` 是相对上游的全新 Maven 模块**（上游仓库根目录无此模块），其内部 `ruoyi-chat-api` 子模块在复用上游 `ruoyi-chat`（代码 `@author` 标记均为 `ageerle`，属对上游的适配搬运）的基础上，新增了以下上游完全不存在的领域实体与配套 Service / Mapper：

- `ChatPayOrder`（支付订单）：含 `ChatPayOrderServiceImpl`，实现订单查询、分页、增改与删除等业务逻辑（`chat_pay_order` 表）。
- `ChatToken` / `ChatUsageToken`（令牌与用量/配额计费）：`ChatTokenServiceImpl` 提供按用户+模型查询、重置、编辑令牌；`ChatUsageTokenServiceImpl` 提供用量计费记录的全套 CRUD。
- `McpInfo` / `PromptTemplate`（MCP 服务信息、提示词模板）实体类及对应 VO / BO / Mapper / Service。
- `ChatMessageForUniappBo`：面向 **Uniapp（移动端）** 的聊天消息适配 BO，表明该模块额外提供了移动端 API 适配层。

> 该模块约 359 个 Java 文件、约 2.6 万行，属于在开源聊天基座之上的**功能扩展（支付 + 配额 + 移动端 API）**，而非从零自研的聊天内核。

### 6.4 与上游的差异说明（重要）

- 经 `diff` 整库比对，约 531 个文件内容不同。差异分两类：
  1. **功能性新增（本仓库贡献）**：`ruoyi-modules-api/` 模块（见 6.3），其中的 `ChatPayOrder` / `ChatToken` / `ChatUsageToken` / `McpInfo` / `PromptTemplate` 等实体在整个上游仓库中均检索不到（0 个），属本仓库新增领域模型与配套 CRUD 逻辑；`ChatMessageForUniappBo` 表明新增了移动端 API 适配。
  2. **版本漂移（非本仓库修改）**：既有上游模块（`ruoyi-common` / `ruoyi-system` / `ruoyi-generator` / `ruoyi-admin`）中约 155 个 `.java` 文件内容不同，经抽样核对（如 `AuthController`、`StringUtils`）均为上游后续版本演进所致（上游已引入 SA-Token、JustAuth 社交登录、SSE、限流注解等，本仓库副本为较旧快照），**并非本仓库对既有逻辑的功能性修改**。
- 综上：本仓库**既有业务功能（AI 对话、数字人、文件管理、权限管理）来自上游基座，且未被改写**；但**确有新增的 `ruoyi-modules-api` 模块**，贡献了支付订单、令牌与用量计费、移动端 API 适配三类上游原本没有的能力。
- `.pnpm-store/` 等依赖缓存已通过 `.gitignore` 排除版本追踪。

### 6.5 简历使用提示

若用于简历 / 作品集，可如实表述为「基于开源 RuoYi-AI 进行二次开发，新增支付订单、令牌与用量计费、Uniapp 移动端 API 适配模块，并完成需求建模、架构设计与本地部署」，并附本仓库的设计文档作为分析能力佐证。

需注意的边界：**AI 对话、数字人、文件管理、RBAC 权限等基座功能来自上游，不宜表述为个人从零实现**；可主张的是在基座之上扩展出的 `ruoyi-modules-api` 模块（支付 / 配额 / 移动端适配）以及完整的软件工程分析设计文档。

---

## 七、许可证

遵循上游 RuoYi-AI 的许可证（详见各子目录 `LICENSE` 与上游仓库），本说明文件不额外主张任何上游代码的著作权。
