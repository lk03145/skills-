---
name: seedance-storyboard-generator
description: Generate Chinese Seedance 2.0 AI-video production packages from stories, articles, novels, outlines, character briefs, scene briefs, or existing storyboards. Use when the user asks Codex to create or refine short-drama scripts, AI video storyboards, Seedance 2.0 enhanced video prompt packages, LibTV canvas execution prompts, text-to-image prompts, image-to-video prompts, character/scene/prop/blocking-reference lists, LibTV story-board image prompts, black-and-white storyboard-board blocking references, 15-second rhythm budgets, camera-position maps, movement-trajectory arrows, anti-cross-axis continuity plans, dialogue mapping, lip-sync guidance, or episode-continuity packages.
---

# Seedance Storyboard Generator

Use this skill to turn source material into a production-ready Seedance 2.0 package: story/script analysis, material routing, character/scene/prop/blocking-reference assets, storyboard tables, first-frame prompts, enhanced image-to-video prompts, LibTV canvas execution prompts, rhythm budgets, continuity notes, anti-cross-axis controls, dialogue mapping, and final QA.

This skill follows the `Seedance 2.0 分镜脚本整合优化版 1.5`口径:

- 人物图白底无场景，默认画幅 `3:4`。
- 场景图无人空镜，默认画幅 `16:9`。
- 站位参考分镜图用于锁定空间关系，默认输出为 `16:9` 黑白手绘分镜板：顶部项目信息栏、4-8 个逐镜头画面格、角色与道具索引、环境设计、机位/俯视调度图、角色运动轨迹箭头、文字脚本表格，同时包含当前段落人物和环境锚点。
- 分镜图服务剧情，可以有人物、场景、动作。
- Seedance 视频提示词按轻量发包流程执行：素材绑定、整体设定、必要连续性约束、分镜时序、风格约束清楚即可；节奏预算、角色表演逻辑、完整台词映射保留在前期分析和分镜设计中，不原样塞进最终提示词。
- 在 LibTV 画布执行时，正式制片包之后追加紧凑的 `LibTV 执行版提示词`，用 `人物/环境/首帧参考/剧情/镜头/约束` 结构直接服务视频节点。
- 每个 15 秒段落必须先做节奏预算预检；超时要拆段，且每段重新绑定素材、站位、轴线和台词；预算结论只体现在拆段、镜头时长和分镜时序里。
- 多人关系镜头前期必须做空间轴线设定，防止越轴；最终提示词只保留必要的一句轴线/左右关系约束。
- 有对白、旁白、画外音、内心独白时，必须建立台词映射表并逐镜绑定；最终提示词只保留必要的台词原文、口型要求和镜头内说话安排。

## Reference Loading

Load these files only when needed:

- `references/seedance-manual.md`: Seedance 2.0 prompt patterns, templates, camera movement, multimodal reference syntax, and platform constraints.
- `references/故事转视频脚本-转换工具.md`: Detailed story-to-video adaptation template.
- `references/好剧本.md`: Quality bar and example script style.
- `references/优化分镜.md`: Refinement checklist for improving storyboard prompts.
- `references/生图提示词层级法.md`: Image-prompt hierarchy for subject/environment/style/lighting/technical layers.

## Intake

First identify the requested output type. Do not mix these categories unless the user explicitly asks for a full package.

| Type | Default visual rule | People allowed | Primary purpose |
| --- | --- | --- | --- |
| 人物图 / 角色图 / 人物设定图 | 白底或浅灰白纯色背景，不出现场景，默认 `3:4` | 只能出现指定角色 | Lock face, hair, costume, body, temperament |
| 场景图 / 空镜图 / 环境图 | 无人空场景，只表现空间和氛围，默认 `16:9` | 不允许 | Lock layout, architecture, props, light, era |
| 站位参考分镜图 / 空间关系图 / LibTV 故事板 | `16:9` 黑白手绘分镜板，含每个镜头的分镜图、角色道具索引、环境设计、机位/俯视调度图、运动轨迹箭头和脚本表格 | 必须出现当前段落角色 | Lock blocking, left/right position, depth, gaze, camera side, camera map, motion path |
| 分镜图 / 首帧图 / 剧情镜头图 | 人物、场景、动作共同服务剧情 | 允许 | Express a static story moment for first frame |
| 图生视频 / 视频提示词 | 基于首帧或参考素材生成连续镜头 | 按剧情需要 | Build action chain, camera motion, emotion progression |

Ask concise clarifying questions only when key production parameters are missing and cannot be reasonably inferred:

- Target duration: 5s, 10s, 15s, 30s, 120s, or episode count.
- Aspect ratio: default to `9:16` vertical unless specified.
- Visual style: realistic cinematic, Chinese period drama, 1999 county realism, dark eastern myth, animation, 3D, ink wash, cyberpunk, etc.
- Camera perspective: third person, first person, POV, selfie, surveillance, over-the-shoulder, etc.
- Reference material: character image, scene image, motion video, audio rhythm, first frame, ending frame.
- Output scope: script, asset prompts, storyboard table, image prompts, video prompts, continuity plan, or all.
- Spatial relationship: whether two or more characters share a scene, exchange objects, talk, negotiate, walk together, enter/exit, or need a blocking-reference image.

Hard routing rules:

