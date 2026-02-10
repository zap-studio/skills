---
name: validation
description: Guides Standard Schema validation flows with @zap-studio/validation.
---

# validation

This skill helps normalize schema validation across Standard Schema-compatible libraries.

## When to Use This Skill

- You are validating input or API payloads with Standard Schema-compatible schemas
- You need a consistent error type across Zod, Valibot, ArkType, etc.
- You want a single validation path for sync and async schemas

## Workflow

1. Check schemas with `isStandardSchema` before validation.
2. Use `standardValidate` for normalized async or sync validation.
3. Catch `ValidationError` for structured issues when using throwing mode.
4. Use `createSyncStandardValidator` only for schemas that are strictly sync.

## Expectations

- Prefer the shared `ValidationError` type for error handling.
- Keep validation boundary logic centralized.
- Treat `allowSubdomains: false` as a best-effort guard when using email validation.

## Inputs to Request

- Schema library (Zod, Valibot, ArkType, etc.)
- Whether validation should throw or return issues
- Expected error handling behavior

## Resources

- https://www.zapstudio.dev/packages/validation
- https://www.zapstudio.dev/llms.txt
