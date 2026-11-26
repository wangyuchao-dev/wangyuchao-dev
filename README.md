# 王玉超 · AI 应用开发

> 把大模型接进真实业务：RAG 检索、Agent 编排、结构化输出、缓存与评测。
> 求职方向：AI 应用开发 / Java 后端。

---

## 我在做什么

不满足于「调通一个 API」，更在意三件事：**检索准不准、输出稳不稳、成本能不能压下来**。
所以每个项目里都会留下评测数据、阈值标定的过程，以及踩过的坑。

| 方向 | 具体做法 |
|---|---|
| RAG | 文档切分 → 向量入库 → 多查询融合 → 阈值标定 → 降级重查 |
| Agent | 需求解析与结果生成拆成两级，各自 prompt 更短、约束更强 |
| 工程化 | 异步任务 + 进度轮询、结果缓存、限流、在途锁、失败降级 |
| 评测 | 检索评测集 + 端到端用例，改动前后跑基线对比 |

---

## 项目

### [知旅 · RAG 行程规划系统](https://github.com/wangyuchao-dev/zhilv)
`FastAPI` `LangChain` `Chroma` `PostgreSQL` `Redis` `Vue 3` ｜ 4,878 行 ｜ 23 个单元测试

结构化表单 → 需求解析 Agent → 攻略检索 → 行程编排 Agent → 结构化输出 → 入库回看。

- **两级 Agent**：Agent 1 只把表单翻译成检索查询，Agent 2 只拿素材排行程；prompt 更短，出问题好定位
- **阈值不是拍脑袋定的**：0.80 卡在「无关最高 0.7863」和「有用最低 0.807」之间，由 21 次实测 / 105 个片段倒推
- **缓存只加净收益为正的环节**：结果缓存 14.1s → 0.03s，检索缓存 4.18s → 0.32s；Chroma 查询 50ms 反而没加
- **后置覆写**：日期、序号、预算合计由程序重算，不交给模型
- **降级设计**：检索不足 2 条时放宽到 0.79 只取 top-1，并写 WARNING 日志——降级次数就是重调阈值的信号

> README 里完整写了「为什么这么拆」「阈值怎么测出来」「语料为什么不能给现成行程」，比代码更能说明问题。

### [小驾同学 · LangChain4j 驾考助手](https://github.com/wangyuchao-dev/xiaojia-langchain4j)
`Spring Boot` `LangChain4j` `Ollama` `MySQL` `PGVector`

- AI Service 声明式接口，业务代码不直接碰模型
- 多轮会话记忆用 `MysqlChatMemoryStore` 持久化，重启不丢上下文
- RAG 检索 + 工具调用，答驾考题时能引用题库原文

### [pdf-pipeline · PDF 自动化解析入库](https://github.com/wangyuchao-dev/pdf-pipeline)
`boto3` `MinIO` `MinerU 4.x` `LangChain` `Chroma`

- MinIO 下载 → MinerU 解析 → 解包取 markdown → 向量入库 → 回存
- 处理了 MinerU 4.x 的 202 异步轮询、zip 解包、路径寻址这些坑
- 3 份 PDF（各 3 页）单份 4 秒，总计 15.2 秒

### 驾考云平台 · 校企合作项目
`Spring Boot` `Nacos` `OpenFeign` `MyBatis-Plus` `Vue 3` ｜ A 组组长

- 负责驾校管理、培训管理、统计分析三个模块
- 排查出 OpenFeign 多个 `@SpringQueryMap` 静默丢参的框架级缺陷

---

## 技术栈

**语言**：Python、Java、TypeScript、SQL
**AI**：LangChain、LangChain4j、Chroma、PGVector、Ollama、MCP、Prompt 工程、RAG、Agent
**后端**：FastAPI、Spring Boot、MyBatis-Plus、Redis、PostgreSQL、MySQL
**前端**：Vue 3、Vite
**其他**：Docker、MinIO、Git

---

## 联系

项目细节、设计取舍都在各仓库 README 里，欢迎直接看代码。
