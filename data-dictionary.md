# 评分与字段说明

[报告总览](README.md) · [原始实验](original-report.md) · [重算实验](rerun-report.md)

## 评分口径

| 指标 | 定义 |
|---|---|
| 原生任务分（PS）↑ | 原生评分判定全通过的原始问题数 ÷ 本轮题数。一次交付只计一次；不是 HTTP 成功率或交付率。 |
| 偏好违背率（Violation）↓ | 至少一条适用偏好被判违背的原始问题数 ÷ 本轮题数，分母保留不适用题。 |
| 仅适用且已裁决的题：违背率 ↓ | `fail` ÷ (`pass` + `fail`)；不同实验组可能有不同分母。 |
| 约束槽得分（SPS）↑ | 每题按通过约束槽数 ÷ 约束槽总数计算；排除无约束槽的题，在题组内取均值，再对有适用槽的题组等权平均。 |
| 整组通过率（SR）↑ | 题组内所有题均原生通过的题组数 ÷ 题组总数。 |

百分比列用 0–100 表示，`_pct` 列已经乘以 100。正文将精确值四舍五入至两位小数；各项差值由原始计数计算，可能与两个显示值相减相差 0.01。

## CSV 字段

各表中的 `group_id` 是公开基准的题组编号，`question` 是题组内从 1 开始的序号，可用这两个字段对齐两轮。`A_`、`B_` 表示实验组；同一行的 A 和 B 对应同一道题。题组大小由 `questions` 表示。二值列使用 `0` 和 `1`，不是百分比。

| 字段 | 含义 |
|---|---|
| `*_task_pass` | 逐题原生全通过标记。 |
| `*_preference_state` | `pass`：适用且无违背；`fail`：已确认至少一条违背；`not_applicable`：无适用偏好；`unknown`：未能确定。两轮均无最终未知裁决。 |
| `*_slots_passed`、`*_slots_total` | 逐题通过的原生约束槽数、总槽数。 |
| `*_task_passed`、`*_task_score_pct` | 逐组任务通过数、原生任务分。 |
| `*_preferences_violated`、`*_violation_pct` | 逐组违背题数、主口径的违背率。 |
| `*_preferences_passed`、`*_preferences_not_applicable`、`*_preferences_unknown` | 逐组其余偏好裁决状态计数，和违背计数一起加总为题数。 |
| `*_violation_applicable_pct` | 仅适用且已裁决题内的违背率；无适用且已裁决题时留空。 |
| `*_sps_pct`、`*_all_pass` | 题组范围的约束槽得分、是否全组原生通过。 |
| `A_recall_events`、`A_delivered_events` | 原始实验中 A 的冻结 Skill 召回事件数、经回执确认进入求解请求的事件数；召回按来源题、送达按目标题计。 |
| `*_old_task_pass`、`*_old_preference_state` | 同一问题在原始实验的结果；仅供重算前后对照。 |
| `A_old_missed` | 原始 A 在该题有召回、但同题没有任何建议送达。逐组表中为符合条件的题数。 |
| `B_old_used_excluded_route` | 原始 B 在该题至少有一个成功求解步骤命中后来停用的通道。逐组表中为符合条件的题数。 |
| `A_opening_delivery` | 重算逐题：开场建议在首个可见动作前送达；逐组：符合条件的题数。 |
| `A_post_tool_deliveries` | 重算中，工具结果返回后新增的建议，在对应续轮首个可见动作前送达的次数。一道题可有多次。 |

`coverage.csv` 列出原始全部 60 个题组，`rerun_status` 为 `rerun` 或 `not_rerun`。未重跑的题组 `rerun_questions=0` 表示未纳入重算，不表示重算得零分。`metrics.json` 单独列出原始全量、重算全量及原始实验中对应重算题单的 377 题子集。
