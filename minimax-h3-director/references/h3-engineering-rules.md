<!--
MiniMax H3 工程规范速查表。写提示词时按需查阅，无需全文背诵。
-->

# H3 工程规则速查表

## 1. 硬规格（写提示词前先记住）

| 项 | 值 |
|---|---|
| 输出 | 24fps，32kHz 原生立体声；**分辨率两档**：768p（短边 768，轻量）与 1440p（官方推荐，短边 1440；21:9 时 ≈ 2976×1248；768p 结果可升级至 1440p） |
| 画幅（文生/全能参考） | 21:9 / 16:9 / 4:3 / 1:1 / 3:4 / 9:16；首尾帧模式遵循原图比例 |
| 时长 | 4–15 秒（整数秒），单次生成 |
| 提示词上限 | 约 7000 字符 |
| 参考素材 | 图 ≤9 张（宽高 [256,5760]，**单图 ≤30MB**）、视频 ≤3 段（各 2–15s，总 ≤15s，单视频 ≤50MB）、音频 ≤3 段（各 2–15s，总 ≤15s，单音频 ≤15MB）；**混合总上限 12 个文件** |
| 输入格式 | 视频 H.264/AVC、H.265/HEVC；图片 JPG/JPEG/PNG/WEBP/HEIC/HEIF；音频 WAV/MP3；API 请求体 ≤64MB（推荐用 url 传入） |
| 音频限制 | 音频不能单独提交，须搭配 ≥1 图/视频；完全无文件 → 自动走 T2VA |
| 对白语言 | 中/英/日/韩/法/德/意/西/葡/俄/阿，11 种原生口型；另有 40+ 衍生语言（阿拉伯/泰/印尼/印地等）可探索 |

## 2. 镜头运动表（类型 + 幅度 + 速度）

| 运动类型 | 含义 | 关键词 |
|---|---|---|
| Zoom In / Out | 机身不动，变焦推/拉 | Zoom In / Zoom Out |
| Push In / Pull Out | 机身前进/后退 | Push In / Pull Out |
| Pan Left / Right | 原地左右摇 | Pan Right / Pan Left |
| Truck Left / Right | 整体左右平移 | Truck Right / Truck Left |
| Tilt Up / Down | 原地上下摇 | Tilt Up / Tilt Down |
| Arc Shot | 绕主体弧线运动 | Arc Shot / Orbit（**不推荐单写 orbit**：H3 对"环绕/orbit"指令不稳定，实测改用 `truck left + pan right` 或 `truck right + pan left` 组合更稳） |
| Tracking Shot | 跟拍移动主体 | Tracking Shot |
| Static Shot | 完全不动 | Static Shot |
| POV | 第一人称 | POV |

- 幅度：`with small amplitude`（小幅）/ `with large amplitude`（大幅）
- 速度：`at slow speed`（慢）/ `at fast speed`（快）
- 进阶（有效但不可预测，慎用）：crash zoom、dolly zoom（眩晕）、whip pan（甩镜）、steadicam follow、rack focus（拉焦）、bird's eye、dutch angle（倾斜）。

## 3. 时间戳与镜头预算

- **标准写法（主推）**：后续镜头以 `At 00:0X.000, the camera cuts to ...` 开头（毫秒级绝对切点，官方 guide 规范写法，最严谨、连续无重叠最易保证）。首镜不带时间戳。
- **简化写法（允许）**：短视频简单分段可用 `[0-4s] / [4-8s] / [8-15s]` 区间（官方示例亦可见 `[2s-4s]`），但必须顺序连续、无 gap、无 overlap，末区间结束于总时长。**推荐优先用精确切点**。
- 连续、无 gap、无 overlap；首镜 00:00 起，末镜 = 总时长；每镜 2–5s（低于 1.5s 帧数不足）。

| 时长 | 建议镜头数 |
|---|---|
| 4–6s | 1–2 |
| 7–10s | 2–3 |
| 11–15s | 3–5（最多可到 6 镜快速蒙太奇，但须每段明确一个景别与镜头任务） |

## 4. 声音语法

- **音效绑动作**："软木塞飞出瞬间一声轻响"（对齐）> "一声轻响"（漂）。
- **环境声叠层**：雨声 + 人声嘈杂 + 音乐可共享声床。
- **音乐 brief**：曲风 + 速度 + 情绪 + 配器；画面主导时加"无人声"；不点名歌曲/艺人。
- **禁配乐**：写 `non_diegetic_music: N/A` 或 `Music: None`（否定诉求不写 = 被忽略）。
- **台词**：引号内逐字 + `[语言]` 标签；说话人 `(S1)/(S2)`；音色写标签外；旁白紧跟"lips remain completely closed"。

