---
name: uaa
description: Universal Architecture Application (UAA) specification
---
# Universal Architecture Application (UAA)

This skill explains the UAA playbook. It helps agents share the idea of a reusable core and thin framework shells.

## Purpose

Apps run on many surfaces (web, mobile, CLI, desktop, servers, workers). They need data, business logic, state, observability, and security. UAA shows how to keep that core the same.

## Core Principles

- **Components** handle small UI or interaction pieces.
- **Services** hold business rules and can be reused.
- **State** stays clear and is updated by domain-aware code.
- **Features** connect services and emit events.
- **Frameworks** stay at the edge and call the core.
- **Cross-layer concerns** live in shared infrastructure.
- **Observability** runs in every layer for logging, tracing, and metrics.

## Architectural Zones

- **Core Architecture**: reusable code for domain, state, and features.
- **Framework Surface Shell**: routes, parses input, and starts requests.
- **Cross-Layer Infrastructure**: observability, security, auditing, caching, config.

## Cross-Layer Infrastructure

It includes:

- **Observability**: logs, traces, metrics, domain events, error reports.
- **Security**: authentication, authorization, secrets.
- **Auditing**: records for compliance.
- **Caching**: data caches outside component logic.
- **Event streaming**: Kafka, NATS, webhooks.
- **Configuration & feature flags**: global toggles and environment settings.

## Observability Specification

Use shared helpers for:

- Logging
- Tracing
- Metrics
- Domain events
- Error reporting

Keep logging structured and trace spans linked from shell to services. Emit metrics from services/shell and keep components focused on UI. Record domain events when key business things happen and report errors with trace IDs.

## Framework Surface Shell

The shell follows the framework's rules (Next.js, TanStack Start, Express, Expo, CLI). It should:

- Start traces and logs.
- Parse request data.
- Call feature entrypoints.
- Never hold business logic or query databases.

## Feature Entrypoint Rule

Each feature has one entry point. It starts spans, calls services, emits events, and updates state.

## Canonical Structures

All shells reuse `/src` as the core:

- **Next.js**: `/app` routes to `/src` which holds features, services, state, components, observability, and primitives.
- **TanStack Start**: `/app/routes` is shell; `/src` is core.
- **Web server**: `/server/routes` is shell; `/src` keeps framework-neutral logic.
- **Expo**: `/app/screens` is shell; `/src` is shared logic.
- **CLI**: `/commands` is shell; `/src` is domain logic and observability.

## Observability Folder Spec

 `/src/observability` keeps helpers:

- `logger.ts`
- `tracer.ts`
- `metrics.ts`
- `events.ts`
- `error-reporter.ts`

## Data + Trace Flow Model

```
Input
 ↓
Shell (start trace)
 ↓
Feature (span + orchestration)
 ↓
Service (span + logs + metrics)
 ↓
External system
 ↓
State update
 ↓
Component render
```

## Design Goals

- Full execution visibility
- Cross-surface traceability
- Structured logging
- Event-driven extensibility
- Compliance readiness

## Anti-Patterns

- Logging only in the shell
- No trace propagation
- Logging business events as strings
- Metrics inside components
- Tying observability to one framework

## Summary

UAA covers core layers (primitives → services → state → components → features), the shell (routing and entrypoints), and cross-layer infrastructure (observability, security, auditing, caching, events). This makes applications portable, observable, and scalable.

## Credits

- **[Component-based architecture](https://en.wikipedia.org/wiki/Component-based_software_engineering)** – splitting the UI into widgets inspired the component/feature layers.
- **[Hexagonal Architecture](https://alistair.cockburn.us/hexagonal-architecture/)** – keeping ports/adapters at the edge mirrors the shell/core split.
- **[Clean Architecture](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html)** – layered policies and use cases shaped the core taxonomy.
- **[MVC](https://developer.mozilla.org/en-US/docs/Glossary/MVC)** – separating model, view, and controller helped keep orchestration outside components.
