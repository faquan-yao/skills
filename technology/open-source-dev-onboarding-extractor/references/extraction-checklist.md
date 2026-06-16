# 提取检查清单 — 十个方面

使用此清单检查任意开源仓库。每项在草稿中记录依据（文件路径、符号、文档 URL）或标记 `未找到`；最终报告不设独立证据字段。

**产出语言：** 据此清单撰写的报告默认使用**简体中文**（路径、命令、符号名保留原文）。

---

## 方面 1：问题域与边界

**目标：** 新开发者知道*解决什么问题*以及*明确不做什么*。

| 问题 | 去哪找 |
|------|--------|
| 一句话问题陈述？ | README 开头、docs 索引、VISION/ABOUT |
| 主要角色（persona）？ | README、docs（用户 vs 运维 vs 插件作者） |
| 3～5 个核心用例（happy path）？ | README、docs/教程、e2e 测试 |
| 1～2 个失败/边界场景？ | docs、主链路中的错误处理 |
| 范围内 / 范围外？ | README、ARCHITECTURE、issue 模板 |
| 成功指标（延迟、可靠性、安全）？ | docs、ADR、热路径注释 |

**记录：** 问题段落、角色表、范围 bullet 列表。

---

## 方面 2：系统上下文

**目标：** 画出上下文 + 容器视图 — 可部署单元与外部系统。

| 问题 | 去哪找 |
|------|--------|
| 外部系统（API、DB、队列、SaaS）？ | docs、docker-compose、k8s manifest、env 示例 |
| 可运行单元（CLI、server、worker、UI）？ | `cmd/`、`apps/`、`services/`、Dockerfile、Procfile |
| 通信方式（HTTP、gRPC、WS、事件）？ | server 启动、proto/openapi、客户端 SDK |
| 数据存储与路径？ | config、migrations、docs 中的 data 目录 |
| 部署拓扑（单机、k8s、serverless）？ | deploy/、docs/install、helm chart |
| 本仓库 vs  sibling 仓库边界？ | README 链接、monorepo workspace 配置 |

**记录：** Mermaid `C4Context` 或 `graph TB` + 容器表（名称、技术、职责、端口/协议）。

---

## 方面 3：主执行链路

**目标：** 追踪**一条**端到端路径，带代码锚点。

| 问题 | 去哪找 |
|------|--------|
| 最具代表性的触发点？ | HTTP 路由、CLI 子命令、消息消费者、cron |
| 入口 handler / 调度器？ | router、main、consumer 注册 |
| 核心领域逻辑循环？ | service 层、agent loop、pipeline |
| 持久化 / 副作用？ | repository、ORM、文件写入 |
| 响应 / 出站？ | response 序列化、reply 发送 |
| 链路上的扩展点？ | middleware、hook、plugin |
| 同步 vs 异步？重试？幂等？ | 队列用法、dedupe key、测试 |

**方法：**
1. 选定触发点（如 `POST /api/foo`、`cli run`、「收到消息」）。
2. 向下跟踪 **6～12** 跳调用；记录 file:line 或 file:function。
3. 简要追踪**一条**错误分支。

**记录：** 编号步骤列表 + Mermaid `sequenceDiagram` + 「改 X 从文件 Y 开始」。

---

## 方面 4：架构原则与约束

**目标：** 设计意图与贡献者不可违反的规则。

| 问题 | 去哪找 |
|------|--------|
| 文档化原则？ | ARCHITECTURE.md、AGENTS.md、CONTRIBUTING、ADR |
| 分层边界 / 禁止 import？ | lint 规则、import cycle、package.json exports |
| 依赖方向？ | 包结构、`internal/` 目录、eslint boundary |
| 扩展契约？ | plugin SDK、trait/interface 文档 |
| 已知 trade-off？ | ADR、注释、CHANGELOG 中的 refactor PR |
| 公开 API 稳定性策略？ | semver、deprecation 公告、CHANGELOG |

**记录：** 规则 bullet 列表（有文档时引用短句）+ 非显而易见时的依赖图。

---

## 方面 5：代码地图（按能力）

**目标：** 按*做什么*导航，而非按文件夹字母序。

| 能力域 | 找入口 | 典型深度 |
|--------|--------|----------|
| 启动 / CLI | main、cmd 根 | 1～2 文件 |
| 核心服务 / 运行时 | server 启动、app factory | 2～3 文件 |
| 主链路枢纽 | 来自方面 3 | 锚点 |
| 存储 / 持久化 | db 包、migrations | 1～2 文件 |
| 配置 | config loader、schema | 1～2 文件 |
| 扩展 / 插件 | loader、registry | 2～3 文件 |
| API 面 | routes、handlers、openapi | 2～3 文件 |
| UI（若有） | app 入口、pages 根 | 1～2 文件 |

