# Voyager Plugins — Official Marketplace

The official plugin marketplace for **[Voyager](https://github.com/Nagi-ovo/gemini-voyager)**.
Plugins enhance AI chat sites (Claude, ChatGPT, …) with **declarative** style/DOM
tweaks. Voyager fetches this catalog at runtime and lets users browse and install
plugins from its popup.

## What a plugin is

A plugin is **pure data** — a `plugin.json` manifest containing metadata + CSS +
declarative DOM operations. No executable code ships from here; Voyager's bundled
engine interprets the data. That makes plugins:

- **Web Store compliant** (data/CSS is not "remotely-hosted code")
- **Cross-platform** (Chrome / Firefox / Safari)
- safe to load from a remote registry

```jsonc
{
  "id": "voyager.claude-cjk-render-fix",
  "name": "Claude · CJK Render Fix",
  "version": "1.0.0",
  "description": "…",
  "author": "…",
  "category": "render-fix",          // render-fix | theme | layout | readability | productivity | integration | other
  "license": "MIT",
  "homepage": "https://github.com/you/your-plugin",
  "engine": ">=1.0.0",
  "tier": "declarative",
  "matches": ["https://claude.ai/*"],
  "contributes": {
    "styles": [{ "css": "…" }],
    "domOps": [{ "op": "addClass", "target": "body", "className": "gv-plugin-…" }]
  }
}
```

`domOps` are reversible and interpreted by the engine: `addClass`, `hide`,
`setAttribute`, `setStyle`. `target` is a CSS selector string (or a semantic key
resolved by the site adapter). Injected classes must be `gv-` prefixed.

## Localized name & description (optional)

Add an optional top-level `i18n` object to translate `name` and `description`.
Keys are Voyager's locale codes — `en ar es fr ja ko pt ru zh zh_TW` — and each
value is `{ name?, description? }`. `en` is taken from the top-level fields, so
you don't repeat it. Fallback is per-field: any missing locale or missing field
falls back to the top-level English. The whole object (and any field within it)
is optional.

```jsonc
{
  "name": "Claude · CJK Render Fix",     // English fallback
  "description": "Fixes uneven CJK text…",
  "i18n": {
    "zh": { "name": "Claude · 中日韩字体渲染修复", "description": "修复…" },
    "ja": { "name": "Claude · CJK 表示崩れ修正", "description": "修正…" }
  }
}
```

## Repository layout

```
marketplace.json                       # the catalog Voyager reads
plugins/
  claude-cjk-render-fix/plugin.json    # official example
  claude-reading-width/plugin.json     # official example
```

## Publishing your own plugin

1. Host your `plugin.json` (your own repo is fine — you keep your copyright and
   choose your own license).
2. Open a PR adding an entry to `marketplace.json`:
   ```json
   { "name": "your-plugin", "source": "https://raw.githubusercontent.com/you/your-plugin/main/plugin.json" }
   ```
3. Official plugins live in `plugins/` here with `"official": true`.

## Licensing

Official plugins in this repo are authored by voyager-official. Third-party plugins
are owned by their authors under whatever license they choose — listing here does
not change that.
