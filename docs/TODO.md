# 4 周加速版任务清单 / 待办表

说明：每项任务后给出预估耗时和验收标准。每天建议投入 2–4 小时，可按实际情况调整。

## Week 0 — 准备（Day 0，0.5h）

- [√] 准备开发环境（Java 17、Maven/Gradle、Git、IDE、Docker） — 0.5h
  - 验收：`java -version`、`mvn -v`、`docker --version` 能正常返回。
- [√] 创建 GitHub 仓库并 clone 到本地 — 0.5h
  - 验收：本地已能 git pull / git push。

## Week 1 — 后端骨架 + CRUD（Day 1–5）

### Day 1 — 项目初始化（2–3h）
- [ ] 用 Spring Initializr 创建项目（依赖：spring-web、webflux、validation、lombok 可选） — 1h
- [ ] 建立项目目录结构和 README 初稿 — 0.5h
- [ ] 验证应用能正常启动 — 0.5–1h
  - 验收：`mvn spring-boot:run` 成功，访问 `/api/health` 返回 `OK`。

### Day 2 — 数据模型设计（2–3h）
- [ ] 设计最小业务模型（例如：nodes、edges、assets（valves/wells）） — 1.5h
- [ ] 写 SQL 或 JPA Entity 定义 — 1h
  - 验收：本地数据库（Postgres 或 H2）能建表且 JPA 启动无错误。

### Day 3 — Repository + Service + CRUD Controller（3h）
- [ ] 实现 Entity、Repository、Service 层 — 1.5h
- [ ] 实现至少一个资源的 Controller（CRUD） — 1.5h
  - 验收：用 Postman 或 curl 执行 POST/GET/PUT/DELETE 能正常操作并写库。

### Day 4 — 参数校验、异常处理、分页（2–3h）
- [ ] 添加 `@Valid` 参数校验 — 0.5h
- [ ] 添加统一异常处理 — 0.5h
- [ ] 实现分页查询接口 — 1h
  - 验收：非法请求返回 4xx，分页返回 `total/page/size`。

### Day 5 — 测试与 CI（2h）
- [ ] 编写不少于 2 个测试（单元或集成） — 1h
- [ ] 配置 GitHub Actions 简单 CI（`mvn test`） — 1h
  - 验收：本地 `mvn test` 通过，CI 能在推送时自动运行。

### Week 1 输出物
- 可运行的 Spring Boot 项目
- CRUD 接口可用
- 基础 README 和 CI 配置

## Week 2 — AI 接入（Day 6–10）

### Day 6 — 设计 AI 服务契约（1.5–2h）
- [ ] 定义 AI 服务接口（请求/响应格式、错误码、超时策略） — 1h
- [ ] 写一份 `docs/spec.md` 或 README 里的接口文档 — 0.5h
  - 验收：接口草案清晰，说明输入输出与错误场景。

### Day 7 — 实现 OpenAIService（2–3h）
- [ ] 使用 `WebClient` 封装 OpenAI Chat API 与 Embeddings 接口 — 2h
- [ ] 配置 API Key 环境变量 — 0.5h
  - 验收：本地测试能返回模型响应。

### Day 8 — ChatController + 日志（2–3h）
- [ ] 实现 `POST /api/chat` — 1h
- [ ] 记录请求/响应日志（日志文件或数据库） — 1h
  - 验收：curl 请求返回 `reply`，日志中能看到请求与输出。

### Day 9 — 错误处理、超时和限流（2h）
- [ ] 增加超时、重试和错误处理 — 1h
- [ ] 增加简单限流/重试策略 — 1h
  - 验收：模拟 429/5xx/超时时，服务返回友好错误或自动重试。

### Day 10 — Prompt 模板与示例（2h）
- [ ] 编写 3 个业务 prompt 模板（例：拓扑检查清单、碰撞检测建议、阀门隔离步骤） — 1.5h
- [ ] 记录至少 3 个示例输出 — 0.5h
  - 验收：模板能产出可读且可复现的响应。

### Week 2 输出物
- `/api/chat` 接口可运行
- prompt 模板库
- AI 调用错误处理和日志

## Week 3 — RAG 与向量检索（Day 11–15）

### Day 11 — 部署向量库（Qdrant / Pinecone） — 1.5–2h
- [ ] 启动 Qdrant（推荐）或其他向量库 — 1h
- [ ] 测试 `/collections` 或相应接口 — 0.5h
  - 验收：向量库能正常回应请求。

### Day 12 — 文档抽取和分块（2h）
- [ ] 读取文本/Markdown/小 PDF 文档 — 0.5h
- [ ] 切分成 chunk（按 token 或长度分块） — 1h
  - 验收：生成多个 chunk，并保留 metadata。