- Do not use a character image as a scene reference; it must stay white-background/no-scene.
- Do not use a scene image as a blocking reference; it must stay empty/no-people.
- When a segment has two or more characters with a clear spatial relationship, or when the user asks for a LibTV `故事板`, create or request a `站位参考分镜图` before writing Seedance video prompts.
- A `站位参考分镜图` is not a beauty still. It is a production-control storyboard board: 16:9 black-and-white hand-drawn sheet with one panel for every shot in the 10-15s segment, character/prop index, environment design, top-down camera-position map, movement-trajectory arrows, and script table. It must still lock environment anchors, left/right positions, depth, gaze direction, camera side, lens direction, and axis. Planned Chinese labels, shot numbers, timecodes, arrows, camera marks such as `C1/C2`, and table text are allowed; avoid garbled text, watermarks, subtitles, or random extra words.

## Core Prompt Principles

Use engineering-style director instructions. Avoid relying on vague adjectives such as `cinematic`, `epic`, `beautiful`, or `high quality` without concrete executable detail.

Stable formula:

```text
精准主体 + 动作细节 + 场景环境 + 光影色调 + 镜头运镜 + 视觉风格 + 画质参数 + 约束条件
```

Pre-production control chain:

```text
素材绑定清楚 -> 场景锚点清楚 -> 人物站位清楚 -> 空间轴线清楚 -> 台词映射清楚 -> 分镜时序清楚 -> 节奏预算清楚 -> 风格约束清楚
```

Seedance prompt send chain:

```text
素材说明 -> 整体设定 -> 必要连续性约束 -> 分镜时序 -> 风格与约束
```

Prompt layering rules:

- Treat rhythm budgets, character-performance logic, dialogue mapping tables, scene-anchor tables, and axis planning as pre-production analysis.
- Do not paste those analysis tables into the final Seedance prompt unless the user explicitly asks for an audit-style package.
- Express analysis conclusions inside `分镜时序`: shot duration, shot scale, camera move, visible action, eye contact, pause, reaction, exact dialogue, and tail-frame state.
- Convert character-performance logic into visible behavior such as eyes, posture, hand movement, breath, walking rhythm, mouth movement, and silence. Do not write abstract psychology as a separate prompt section.

LibTV execution-layer formula:

```text
人物/角色节点 -> 环境/场地节点 -> 首帧/故事板/音频节点 -> 剧情连续动作 -> 镜头调度 -> 明确约束
```

Use this execution layer when the user wants prompts that can be pasted directly into a LibTV video node. It is a compact director prompt derived from pre-production analysis; do not include rhythm-budget tables, character-performance logic tables, or full dialogue mapping inside it.

LibTV execution-layer rules:

- Bind concrete canvas assets with `{{Node <nodeKey>}}` when exact LibTV nodes are known; otherwise use upload slots such as `@图片1`, `@视频1`, or `@音频1`.
- Start with asset binding lines: `人物：...`、`环境/场地：...`、`首帧参考：...`、`故事板/站位参考：...`、`音色/音频：...`.
- Write `剧情：` as one continuous executable event chain in natural Chinese, keeping actions, visible emotions, dialogue, and reactions in the order they should happen.
- Write `镜头：` with concrete camera language such as 多角度、多机位、特写、中景、大景、手持拍摄、跟拍、低角度、超微距、升格慢动作、大特写.
- State negative controls plainly when needed: `无bgm`、`无字幕`、`无多余人物`、`表演自然`、`身体不要僵硬`、`有呼吸感`.
- For songs, voiceover, or lip-sync clips, add: `口型百分百同步音频中的歌词/台词，语气、语速和内容完全相同，不要改变`.
- For local edits, use the stable pattern: `基于 @视频1 / {{Node ...}}，仅修改【对象/区域/动作】为【新内容】，其他不变`.

Writing rules:

- Write who and where first, then what happens, then how it is filmed.
- Externalize emotion as eyes, posture, hands, breathing, walking rhythm, facial micro-movement.
- Keep one primary camera movement per shot.
- Split complex action into multiple shots instead of packing it into one long sentence.
- For multiple characters, bind each person to a reference image, screen side, costume, action, and gaze direction.
- Before video prompts, estimate whether dialogue, action, emotion, camera movement, and safety margin fit the target duration; use the result to split segments and assign shot time ranges, not as a pasted prompt paragraph.
- Every scene with meaningful spatial continuity should name fixed anchors such as door, window, bed, desk, sofa, shoe cabinet, counter, corridor direction, or light source.

## Workflow

1. Analyze source material.
   - Extract protagonist, supporting characters, conflict, story arc, key scenes, emotional beats, hook, and production risks.
   - Preserve recognizable plot while compressing for short-form pacing.

2. Route material type before writing prompts.
   - Classify each requested output as 人物图, 场景图, 站位参考分镜图/LibTV 故事板, 分镜图, or 图生视频.
   - Keep character references, empty scene references, blocking-reference images, first-frame storyboard images, and video prompts separated.

3. Create the script package when needed.
   - Use `起 / 承 / 转 / 合` structure.
   - Include core hook, synopsis, one-sentence selling point, character bios, episode outline, and script body.

4. Plan assets and references.
   - Number characters as `C01-C99`.
   - Number scenes/locations as `S01-S99`.
   - Number blocking-reference storyboard images as `B01-B99`.
   - Number props/objects as `P01-P99`.
   - Number videos/audio references as `V01`, `A01` or platform upload slots like `@视频1`, `@音频1`.
   - Start related asset prompts with a unified visual style prefix.
   - Give major characters distinct identity anchors: face shape, hair, costume silhouette, signature color, prop, expression baseline.

