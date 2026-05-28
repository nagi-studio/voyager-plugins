---
id: voyager.claude-reading-width
name: Claude · Comfortable Reading Width
category: readability
version: 1.1.1
author: Nagi Studio
license: MIT
matches:
  - https://claude.ai/*
engine: ">=1.1.0"
settings:
  width: "number (40–120, default 70) — max reading width in characters"
---

# Claude · Comfortable Reading Width

Constrains Claude chat messages to a comfortable maximum reading width and
centers them, so long lines don't sprawl across wide screens.

## Adjustable

Exposes a **width** setting (40–120 characters, default 70). In the Voyager
popup the plugin shows a slider; dragging it updates the width live. The value is
substituted into the plugin's CSS (`max-width: {{width}}ch`) by Voyager's engine.

This is a **declarative** plugin: pure CSS + a typed setting, interpreted by
Voyager's bundled engine. No executable code.
