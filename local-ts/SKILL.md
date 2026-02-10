---
name: local-ts
description: Guides setup and development workflows for the local.ts local-first starter kit.
---

# local-ts

This skill helps when creating or modifying apps based on the local.ts starter kit.

## When to Use This Skill

- You are starting a new app from the local.ts template
- You are updating local.ts features (settings, tray, notifications, database, theming, logging, window state, autostart, splash)
- You are troubleshooting local.ts dev workflows

## Workflow

1. Confirm prerequisites: Node.js v18+, Rust, and pnpm.
2. Install dependencies with `pnpm install`.
3. Run the desktop app in dev mode with `pnpm run tauri dev`.
4. For changes that affect native behavior, keep Rust and Tauri config in sync with the frontend.

## Expectations

- Keep local-first behavior intact and avoid introducing network dependencies into core flows.
- Prefer small, reviewable changes and keep UI and native logic consistent.
- Validate any config changes early so failures are loud and actionable.

## Inputs to Request

- Target platform(s): desktop, mobile, or both
- Features to add or modify
- Relevant error logs or reproduction steps

## Resources

- https://www.zapstudio.dev/local-ts
- https://www.zapstudio.dev/llms.txt