**对白与音频避坑（实测高频翻车点）**：
- 中文台词口型准确度低于英文/日文；关键台词**字数要少、语速要慢**，5 秒镜头配 1-2 句，塞长句要么语速赶、要么说不完。
- 多人对话务必给每句标说话人 ID 并预留足够秒数，否则谁在第几秒说话易乱；跨镜头同一人保持同一 ID。
- 唱歌/说唱边界不清，H3 主攻对白口型，音乐性演唱优先写进 `non_diegetic_music`（观众向配乐）而非角色唱歌。
- 方言未验证稳定支持，优先用普通话/英语/日语等官方列明语言。
- 原生音频偶发"嗡嗡底噪/电流声"：在 `overall_soundscape` 明确写"quiet room tone, no hum"或在 Negative list 加"no electrical hum"。
- **跨镜头台词（J-cut / L-cut）**：H3 能响应一句台词跨多个 shot——只要明确写出"这句台词跨了 Shot 2–3 继续说"即可，不必强行塞进单镜；一句长台词跨 shot 比塞进 3s 短镜口型更准。

## 5. 参考素材 12 角色（必须声明用途）

角色参考、物体参考、场景参考、关键帧（明说首/尾帧）、声音参考、分镜稿、风格参考、构图参考、音频复用、音频部分复用、动作参考、运镜参考、视频编辑。

> 未标注用途的参考文件 = 参考被无视的头号原因。

## 6. 风格词（只选一个）

Cinematic / live-action / 2D-animated / 3D CG / claymation / watercolor / vintage film / anime / cyberpunk / steampunk / film noir / documentary / black and white / retro VHS / 国风 3D / MG 动画 / 二次元赛璐璐。

**实测稳定性标注（社区 + 官方示例）**：
- ✅ 稳：live-action / cinematic / claymation（官方有验证案例）/ film noir / documentary / black and white — 物理可信、H3 强项。
- ⚠️ 谨慎：anime / 3D CG / cyberpunk / steampunk — 易出现角色漂移、材质塑料感、霓虹过载；务必用参考图锁角色/风格，并加动机光与 Negative list 兜底。
- 🧪 实验：watercolor / retro VHS / vintage film — 风格越抽象越考验一致性，优先短时长（4-6s）单镜验证。
- 通用铁律：只选一个；不要"photorealistic anime"这类混搭；写实风格优先 live-action/cinematic。

## 7. 禁止事项清单

| 类别 | 禁止 |
|---|---|
| 元指令 | 8K、4K、masterpiece、highly detailed、award-winning、viral、trending、cinematic masterpiece |
| 转场语言 | dissolve to、smooth transition into、fade to black 收尾（H3 自动切镜；**镜头间叠化 fade 允许**，仅禁 fade to black 黑场收尾除非用户要求） |
| 文字叠加 | 默认不要画面内文字/logo/字幕/水印（H3 出片默认干净）；**但 H3 支持文字渲染**——用户明确要时须写出文字原文，乱码可图承（见 SKILL 第五步说明），不要一刀切禁止用户要的文字 |
| 参数指令 | 指定 FPS、帧数（用时间戳控时长） |
| 幻灯片式 | "Show A. Then B. Then C." → 写成连续运动 |
| 风格混搭 | photorealistic anime、watercolor hyperrealistic |
| 图生视频 | 重述首帧外观（写"变化"） |
| 运镜堆叠 | 单镜多运镜、短片段堆五个不相关运动 |

## 7.5 提示词写作原则（官方实测，影响出片质量）

- **少用比喻，多写可见画面**：H3 更擅长明确画面指令（景别/动作/光/物体），不是抽象比喻。画面过程段禁止"像被雨声洗过的记忆""时间膨胀"等比喻，改写成可见描述（表盘滴答、雨滴在玻璃上缓慢停滞、瞳孔里流动的光）。比喻只可出现在核心创意的一句话概括。
- **一镜到底（no-cut）写法**：想一镜到底就**删掉【镜头 N】分镜结构**，全文保持一段连续情节描述，不要写切镜；首尾帧模式下 H3 不会自动加切镜，只补两帧之间的动作/光影/声音。

### 7.6 切镜风格（写进核心创意）

- 普通硬切 `cut`（默认，H3 自动处理切点）
- 镜头间叠化 `fade`（允许，用于情绪过渡）
- 跟随节奏卡点切（音乐视频/广告常用）
- 快切 `fast cut`（高剪辑节奏）

## 8. 指令式编辑模板（只改一处不重抽）

```text
修改：
1. 把 @video1 中的 {A} 替换为 {B}。
2. 移除 {C}，使 {期望终态}。
3. 把 {D} 的 {E} 调整为 {F}。
保持：{G} 不变。
```

> 每条指令"目标 → 结果"平铺；删除同时写期望终态（防空洞/畸变）；不重述场景。

## 9. 音色克隆 / 台词替换三规则

1. 参考音频必须说**与目标台词不同的话**（否则复制整轨而非迁移音色）。
2. 台词永远来自提示词；音色参考只迁移音色，不迁移口音/情绪/节奏。
3. 参考音频要干净（无 BGM、单人、无重叠说话）。
