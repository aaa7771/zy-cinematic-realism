<!--
zy-cinematic-realism → MiniMax H3 适配翻译层。
zy 原版为"单帧生图"导向，H3 为"视频时间轴"导向。本文件说明每个 zy 知识块在 H3 下如何用、哪些概念需要转换。
-->

# zy → H3 适配翻译层

## 核心差异

zy 原版输出**一张静态电影单帧**（生图模型的 negative prompt 参数、静态手持质感、无时间轴）。
H3 输出**一段 4-15 秒视频**（时间戳分镜、运镜三要素、原生声音层）。

zy 知识在 H3 下**全部可用**，但以下四处需要"翻译"。

---

## 1. 负面提示词：独立 Avoid 字段 → 正文内 "Negative list:" block（已实测确认）

- **zy 用法**：输出一个独立的 `Avoid: ...` 字段（对应生图模型的 negative prompt 参数）。
- **H3 现实（实测确认）**：H3 **没有独立 negative_prompt 参数**。MiniMax 视频接口的请求参数只有 `model / prompt / prompt_optimizer / fast_pretreatment / duration / resolution / callback_url / aigc_watermark`，不含 negative_prompt；社区（CFG-distilled）也确认 "There are no negative prompts. Your prompt is the only lever."。部分第三方套壳界面的"负面提示"输入框是它们自己加的预处理，非模型原生能力。
- **官方正解**：官方 45 条示例的提示词由 6 个 block 构成——Style / Timeline / Camera / Audio / Text / **Negative list**。最后一块写在正文里，集中列举要拒绝的转场、物体、陈词滥调（官方例句："No soft dissolves, no fluid transitions, do not add Chinese, do not repeat a job title"）。
- **翻译规则**：从 `negative-prompts.md` 词库挑最相关的 3-5 项，集中在正文末尾写成一个 **`Negative list:`** block（与 Style/Audio 等并列），而非散落自然句、也非独立字段。
  示例：
  ```
  Negative list: No subtitles, no on-screen text, no watermark; no background music (non_diegetic_music: N/A); do not add Chinese characters unless requested; no soft dissolves, no poster composition.
  ```
- **必写项推论**（H3 否定诉求不写 = 被忽略）：
  - 不要字幕/文字/水印（除非用户明确要画面内文字）；
  - 不要背景音乐（除非用户要配乐，此时写进 Music 字段）；
  - 图生视频/首尾帧：不要切镜头（`Do not cut to another shot`）。
- **其余 zy 词库项**（plastic skin / hero pose / unmotivated rim light / cyberpunk neon 等）→ **改用正面描述替代**：写"真实皮肤质感""观察式构图""动机光""克制色彩"。H3 对负面形容词堆砌不敏感，正面写更有效。

> 结论：H3 输出**不再有独立 `Avoid` 字段**，改为「正文末尾一个 `Negative list:` block 集中写 3-5 条最关键拒绝项 + 其余用正面质感描述」。

---

## 2. 镜头运动：静态质感词 → H3 运镜三要素

- **zy 的 Movement 维度**（locked-off / restrained handheld / vehicle vibration / slow observational drift）是**画面质感**，不是**镜头运动指令**。
- **H3 需要的是**：运镜三要素 = 类型 + 幅度 + 速度（写成动作句，别堆句尾）。
- **映射表**：

| zy 静态质感 | H3 运镜 |
|---|---|
| locked-off（锁定） | `static shot` |
| restrained handheld（克制手持） | `handheld, small amplitude, slow speed` |
| slow observational drift（缓慢观察漂移） | `slow dolly in/out, small amplitude` |
| vehicle vibration（车载震动） | 不写运镜，改写进 Sound 段（`engine hum, mild cabin vibration`） |
| 观察式静止哲学 | 让主体动作成为焦点，不炫技运镜；每镜只一个运镜 |

- 保留 zy 的"观察式构图"精神 = H3 里"static shot + 主体小动作"优先，而非连环炫技运镜。

---

## 3. 示例：单帧校准标杆 → 叠加时间轴

- `examples.md` 三个示例（夜班车失败后/公寓电话后/案件结束后城市）是**画面质感、叙事、动机光**的校准标杆，但缺时间轴。
- **H3 输出时**：保留 zy 的叙事选择 + 动机光 + 观察式构图精华，再叠加 → 时间戳分镜 + 每镜一运镜 + `Sound:` + `Music:`。

---

## 4. 质检清单：frame → shot

- `quality-checklist.md` 的条目对视频**逐条成立**，把 "frame" 读作 "shot" 即可。
- **叠加 H3 专属质检**（zy 没有的）：
  - 时间戳连续、无 gap、无重叠；首镜 00:00、末镜 = 总时长；
  - 每镜 2-5s，镜头数符合预算表；
  - 一镜一运镜，无运镜堆叠；
  - `Sound:` 音效绑可见动作；
  - 禁配乐已明写（`N/A` / `No background music`）。

---

## 5. 完全通用、直接套用（无需翻译）

- `cinematic-principles.md` — 叙事/环境证据/观察式构图/动机光，视频场景更贴合（视频本身就是 sequence）。
- `anti-ai-cleanup.md` — 去 AI 感全局清理层，视频单帧同样需要，每次输出前必读。
- `camera-and-light.md` — 相机决策顺序/焦距表/动机光组合，通用（仅 Movement 维度按上表翻译）。
- `director-routing.md` + `directors/` 47 位四轴指纹库 + `recommendation-matrix.md` — 四轴（光/色/镜头/构图）对视频单帧同样成立，通用。
- `basic-prompt-template.md` — 仅当用户只要**静态电影单帧**（非视频）时用，此时不套时间戳/运镜/声音层。