**记录：** 表格 — 能力 | 入口路径 | 关键符号 | 测试路径 | 一行描述。

**上限约 12 行。** 合并小目录。

---

## 方面 6：配置与状态

**目标：** 知道真相来源与覆盖规则。

| 问题 | 去哪找 |
|------|--------|
| 配置文件与格式？ | `.env.example`、`config.*`、yaml/json 样例 |
| 优先级（env > file > default）？ | config loader 源码 |
| 必填 vs 可选？ | schema、docs、启动校验 |
| 密钥 / 凭据处理？ | docs/security、secret manager |
| 运行时 vs 持久化状态？ | state 目录、DB、cache |
| 迁移 / 升级路径？ | migrations、doctor、upgrade docs |
| Feature flag？ | 环境变量、配置键 |

**记录：** 配置优先级链、关键配置键表、状态存放路径。

---

## 方面 7：构建、运行、调试

**目标：** 可动手闭环 — 从 clone 到跑起来。

| 问题 | 去哪找 |
|------|--------|
| 语言/运行时版本？ | README、`.nvmrc`、`go.mod`、toolchain 文件 |
| 安装依赖？ | README、Makefile、包管理 lockfile |
| 构建命令？ | Makefile、package scripts、cargo/go build |
| 开发/生产运行？ | README、compose、systemd 样例 |
| 主链路最小 repro？ | docs、e2e、examples/ |
| 日志 — 位置与级别？ | logging 配置、`--verbose` 等 flag |
| 常见安装失败？ | docs/troubleshooting、issue 标签 |

**记录：** 可复制命令块（install、build、run、smoke）。区分实际执行 vs 仅文档。

---

## 方面 8：测试与质量门禁

**目标：** 安全证明改动。

| 问题 | 去哪找 |
|------|--------|
| 测试框架？ | devDependencies、test 配置 |
| 单元测试位置/约定？ | `*_test.go`、`*.test.ts`、`tests/` |
| 集成 / e2e？ | e2e 目录、docker test compose |
| 单文件测试命令？ | package scripts、Makefile |
| CI workflow？ | `.github/workflows`、其他 CI 配置 |
| Lint / format / typecheck？ | pre-commit、ci 步骤、eslint/ruff/golangci |
| PR 前检查清单？ | CONTRIBUTING、PR 模板 |

**记录：** 表格 — 门禁 | 命令 | 何时需要 | 备注。

---

## 方面 9：扩展与贡献模型

**目标：** 如何扩展且不破坏边界。

| 问题 | 去哪找 |
|------|--------|
| 插件/扩展模型？ | plugin docs、loader、examples/plugins |
| 公开 SDK / API barrel？ | 导出包、`api.ts`、openapi |
| 最小扩展示例？ | examples/、模板 plugin、cookiecutter |
| Issue / PR 流程？ | CONTRIBUTING、issue 模板 |
| CODEOWNERS / review 期望？ | CODEOWNERS、CONTRIBUTING |
| Commit / changelog 约定？ | CONTRIBUTING、release docs |
| 贡献者 license 含义？ | LICENSE、CLA |

**记录：** 「添加插件」或「第一次 PR」的编号步骤；公开 vs 内部 API 列表。

---

## 方面 10：演进与地雷

**目标：** 避开遗留陷阱与文档谎言。

| 问题 | 去哪找 |
|------|--------|
| 近期大 refactor？ | CHANGELOG、migration 指南 |
| 仍活跃的遗留路径？ | `@deprecated`、注释、feature flag |
| 「不要动」的区域？ | CODEOWNERS、AGENTS.md 警告 |
| Flaky test / 性能热点？ | issues、skipped tests、benchmark |
| 文档已知过时？ | 对比 README 声称与入口实现 |
| 计划删除？ | deprecation 公告、roadmap |

**记录：** 地雷表 — 区域 | 风险 | 更安全路径（可含路径引用）。

---

## 必读 10 文件启发式

完成方面 1～10 后，为新开发者按阅读顺序选 **10 个文件**：

1. 贡献者/架构政策文档  
2. 架构或概念总览文档  
3. 主入口（二进制或库）  
4. Server/bootstrap 或 app factory  
5. 主链路枢纽（来自方面 3）  
6. 领域核心（loop、engine、service）  
7. 扩展 loader 或公开 API barrel  
8. Config loader 或 schema  
9. 主链路的一条代表性测试  
10. CI workflow 或 CONTRIBUTING 测试章节  

按仓库体量调整；记录每个文件的选择理由。
