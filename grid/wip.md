# [bltcore](https://github.com/bltcore-com)


### YAML schema for grid panels

Each panel is defined by a YAML file in `src/data/` and rendered by `GridCard` and `GridItem`. A panel describes:
- **panel metadata**: name/title and primary/secondary actions
- **links grid**: rows of up to 4 columns, each column is an optional link cell

Minimal panel:

```yaml
name: example
title: Sample Panel
href1: https://example.com          # Title link
href2: "*/settings"                 # Secondary action; if starts with '*', navigates internally
links:                              # Array of rows
  - pre1: " "                       # Row with spacing
```

#### Top-level fields (panel)
- **name**: string; internal identifier.
- **title**: string; rendered as the card title and linked to `href1`.
- **href1**: string; external URL for the title link.
- **href2**: string; secondary action:
  - If it starts with `"*"` the path after `*` is used for internal navigation (e.g., `*/settings`).
  - Otherwise opened as an external URL on button click; the button label uses `title2` or defaults to "⛭".
- **title1**: string; optional extra text appended to title display.
- **title2**: string; optional subtitle/button label.
- **showas**:
  - `"table"` renders links in a compact 4-column table (center aligned).
  - `"ltable"` renders the same 4-column table but left-aligned cell content.
  - `"table2"` renders a 2-column table (center aligned); use `name1`/`href1` and `name3`/`href3`.
  - `"ltable2"` renders the same 2-column table but left-aligned.
  - `"table3"` renders a 3-column table (center aligned); use `name1`/`href1`, `name2`/`href2`, and `name3`/`href3`.
  - `"ltable3"` renders the same 3-column table but left-aligned.
  - any other value (or omitted) renders default stacked rows.
- **links**: array of row objects (see below).

Note: Files like `0-blank.yaml` are valid empty templates you can start from.

#### Rows and cells (`links` items)
Each item in `links` is a "row". A row can contain up to four "cells", addressed by suffix 1..4. For example, `name1`, `href1`, `title1`, `pre1`, `post1` define the first cell; `name2/...` define the second, etc.

Row-level optional fields:
- **label**: if present and starts with `"-"`, the row renders as a horizontal divider instead of cells.

Per-cell fields (1..4):
- **preN**: string; small leading content before the link text.
  - If the string is wrapped in parentheses and contains a URL like `(https://...favicon...)`, an icon or remote content is attempted and shown inline.
  - If the string starts with `"~"`, the remainder is rendered as small emphasized text; if an `hrefN` exists, it is clickable.
- **nameN**: string; main link text. If it starts with `"-"`, the row is rendered as a section divider with the text (minus the leading dash).
- **hrefN**: string; URL opened when clicking `nameN`.
- **titleN**: string; tooltip/title for the link.
- **targetN**: string; target for the link (defaults to `"_blank"` when omitted).
  - For `chrome-extension://` URLs (e.g. Secure Shell profiles), the BLT extension opens a **new browser tab** via `chrome.tabs.create`; do not use `"_self"` for those links.
- **postN**: string; small trailing content after the link text (plain text).

Behavioral notes:
- Empty rows (no `name1..name4`) render as small vertical spacers.
- In "table" mode (`showas: table`), rows render in a compact table layout with up to 4 cells.
- Remote content in `preN`:
  - URLs in the `(<url>)` form are fetched or rendered as images; favicon service URLs are normalized to `size=16`.
  - If the fetched content is text (e.g., a small status), an abbreviated snippet may be shown.

#### Examples

Divider row:

```yaml
links:
  - label: "-tools"
```

Row with two columns and an icon in the first cell:

```yaml
links:
  - pre1: (https://t3.gstatic.com/faviconV2?client=SOCIAL&type=FAVICON&url=https://chat.openai.com/&size=10)
    name1: chatgpt
    href1: https://chat.openai.com/
    post1: "  "

    name2: docs
    href2: https://platform.openai.com/docs
    title2: OpenAI Docs
```

Table layout:

```yaml
showas: table
links:
  - name1: one
    href1: https://one.example
    name2: two
    href2: https://two.example
  - name1: three
    href1: https://three.example
```

Left-aligned table layout:

```yaml
showas: ltable
links:
  - name1: left
    href1: https://left.example
  - name1: also left
    href1: https://left2.example
```

Internal secondary action:

```yaml
title: Settings
href1: https://example.com
href2: "*/settings"   # Navigates internally to /settings
title2: Configure
```

#### Custom grids (import/export)
- From Settings → General:
  - Export current grid to JSON.
  - Import custom grid from a file (.json, .yaml, .yml) or from a URL.
- The JSON/YAML structure mirrors the objects produced by the YAML files above:
  - Top-level is an object whose keys map to panels (e.g., `p1..p8` or `custom1..custom8`), each panel shaped as described.
- Custom grid data is stored in `localStorage` under the key `bltcore.custom`. When enabled, it replaces the built‑in YAML panels.


---


## Supabase Projects

| Ref | Display Name | Status | Proposed Mapping | Notes |
|-----|--------------|--------|------------------|-------|
| `sinkqyhpejtzuivuvvlg` | `laf-blt-pos` | canonical | `laf-prod-pos` candidate | Likely current canonical LAF POS backend; confirm before any display-name rename. |
| `iqzelztxknfitqkosdvw` | `laf1-blt-pos` | needs-decision | `laf-store1-prod-pos` or migration target | Numbered LAF name needs mapping to Store 1/prod/test role. |
| `ckpvkgqekxttaiqdtcrg` | `laf-clone-blt-core` | needs-decision | `laf-clone-core` candidate | Created 2026-06-25; clone purpose should be documented. |
| `aztdmdunqorpglfgaufa` | `blt-test-laf` | needs-decision | `laf-test-pos` candidate | Different organization and grammar; map before using. |
| `xpuspdtowohlxtmskyct` | `test-blt-core` | needs-decision | `test-core` or `test1/test2` mapping | Generic `test` conflicts with `test1`/`test2` infra naming. |
| `dxanewewxwxsapiwxazt` | `demo-blt-core` | canonical | `demo-core` candidate | Demo backend naming is understandable but not formula-aligned. |
| `pjasnccutstsoftskein` | `bacon-and-eggs` | canonical | `bae-prod-pos` candidate | Customer brand name maps to `bae`; current name is human-friendly but not formula-aligned. |
| `ovpmvfheekvipqipsogk` | `core-flow-point` | needs-decision | unknown | Outside current BLT naming grammar. |
| `nyjzvnuxlnipicigbcqd` | `GridLinks` | needs-decision | unknown | Inactive; classify as external, legacy, or unrelated. |