# 文风优化·犹疑校准强制工作流：侦察 → 分析 → 执行 → 验收

> **加载时机**：接手任何“犹疑校准 / 未定感 / 不言尽 / 伪自述修复 / 语料去陈述化”任务前必须读取。本文件定义强制输出格式、持久化规则、字段校验和失败判定，防止 AI 用“感觉上更犹豫了”糊弄。

---

## 总则

本 Skill 的执行顺序固定为：

| 阶段 | 输出块 | 持久化 | 目标 |
|---|---|---|---|
| 一、侦察 | `<reconnaissance>` | 暂不持久化 | 明确叙述层、触发点、关系压力、能量等级、角色可知范围 |
| 二、分析 | `<analysis>` | 逐字写入 `.wenfengyouhua-hesitation-analysis-cache.md` | 按 H1-H9 完成判定，给出证据、影响范围、改写方案 |
| 三、执行 | `## 改写结果` | 执行中若出现意外，追加 `<decision_point>` 到缓存 | 输出完整改写，不省略，不用“其余同理” |
| 四、验收 | `<output_quality_review>` | 追加到缓存，并归档到 `.wenfengyouhua-hesitation-analysis-archive/{YYYY-MM-DD}_{HHmm}_{task_summary}.md` | PASS/FAIL 扫描 H1-H9 与黑名单句式 |

除非用户明确要求“只给最终改写，不展示过程”，否则必须展示 `<reconnaissance>`、`<analysis>`、`## 改写结果`、`<output_quality_review>` 四个部分。

即使用户要求只给结果，内部仍必须执行完整流程；对外最少输出 `## 改写结果` 和 `## 简短自检`。

---

## 留痕命名

为避免与 `yokenken-editor` 和工程师类 Skill 的留痕路径冲突，犹疑校准所有任务级缓存与归档一律使用 `.wenfengyouhua-hesitation-` 前缀：

- 临时缓存：项目根目录 `.wenfengyouhua-hesitation-analysis-cache.md`
- 任务归档目录：项目根目录 `.wenfengyouhua-hesitation-analysis-archive/`
- 单次任务归档文件：`.wenfengyouhua-hesitation-analysis-archive/{YYYY-MM-DD}_{HHmm}_{task_summary}.md`

禁止写入 `.analysis-cache.md`、`.analysis-archive/`、`.yokenken-analysis-cache.md`、`.yokenken-analysis-archive/`。

如果运行环境没有文件写入工具，必须在 `<reconnaissance>` 中声明：`persistence_mode: conversation_only`，并在对话中完整展示 `<analysis>` 与 `<output_quality_review>`。

---

## 阶段一：侦察

输出 `<reconnaissance>` 块：

```xml
<reconnaissance>
goal: [本次要修复的核心问题，不得只写“优化文风”]
input_scope: [原文范围：全文/段落/句子/角色语料]
narrative_layer: [第一人称自述 / 第三人称贴身 / 对话层 / 独白层 / 混合]
trigger_point: [触发角色反应的具体话、动作、物件；缺失则写 MISSING 并说明影响]
relationship_pressure: [角色为什么不能说满；缺失则写 MISSING]
energy_level: [低能量 / 平静 / 嘴硬 / 高能量 / 亢奋 / 咄咄逼人]
knowledge_boundary: [角色能知道什么，不能替谁解释动机]
source_material:
  - [原文证据或用户需求摘要]
missing_context:
  - [缺失项 1；没有则写 NONE]
persistence_mode: [file_cache / conversation_only]
</reconnaissance>
```

### 侦察硬规则

- `trigger_point` 不能空泛写“对方的话”，必须指出具体句子、动作或物件。
- `energy_level` 必须明确，不允许写“正常”。
- `knowledge_boundary` 必须说明不能替谁解释动机。
- 如果 `missing_context` 不是 `NONE`，后续 `<analysis>` 必须说明采用保守方案。

---

## 阶段二：分析

输出 `<analysis>` 块，并逐字写入 `.wenfengyouhua-hesitation-analysis-cache.md`。

```xml
<analysis>
context: [侦察得到的上下文，不是一句话摘要]
needs: [用户的本质需求，例如“去掉伪自述”“修复不言尽失败”“避免二选一情绪摘要”]
key_challenges: [本段最容易偷懒或误写的点]
confidence: [HIGH / MEDIUM / LOW + 理由]
hit_summary: [命中的 H 编号列表，例如 H5,H7,H8；没有命中也必须写 NONE]
findings:
  - id: H[编号]
    label: [失守类型名称]
    evidence: [原文中的具体句子]
    why_it_fails: [为什么不像人类自述/为什么是偷懒摘要]
    required_fix: [半截话 / 错位接话 / 动作替口 / 物件承担 / 高能量找补 / 尾巴词与舒缓词承担 / 硬落点松动 / 作者角色化自语 / 表演性自我反思拆除]
non_hits_checked:
  - H1: [PASS/FAIL + 理由]
  - H2: [PASS/FAIL + 理由]
  - H3: [PASS/FAIL + 理由]
  - H4: [PASS/FAIL + 理由]
  - H5: [PASS/FAIL + 理由]
  - H6: [PASS/FAIL + 理由]
  - H7: [PASS/FAIL + 理由]
  - H8: [PASS/FAIL + 理由]
  - H9: [PASS/FAIL + 理由]
affected_scope: [涉及的句子/段落清单]
execution_plan:
  - step_1: [具体改写方案：源句 → 承担方式 → 风险控制]
  - step_N: [...]
degradation_check:
  - 是否只说“更自然/更犹疑”而没有具体判定？ -> [YES/NO + 理由]
  - 是否遗漏 H1-H9 任一检查项？ -> [YES/NO + 理由]
  - 是否打算用“整体还行”放过命中项？ -> [YES/NO + 理由]
  - 是否可能回流“后来没说/不是不X/不知道该A还是B/愣了一下/心情复杂”？ -> [YES/NO + 风险控制]
  - 是否有具体触发点和承担方式？ -> [YES/NO + 证据]
  - 若用户反馈“不像人话/太肯定”，是否已完成硬落点、舒缓字密度、尾巴词扫描？ -> [YES/NO/不适用 + 证据]
  - 若涉及作者声部，是否把作者当作有限角色写，而不是外部评价者？ -> [YES/NO/不适用 + 证据]
</analysis>
```