5. Run a 15-second rhythm-budget precheck for each video segment.
   - Estimate dialogue time, action time, emotion time, camera-move time, and 1-2 seconds of safety margin.
   - If the total exceeds the target duration, split the segment before writing Seedance prompts.
   - When splitting, each new segment must re-bind materials, standing positions, axis rules, and dialogue mapping.
   - Keep the budget as analysis; fold its conclusion into shot count, time ranges, pauses, and split decisions.

6. Build scene space controls.
   - Create a scene-anchor table for doors, windows, beds, tables, sofas, shoe cabinets, counters, corridor direction, light source, and other fixed objects.
   - Create a character-position table covering screen left/right, foreground/midground/background, body direction, gaze direction, and relationship to anchors.
   - For each 10-15s segment that needs a LibTV story-board image, first split the segment into 4-8 shots and assign timecodes, shot scale, camera position, visible action, and motion path.
   - When two or more characters share spatial continuity, or when the user asks for a story-board/standing-reference image, generate a `16:9` black-and-white storyboard-board `站位参考分镜图` prompt before the video prompt.

7. Build storyboard and video prompts.
   - For 15s videos, default to 3-5 shots of roughly 3 seconds each.
   - For LibTV `故事板` image nodes, default to 4-8 continuous storyboard panels per 10-15s segment; include every shot panel, a camera-position/top-down map, and trajectory arrows in the image prompt.
   - Use shorter shots for action/fight/explosion/chase, and 3-5s shots for dialogue, reaction, or atmosphere.
   - For each Seedance prompt, use the lean order:
     `素材说明 -> 整体设定 -> 必要连续性约束 -> 分镜时序 -> 风格与约束`.
   - Do not include standalone `节奏预算`, `角色表演逻辑`, or full `台词映射` sections in the final Seedance prompt.
   - If the target is a LibTV canvas or the user asks for directly usable node prompts, append a compact `LibTV 执行版提示词` after the enhanced Seedance prompt.
   - Include only a concise axis-lock sentence when there is a multi-character spatial relationship.
   - Include exact dialogue or lip-sync requirements inside the relevant shot line; keep the full mapping table outside the final prompt.
   - Omit `站位参考分镜图` only when the user does not need a story-board image and the segment has no meaningful multi-character blocking or left/right continuity risk.

8. Verify before finalizing.
   - Check material routing, upload references, rhythm budget, duration coverage, scene anchors, standing positions, camera concreteness, continuity, anti-cross-axis rules, and dialogue integrity.
   - Flag likely sensitive-word or instruction-following risks and propose neutral alternatives.

## 15-Second Rhythm Budget Precheck

Use this before outputting storyboard tables or Seedance video prompts. The goal is natural pacing, not filling every second. This is a planning artifact; do not paste the budget table into the final Seedance prompt unless the user explicitly requests the analysis.

Formula:

```text
台词预计时间 + 动作完成时间 + 情绪表达时间 + 运镜时间 + 安全余量 <= 15 秒
```

Estimation rules:

| Item | Estimate | Handling rule |
| --- | --- | --- |
| 台词 | 普通语速约每秒 3-4 个中文字符；情绪台词更慢 | Long lines split by comma, pause, or emotional beat |
| 动作 | 轻微动作 1-2s；递物/转身/走位 2-4s；复杂动作 4s+ | Split excessive action before adding dialogue |
| 情绪表达 | 眼神停顿、迟疑、压抑、反应 usually 1-3s | Important emotion must not be squeezed out by dialogue |
| 运镜 | Fixed shot costs no extra time; slow push/pull often 2-4s | One primary camera move per shot |
| 安全余量 | Reserve 1-2s per 15s segment | Prevent rushed speech and compressed action |

If the estimate exceeds the segment duration:

- Split into two or more segments.
- Reduce motion before shortening dialogue.
- Use fixed shots or a slow push-in for dense dialogue.
- Place important emotional reaction after dialogue.
- Re-bind materials, standing positions, spatial axis, and dialogue mapping in every new segment.
- In the final Seedance prompt, express the result only through segment split, shot time ranges, pauses, visible reactions, and tail-frame state.

Template:

```markdown
### 节奏预算预检

| 项目 | 预计时长 | 说明 |
| --- | ---: | --- |
| 台词 | X秒 | [按原文估算，不压缩语速] |
| 动作 | X秒 | [核心动作] |
| 情绪表达 | X秒 | [停顿/反应] |
| 运镜 | X秒 | [固定/推近/后拉/横摇] |
| 安全余量 | X秒 | [1-2秒] |
| 合计 | X秒 | [不超过目标时长；超时则拆段] |
```

## Scene Anchors and Character Blocking

Use this whenever a scene contains fixed environment layout, two or more characters, object handoff, negotiation, dining-table action, hospital-room action, office action, hallway action, entry/exit blocking, or continuity between shots.

Scene-anchor examples:

| Anchor type | Example wording |
| --- | --- |
| 门/出入口 | 入户门位于背景中央偏左，角色从画面右侧玄关区域出门 |
| 桌/床/沙发 | 病床位于画面中部偏右，床头靠右侧墙面 |
| 柜/鞋凳/办公桌 | 鞋柜和换鞋凳位于画面右侧，办公桌位于画面右后方 |
| 窗/光源 | 窗户位于背景左侧，日光从左后方进入 |
| 通道方向 | 走廊从前景延伸到背景，人物沿画面右侧向内走 |

Character-position template:

```markdown
### 人物站位关系表

| 人物 | 画面位置 | 前后层次 | 身体朝向/视线 | 空间锚点关系 |
| --- | --- | --- | --- | --- |
| 角色A | 画面左侧 | 中景 | 面向右侧，看向角色B | 靠近门/桌/床的左侧 |
| 角色B | 画面右侧 | 中景或后景 | 面向左侧，看向角色A | 位于鞋柜/办公桌/病床旁 |
| 角色C | 画面左后方 | 后景 | 低头整理道具，不抢台词 | 靠近柜台或床尾 |
```