### Day 13 — Embeddings 与写入向量库（2–3h）
- [ ] 使用 OpenAI embeddings 或远程模型生成向量 — 1.5h
- [ ] 写入向量库并可进行相似度检索 — 1h
  - 验收：能查询出最相关文档/片段。

### Day 14 — 实现 `/api/qa`（2–3h）
- [ ] 接收问题 -> 检索 top-k chunks -> 构造 RAG prompt -> 调用 LLM -> 返回答案 — 2h
- [ ] 包含来源引用或文档 ID — 0.5h
  - 验收：能基于文档回答问题，并回显来源信息。

### Day 15 — 调优与 QA 验证（2h）
- [ ] 调整 chunk size、top-k 数量、相似度阈值 — 1h
- [ ] 记录 3 个 QA 用例及结果 — 1h
  - 验收：至少 3 个问答场景能稳定返回合理答复。

### Week 3 输出物
- 向量库可用
- 文档导入脚本
- `/api/qa` 可运行并返回带来源的答案

## Week 4 — 业务整合、容器化与交付（Day 16–20）

### Day 16 — 业务端点整合（2–3h）
- [ ] 实现至少 1 个业务化端点（例如 `/api/inspect-topology`） — 2h
- [ ] 结合 AI 模型输出建议和 RAG 上下文 — 0.5–1h
  - 验收：从输入到输出全链路可演示。

### Day 17 — 准备演示脚本（2h）
- [ ] 编写 Postman collection 或 curl 示例脚本 — 1.5h
- [ ] 准备 5 个最小 demo 流程 — 0.5h
  - 验收：能在 5 分钟内完成一次回放 demo。

### Day 18 — Dockerize + docker-compose（2–3h）
- [ ] 编写 `Dockerfile` 和 `docker-compose.yml` — 2h
- [ ] 验证本地启动服务是否成功 — 0.5–1h
  - 验收：`docker-compose up` 后应用启动正常。

### Day 19 — 文档和 README（2h）
- [ ] 补全 README（启动方式、接口、演示、限制、后续计划） — 1.5h
- [ ] 记录架构说明和 troubleshooting — 0.5h
  - 验收：其他开发者能按 README 复现 demo。

### Day 20 — 收尾、测试和反思（2h）
- [ ] 检查 secrets 是否泄漏，补充 `.gitignore` / `.env.example` — 0.5h
- [ ] 进行最终回归测试 — 0.5h
- [ ] 总结下一步任务和项目可改进点 — 1h
  - 验收：项目能按文档完整运行，且有清晰待办清单。

### Week 4 输出物
- Dockerized app
- README 完整
- 演示脚本和可复现流程
- 未来迭代 backlog

## 进度记录模板

- Date: YYYY-MM-DD
- Day: WeekX-DayY
- 今日目标:
- 实际完成:
- 遇到的问题:
- 解决方案 / 待办:
- 验证命令：
- 输出/截图：
- Commit / PR 链接：

## GitHub 管理建议

### Milestones
- Week1 - Base Backend
- Week2 - AI Integration
- Week3 - RAG Demo
- Week4 - Delivery

### Labels
- `todo`, `doing`, `done`
- `priority:high`, `priority:medium`, `priority:low`
- `type:feature`, `type:bug`, `type:doc`

### Issue 命名示例
- `Week1 / Day3 - 实现 CRUD Controller`
- `Week2 / Day7 - OpenAIService 框架接入`
- `Week3 / Day14 - 实现 /api/qa`

## 常用命令示例

```bash
# 启动项目
mvn spring-boot:run

# 健康检查
curl http://localhost:8080/api/health

# AI chat
curl -X POST http://localhost:8080/api/chat \
  -H "Content-Type: application/json" \
  -d '{"message":"请给我一个管线拓扑检查建议"}'

# QA Demo
curl -X POST http://localhost:8080/api/qa \
  -H "Content-Type: application/json" \
  -d '{"question":"什么是拓扑检查的关键点？"}'
```

## 风险提示

- 不要把 API Key 直接写入代码库。
- 在开发阶段优先用小模型或 mock，避免成本失控。
- 用 AI 生成代码时要主动做 Code Review 和测试。
- 对关键调用要增加超时、重试和错误降级策略。

## 下一步

- 将该任务清单升级为 `docs/TODO.md`
- 每天更新一次进度记录
- 以 GitHub Issues 维护每个 Day 任务
- 在 Week 3 和 Week 4 把 AI 能力落到更真实的业务场景中

