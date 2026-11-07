# rcp-core · 平台基座

所有服务共享的风控底座。**纯 Java 库，无 Spring 依赖**，服务经 `com.risk.platform:rcp-core:1.0.0` 依赖。

## 内容

| 包 | 是什么 |
|---|---|
| `com.risk.platform.model` | `Decision`（决策枚举 + 风险分映射）、`PatrolVerdict`（巡查结论） |
| `com.risk.platform.pipeline` | `PatrolPipeline` / `AbstractPatrolPipeline`：四条巡查线同构骨架 |
| `com.risk.platform.rule` | `Rule` / `Condition` / `RuleEngine`：条件-判定-分值规则内核 |

## 为什么单独成仓

- **决策语义唯一真源**：四条巡查线共用一套 `Decision`，各仓不得私造同名枚举（红线 RL-003）。
- **升版纪律**：改动即影响全部服务，消费仓按**钉版 ref** 依赖（CI 里 `core-ref: v1.0.0`），不允许跟 main 浮动。

## 构建

```bash
mvn -B verify            # 编译 + 测试
mvn -B install -DskipTests   # 供本地其他仓依赖
```

## 版本约定

- `1.0.x`：只加字段/加方法（兼容）；
- `1.x.0`：语义变更（如新增决策等级、改风险分口径），必须与 `rcp-ontology/ontology/tbox/decisions.ttl`（唯一真源）、以及由它生成的 `rcp-contracts/contracts/schemas/decision.yaml` 同批改；
- 打 tag `vX.Y.Z`，消费仓的 `core-ref` 显式抬版，禁止"改 core 不改消费仓"。