Blocking-reference trigger:

- If a segment has two or more characters in the same space, or the user asks for LibTV `故事板`, first create `B01` as a `站位参考分镜板`.
- Use `B01` to lock every shot panel, left/right position, foreground/midground/background, scene anchors, gaze direction, camera side, shot-panel rhythm, character/prop index, environment notes, top-down camera-position map, and movement-trajectory arrows.
- In the Seedance prompt, bind `@图片X` to `B01` and say it controls blocking and spatial axis, not beauty styling or final visual color.

Storyboard-board planning rule:

- Before writing the `Bxx` image prompt, create a compact shot plan for the exact 10-15s segment: `Shot 1-N`, timecode, shot scale, camera mark (`C1`, `C2`, etc.), camera direction, character action, and trajectory arrow.
- The `Bxx` prompt must embed those exact shots. Do not write generic wording such as "4-6 shots" without listing what each shot shows.
- In the top-down map, use camera icons/cones labeled `C1/C2/C3...`, solid arrows for the main moving subject, dashed arrows for secondary movement, and a visible 180-degree axis line when spatial continuity matters.

## Script Format

For a full script, use this structure:

```markdown
# [标题] - 剧本

一、核心梗
[短语]

二、故事梗概
故事背景：
开场冲突：
主角画像：
主线事件：
结局：

三、一句话卖点
[一句强钩子文案]

四、人物小传
**角色名**
视觉形象：
身份背景：
核心标签：
性格特点：
金句：

五、剧本大纲
前期（起）：
中期（承/转）：
后期（高潮）：
尾声（收尾）：

六、剧本正文
```

For each episode script body:

```markdown
第X集
X-X [日/夜] [内/外] [场景名称]
道具：
出场人物：

△ 【空镜/开场镜头】[镜头描述]
△ [具体镜头：景别 + 运镜 + 动作 + 光影 + 氛围]
角色名（os）：[内心独白/画外音]
角色名（vo）：[旁白]
角色名（情绪）：[对白]
【字幕：xxx】
△ 【闪回】[闪回镜头]
【闪回结束】
△ 【空镜】[结尾氛围镜头]
```

Rules:

- Start every shot line with `△ `.
- Use concrete shot scale and camera movement: 大特写, 特写, 近景, 中景, 全景, 远景, 过肩镜头, POV, 推镜头, 拉镜头, 摇镜头, 移镜头, 跟镜头, 升降镜头, 手持晃动, 希区柯克变焦, 一镜到底.
- Use `os` for off-screen sound or inner monologue and `vo` for narration/voiceover.
- Use sensory details: light, color, texture, weather, sound, emotional pressure.

## Asset Prompt Formats

### Character Image: White Background, No Scene

Use when the user asks for 人物图, 角色图, 人物设定图, 角色参考图. Default aspect ratio is `3:4` unless the user explicitly specifies another ratio.

Hard rule: only the specified character appears. Use `3:4` portrait framing by default. Do not include rooms, buildings, furniture, mountains, streets, courtyards, background characters, battle scenes, or narrative environments.

```markdown
### C01 - [角色名/正面全身或半身]

白底人物设定图，3:4 竖幅画幅，单人全身，角色为【身份】，年龄约【年龄】，性别【性别】，五官【五官】，发型【发型】，穿着【服装】，体型【体型】，站姿【姿态】，神情【表情】，整体气质为【气质】，画面干净，角色居中，人物比例正常，结构准确，高清细节。纯白背景，不出现场景，不出现室内外环境、建筑、家具、山水、街道、房间、庭院，不出现其他人物，无文字，无水印。
```

### Scene Image: Empty Establishing Shot

Use when the user asks for 场景图, 空镜图, 环境图, 建筑场景, 室内/室外场景. Default aspect ratio is `16:9` unless the user explicitly specifies another ratio.

Hard rule: no people. Use `16:9` widescreen framing by default. Also avoid backs, silhouettes, distant figures, reflections, shadows, hands, feet, hair, clothing edges, or any body part. Traces of human activity are allowed when useful.

```markdown
### S01 - [场景名]

【景别】，16:9 横幅画幅，【场景名称】空场景，整体空间为【空间结构】，画面内包含【家具/建筑/道具/环境细节】，氛围为【情绪氛围】，时间为【日/夜/黄昏/雨夜】，光影为【光线描述】，色调为【色调描述】，整体呈现【视觉风格】，画质高清，细节清晰，环境真实。画面中不要出现任何人物，不要出现主角、配角、群演，不要出现背影、剪影、远处模糊人影、人物倒影、人物影子、肢体局部，保持纯场景空镜效果，无文字，无水印。
```

### LibTV Storyboard Board / Blocking Reference: 16:9

Use when the user asks for 站位参考分镜图, 空间关系图, LibTV 故事板, 多人对话分镜参考, or when the segment has two or more characters with continuity risk.

Hard rule: this is a production-control storyboard board for Seedance/LibTV, not a close-up beauty frame or single still. It must be a complete 16:9 black-and-white hand-drawn sheet for one 10-15s segment, with every shot panel, current segment characters, environment anchors, camera-position map, movement-trajectory arrows, and readable production notes visible.

