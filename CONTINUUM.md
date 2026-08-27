<!-- CONTINUUM AGENT GUIDE
Rules for Claude CLI / Cursor (and humans):

1. CONTINUUM.md is the project brain at repo root. Canvas media may live under .continuum/assets/ only.
2. Update the fenced continuum block when something meaningful changes (goal, tasks, decisions, handoff, canvas map).
3. Do NOT dump full chat transcripts into this file. Keep summaries short.
4. Prefer updating tasks/decisions/currentState/handoff after real progress.
5. If nothing meaningful changed, do not rewrite this file.
6. Canvas nodes: chat | note | image | link | decision | task.
   - image/link nodes use url= (project-relative path or https URL).
   - Agents may POST /api/canvas/nodes or /api/canvas/assets — same as the UI.

## Cursor local API (curl)

Continuum desktop exposes localhost HTTP. Default port 3927.

GET context (decisions, tasks, handoff, canvas):
  curl -s http://127.0.0.1:3927/api/context \
    -H "Authorization: Bearer $CONTINUUM_TOKEN"

Update brain / canvas (JSON body merges into brain):
  curl -s -X PATCH http://127.0.0.1:3927/api/context \
    -H "Authorization: Bearer $CONTINUUM_TOKEN" \
    -H "Content-Type: application/json" \
    -d '{"handoff":"Next: fix tests","currentState":"..."}'

Add link or note on canvas:
  curl -s -X POST http://127.0.0.1:3927/api/canvas/nodes \
    -H "Authorization: Bearer $CONTINUUM_TOKEN" \
    -H "Content-Type: application/json" \
    -d '{"type":"link","title":"Docs","url":"https://example.com","x":200,"y":120}'

Upload image to canvas (base64, lands in .continuum/assets/):
  curl -s -X POST http://127.0.0.1:3927/api/canvas/assets \
    -H "Authorization: Bearer $CONTINUUM_TOKEN" \
    -H "Content-Type: application/json" \
    -d '{"filename":"shot.png","dataBase64":"...","title":"Screenshot","x":240,"y":160}'

Create / move task:
  curl -s -X POST http://127.0.0.1:3927/api/tasks \
    -H "Authorization: Bearer $CONTINUUM_TOKEN" \
    -H "Content-Type: application/json" \
    -d '{"title":"Add login","status":"ready","priority":"high"}'

  curl -s -X PATCH http://127.0.0.1:3927/api/tasks/t1 \
    -H "Authorization: Bearer $CONTINUUM_TOKEN" \
    -H "Content-Type: application/json" \
    -d '{"status":"done"}'

Token and port: Continuum Settings. Prefer curl GET before big Cursor work.
-->

# ubuntu-dock-setup

Project brain lives in the fenced `continuum` block below.
Edit via Continuum UI, agents, or curl — Continuum keeps the board in sync.

