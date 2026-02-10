---
name: fetch
description: Helps build typed HTTP clients with @zap-studio/fetch and Standard Schema validation.
---

# fetch

This skill focuses on type-safe HTTP requests validated with Standard Schema.

## When to Use This Skill

- You need type-safe API calls with runtime validation
- You want to wrap `fetch` with a consistent, typed API
- You are handling validation and network errors explicitly

## Workflow

1. Define Standard Schema-compatible schemas for API responses.
2. Use the `api` client to make requests such as `api.get(url, schema)`.
3. Handle `ValidationError` for schema failures and `FetchError` for network issues.
4. For shared configuration, create a pre-configured client using the factory helpers.

## Expectations

- Avoid unsafe `as` assertions for response typing.
- Validate all external data at the boundary.
- Keep error handling explicit and context-rich.

## Inputs to Request

- Base URL and auth requirements
- Expected response schemas
- Error handling preferences
- Required HTTP methods

## Resources

- https://www.zapstudio.dev/packages/fetch
- https://www.zapstudio.dev/llms.txt