```markdown
### B01 - [场景名/段落名] 站位参考分镜板

先根据本段 10-15 秒剧情写出镜头清单，再把清单画进同一张故事板。镜头清单必须具体，不要泛写：
| 镜头 | 时间段 | 分镜画面 | 机位/景别 | 角色运动轨迹 |
| --- | --- | --- | --- | --- |
| Shot 1 | 0-X秒 | 【本镜头画面内容】 | C1，【远景/全景/中景/特写】，【机位方向】 | 【角色A从何处到何处，用实线箭头；角色B用虚线箭头】 |
| Shot 2 | X-X秒 | 【本镜头画面内容】 | C2，【景别】，【机位方向】 | 【移动/停顿/转身/冲刺/后退】 |
| Shot 3 | X-X秒 | 【本镜头画面内容】 | C3，【景别】，【机位方向】 | 【移动路径】 |
| Shot 4 | X-X秒 | 【本镜头画面内容】 | C4，【景别】，【机位方向】 | 【移动路径】 |
| Shot 5 | X-15秒 | 【本镜头画面内容】 | C5，【景别】，【机位方向】 | 【移动路径/尾帧站位】 |

生成一张完整的 `16:9` 横幅黑白手绘故事板，不是单张剧照。版式参考专业动画/动作片分镜稿：白纸底、黑色铅笔线稿、粗黑分栏、顶部项目信息栏、镜头编号黑底白字、手绘透视线；如果剧情需要，可用极少单色点缀关键元素，例如怪物红眼、警示灯、路径箭头或能量核心。

顶部中央信息栏写：项目名称：【项目名】｜片段编号：【B01/Scene 01】｜目标时长：【15秒/指定时长】｜限制规则：【轴线/追击方向/关键道具位置】｜风格：黑白手绘专业故事板｜画幅：16:9。

版面必须包含这些区域：
1. 左上角色与场景参考区：用小头像/小全身线稿标注【角色A】【角色B/怪物】【关键道具】【关键场景锚点】，保持与上传资产一致。
2. 中央主视觉区：按时间顺序画出 Shot 1 到 Shot N 的每个分镜画面，每格左侧或角标写 `Shot 1 (0-2s)` 这类时间码。每个镜头格必须呈现具体画面，不得只画示意符号。
3. 右上或右侧场景俯视平面图/机位调度图：画出门、窗、走廊、桌子、消防箱、障碍物等固定锚点；用摄像机图标或视锥标注 `C1/C2/C3...` 机位和镜头朝向；用实线箭头表示主角色运动轨迹，用虚线箭头表示次要角色或怪物运动轨迹；需要时画出红色或黑色 180 度轴线。
4. 底部文字脚本表格：列出镜头号、时间轴、画面简述、备注/音效；每行必须对应上方一个镜头格。

画面内容必须根据本段剧情填写：【把本段 10-15 秒剧情压缩成 Shot 1-N，每个镜头写清楚人物位置、动作、机位、运动方向、道具变化、尾帧状态】。

约束：整体是黑白线稿故事板，线条清晰、有专业分栏和表格感；允许必要中文标签、镜头号、时间码、机位编号和表格文字，但必须工整清楚；不要乱码、不要水印、不要无关装饰字；人物外观、场景结构、道具形态与上传资产一致，适合作为 LibTV `故事板` 图片节点和 Seedance 2.0 站位、轴线、机位、轨迹调度锁定参考。
```

### Storyboard / First-Frame Image

Use when the user asks for 分镜图, 首帧图, 剧情镜头画面.

Hard rule: write a clear static moment, not a continuous action chain.

```markdown
### F01 - [镜头名/首帧]

【景别】，【角色完整外观】处于【静态动作瞬间】，【人物站位】，【场景锚点】，【构图方式】，【场景环境】，【情绪氛围】，【视觉风格】，【光影色调】。画面是可延展的视频起点，角色、道具和空间位置清楚，适合作为图生视频首帧，无文字，无水印。
```

Bad: `男主冲上前拔剑砍向敌人，然后火焰爆炸。`

Good: `男主拔剑前的瞬间，身体微微前倾，右手握住剑柄，眼神冰冷，背景火焰静静燃烧。`

## Seedance Video Prompt: Lean Director Package

Use this structure for each Seedance 2.0 video prompt. Analyze rhythm, performance, blocking, axis, and dialogue first, but keep the final prompt lean.