```continuum
goal:
  Floating Ubuntu Dock one-shot setup (dash-to-dock) with verify + GTK config UI — Continuum board seeded from canvas Nucleus reference.

requirements:
  - One-shot curl|bash install that applies floating bottom dock defaults
  - Verify mode diffs gsettings + asserts no screen strut
  - GTK config UI with live apply, stay presets, style packs, export/import
  - Optional indicators-bottom patch (sudo) to keep running dots under icons
  - Documented SHA256SUMS / DOCK_SETUP_EXPECT_SHA256 trust path

architecture:
  - dock.sh — apply/verify/reset/show/config + indicators-* modes
  - dock-config.py — GTK3 UI over org.gnome.shell.extensions.dash-to-dock
  - indicators_patch.py — user-local ubuntu-dock CSS/override for indicator position
  - docs/ — hero screenshots; CONTINUUM.md — project brain + board + canvas

constraints:
  - Settings changes need no sudo; indicator patch does (system extension path)
  - verify checks script defaults, not live UI tweaks
  - Strut check via xprop is X11-only; Wayland skips that assertion
  - Canvas media only under .continuum/assets/

currentState:
  Core dock tooling is shipped on main. Continuum brain was still template; board+canvas now synced from the 2026-08-21 Nucleus kanban screenshot on the canvas.

handoff:
  Board cards (NUC-*) mirror the canvas screenshot. Next for dock-setup: fill empty tests/ and decide whether to keep demo board cards or swap to a dock backlog.

tasks:
  - [id=NUC-344 status=todo priority=high assignee=Aya notes=tag:BILLING] Optimize experience for mobile web
  - [id=NUC-360 status=todo priority=medium assignee=Sofia notes=tag:ACCOUNTS] Onboard workout options (OWO)
  - [id=NUC-337 status=todo priority=high assignee=Dev notes=tag:ACCOUNTS] Multi-dest search UI mobileweb
  - [id=NUC-339 status=todo priority=high assignee=Layla notes=tag:FORMS] Billing system integration - frontend
  - [id=NUC-340 status=todo priority=medium assignee=Arjun notes=tag:ACCOUNTS] Account settings defaults
  - [id=NUC-342 status=running priority=high assignee=Ravi notes=tag:ACCOUNTS] Fast trip search
  - [id=NUC-335 status=running priority=medium assignee=Maya notes=tag:BILLING] Affelite links integration - frontend
  - [id=NUC-341 status=running priority=high assignee=Sofia notes=tag:FORMS] Shopping cart purchasing error - quick fix required
  - [id=NUC-367 status=blocked priority=high assignee=Layla notes=tag:ACCOUNTS] Revise and streamline booking flow
  - [id=NUC-358 status=blocked priority=medium assignee=Sofia notes=tag:ACCOUNTS] Travel suggestion experiments
  - [id=NUC-354 status=blocked priority=medium assignee=Sofia notes=tag:ACCOUNTS] Ongoing customer satisfaction
  - [id=NUC-351 status=blocked priority=low assignee=Aya notes=tag:FEEDBACK] Planet Taxi Device exploration & research
  - [id=NUC-340d status=done priority=high assignee=Arjun notes=tag:BILLING] High outage: Software bug fix - BG Web-store app crashing
  - [id=NUC-341d status=done priority=high assignee=Maya notes=tag:FORMS] Web-store purchasing performance issue fix

decisions:
  - [id=d1] Floating dock via dock-fixed=false + intellihide
    reason: Stops strut so maximized windows use full screen; hide-when-covered is the supported overlap model.
    alternatives: Pinned dock-fixed=true (reserves space) | Unsupported always-visible overlap
  - [id=d2] Seed Continuum board from canvas Nucleus screenshot
    reason: User dropped board screenshot and asked to update CONTINUUM.md / board / canvas together.
    alternatives: Only document ubuntu-dock backlog tasks

activity:
  - [2026-08-20T18:07:56.657Z] Continuum project initialized
  - [2026-08-21T06:58:58.000Z] Synced brain, board (NUC cards from screenshot), and canvas via Cursor API
  - [2026-08-21T06:58:58.420Z] Updated via curl PATCH /api/context
  - [2026-08-21T06:59:04.090Z] Updated from Continuum UI
  - [2026-08-21T06:59:51.679Z] Updated via curl PATCH /api/context
  - [2026-08-21T07:15:52.352Z] Updated from Continuum UI
  - [2026-08-21T07:15:55.666Z] Updated from Continuum UI
  - [2026-08-21T07:15:56.936Z] Updated from Continuum UI
  - [2026-08-21T07:15:58.272Z] Updated from Continuum UI
  - [2026-08-21T07:15:59.391Z] Updated from Continuum UI
  - [2026-08-21T07:16:01.040Z] Updated from Continuum UI
  - [2026-08-21T07:20:44.641Z] Updated from Continuum UI
  - [2026-08-21T07:25:52.225Z] Updated from Continuum UI
  - [2026-08-21T07:25:53.683Z] Updated from Continuum UI
  - [2026-08-21T07:25:55.939Z] Updated from Continuum UI
  - [2026-08-21T07:25:58.166Z] Updated from Continuum UI
  - [2026-08-21T07:26:39.492Z] Updated from Continuum UI
  - [2026-08-21T07:28:54.205Z] Updated from Continuum UI

canvas:
  nodes:
    - [id=n1787295189356 type=image x=-64 y=-160] Nucleus board reference
      url: .continuum/assets/a1787295189356-nucleus-board.png
      summary: Source screenshot for Continuum board seed (TO DO / IN PROGRESS / IN REVIEW / DONE).
    - [id=n-board-note type=note x=424 y=-184] Board seed
      summary: 14 visible NUC cards imported into Continuum tasks. IN REVIEW → blocked column. Duplicate screenshot IDs → NUC-340d / NUC-341d.
    - [id=n-repo-link type=link x=440 y=8] GitHub repo
      url: https://github.com/codebyshoaib/ubuntu-dock-setup
      summary: ubuntu-dock-setup upstream
    - [id=n-arch-note type=note x=-96 y=104] Dock stack
      summary: dock.sh + dock-config.py + indicators_patch.py. Empty tests/ is the next real dock work item.
  edges:
    - [id=e-img-board] n1787295189356 -> n-board-note label=seeded
    - [id=e-arch-repo] n-arch-note -> n-repo-link label=code
```
