---
name: uaa
description: Universal Architecture Application (UAA) specification
---
# Universal Architecture Application (UAA)

This skill explains the UAA playbook in simple terms. It focuses on the layers that keep domains reusable and the rare cases where a capability truly touches every layer.

## Purpose

UAA helps teams build apps across web, mobile, CLI, desktop, servers, and workers. The goal is to keep data, business rules, state, and features shared, while letting each framework handle routing and parsing.

## Core Principles

- **Components** focus on one interaction or UI piece.
- **Services** hold business processes and expose clear methods.
- **State** stays explicit and is updated through domain-aware code.
- **Features** coordinate services, events, and components.
- **Framework Shells** stay thin and hand work to the core.
- **Cross-layer concerns** live in infrastructure helpers, not inside the core layers.
- **Observability** runs in every layer but never replaces layer responsibilities.

## Architectural Zones

- **Core Architecture** houses the reusable layers.
- **Framework Surface Shell** handles routing, parsing, and lifecycles.
- **Cross-Layer Infrastructure** covers the rare concerns that must touch every layer.

## Core Layer Taxonomy

```
┌──────────────────────────────┐
│ 5. Features / Composition    │
├──────────────────────────────┤
│ 4. Components / Widgets      │
├──────────────────────────────┤
│ 3. State & Events            │
├──────────────────────────────┤
│ 2. Services / Domain Logic   │
├──────────────────────────────┤
│ 1. Primitives / Core         │
└──────────────────────────────┘
```

### Layer 1: Primitives / Core

Primitives are the lowest-level pieces: shared types, guards, helpers, and utilities. They never import higher layers and stay framework-neutral.

### Layer 2: Services / Domain Logic

Services group business rules and data access. They depend only on primitives or other services and expose intent-driven methods for features.

### Layer 3: State & Events

State stores the facts the UI needs and keeps them in one place. Events capture domain signals and may trigger services or update state. This layer prevents components from mutating shared facts directly.

### Layer 4: Components / Widgets

Components render UI, read state, and emit events. They do not call services. Instead, they call feature entrypoints or raise events that features handle.

### Layer 5: Features / Composition

Features orchestrate services, state, and components. Each feature exposes one entrypoint, starts a trace span, coordinates services, updates state, and tells components what to show.

## Layer Relationships

Features compose components and use services to change state. Services talk to primitives. State sits between services and components, keeping reads and writes clear. Events travel from components up to features so side effects stay centralized.

## Cross-Layer Infrastructure

Some concerns must touch every layer. We keep them in small infrastructure helpers so the main layers stay focused.

- **Observability** (logs, traces, metrics, domain events, error reporting) uses helpers in `/src/observability`. Services and features call those helpers without pulling framework code into the core layers.
- **Security** (authentication, authorization, secrets, guards) wraps features and services via middleware or guards.
- **Auditing** (immutable change logs) records actions without changing state or UI logic.
- **Caching** lives in services or infrastructure helpers and never inside components.
- **Event streaming** (Kafka, NATS, webhooks) is handled by services or adapters that speak the protocol.
- **Configuration & feature flags** stay in a config layer and expose shared values to every surface.

Treat each cross-layer helper as a thin module. Observability is the main example we document, but use the same pattern for any rare cross-layer need.

## Framework Surface Shell

Shell files follow the framework (Next.js, TanStack Start, Express, Expo, CLI). They should:

- Start traces and logs.
- Parse input.
- Call feature entrypoints.
- Never run business logic or query databases directly.

### Thin Shell Rule

Shells may start observability spans and add request metadata, but they cannot implement domain logic.

## Feature Entrypoint Rule

Each feature exposes one entrypoint. It starts a span, calls services, emits events, and updates state.

## Canonical Structures

All shells hook into `/src` so the core layers stay the same.

- **Next.js**: the `app` directory is the shell and can sit at the root or inside `src/app` ([Next.js docs](https://nextjs.org/docs/app/building-your-application/routing)). `/src` holds features, services, state, components, observability, and primitives.
- **TanStack Start**: routing lives in `src/routes` and `src/router.tsx`, with `/src` keeping the shared layers ([TanStack Start docs](https://tanstack.com/start/latest/docs/framework/react/guide/routing)).
- **Web server**: `/server/routes` is the shell; `/src` keeps framework-neutral logic.
- **Expo**: `/app/screens` is the shell; `/src` keeps shared logic.
- **CLI**: `/commands` is the shell; `/src` keeps domain logic and observability helpers.

## Observability Folder Spec

The `/src/observability` folder stores helpers like `logger.ts`, `tracer.ts`, `metrics.ts`, `events.ts`, and `error-reporter.ts`. Use them from services or features when you need cross-layer insights.

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
- Not passing trace information
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
