<!--
MiniMax H3 成片翻车诊断表。生成后逐镜回看，定位问题 Shot，按"修法"重试。
单点问题用指令式编辑（@video1 提交出片，只列目标→结果），系统性问题重写该段时间轴。
-->

# H3 成片翻车诊断表

## 用法

生成后逐镜回看（带声音），对照下表定位出问题的 Shot / 维度，再用对应修法重生成。
- **单点修正**：把出片作为 `@video1` 提交，用 `references/h3-engineering-rules.md` 第 8 节的指令式编辑模板，只列"目标→结果"，保留其余不变。
- **系统修正**：光/色/构图/节奏类问题，重写该段时间轴或整条 prompt 重生成。

---

## 1. 手 / 脸漂移、手指异常、面部畸变

- **现象**：手指数错、手指融合、脸在运动中变形、五官漂移。
- **原因**：H3 对身体末端与高速运动的刻画仍是弱项；动作幅度过大或镜头过近放大了弱点。
- **修法**：
  - 减少手部特写与复杂手势；让手"自然持物/垂放"而非精细动作。
  - 该镜头用 `static shot` 或 `slow speed` 小幅推进，降低形变概率。
  - 指令式编辑：`Replace the deformed hands in Shot X with natural resting hands at the subject's sides.`

## 2. 画面内文字 / logo 错乱

- **现象**：招牌、字幕、包装字样变成乱码字母或拼贴噪声。
- **原因**：H3 不擅长精确渲染文字；未声明"不要文字"时易自发生成装饰性错字。
- **修法**：
  - 正文 `Negative list:` 加 `no on-screen text, no subtitles, no watermark, no letterform noise`（除非用户要画面内文字，此时只给少量、简单、重要字样并写明位置）。
  - 产品/包装字样：用参考图锁外观，提示词写明"label text preserved exactly as in reference"。

## 3. 运动伪影、物体形变、物理穿帮

- **现象**：物体凭空出现/消失、穿模、布料/液体运动不自然、地面穿脚。
- **原因**：物理一致性未写清；动作与因果关系模糊。
- **修法**：
  - 写清"动作 → 结果 → 落点"，如 `she sets the cup down, it rests steadily on the saucer`。
  - 避免"瞬间移动/凭空出现"；物体出现要写进入路径。
  - 指令式编辑修复单点：`Remove the floating object near the window; keep the frame empty there.`

## 4. 音画不同步、口型错位

- **现象**：台词与嘴型对不上、声音晚于画面、对白被环境声盖住。
- **原因**：台词过长/语速不当；音效未绑可见动作；旁白未声明"嘴唇不动"。
- **修法**：
  - 台词字数要少、语速慢；5 秒镜头 1-2 句；标 `(S1)` 音色与 `[语言]`。
  - 旁白必须写 `while his lips remain completely closed`。
  - 音效写清进入时机：`at 6 seconds the jazz bass joins`。

## 5. 嗡嗡底噪 / 电流声

- **现象**：音频层有持续低频嗡鸣或电流质感。
- **原因**：原生音频偶发底噪；未声明干净房间声。
- **修法**：
  - `overall_soundscape` 写 `quiet room tone, no electrical hum`；`Negative list:` 加 `no electrical hum, no static`。

## 6. 身份跳变（跨镜头人物不一致）

- **现象**：同一个人在不同 Shot 里脸/发型/衣服变了。
- **原因**：未声明不变量；切镜后未重述身份。
- **修法**：
  - I2VA/Ref2VA：用参考图锁身份，提示词写 `Keep the same face, hairstyle, clothing throughout`。
  - T2VA 多镜：每个新 Shot 重述关键身份特征（年龄/发型/服装颜色）；切镜后写 `Same woman as Shot 1`。

## 7. 风格跳变（多镜头不连贯）

- **现象**：第 1 镜电影感、第 3 镜突然卡通/过曝。
- **原因**：未维持视觉语法基线；每个镜头各自定义风格。
- **修法**：
  - 风格词只在 Shot 1 出现一次。
  - 点名导演时按 SKILL.md「多镜头导演贯穿」用"继承/演进"句；未点名时保证光/色/焦距逻辑跨镜一致。

## 8. 镜头乱晃 / 运镜被忽略

- **现象**：画面无规律抖动；写的运镜没生效（默认缓慢 dolly）。
- **原因**：单镜堆了多个运镜指令；或没写明幅度/速度被模型自行发挥。
- **修法**：
  - 一镜一运镜，写全"类型 + 幅度 + 速度"。
  - 想要锁定：`locked off, static wide shot, no push in, no cuts`（官方例句）。

## 9. 背景音乐乱入

- **现象**：没要配乐却出现电梯感 BGM。
- **原因**：未明写禁配乐，模型默认补环境音乐。
- **修法**：`non_diegetic_music: N/A` 或 `Music: None`，并在 `Negative list:` 加 `do not add background music`。

## 10. 时长 / 画幅不符预期

- **现象**：切了 15 秒却只动 3 秒；图生视频画幅被改。
- **原因**：动作挤在前几秒（缺分镜节拍）；图生视频不应另设 aspect_ratio。
- **修法**：
  - 按时间轴分配每段一个视觉任务，避免"一个想法拉长整段"。
  - I2VA/FL2VA：画幅由首帧决定，不在提示词里写 aspect_ratio。
