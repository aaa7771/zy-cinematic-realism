<!--
Copyright (c) 2026 ZY / popopo-99
SPDX-License-Identifier: CC-BY-NC-4.0
-->

# Examples

Use these examples for calibration, not as text to copy mechanically.

## 1. Night Bus After Failure

**Input:** Two detectives ride the night bus home after a failed investigation.

**Story choice:** Failure is shown through distance and silence rather than confrontation.

**Prompt excerpt:**

```text
A restrained frame from a late-1980s New York crime drama. Near midnight, inside a nearly empty city bus moving through a rain-soaked neighborhood, two detectives sit several rows apart after their strongest lead collapses. The older detective remains near the aisle, his coat still wet, holding a paper cup without drinking. The younger detective watches storefronts break apart in rain and window reflections. Faded plastic seats, scratched poles, old advertisements, water marks on the floor, and a forgotten newspaper make the bus feel long used. The camera observes from the last row at seated eye level, partly interrupted by seat backs and reflections, leaving both figures small inside the empty interior. Aged green fluorescent fixtures provide the main light; passing amber streetlights briefly touch wet fabric and glass. Natural exposure, soft highlight roll-off, restrained 35mm grain, subtle halation around real streetlights, and slight vehicle motion blur. The frame feels like failure slowly turning into mutual understanding.
```

**Avoid:** `hero pose, dramatic confrontation, looking at camera, promotional poster, perfect symmetry, clean modern bus, fashion photography, unmotivated rim light, excessive lens flare, cyberpunk neon, HDR, CGI`

## 2. Apartment After a Phone Call

**Input:** A person sits alone in a room thinking in the afternoon.

**Story choice:** Make the thought physically specific by placing it after a difficult phone call.

**Prompt excerpt:**

```text
A quiet frame from a contemporary European drama. Late afternoon in a modest apartment, a woman sits at a small dining table after a difficult phone call. The phone lies face down beside an untouched glass of water; one hand rests near the glass while the other stays in her lap. Unopened mail, a jacket over a chair, faint water marks beneath a plant, and dishes drying in the dim kitchen suggest an interrupted ordinary day. The camera watches from the hallway through a half-open door at standing eye level, with the doorway and an out-of-focus chair forming a natural boundary. She is off-center and small within the room. Soft overcast window light is the primary source; a low warm lamp provides a weak secondary pool. Cool domestic shadows, gentle warm highlights, slight hallway underexposure, fine grain, and natural skin texture. The frame feels private rather than performed.
```

**Avoid:** `beauty portrait, dramatic crying, looking directly at camera, luxury interior, commercial lifestyle photography, perfect symmetry, artificial rim light, HDR, plastic skin, CGI`

## 3. City After the Case

**Input:** The case is over and the city returns to normal.

**Story choice:** Remove the detectives and make the city the subject.

**Prompt excerpt:**

```text
An early-morning final frame from a late-1980s New York crime drama. The detectives are gone. Far down a worn avenue, an old police car disappears between delivery trucks as shopkeepers raise metal shutters, a worker moves boxes onto the sidewalk, and a pedestrian waits with a folded newspaper. Wet pavement, faded storefront lettering, curbside trash bags, and steam from a manhole hold the remains of the night. The camera observes from behind a scratched bus-shelter panel at natural eye level; reflections and one passing pedestrian partly obscure the view. No person dominates the composition. Cool pre-sunrise ambient light fills street-level shadows while weak amber sunlight reaches the upper brickwork. Natural 35mm grain, faded color response, soft lens behavior, and minor pedestrian motion blur. The frame suggests that individual stories end while the city continues.
```

**Avoid:** `hero ending, dramatic sunrise, postcard composition, clean modern city, empty polished street, commercial city photography, cyberpunk neon, excessive flare, HDR, digital sharpness, concept art`

## 4. Hand-drawn Tram (H3 官方 goodcase · 手绘特效)

**Input:** 纯文生。15 秒 16:9，将实拍末班电车车厢与手绘发光动画融合。

**Story choice:** 生活感、怀旧、温柔哀愁；相机始终比手绘动画慢半拍，像拍摄者真的在车厢里追着它移动。

**Prompt 结构（核心骨架，非全文）：**

```text
【核心创意】15 秒，16:9 横版。空荡末班电车车厢里，乘客用手机单手临时拍摄，遇到手绘杏橙色发光线在车厢里连续变形（车票→纸燕子→毛毛虫→箭头→小帆船→迷你电车→蜗牛→雨伞→小鱼→整车晚霞云海），最后回到车票碎成纸屑。整体生活感、怀旧、温柔而略带哀愁。

【画面过程描述】
0–3 秒：镜头从拍摄者指尖在起雾车窗上画弧线开始，指尖离开雾痕变杏橙手绘发光线……相机慢半拍追。
3–6 秒：纸燕子绕吊环飞，尾巴勾住扶手环……相机迟一步推进，运动模糊+短暂失焦。
……（每个时段：正向动作 + 反向约束，如"不要平稳广告式构图""不要新角色凭空出现"）

【整体要求补充】手绘平面逐帧发光涂鸦（非 3D/CG）；线条每帧轻微变化有蜡笔毛边；相机始终慢半拍；禁止 3DCG/毛绒风/电影感过强布光/字幕/背景音乐/恐怖怪物化。
环境音：车轮与轨道摩擦、车厢摇晃、扶手环碰撞、纸质车票摩擦、布座椅被碰、拍摄者小声惊呼与脚步声；手绘音：极轻粉笔摩擦、玻璃刮擦、柔软电子颤音。
```

**为何值得借鉴：** 把"持续变形 + 慢半拍手持"写成"正向动作 + 反向约束"成对出现，是 H3 控制复杂连续动画的高信息量写法；其"禁止项"直接嵌入整体要求段，而非独立 Avoid。

## 5. Big-type MV (H3 官方 goodcase · 大字幕 MV)

**Input:** 3 张参考图（场景风格 / 文字包装样式 / 人物形象）+ 纯文生。10 秒 16:9 trap MV。

**Story choice:** 两位 fly detective 兄弟在近景地下空间轮流对镜头 rap，高反差印刷海报质感英文块字随 bass hit 压屏，卡点硬切。

**Prompt 结构：**

```text
【参考素材说明】@图片3 场景视觉风格；@图片2 文字包装样式；@图片1 人物形象（只参考指定维度，不出现真实品牌/原 logo）。
【核心创意】10秒，16:9 横版 trap MV。兄弟搭档地下空间轮流 rap，卡点硬切，高反差印刷海报质感英文块字随 bass hit 压屏。地下音乐录像带 + 时尚杂志拼贴 + 高时装质感。
【画面过程描述】Shot 1 面部极近特写……文字 "TWO FLY" 巨大粗体压入，不遮挡眼睛；硬切。Shot 2 中近景半身……文字 "CLUES"……硬切。（共 6 Shot + Final，每 Shot 写明景别/人物/文字/律动/切法）
【整体要求补充】粗颗粒、胶片抖动、复印纸颗粒、扫描错位、跳帧残影；只用硬切/跳切/鼓点切/遮挡切/闪白硬切；不用淡入淡出/流体转场。
```

**为何值得借鉴：** 文字/Logo 渲染的正确姿势——把英文块字原文逐字写出 + 明确"不遮挡眼睛"的构图约束 + 卡点硬切节奏；证明 H3 支持复杂文字包装，关键是"写出原文 + 给约束"，而非回避文字。
