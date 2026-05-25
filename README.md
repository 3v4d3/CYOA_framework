# EXTINCTION AVERTED // GENERATIVE TEXT ENGINE

A JSON-driven interactive narrative with state-dependent prose, asset mechanics, and relationship tracking.

## Deploy to GitHub Pages

```
repo/
  index.html    ← engine (no story content)
  story.json    ← all narrative content
  README.md
```

1. Push both files to a GitHub repo
2. Settings → Pages → Source: `main` branch, `/ (root)`
3. Done. GitHub serves it at `https://yourname.github.io/repo-name/`

## Local Development

Browser CORS blocks `fetch()` on `file://` protocol. Use any static server:

```bash
# Python (built-in)
python -m http.server 8000
# then open http://localhost:8000

# Node
npx serve .

# VS Code: Live Server extension works too
```

## Story Format

All content lives in `story.json`. The engine interprets it — no JS editing required for content changes.

### Node types
- `narrative` — standard scene with prose, D.A.V.E. panel, and choices
- `dead_end` — failure state, red background, rollback button
- `ending` — conclusion state, blue background, restart button

### Prose item types
| type | what it does |
|------|-------------|
| `static` | plain text, always shown |
| `pool_index` | `prose_pool[pool][state.path]` — integer index |
| `pool_key` | `prose_pool[pool][state.path]` — string key |
| `asset_pool` | checks `assets_acquired` array for priority keys, falls back to `default_key` |
| `conditional` | if/elif/else branches; each branch can have `text`, `pool_ref`, and `side_effects` |

### Condition operators
`eq` `neq` `gt` `gte` `lt` `lte` `array_includes` `array_includes_any` `array_nonempty` `array_empty` `or` `and` `not`

### Action operators (on choices and prose side_effects)
`set` `clamp_inc` `push` `set_min` `set_max`

### Dev panel
Click `[SYS_DIAGNOSTICS]` (bottom-right) to open the dev panel.  
Type any node ID and hit Enter to jump directly to it.  
Full ENGINE_STATE dump shown live.

## Adding Content

To add a new node, add an entry to `nodes` in `story.json`. To add new prose variants, add entries to `prose_pool`. No engine changes needed.

Example minimal node:
```json
"n_my_scene": {
  "type": "narrative",
  "chapter": "MODULE X — TITLE",
  "prose": [
    { "type": "static", "text": "Something happened." }
  ],
  "dave": null,
  "choices": [
    { "clause": "§ X.", "text": "Continue.", "leads_to": "n_next_scene", "actions": [] }
  ]
}
```
