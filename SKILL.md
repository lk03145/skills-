---
name: seedance-storyboard-generator
description: Generate Chinese Seedance 2.0 AI-video production packages from stories, scripts, outlines, novels, or short-drama episodes. Use when the user needs short-drama adaptation, character/scene/prop prompt extraction, storyboard scripts, batch image prompt queues, relationship and blocking reference diagrams, Seedance time-axis prompts, continuity checks, or reusable AI short-drama workflow folders.
---

# Seedance Storyboard Generator

Use this skill to turn Chinese story material into a Seedance-ready AI short-drama production package. The upgraded workflow supports complete episode folders, Word/Excel-friendly tables when needed, batch reference-image prompts, and relationship/blocking diagrams for Seedance video continuity.

## Reference Loading

Load only the files needed for the current task:

- `references/seedance-manual.md`: Seedance 2.0 prompt patterns, camera movement, multimodal reference syntax, and platform constraints.
- `references/故事转视频脚本-转换工具.md`: story-to-video adaptation template.
- `references/好剧本.md`: short-drama quality bar and example script style.
- `references/优化分镜.md`: storyboard refinement checklist.
- `assets/workflow-template/`: reusable project folder template for AI短剧工作流.

## Intake

Identify the input type:

- Complete script: preserve plot and restructure it for production.
- Outline or topic: expand into a short-drama script with conflict, reversal, suspense, and emotional progression.
- Existing storyboard or prompt pack: normalize, refine, or convert it to Seedance format.

Ask concise clarifying questions only when key production parameters are missing and cannot be reasonably inferred:

- Visual style: default to `电影级写实`.
- Episode duration: follow the user request; otherwise default to 15 seconds per episode.
- Aspect ratio: default to 9:16 vertical unless specified.
- Output format: Markdown by default; use Word or Excel-friendly tables when the user needs WPS/Office formatting.

## Production Workflow

1. Create or copy the project folder.
   - Use `assets/workflow-template` as the default structure.
   - For a formal episode, create one project folder named like `项目名_第X集_Seedance生产包`.

2. Normalize the script.
   - Extract hook, conflict, reversal, scene beats, emotional beats, and ending frame.
   - For fixed duration episodes, compress action so total storyboard time stays within the requested limit.

3. Extract production assets.
   - Characters: `C01-C99`.
   - Scenes: `S01-S99`.
   - Props: `P01-P99`.
   - Generate image prompts for each character, empty scene, and key prop with one unified visual style prefix.

4. Create the storyboard script.
   - Include shot number, shot size, camera angle, camera movement, visual description, duration, audio/dialogue, asset references, and continuity notes.
   - Use concrete camera language: 远景, 全景, 中景, 近景, 特写, 推镜, 拉镜, 跟拍, 环绕, 升降, 手持晃动, 希区柯克变焦.

5. Prepare batch image generation.
   - Use `assets/workflow-template/06_本集人物场景道具图/批量生图清单.md`.
   - Put character images in `人物_C`, scene images in `场景_S`, prop images in `道具_P`.
   - Validate outputs with `图片验收表.md`.

6. Prepare relationship and blocking references.
   - Use `assets/workflow-template/07_人物关系站位图/人物关系站位图清单.md`.
   - Generate `REL` relationship diagrams for complex character relationships.
   - Generate `BLK` blocking diagrams for multi-person scenes.
   - Reference `REL/BLK` images in Seedance prompts when spatial continuity matters.

7. Generate Seedance prompts.
   - Include upload lists, time-axis prompts, sound design, reference image mapping, tail-frame descriptions, and continuation notes.
   - For episode continuation, use wording like `将@视频1延长15秒`.

8. Run continuity and quality checks.
   - Verify asset IDs, timing coverage, role consistency, scene lighting continuity, prop states, REL/BLK references, and sensitive-risk substitutions.

## Template Outputs

For each episode, produce:

- `00_原始剧本`: normalized source script.
- `01_场景人物道具提示词`: asset extraction table.
- `02_剧本分镜头脚本`: storyboard script.
- `03_Seedance剧本提示词`: Seedance time-axis prompt file.
- `04_连续性与质检`: continuity and QA report.
- `06_本集人物场景道具图`: batch image queue and image acceptance table.
- `07_人物关系站位图`: relationship and blocking reference queue.
- `99_成片与素材输出`: final media/material export area.

Keep final production output in Chinese unless the user asks otherwise.
