---
name: permit
description: Helps define and evaluate type-safe authorization policies using @zap-studio/permit.
---

# permit

This skill focuses on modeling authorization policies and permission checks with `@zap-studio/permit`.

## When to Use This Skill

- You need declarative authorization policies
- You want type-safe permission checks across resources and actions
- You are composing policies or role hierarchies

## Workflow

1. Define resource schemas compatible with Standard Schema.
2. Define actions per resource using typed `Actions`.
3. Create a policy with `createPolicy`, using `allow`, `deny`, and `when` rules.
4. Compose conditions with `and`, `or`, and `not` where needed.
5. Use `policy.can(ctx, action, resourceName, resource)` to evaluate permissions.
6. If combining multiple policies, choose `mergePolicies` (deny-overrides) or `mergePoliciesAny` (allow-overrides).

## Expectations

- Keep policies explicit and easy to audit.
- Avoid hidden side effects in condition functions.
- Prefer readable conditions and small, composable helpers.

## Inputs to Request

- Resource types and their schemas
- Action lists per resource
- Context shape (user, role, tenancy, etc.)
- Rules or edge cases that must be enforced

## Resources

- https://www.zapstudio.dev/packages/permit
- https://www.zapstudio.dev/llms.txt
