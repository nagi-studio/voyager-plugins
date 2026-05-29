# Voyager Plugins — Official Marketplace

The official plugin marketplace for **[Voyager](https://github.com/Nagi-ovo/gemini-voyager)**.
Plugins enhance AI chat sites (Claude, ChatGPT, …) with **declarative** style/DOM
tweaks. Voyager fetches this catalog at runtime and lets users browse and install
plugins from its popup.

## What a plugin is

A plugin is **pure data** — a `plugin.json` manifest containing metadata,
declarative DOM operations, settings, and CSS references. No executable code
ships from here; Voyager's bundled engine interprets the data. That makes
plugins:

- **Web Store compliant** (data/CSS is not "remotely-hosted code")
- **Cross-platform** (Chrome / Firefox / Safari)
- safe to load from a remote registry

```jsonc
{
  "id": "voyager.claude-reading-width",
  "name": "Claude · Comfortable Reading Width",
  "version": "1.0.0",
  "description": "…",
  "author": "…",
  "category": "readability", // render-fix | theme | layout | readability | productivity | integration | other
  "license": "MIT",
  "homepage": "https://github.com/you/your-plugin",
  "engine": ">=1.2.0",
  "tier": "declarative",
  "matches": ["https://claude.ai/*"],
  "contributes": {
    "settings": {
      "width": {
        "type": "number",
        "label": "Reading width (px)",
        "minLabel": "Narrower",
        "maxLabel": "Wider",
        "default": 768,
        "min": 600,
        "max": 1600,
      },
    },
    "styles": [{ "file": "style.css" }],
    "domOps": [
      { "op": "addClass", "target": "body", "className": "gv-plugin-…" },
      {
        "op": "setStyle",
        "target": "body",
        "styles": { "--gv-plugin-reading-width": "{{width}}px" },
      },
    ],
  },
}
```

Keep non-trivial CSS in a `style.css` file next to `plugin.json`. Inline
`{ "css": "..." }` still works for tiny snippets, but `style.css` is the
preferred shape because it is readable, reviewable, and formatter-friendly.

`domOps` are reversible and interpreted by the engine: `addClass`, `hide`,
`setAttribute`, `setStyle`. `target` is a CSS selector string (or a semantic key
resolved by the site adapter). Injected classes must be `gv-` prefixed. Setting
tokens such as `{{width}}` can be used in CSS text or in `setAttribute` /
`setStyle` values.

## Localized name & description (optional)

Add an optional top-level `i18n` object to translate `name`, `description`, and
setting labels.
Keys are Voyager's locale codes — `en ar es fr ja ko pt ru zh zh_TW` — and each
value is `{ name?, description?, settings? }`. `en` is taken from the top-level
fields, so you don't repeat it. Fallback is per-field: any missing locale or
missing field falls back to the top-level English. The whole object (and any
field within it) is optional.

```jsonc
{
  "name": "Claude · Comfortable Reading Width", // English fallback
  "description": "Widen Claude's conversation…",
  "i18n": {
    "zh": {
      "name": "Claude · 舒适阅读宽度",
      "description": "将 Claude 的对话拓宽…",
      "settings": {
        "width": {
          "label": "阅读宽度（px）",
          "minLabel": "更窄",
          "maxLabel": "更宽",
        },
      },
    },
    "ja": {
      "name": "Claude · 快適な読書幅",
      "description": "Claude の会話を…",
    },
  },
}
```

## Repository layout

```
marketplace.json                       # the catalog Voyager reads
plugins/
  claude-cjk-render-fix/plugin.json    # official example
  claude-cjk-render-fix/style.css
  claude-reading-width/plugin.json     # official example
  claude-reading-width/style.css
```

## Publishing your own plugin

1. Host your `plugin.json` (your own repo is fine — you keep your copyright and
   choose your own license).
2. Open a PR adding an entry to `marketplace.json`:
   ```json
   {
     "name": "your-plugin",
     "source": "https://raw.githubusercontent.com/you/your-plugin/main/plugin.json"
   }
   ```
3. Official plugins live in `plugins/` here with `"official": true`.

## Licensing

Official plugins in this repo are authored by voyager-official. Third-party plugins
are owned by their authors under whatever license they choose — listing here does
not change that.
