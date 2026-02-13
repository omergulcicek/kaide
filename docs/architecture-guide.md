# Architecture Constitution

## Technical Stack (SSOT)

| Layer | Technology | Authority (Rule Ref) |
| :--- | :--- | :--- |
| **Server State** | TanStack Query | `tanstack-query.mdc` |
| **Client State** | Zustand | `state-management.mdc` |
| **Validation** | Zod | `typescript.mdc` |
| **UI / Styling** | shadcn/ui + Tailwind | `ui-components.mdc` |
| **API Client** | Axios (Centralized) | `api.mdc` |

## Framework Isolation Boundary

Implementation details for Next.js and TanStack Start are strictly segregated. Framework-specific patterns are delegated to their respective .mdc rules.

- **Next.js 16:** See `.cursor/rules/frontend/nextjs.mdc`
- **TanStack Start:** See `.cursor/rules/frontend/tanstack-start.mdc`

**Shared Layer:** Reserved strictly for framework-agnostic code (`shared/schemas`, `ui-primitives`).

## Global Directory Hierarchy

Adhere strictly to the following tree structure. Creating arbitrary folders is FORBIDDEN:

```text
src/
├── app/ | routes/        # Routing layer (See Framework Rules)
├── features/             # Domain & business logic
│   └── [feature-name]/
│       ├── api/
│       ├── components/
│       ├── helpers/
│       ├── hooks/
│       ├── schemas/
│       ├── stores/
│       ├── types/
│       └── index.ts
├── components/
│   ├── ui/               # Atomic (shadcn)
│   ├── layout/           # Layout parts
│   └── shared/           # Reusable functional
├── config/               # App/SEO config
├── constants/            # Static values
├── helpers/              # Framework-agnostic pure functions
├── hooks/
├── i18n/                 # Localization setup
├── lib/                  # Third-party configurations
├── messages/             # Translations
├── providers/            # Global context
├── schemas/              # Shared validation
├── stores/               # Global state
├── styles/               # Tailwind/base CSS
├── types/                # Global TS types
└── env.ts                # Env validation
```

## Shared vs. Feature Matrix (The Rule of Three)

Apply the following metric hierarchy to determine code location:

- **Default Scope:** All logic and components originate within `features/[feature]/`.
- **Promotion:** Any logic or component requested by 2 different features MUST be moved to the global `src/` (shared) layer.
- **Cross-Import Ban:** Direct imports between features are FORBIDDEN. Communication is restricted to the `src/shared` layer or via prop-drilling at the Page level.
- **Helpers:** Pure TypeScript functions that strictly transform input to output without side effects.
- **Lib:** Contains project-specific configured instances of external libraries. Components MUST import the wrapped instance from `src/lib` rather than the external package directly.
- **Zod Schemas:** All feature-related schemas MUST be stored in `features/[feature]/schemas/`. API functions and UI components MUST import from this single source to prevent duplication.

## Server State & Ownership

Server state is global, but access is hierarchical:

- **Ownership:** Every API hook belongs to its respective feature. Promote to `src/hooks/api` only if shared.
- **Invalidation:** Cross-feature cache invalidation is permitted only via the global `Query Key Factory`.
- **Defaults:** StaleTime: ~60s (Lists), ~5m (Details). Centralized management: `src/providers/query-provider.tsx`.

## Rendering & Access Control

Rendering strategies (RSC vs Client) and Data Fetching patterns are governed by the active framework's specific rule file.

- **No HTTP in UI:** UI layer is FORBIDDEN from using `axios` or `fetch` directly. Consume only `features/api` functions.
- **Insecure Scripts:** All content MUST adhere to the project's Content Security Policy; `unsafe-inline` and `unsafe-eval` are strictly forbidden.

## Canonical Rules (MDC Refs)

This document is the architectural map. Enforceable laws are located in:

- `Ref: .cursor/rules/core-principles.mdc`
- `Ref: .cursor/rules/frontend/api.mdc`
- `Ref: .cursor/rules/frontend/forms.mdc`
- `Ref: .cursor/rules/frontend/i18n.mdc`
- `Ref: .cursor/rules/frontend/nextjs.mdc`
- `Ref: .cursor/rules/frontend/performance.mdc`
- `Ref: .cursor/rules/frontend/react-best-practices.mdc`
- `Ref: .cursor/rules/frontend/state-management.mdc`
- `Ref: .cursor/rules/frontend/tanstack-query.mdc`
- `Ref: .cursor/rules/frontend/tanstack-start.mdc`
- `Ref: .cursor/rules/frontend/test.mdc`
- `Ref: .cursor/rules/frontend/typescript.mdc`
- `Ref: .cursor/rules/frontend/ui-components.mdc`
