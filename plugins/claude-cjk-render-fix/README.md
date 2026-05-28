---
id: voyager.claude-cjk-render-fix
name: Claude · CJK Render Fix
category: render-fix
version: 1.0.0
author: Nagi Studio
license: MIT
matches:
  - https://claude.ai/*
engine: ">=1.0.0"
---

# Claude · CJK Render Fix

Fixes uneven / blurry **CJK (Chinese · Japanese · Korean)** font-weight rendering
in Claude responses **on macOS**.

## The problem

Claude's response markdown forces `-webkit-font-smoothing: antialiased` (a
macOS-only property) and uses non-standard font weights (body `360`, bold `530`)
on the Anthropic Sans/Serif fonts, which contain **no CJK glyphs**. CJK text
falls back to the system font (PingFang); macOS then **faux-bolds** the bold runs
because PingFang has no `530` face — producing the "some thick, some thin" look.
It's macOS-only because `-webkit-font-smoothing` is a no-op elsewhere.

## The fix

A single, surgical declaration — `font-synthesis-weight: none` on Claude's
`.standard-markdown` — which stops the browser from synthesizing fake bold
weights. Bold falls back to the nearest *real* weight and renders cleanly and
evenly, while preserving Claude's intended lightness (no smoothing/weight
changes). No-op off macOS.

This is a **declarative** plugin: pure CSS data, interpreted by Voyager's bundled
engine. No executable code.