### 分析硬规则

- `findings` 必须引用原文证据，不能只写“语气太陈述”。
- `non_hits_checked` 必须覆盖 H1-H9。少一个编号即为格式不合格。
- `execution_plan` 必须覆盖 `affected_scope` 的每一处，不允许“其余同理”。
- 命中 H7/H8/H9 时，不允许只把摘要词删掉，必须补出触发点或承担动作。
- `degradation_check` 中任一关键项为 `YES` 且未给风险控制时，不得进入执行。

---

## 阶段三：执行

输出：

```markdown
## 改写结果

[完整改写文本，不省略，不用“其余同理”。]
```

### 执行硬规则

改写结果不得出现以下句式，除非在 `<output_quality_review>` 中明确解释其承担了具体功能：

- “后来没说/没接/没问/没再讲”
- “不是不 X，是 Y”
- “不知道该 A 还是该 B”
- “愣了一下 / 怔了一瞬 / 顿了顿 / 一时没反应过来”
- “心情很复杂 / 说不上来什么感觉 / 很难形容”

执行中如果发现原计划不足，必须输出 `<decision_point>` 并追加到缓存：

```xml
<decision_point>
issue: [发现的新问题]
impact: [影响哪些句子/段落]
options:
  - option_a: [方案 A + 利弊]
  - option_b: [方案 B + 利弊]
chosen: [选择方案]
reason: [选择理由]
</decision_point>
```

---

## 阶段四：验收

输出 `<output_quality_review>` 块，追加到缓存，并归档。

```xml
<output_quality_review>
residual_h_issues:
  - H1: [PASS/FAIL + 证据]
  - H2: [PASS/FAIL + 证据]
  - H3: [PASS/FAIL + 证据]
  - H4: [PASS/FAIL + 证据]
  - H5: [PASS/FAIL + 证据]
  - H6: [PASS/FAIL + 证据]
  - H7: [PASS/FAIL + 证据]
  - H8: [PASS/FAIL + 证据]
  - H9: [PASS/FAIL + 证据]
forbidden_phrase_scan:
  completed_non_speech: [PASS/FAIL；检查“后来没说/没接/没问”]
  negative_affirmative: [PASS/FAIL；检查“不是不 X，是 Y”]
  binary_emotion_summary: [PASS/FAIL；检查“不知道该 A 还是该 B”]
  instant_reaction_summary: [PASS/FAIL；检查“愣了一下/顿了顿”等]
  abstract_complexity: [PASS/FAIL；检查“心情复杂/说不上来”等]
anchor_check:
  trigger_point_present: [YES/NO + 证据]
  carrier_present: [YES/NO + 半截话/动作/物件/错位接话]
  energy_preserved: [YES/NO + 理由]
  human_speech_check: [PASS/FAIL；检查硬落点、舒缓字密度、尾巴词是否足够]
  author_as_character_check: [PASS/FAIL/不适用；作者声部是否有当场卡壳、自我推翻、口语呼吸，而非外部评价]
format_check:
  reconnaissance_present: [YES/NO]
  analysis_cached: [YES/NO；conversation_only 时说明原因]
  h1_to_h9_complete: [YES/NO]
  no_unexplained_blacklist_phrase: [YES/NO]
final_decision: [ACCEPT / REVISE]
</output_quality_review>
```

### 验收硬规则

- 任何 `FAIL` 都必须回到执行阶段，不得交付。
- `final_decision` 只有在全部关键项 PASS/YES 时才能写 `ACCEPT`。
- 不允许用“整体自然”代替逐项验收。
- `format_check` 任一项为 `NO` 时，视为格式失败。

---

## 最小展示模式

用户明确要求“别展示过程，只给结果”时，仍必须在内部执行完整流程。对外最少输出：

```markdown
## 改写结果

[完整改写文本]

## 简短自检
- 命中问题：[H 编号]
- 已避开：[列出完成式未言/否肯式解释/二选一摘要/瞬时反应摘要/复杂感摘要等]
- 承担方式：[半截话/动作替口/错位接话/物件承担/高能量找补/尾巴词与舒缓词承担/硬落点松动/作者角色化自语]
```

---

## 格式违规判定

以下情况视为执行失败：

- 没有输出 `<reconnaissance>`。
- 没有输出 `<analysis>`。
- `<analysis>` 没有覆盖 H1-H9。
- 没有原文证据。
- 没有 `execution_plan`。
- 没有 `<output_quality_review>`。
- 只说“更自然/更犹豫/更像人”但不给判定理由。
- 改写结果出现黑名单句式且未解释承担功能。
- 用“整体通过”代替逐项 PASS/FAIL。
