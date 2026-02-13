# Kaide

A discipline-driven, **AI-native frontend architecture kit** for Next.js 16 and TanStack Start. Optimized for Cursor LLM and high-performance engineering.

> **Kaide** (Etymology: Turkish): A fundamental rule, principle, or base that provides structural integrity.

## 🎯 The Problem We Solve
Traditional boilerplates fail when used with LLMs because they lack explicit architectural constraints. This project solves:
- **Context Drift:** Prevents AI from mixing Next.js and TanStack patterns.
- **Architectural Erosion:** Enforces strict feature-based modularity via global "Constitutions".
- **Performance Degradation:** Hard-coded constraints to guarantee Lighthouse 95+ scores by default.
- **Security Gaps:** Integrated CSRF, SSRF, and CSP rules directly into the AI's coding logic.

## 🏗️ Core Architecture
The system operates on a **Three-Layer Governance** model:

- **Global Constitution (`core-principles.mdc`):** Universal rules for code integrity, naming, and error management.
- **Framework Adapters (`nextjs.mdc`, `tanstack-start.mdc`):** Isolated rules for routing, rendering strategies (RSC/SSR), and server functions.
- **Technical Standards (`api.mdc`, `forms.mdc`, `testing.mdc`, etc.):** Domain-specific constraints for state, validation, and performance.

## 🚀 Key Features
- **Feature-Driven Design:** Everything is organized by domain in `src/features/`.
- **Zod-First Type Safety:** End-to-end type safety derived from schemas.
- **Hybrid Framework Support:** Switching between Next.js and TanStack Start without losing architectural discipline.
- **Performance-First:** Built-in rules for LCP optimization, CLS prevention, and tree-shaking.

## 🛠️ Getting Started

### Installation & Setup

1. Copy the `.cursor/rules/` folder, `docs/` directory, and `AGENTS.md` file to your project root.
2. (Recommended) For maximum adherence, point Cursor to your identity in **Settings > Project Rules**:

> **Adhere to `AGENTS.md` for persona/planning and `docs/` for architectural constitutions.**

### Usage Workflow
This kit follows a **Plan-First Protocol**:
- **Initialize:** Ask the AI to read the rules: `Read all .mdc files and AGENTS.md.`
- **Plan:** Propose a feature: `I want to build a [Feature Name]. Create an architectural plan.`
- **Execute:** After approving the plan, the AI will generate code following the strict constraints of the relevant `.mdc` files.

## 📊 Technical Standards
| Layer | Tech Stack | Tooling |
| :--- | :--- | :--- |
| **Server State** | TanStack Query | `tanstack-query.mdc` |
| **Client State** | Zustand | `state-management.mdc` |
| **Validation** | Zod | `typescript.mdc` |
| **Testing** | Vitest + RTL | `testing.mdc` |
| **Styling** | Tailwind CSS | `ui-components.mdc` |

## 🛡️ Security & Performance
- **Zero Inline Script:** Strictly enforced CSP compliance.
- **Locked Boundaries:** Server-only logic is isolated from client bundles.
- **Optimized Assets:** Automatic enforcement of `next/image` and modern formats.