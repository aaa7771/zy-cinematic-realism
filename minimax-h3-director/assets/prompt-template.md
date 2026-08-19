<!--
MiniMax H3 提示词母版模板。替换每个 [方括号] 字段，删除用不到的段落。（默认输出中英双语：英文完整 prompt 在前、整段中文翻译在后，不逐行交错，写法见文末「双语写法」段）
-->

# H3 提示词母版模板

## 母版（T2VA · 无参考图）

```text
[Shot 1] {视觉风格}, {景别} frames {主体 + 身份细节} in {环境 + 时间 + 天气}, {第一个具体动作}. The camera {运镜类型 + 幅度 + 速度}. {动机光 + 构图}. {灯光/质感}

[Shot 2] At 00:0X.000, the camera cuts to {新景别} as {第二个动作/反应}. {新景别的光与细节}

{收尾节拍：最终构图 / 结束状态}

Sound: {环境声层}, {与动作绑定的音效}.

Music: {乐器 + 速度 + 动态变化} / None.
```

## 变体 1 · I2VA（1 张首帧图）

```text
The supplied image is the first frame at 00:00. Preserve {身份/发型/服装/构图/光线}。

[Shot 1] {只写变化：动作 + 运镜 + 声音}。The camera {运镜}。{需要保持稳定的细节}

Sound: {环境声 + 动作音}。

Music: {配乐} / None.
```

> 铁律：图片已承载外观，文字全部用来买"变化"。不要重述图中已有的服装/环境。

## 变体 2 · FL2VA（首 + 尾两张图）

```text
@image1 是首帧，@image2 是尾帧。保持首尾的人物/构图/光线一致，让动作自然地从首帧过渡到尾帧。

[Shot 1] 从首帧出发，{中间的运动/变换过程}，最终落到尾帧的{姿势/状态}。The camera {运镜}。不要切镜头。

Sound: {过渡中的动作音}。

Music: {配乐} / None.
```

## 变体 3 · L2VA（1 张尾帧图）

```text
@image1 是尾帧。反推一个合理的前情，让动作自然落到这张图上。

[Shot 1] {前情动作}，最终停在尾帧呈现的{姿势/结果}。The camera {运镜}。不要切镜头。

Sound: {前情动作音}。

Music: {配乐} / None.
```

## 变体 4 · Ref2VA（参考图锁角色/风格/动作）

```text
[References]
@image1 是角色参考（锁定 {脸与发型/服装}）。
@image2 是场景参考（锁定 {空间与光线}）。
@image3 是物体参考（锁定 {外观，含标签}）。
@video1 是运镜参考（采用其中的镜头运动，不采用人物与场景）。
@audio1 是声音参考（{音色}）。

[Core idea] {主体 + 地点 + 事件 + 风格}。

[Process] 0–5 秒：……；5–10 秒：……；10–15 秒：……。

[Sound] ……（音效绑动作；不要 BGM 时写 non_diegetic_music: N/A）。

[Negative list] 不要字幕/文字/水印（除非用户明确要文字并已写出原文）；不要背景音乐；不要……（3-5 条最关键拒绝项，与 Style/Sound 并列）。

[Limits] ……
```

## 对白片段（插入任意 Shot 内）

```text
{音色描述} character (S1) says: "{台词，逐字}"

# 旁白（画面里嘴唇没动）
The man (S1) says in an off-screen voiceover: "……" while his lips remain completely closed.

# 多人
The two children (S1, S2) shout together: "……"
```

## 产品/主体锁定片段

```text
Keep the {product shape, label colors, proportions} unchanged throughout. End with {居中/三面视角}, label facing the viewer.
```

## 结尾状态片段（收尾构图，防止漂移）

```text
The shot ends with {主体} centered in frame, the camera completely still, and {关键面} facing directly toward the viewer.
```

## 使用规则

1. 只保留当前任务类型需要的那一个模板，其余段落删掉。
2. 每个 `[Shot N]` 必须：写明景别、一个运镜（类型+幅度+速度）、主体动作；切换时间戳连续。
3. `Sound` 段音效绑动作；`Music` 段写乐器/速度/动态，没有配乐就写 None（`non_diegetic_music: N/A` 也行）。
4. 风格词只在 Shot 1 开头出现一次，不要到处重复。
5. 输出前删掉所有 [方括号] 占位符，不留空行占位。
6. **画面过程说明段末尾统一列"不想要"**（即 `Negative list:` 块）：不想出现的物体/风格/转场/文字水印集中写 3-5 条，与 Style/Sound 并列，不要散落在自然句里。
7. **文字/Logo 渲染**：默认不写画面内文字（出片干净）；若用户明确要文字/标题/品牌名，**必须写出原文**（如 `画面出现英文："H3"`），不要用"一些文字"模糊带过；遇乱码可把文字做成图片参考并声明"按图片理解，不按文字处理"。
8. **一镜到底**：想一镜到底就去掉所有 `[Shot N]` 分镜标题，全文写成一段连续情节，不写切镜。
9. **切镜风格**：在核心创意写明 cut / fade / 卡点切 / fast cut；默认硬切。

## 双语写法（默认输出，不逐行）

海螺 H3 对中文与英文均为原生一等支持（中文甚至是强项，指令遵循与文字渲染不输英文），默认出**中英双语**提示词，双保险覆盖两种解析路径、且更贴合中文市场内容。

**写法（整段对照，不逐行交错）**：先用英文写完整 prompt（可直接粘贴给海螺 App/ComfyUI），其后再附**一整段中文翻译**（以 `[中文翻译]` 起头）。英文承载电影语法词（tracking / medium-wide / practical contrast），中文整段复述保证 H3 中文优化生效，也方便你阅读理解。不要把每一行拆成"英文一行 + 中文一行"交错——太长且易乱。

```text
[Shot 1] Cinematic, medium shot of a woman lifting a coffee cup; the camera tracks right at slow speed.

[中文翻译]
镜头1：电影感，中景，一名女子端起咖啡杯；镜头缓慢向右跟拍。
```

**注意**：双语（英+中两段）约使长度翻倍，复杂多镜（≥5 镜）优先保证英文 prompt 完整，中文翻译段可适度精简（保留风格 / 关键动作 / 声音），勿超 7000 字符上限。用户明确要纯英文 / 纯中文时，只保留对应那段即可。

## 一句话 → 最小可用 prompt（少样本兜底）

用户只给一句话、缺细节时，用这个最小骨架补足可执行信息，**不要追问**（除非缺的是会颠覆结果的元素，如"要图生视频但没给图")。

```text
[Shot 1] {风格, cinematic}, {景别} of {主体}, {一句话里的核心动作}. The camera {一个运镜}. {天气/时间}.

Sound: {与动作绑定的环境声}.
Music: None.
```

示例（输入"一个女孩在雨中奔跑"，默认中英双语）：

```text
[Shot 1] Cinematic, medium shot of a young woman in a light coat running through a rain-soaked street at night, hair and clothes wet, breath visible. The camera tracks beside her at slow speed. Rain on lens, neon reflecting in puddles.

Sound: Steady rain, rapid footsteps splashing in puddles, her quick breathing.
Music: None.

[中文翻译]
镜头1：电影感，中景，一名穿浅色外套的年轻女子在雨夜的湿街上奔跑，头发和衣服湿透，呼吸可见。镜头以缓慢速度在身侧跟拍。镜头上有雨珠，霓虹在积水里反光。
声音：持续的雨声，快速踩过水洼的脚步声，她急促的呼吸。
音乐：无。
```

> 兜底只保证"能生成"，不保证电影感。补完细节后按母版扩展运镜 / 分镜 / 导演层。
