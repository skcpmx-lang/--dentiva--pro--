# Design system — Dentiva Pro

## Product visual language

A clinical editorial workspace, not a generic analytics dashboard: quiet navy navigation, warm near-white canvas, teal focus on the next action, strong patient identity, compact legible records and restrained financial numerals. Clear boundaries and hierarchy before ornament. No stock patient photography, fake graphs or noisy gradients.

## Tokens

| Token | Light value | Use |
|---|---|---|
| primary | #123C43 | Core action, clinical identity |
| secondary | #244C62 | Supporting blue/navy |
| accent | #087F8C | Interactive emphasis |
| background | #F5F7F8 | App canvas |
| surface | #FFFFFF | Cards/forms |
| elevated | #FFFFFF | Dialogs/popovers |
| border | #DCE4E7 | Subtle boundaries |
| text-primary | #152C36 | Body/title |
| text-secondary | #526772 | Captions/metadata |
| success | #16634D | Positive text/icon, pale #EAF5F0 surface |
| warning | #805309 | Needs attention, pale #FFF6E5 |
| danger | #AA3044 | Error/destructive, pale #FFF0F2 |
| info | #225E8C | Information, pale #EDF4FA |
| disabled | #A1AFB7 | Noninteractive state; labels remain understandable |
| sidebar | #102D35 | Navigation |
| sidebar-text | #DCE7EA | Inactive label |
| focus | #087F8C | 3px high-contrast outline with 2px offset |

Dark theme requires independently audited surfaces/contrasts; do not simply invert print documents. Contrast is measured, not assumed. Color always paired with text/icon/status label.

## Typography

Bundled Inter UI font; `Noto Sans Bengali` for Bengali and print shaping, system sans fallback. No fonts fetched from a network.

| Role | Size/line height/weight |
|---|---|
| display | 32/40 px, 600 |
| page title | 28/36 px, 600; compact 24/32 |
| section | 20/28 px, 600 |
| card | 16/24 px, 600 |
| body/clinical | 14/22 px, 400; clinical textarea 15/24 |
| label | 13/20 px, 500 |
| caption | 12/18 px, 400 |
| table | 13/20 px, 400; column labels 11/16 uppercase restrained tracking |
| KPI | 28/34 px, 600 tabular-nums |
| money/time/code | tabular-nums; never truncates totals |
| print | 10–12 pt normal width; Bengali 11–12 pt; narrow documents minimum ~9 pt after test |

Avoid all-caps clinical content. Long text wraps; only deliberate list summaries ellipsize with accessible full text. Patient identity, allergies, balances, visit date are readable priorities.

## Geometry

Base spacing 4 px: 4/8/12/16/20/24/32/40/48. Controls 40 px comfortable, 36 px compact; icon 16 or 18, exact center; primary touch target at least 40 px. Radius: 8 controls, 12 cards, 16 dialogs. Border 1px. Shadow: small `0 2px 6px rgb(16 45 53 / .04)`; raised dialog `0 16px 48px rgb(16 45 53 / .16)`. No glass overlay behind clinical reading.

Sidebar expanded 232 px, collapsed 76 px; independent scroll; group labels; active inset pale teal with accent marker. Main page padding 28 standard/18 compact. Header 72 px with date/current user balanced; no unbounded name taking all action space.

## Components and state contract

- Buttons: explicit text/icon, inline-flex center/gap 8, min-size; default/hover/focus/pressed; disabled `disabled` attr; loading preserves width and label; success feedback outside button; destructive distinct and confirmed.
- Inputs: visible label, required indicator and explanation, native autocomplete where safe, visible invalid border+linked error, retained input after server rejection. Date/number/currency helpers never rely on placeholders alone.
- Forms: sections with helpful next action, max readable widths, save/cancel always reachable, native submit/keyboard support, dirty-state prompt. Long wizard/modal scrolls internally; modal focus trap, Escape with dirty protection, focus restore and `aria-modal`.
- Tables: real semantic tables, sticky header when useful, bounded overflow, prioritizable columns, sortable accessible headers, page controls, row-link + clear action (no nested controls inside links). Currency aligned right. Empty rows replaced by meaningful explanation/action. Data is never invented.
- Cards/KPIs: balanced 6→3→2 columns deliberately; normal neutral outline; numeric amounts right/left consistently; permission-aware content absent from server response and DOM.
- Status chip: label and optional icon, color as supplementary signal. Compact enough for tables, never tiny unreadable text.
- Tabs: correct selected role/state and keyboard movement; no dead tabs; profile sections use stable identifiers and accessible heading.
- Loading: skeleton for async cards/list, `aria-busy`; spinner+progress for backup/restore; cannot double-submit financial creation. Error: clear context/retry, no stack trace. Success: role=status, durable receipt for long tasks. Validation: aria-describedby/invalid and error summary focus.
- Empty: small contextual icon, specific headline, short explanation, authorized action. No misleading success/count for absent data.
- Notifications: actual local event, timestamp/severity/link/read state; scoped to permission; no fake bell badges. Toasts polite non-blocking except urgent destructive validation.
- Tooltips: collapsed sidebar/icon meaning, keyboard as well as hover; never only accessible label source.
- Charts: SVG/local rendering only from actual grouped data, legends/data summary, zero-data state; no decorative fabricated trend lines.

## Responsive and accessibility

Test 960×600/1280×720/1920×1080/2560×1440/3840×2160 and 100/125/150/200% Windows DPI. Main min-width 0, child min-width 0; table overflow stays local. Compact sidebar at tight desktop widths; drawer at truly narrow window. Dialogs max-width `calc(100vw - 32px)`, max-height `calc(100dvh - 32px)`, internal scroll, reachable controls. Tooltip/dropdown portals avoid clipping. Six equal cards never accidental 4+2. Two-column clinical forms collapse meaningfully, not by squashing controls.

Visible focus, real labels, native controls, correct DOM/tab order, tooltip names, keyboard operation, no color-only warnings. Reduced-motion removes transitions. Default animation 120–180ms opacity/transform, no delayed critical actions/layout shifts. Screen audit measures contrast and axe plus manual focus/zoom. Print layouts always light, no UI chrome, no shadows.

## Icon and brand

Owned geometric tooth/DP mark, centered on deep clinical-teal tile with rounded transparent corners; strong white silhouette recognizable 16px. Generate original concept and prepare SVG/PNG/ICO (16/24/32/48/64/128/256). Audit alpha edges and no background halo. Test actual Windows shortcut/taskbar/start/uninstaller/installer/window icon. Asset existence alone does not pass Windows icon acceptance.