```markdown
## 第X集 / 第X段：[标题]

### 素材上传列表

| 上传位置 | 素材ID | 用途 |
| --- | --- | --- |
| @图片1 | C01 | 主角外观参考，锁定脸型、发型、服装和体型 |
| @图片2 | S01 | 场景参考，锁定空间布局、建筑风格和光影氛围 |
| @图片3 | B01 | 站位参考分镜板/LibTV 故事板，锁定逐镜画面、人物左右站位、前后层次、空间锚点、视线方向、C1/C2 机位图与角色运动轨迹箭头 |
| @视频1 | V01 | 运镜参考，只学习镜头运动轨迹，不复制人物、场景或剧情 |
| @音频1 | A01 | 背景音乐、对白或节奏参考 |

### 发包前分析摘要（不要粘贴到 Seedance Prompt）

- 节奏结论：【本段可在 X 秒内完成 / 需要拆成 X 段；核心原因】
- 表演逻辑：【角色的情绪起点 -> 可见动作/眼神/停顿 -> 情绪落点】
- 台词/口型：【哪些镜头说哪些原文台词；OS/VO/内心独白是否开口】
- 空间风险：【是否多人同场、是否需要站位参考、是否需要轴线锁定】

### Seedance Prompt（只粘贴这一段）

素材说明：@图片1 作为【角色A】外观参考，锁定【脸型/发型/服装/体型】。@图片2 作为【场景】参考，锁定【空间结构/建筑风格/光影氛围】。@图片3 作为本段站位参考分镜板/LibTV 故事板，优先锁定 Shot 1-N 的逐镜画面、人物左右站位、前后层次、空间锚点关系、视线方向、C1/C2 机位图与角色运动轨迹箭头。后续视频镜头在该故事板基础上推进，保持空间关系稳定，不跨轴，不跳位，不随意改变人物左右位置和运动方向。

整体设定：生成一段【时长】秒视频，比例【9:16/16:9/1:1】，场景为【场景】，角色为【角色】，整体风格为【风格】，核心情绪为【情绪】。全程保持角色外观、服装、发型、脸部特征一致，场景结构稳定，光影统一。

连续性约束：【必要时保留】场景中的【门/窗/桌/走廊/光源】位置稳定；【角色A】始终位于画面【左/右】侧，【角色B】始终位于画面【左/右】侧，镜头保持同侧机位，不跨轴，不反转左右关系。

分镜时序：
镜头1（0-3秒）：【景别】，镜头【运镜】，画面内容【动作/环境/可见情绪】。
镜头2（3-6秒）：【景别】，镜头【运镜】，画面内容【动作/反应】；如有对白，角色只说原文：“【台词】”，口型同步。
镜头3（6-9秒）：【景别】，镜头【运镜】，画面内容【冲突/转折/沉默停顿】。
镜头4（9-12秒）：【景别】，镜头【运镜】，画面内容【结果/反应】。
镜头5（12-15秒）：【景别】，镜头【运镜】，画面内容【定格/悬念】，尾帧稳定，便于续接。

风格与约束：全程画面稳定不变形，人物五官清晰，人体结构正常，动作自然流畅，服装和发型一致，场景结构和光影风格一致。若存在多人关系，全程保持 180 度空间轴线稳定，不跨轴，不反转左右关系。若存在台词，所有台词严格按照剧本原文逐字执行，不改写、不省略、不新增、不换说话人。无肢体扭曲，无穿模，无闪烁，无卡顿，无字幕，无水印，无文字。

### 尾帧描述

[主体、背景、光线、构图、氛围；用于下一集衔接]
```

## LibTV Execution Prompt: Compact Canvas Package

Use this after the enhanced director package when the user is working inside LibTV, gives a LibTV canvas/project, asks for prompt writing style from a canvas, or wants prompts that can be pasted into video nodes.

This compact prompt should feel like a director speaking to the video node. Keep it concise, concrete, and executable. Do not include long tables inside this section.

```markdown
### LibTV 执行版提示词（可直接粘到视频节点）

人物：【角色A】{{Node 角色节点}}，【角色B】{{Node 角色节点}}，【群演/同伴】{{Node 角色节点}}。
环境/场地：【场景名】{{Node 场景节点}}。
首帧参考/故事板/站位参考：【用途说明】{{Node 参考节点}}。
音色/音频：【对白/歌词/旁白/节奏用途】{{Node 音频节点}}。（无音频时删除）

无bgm，无字幕，手持拍摄。（按项目需要保留、删除或改写）

剧情：
【按发生顺序写完整事件：角色初始状态 -> 动作 -> 反应 -> 对白/歌词 -> 情绪变化 -> 尾帧状态。对白必须保留原文，不改写。】

镜头：
多角度，多机位，特写镜头，中景镜头，大景镜头，多镜头组合。【补充具体运镜：手持跟拍/缓慢推近/低角度/过肩/超微距/升格慢动作/大特写】。重点拍清【关键动作/表情/道具/空间关系】。

口型/音频：
【有音频或歌词时写】口型百分百同步音频中的歌词/台词，语气、语速和内容完全相同，不要改变。

约束：
表演自然，身体不要僵硬，有呼吸感；人物外观、服装、发型、场景结构和光影保持一致；不新增无关人物；无字幕，无水印，无多余文字。
```

Execution variants:

- Normal story segment: `人物 -> 环境 -> 剧情 -> 镜头 -> 约束`.
- High-emotion or action segment: add `首帧参考/故事板/站位参考`, then specify close-ups, handheld shake, slow motion, macro shots, and reaction beats.
- Song, choir, dialogue, or voice-driven segment: bind `音色/音频`, then add strict lip-sync and verbatim text requirements.
- Local edit or repair: `基于 @视频1 / {{Node ...}}，仅修改【对象/区域/动作】为【新内容】，其他不变`, then list what must remain unchanged.
- Canvas node style: keep `{{Node ...}}` references next to the noun they control, for example `菲（带着耳机）{{Node BQtchnvfWT}}` and `教室 {{Node aEOTTCiZDi}}`.

## Duration and Shot Splitting

Use these defaults unless the user specifies otherwise:

| Shot type | Recommended duration | Rule |
| --- | --- | --- |
| Fight, explosion, chase, high-intensity action | 1-2s | Keep action simple and split aggressively |
| Normal story action | 2-3s | One core action per shot |
| Dialogue, emotional reaction | 3-5s | Prioritize face, mouth, eyes, pause |
| Empty scene / establishing shot | 3-5s | Slow push, slow pan, or fixed shot works well |
| Single 15s video | 3-5 shots | Use one coherent emotional arc |
| 30s+ sequence | Multiple 15s segments | Reuse same character/scene/style constraints |

Hard rules:

- One shot expresses one core emotion.
- One shot completes one core action.
- Complex actions must be split.
- Do not combine multiple spatial changes and multiple camera moves in one shot.

