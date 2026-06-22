# 1013Q_R0A Render Base Scan For Process Stream Renderer

## Final Status

- Stage: `1013Q_R0A_RENDER_BASE_SCAN_FOR_PROCESS_STREAM_RENDERER`
- Related previous stage: `1013Q_R0_PROCESS_STREAM_RENDERER_CONTRACT`
- Status: `PASS_RENDER_BASE_SCAN_READY_FOR_GITHUB_REVIEW`
- Next stage: `1013Q_R1_PROCESS_STREAM_RENDERER_PREP_ROOM_PREVIEW_POLISH`

## Short Conclusion

The system already has a rendering base. The process stream renderer should not become a separate disconnected page line. It should be treated as a new RenderStage / block family that mounts into the existing prep-room rendering base.

There are two important layers:

- `1013L_R0_MAIN_RENDER_SHELL_BASELINE_AND_BACKEND_REUSE_REGISTRY`: persistent shell and backend reuse registry.
- `1013L_R36_EXISTING_PAGE_STATIC_PATCH_CONSOLIDATION`: current durable static prep-room page baseline.

The next implementation should copy R36 for preview polish, then mount the 1013Q process stream fixture into the existing shell/page behavior. It should not mutate the formal R36 chain in this round.

Correction after review: `frontend/workbench/process_stream_renderer_preview_1013Q_R0.html` is only a temporary fixture viewer from R0. It is not the product route and must not be used as the R1 baseline.

## Evidence Read

### Persistent Render Shell

File:

`backend/xiaobei_ai/prep_room_render_shell_registry_1013L_R0.py`

What it proves:

- There is a canonical render shell contract.
- The shell keeps top navigation, dynamic render stage, and bottom agent input persistent.
- Render stages already include `prep_notebook`, `big_unit_design`, `single_lesson_design`, `courseware_workspace`, `classroom_display_preview`, `material_intake`, and `week_calendar`.
- Boundaries remain readonly: no provider, model, database, memory, Feishu, formal apply, or main project push.
- Visible default agent name at the shell boundary is `小教`, with legacy names normalized rather than used as route keys.

Useful mapping for 1013Q:

`process_stream_renderer` should be introduced as a rendering mode inside existing prep-room states, not as a new standalone app.

### R36 Current Page Baseline

Files:

- `outputs/PREP_ROOM_RENDER_CANVAS_DEEPEN_V1/SESSION_HANDOFF_20260622_PREP_ROOM_R36_ART_DEMO_AND_NEXT.md`
- `outputs/PREP_ROOM_RENDER_CANVAS_DEEPEN_V1/1013L_R36_existing_page_static_patch_consolidation/prep_room_render_canvas_deepen_v1_1013L_R36_consolidated.html`
- `outputs/PREP_ROOM_RENDER_CANVAS_DEEPEN_V1/1013L_R36_existing_page_static_patch_consolidation/1013L_R36_report.md`
- `scripts/validate_1013L_R36_existing_page_static_patch_consolidation.py`

What it proves:

- R36 is the current durable page baseline.
- R22, O_R2, and M_R5 are explicitly superseded and should not be used as baselines.
- R36 consolidated many previous page patches into one static style/script layer.
- R36 keeps paragraph-level courseware cards, right-rail draft screen selection, pointed edit bubble, and independent left/center/right scrolling.
- R36 validation keeps formal boundaries false.

Important metrics recorded by R36:

- Script tags reduced from `25` to `13`.
- Style tags reduced from `16` to `4`.
- `setInterval` reduced from `12` to `0`.
- Mutation observers reduced from `2` to `1`.

Useful mapping for 1013Q:

The process stream should reuse R36's existing reading/editing surface and native bubble/panel style where teacher confirmation or block review is needed.

### Older Renderer Protocol

File:

`frontend/workbench/workbench_renderer_protocol_v1.js`

What it proves:

- The project already has a packet-based renderer protocol pattern.
- Supported render types include `markdown_document`, `schedule_table`, `module_call`, and `status_stream`.
- It rejects raw HTML by default through `safety.raw_html_allowed=false`.
- It renders a status stream separately from final content.

Useful mapping for 1013Q:

1013Q process stream blocks can borrow the same safety posture: structured packets, escaped content, no raw HTML, status/process stream separated from final editable draft.

### 1013Q Contract Already Built

Files:

- `docs/audit/process_stream_renderer_1013Q_R0_report.md`
- `docs/foundation/process_stream_renderer_contract_1013Q_R0.md`
- `docs/foundation/process_stream_block_schema_1013Q_R0.json`
- `fixtures/process_stream/*.json`
- `frontend/workbench/process_stream_renderer_preview_1013Q_R0.html` review-only fixture viewer, not product baseline
- `scripts/validate_process_stream_renderer_1013Q_R0.py`
- `docs/audit_packages/process_stream_renderer_1013Q_R0.zip`

What it proves:

- 1013Q is currently contract-only and fixture-only.
- It creates four local fixtures: normal lesson design, missing material, source mismatch warning, and courseware seed preview.
- Every block exposes source status, confidence, and teacher action requirement.
- Missing or mismatched evidence blocks require teacher confirmation and block formal output.
- The preview page is a temporary local renderer for review, not the final R36 integration. R1 must target the existing main shell and a copied R36 preview surface.

## Recommended Integration Shape

For `1013Q_R1_PROCESS_STREAM_RENDERER_PREP_ROOM_PREVIEW_POLISH`:

1. Copy R36 as a reversible preview branch/page.
2. Add a process stream area before or beside the final lesson/courseware draft entry.
3. Use the 1013Q fixtures as local input only.
4. Preserve the `小教` visible name.
5. Reuse R36 native interaction styling, especially existing edit/review bubble and panel patterns.
6. Keep process blocks separate from final draft body.
7. Do not connect provider/model/database/Feishu/PPT export/formal apply.
8. Do not reintroduce R22/O_R2/M_R5 baselines.
9. Do not continue from the standalone 1013Q_R0 HTML preview page.

## Fit With 飞象老师 Analysis

The useful product-form lesson is still "show the process before showing the final result". The local system already has the better place to host this: the 师维备课室 render shell and R36 page baseline.

Therefore the right move is not to imitate 飞象老师's site, API, routes, code, assets, or UI. The right move is to use the existing 师维 rendering base to express the same trust-building pattern in the local product language.

## Boundaries

This scan did not:

- scrape or call 飞象老师
- inspect external APIs
- connect a provider or model
- modify R36
- write formal lesson body
- write database, memory, or Feishu
- implement PPT export
- publish community content
- push the main project tree

## GitHub Review Payload

This report should be uploaded together with the previous 1013Q_R0 report and package:

- `docs/audit/process_stream_renderer_1013Q_R0_report.md`
- `docs/audit/process_stream_renderer_render_base_scan_1013Q_R0A_report.md`
- `docs/audit_packages/process_stream_renderer_1013Q_R0_manifest.json`
- `docs/audit_packages/process_stream_renderer_1013Q_R0.zip`

Expected review conclusion:

`1013Q_R0A_RENDER_BASE_SCAN_READY_FOR_R1`
