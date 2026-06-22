# 1013Q_R1 Process Stream Renderer Main Shell Integration Plan

## Status

- Stage: `1013Q_R1_PROCESS_STREAM_RENDERER_MAIN_SHELL_INTEGRATION_PLAN`
- Depends on: `1013Q_R0_PROCESS_STREAM_RENDERER_CONTRACT`
- Must use existing render base: yes
- New standalone page allowed: no
- Formal apply allowed: no

## Correction

The 1013Q static preview page from R0 is only a review fixture. It must not become the product implementation route.

Do not continue from:

`frontend/workbench/process_stream_renderer_preview_1013Q_R0.html`

Use it only to inspect the four fixtures if needed. The product path must attach the process stream renderer to the existing 师维备课室 main shell and R36 copied preview surface.

## Existing Base To Reuse

### Main Shell Registry

Use:

`backend/xiaobei_ai/prep_room_render_shell_registry_1013L_R0.py`

This is the shell-level source of truth for:

- persistent top shell
- dynamic render stage
- persistent bottom agent bar
- state registry
- backend reuse matrix
- `小教` visible agent normalization

1013Q should be represented as a render-stage capability or sub-mode under existing prep-room states. It should not create a new app surface.

### Current Page Baseline

Use:

`outputs/PREP_ROOM_RENDER_CANVAS_DEEPEN_V1/1013L_R36_existing_page_static_patch_consolidation/prep_room_render_canvas_deepen_v1_1013L_R36_consolidated.html`

R36 is the current page baseline. R1 may copy R36 into a reversible preview directory, then mount process stream blocks into that copy.

Do not use:

- R22
- O_R2
- M_R5
- the R0 standalone process stream preview page as product baseline

## Integration Shape

The process stream should become an intermediate visible layer inside the existing prep-room work flow:

1. Teacher submits request in the persistent bottom agent bar or existing prep-room action area.
2. Main shell keeps the current work state, such as `prep_notebook`, `big_unit_design`, `single_lesson_design`, or `courseware_workspace`.
3. The render stage receives a structured `process_stream` object.
4. Process blocks render before the final draft body or beside the active work object.
5. If missing material or source mismatch appears, the existing review/edit affordance should require teacher confirmation before final draft entry is enabled.
6. Final output remains an editable draft entry, not a formal write.

## Proposed ViewModel Addition

Add a process stream section to the existing render ViewModel, rather than creating a page-level global.

Suggested shape:

```json
{
  "active_view": "prep_notebook",
  "process_stream": {
    "stream_id": "lesson_design_normal_1013Q_R0",
    "status": "preview",
    "visible_agent_name": "小教",
    "blocks": [],
    "teacher_confirmation_state": {
      "required": false,
      "reason": "",
      "allowed_actions": []
    },
    "final_output_ref": {
      "formal_output_created": false,
      "editable_draft_entry": true,
      "label": "课时可编辑稿"
    }
  }
}
```

The `blocks` array should reuse the 1013Q schema fields:

- `block_id`
- `block_type`
- `display_title`
- `summary`
- `status`
- `confidence_level`
- `source_status`
- `teacher_confirmation_required`
- `teacher_action_required`
- `evidence_refs`
- `warnings`
- `actions`

## R36 Mount Points

R1 should inspect R36 and choose one reversible mount point:

- Before the editable lesson/courseware draft body, as a "生成过程" area.
- In the center reading surface above the current selected work object.
- In the right rail as a compact process/status stack only when the center body is already occupied.

Preferred first attempt:

Render process stream in the main center surface before final draft text, because the teacher needs to see reasoning and evidence before accepting content.

## Interaction Rules

Reuse existing R36/native interaction patterns:

- `nb-edit-bubble`
- `nb-edit-panel`
- `nb-edit-surface`
- existing edit/review bubble behavior where available
- existing independent scrolling behavior

Do not introduce a separate floating UI system.

## Render Rules

- Escape all fixture/model text before rendering.
- Do not allow arbitrary HTML from process blocks.
- Missing material must show a confirmation gate.
- Source mismatch must show a warning block and confirmation gate.
- Low-confidence or blocked blocks must require teacher action.
- Teacher-visible assistant name is `小教`.
- `小备` is not used in new teacher-visible copy.

## Route / Registry Direction

Do not add a public product route for the standalone preview page.

If a registry change is needed later, add a shell capability such as:

```json
{
  "state_id": "process_stream_review",
  "teacher_label": "生成过程",
  "render_stage_role": "process_before_final_output",
  "backend_source": "1013Q process_stream fixture or future readonly endpoint",
  "route_or_adapter": "existing prep-room render stage viewmodel",
  "reuse_action": "mount_into_existing_R36_copy_preview",
  "status": "preview_only_needs_R1_copy"
}
```

But R1 should first prove the mount inside a copied R36 preview before altering the registry.

## R1 Work Order

1. Copy R36 into a new reversible preview directory.
2. Inject one local fixture as `process_stream` data inside the copied R36 ViewModel.
3. Add a small renderer function that maps process stream blocks into existing center surface markup.
4. Reuse R36 edit/review bubble affordance for block actions where possible.
5. Keep all formal boundaries false.
6. Add a validator proving:
   - copied R36 source is preserved
   - no standalone product page is used
   - process stream fixture is mounted in R36 copy
   - missing/mismatch gates block final draft entry
   - no provider/model/db/Feishu/PPT/formal apply

## Hard Boundaries

- no standalone product page
- no R36 formal mutation
- no provider/model calls
- no database writes
- no Feishu writes
- no PPT export
- no community publishing
- no external Flying Elephant scraping/copying
- no arbitrary HTML execution

## Next Stage

`1013Q_R1_PROCESS_STREAM_RENDERER_PREP_ROOM_PREVIEW_POLISH`

R1 is a copied-R36 preview integration, not a new page.