## Multi-Reference and @ Syntax

Always explain what each uploaded material controls:

- In normal Seedance packages, use `@图片/@视频/@音频` upload slots and define what each slot controls.
- In LibTV canvas execution prompts, use exact node references such as `{{Node <nodeKey>}}` when the project exposes them; place each node immediately after the role, location, audio, first-frame, storyboard, or blocking noun it controls.
- Do not duplicate a wrong node binding across roles. If a node purpose is unclear, label it as `待确认参考` instead of inventing its control function.

- `@图片1 作为主角外观参考，锁定脸型、发型、服装和体型。`
- `@图片2 作为场景参考，锁定空间布局、建筑风格和光影氛围。`
- `@图片3 作为本段站位参考分镜板/LibTV 故事板，锁定每个镜头画面、人物左右站位、前后层次、空间锚点关系、视线方向、C1/C2 机位图与角色运动轨迹箭头。`
- `@视频1 作为运镜参考，只学习镜头运动轨迹，不复制其中人物和场景。`
- `@音频1 作为背景音乐和节奏参考，画面节奏与鼓点同步。`

For multiple characters:

```text
@图片1 是女主外观参考，穿浅青色长裙，神情冷静。@图片2 是男主外观参考，穿深色武将服，神情克制。@图片3 是本段站位参考分镜板/LibTV 故事板，女主位于画面左侧，男主位于画面右侧，Shot 1-N 的镜头画面、C1/C2 机位、运动轨迹、视线方向和空间锚点保持稳定，不新增其他主要人物。
```

Recommended reference load: 1-2 character images + 1 scene image + 1 blocking-reference image + optional 1 motion video + optional 1 audio reference. Keep total references around 4-5 when possible. Do not use a nine-grid as one complex reference; split it into separate usable images.

## Anti-Cross-Axis Rules

Use this whenever there are two or more characters in dialogue, confrontation, fight, handoff, chase, walking together, or any spatial relationship.

Use axis planning as pre-production control. In the final Seedance prompt, include only one concise axis-lock sentence inside `连续性约束` or the relevant shot line.

Standard patch:

```text
全程保持本场 180 度空间轴线稳定，镜头始终在人物连线同一侧拍摄；【角色A】保持在画面左侧，【角色B】保持在画面右侧；两人的视线方向与左右关系一致，不反转站位，不跨轴，不突然改变屏幕方向。若发生换位，必须通过明确的转身或走位动作完成，不能无故跳变。
```

Blocking lock patch:

```text
全程保持 @图片X 站位参考分镜板/LibTV 故事板建立的人物位置关系和调度关系：【角色A】始终位于画面【左/右】侧，【角色B】始终位于画面【左/右】侧；人物与【门/窗/桌/床/鞋柜/办公桌】等空间锚点关系不变；镜头按故事板中的 C1/C2/C3 机位顺序推进，角色运动方向沿故事板箭头，不跨轴，不跳位，不反转左右关系。
```

Rules:

- If A is screen-left in the first shot, A remains screen-left in continuous shots.
- Screen-left characters usually look right; screen-right characters usually look left.
- Over-the-shoulder shots must name the shoulder and target, such as `从男主左肩后拍向女主，女主始终位于画面右侧`.
- Maintain motion direction across cuts unless using a turn, neutral establishing shot, empty shot, or prop close-up to reset space.
- Use stable anchors: table center, door background-right, window screen-left, stairs behind, counter foreground, etc.
- Avoid 360-degree orbit shots in dialogue; keep orbit within 45-90 degrees unless space is reset.
- For handoff, negotiation, table scenes, hospital rooms, offices, hallways, and entry/exit scenes, name both the physical anchor and the character's relationship to it.

Space reset methods:

- Full shot or wide shot that re-establishes all character positions.
- Prop close-up, such as cup, door handle, contract, phone screen.
- Visible character turn, seat change, walking around table, or crossing action.
- Empty-scene transition, such as street, hallway, doorway, or exterior.
- After reset, state the new left/right relationship, gaze direction, and camera side.

## Dialogue Mapping and Lip-Sync

Use this whenever the source contains dialogue, OS, VO, narration, or inner monologue.

Hard rules:

- Quote script dialogue verbatim. Do not rewrite, summarize, shorten, expand, merge, or localize unless asked.
- Bind every line to speaker, shot number, time range, sound type, mouth requirement, and performance action.
- One shot should carry at most one complete line. Split long dialogue by commas, pauses, or beats.
- Important dialogue should use medium-close or close shots with visible mouth, stable face, and restrained motion.
- Do not let two characters speak multiple lines in the same short shot.
- If using external audio, Seedance prompt controls facial performance and rhythm; final wording follows the audio track.
- Keep the dialogue mapping table outside the final Seedance prompt. In the prompt itself, put exact dialogue, sound type, and lip-sync requirement inside the corresponding shot line.

Sound types:

| Type | Meaning | Prompt rule |
| --- | --- | --- |
| 同期对白 | Character on screen speaks, lip sync required | Face or three-quarter face, mouth clear |
| OS / 画外音 | Speaker off screen | On-screen listener does not move mouth |
| VO / 旁白 | Narrative voice | No on-screen character opens mouth unless specified |
| 内心独白 | Character's inner voice | Character remains silent; mouth does not move |

Dialogue shot template:

```text
【景别】，【角色A】位于画面【左/右】侧，正面/三分侧脸可见，嘴部清晰。镜头【固定/缓慢推近】，角色A只说剧本原文台词：“【完整台词】”。说话过程中动作保持克制，只做【轻微表情/手部小动作】，口型与台词同步，语气为【情绪】。画面中其他角色保持安静聆听，不抢台词，不新增台词。全程不改写台词，不遗漏台词，不增加字幕，不出现无关文字。
```

Dialogue integrity patch:

```text
台词必须严格按照剧本原文执行：本镜头只允许【说话人】说出：“【台词原文】”。不得改写、不得省略、不得新增台词、不得改变说话人、不得调整台词顺序。若本镜头为画外音/旁白/内心独白，画面中的角色不得开口。角色嘴部动作与台词节奏同步，台词结束后再进入下一个动作或镜头。
```

## Video Extension, Stitching, First/Ending Frame, Local Edit

Use these task-specific templates when requested:

| Function | Use when | Key control |
| --- | --- | --- |
| 视频延长 | Dialogue, emotional accumulation, single-path movement, same-scene action | Use `<视频1>`, extend smoothly, keep character/scene/light consistent |
| 分段拼接 | Fight, chase, fast transition, multi-scene movement, complex action | Start from previous tail-frame action and keep the same spatial relationship |
| 首帧控制 | Series openings or high character consistency | Use `@图片1` or a first-frame image and keep opening composition/posture consistent |
| 尾帧控制 | Suspense ending, transition ending, next-episode continuation | End on a specified composition/emotional state |
| 局部修改 | Replace costume/prop, remove object, repair detail | Bind time range and spatial region; all other content unchanged |

Video extension:

```text
将 <视频1> 的内容向后平滑延长 10 秒，保持角色外观、服装、发型、场景结构、光影风格与前段完全一致。角色继续沿原方向缓慢前进，动作自然流畅，镜头运动延续前段节奏，无跳切，无闪烁，无变形。
```

Segment stitching:

```text
本段承接上一段结尾动作，使用同一角色、同一场景结构和同一光影风格。开场画面与上一段尾帧保持一致，动作从【上一段尾帧动作】自然继续，镜头节奏紧凑但不跳切。
```

First-frame control:

```text
使用 @图片1 作为首帧参考，开场构图、角色姿态、人物位置、服装发型、场景锚点和光影方向与 @图片1 保持一致。视频从该静态首帧自然启动，动作缓慢展开，无跳切，无闪烁，无突然变换机位。
```

Ending-frame control:

```text
最后停在【指定构图和情绪状态】，主体位置、背景、光线和道具清楚，尾帧稳定，便于下一段作为首帧继续生成。
```

Local edit:

```text
基于 @视频1 进行编辑，在【时间段】内，仅将画面中【区域/对象】修改为【新内容】。保持人物脸部、发型、动作、背景环境、镜头运动和光影完全不变。最终效果自然，无闪烁，无边缘破损。
```

## Standard Output Order

For a full production package, output in this order:

1. 统一素材编号表
2. 节奏预算预检表（分析，不粘贴进 Seedance Prompt）
3. 场景空间锚点表（分析）
4. 人物站位关系表（分析）
5. 空间轴线设定（分析）
6. 台词映射表（分析）
7. 每段 10-15 秒站位参考分镜板/LibTV 故事板提示词
8. 分镜脚本表
9. Seedance 2.0 轻量发包提示词（只保留可执行内容）
10. LibTV 执行版提示词（需要直接贴画布节点时）
11. 最终校验清单

## Final QA Checklist

Before final output, check:

- Material routing: character image is white-background/no-scene with default `3:4`; scene image is empty/no-people with default `16:9`; storyboard image is static story moment; video prompt uses time sequence.
- Blocking reference / LibTV story board: any requested story-board image or multi-character continuity segment has a `16:9` black-and-white 站位参考分镜板 with every shot panel, camera-position/top-down map, C1/C2 camera marks, movement-trajectory arrows, people, environment anchors, left/right positions, foreground/midground/background, gaze direction, and camera side.
- References: every `@图片/@视频/@音频` has a clear control purpose.
- LibTV node bindings: every `{{Node ...}}` reference is attached to the correct role, scene, audio, first frame, storyboard, or blocking purpose; no role inherits another role's reference by accident.
- Prompt separation: rhythm budget, character-performance logic, full scene-anchor tables, full axis planning, and full dialogue mapping stay in pre-production analysis, not inside the final Seedance prompt.
- Rhythm budget: dialogue + action + emotion + camera movement + safety margin fits the target duration; if not, split the segment and reflect the result in shot time ranges.
- Duration: each segment covers its full time range with sensible shot lengths.
- Action: each shot has one executable core action and one primary camera move.
- Scene anchors: doors, windows, beds, tables, desks, counters, corridors, light sources, and key props do not drift across shots.
- Continuity: character face, hair, costume, body, scene layout, light, palette, and props remain stable.
- Anti-cross-axis: multi-character scenes define left/right relationship, gaze direction, camera side, and reset rules when needed.
- Dialogue: every line is in the dialogue mapping table, quoted verbatim, assigned to the correct speaker and sound type; the final prompt includes only the lines needed in each shot.
- Lip sync: important lines use visible mouth; OS/VO/inner monologue do not make silent characters speak.
- LibTV audio/lip-sync: song, choir, voiceover, and dialogue clips include strict audio sync wording when `{{Node ...}}` or `@音频` is bound.
- Local edit: edit prompts say exactly what changes and explicitly keep all other character, scene, lighting, camera, and motion details unchanged.
- Constraints: no extra people in character/scene references, no text/watermark/subtitles unless requested, no deformation/flicker/stutter/crossing axis.
- Sensitive or unstable wording: flag risks and suggest neutral substitutions.
