# architecture-guide.md

## Tech Stack

* Server State: TanStack Query
* Client State: Zustand (Next.js ve TanStack Start’ta aynı)
* Validation: Zod
* UI: shadcn/ui + TailwindCSS
* i18n:

  * Next.js → next-intl
  * TanStack Start → Paraglide JS
* API Layer: feature-based `features/[feature]/api`
* SSR Server Calls:

  * Next.js → Server Actions / Route Handlers
  * TanStack Start → serverFn

## Overview

Feature-based (domain-driven) mimari. Hem Next.js hem TanStack Start ile uyumlu. Framework farkı yalnızca routing katmanındadır:

* Next.js: `src/app/`
* TanStack Start: `src/routes/`
  Diğer tüm yapı ortaktır.

## Folder Structure

```text
src/
├── app/ | routes/        # Routing layer
├── features/             # Domain & business logic
│   └── [feature-name]/
│       ├── api/          # API calls
│       ├── components/   # Feature UI
│       ├── hooks/        # Feature logic
│       ├── schemas/      # Zod models
│       ├── stores/       # Feature state
│       ├── types/        # TS types
│       └── index.ts      # Public API
├── components/
│   ├── ui/               # Atomic (shadcn)
│   ├── shared/           # Reusable functional
│   └── layout/           # Layout parts
├── hooks/                # Generic hooks
├── lib/                  # SDK, utils, axios, config
├── providers/            # Global context
├── stores/               # Global state
├── schemas/              # Shared validation
├── types/                # Global TS types
├── constants/            # Static values
├── config/               # App/SEO config
├── styles/               # Tailwind/base CSS
├── i18n/                 # Localization setup
├── messages/             # Translations
└── env.ts                # Env validation
```

## Next.js Client/Server Rule

* Default: Server Component
* `"use client"` yalnızca gerekli olduğunda
* Data fetching öncelikle server tarafında
* `features/[feature]/components` hem server hem client içerebilir
* Client boundary en dar noktada tutulur

## API & Server Boundary

* Component içinde HTTP yazılmaz
* Tüm istekler `features/[feature]/api`
* UI sadece tüketir
* SSR sayfalarda:

  * Server işlemleri → serverFn / Server Actions
  * DB, Supabase, secret erişimi sadece server’da
  * Client/Server ayrımı API layer’da çözülür

## State Placement

* Tek feature → `features/[feature]/stores`
* Paylaşılan → `src/stores`

## Cross-Feature Communication

Bir feature başka bir feature'ın store'una veya hook'una erişemez. İletişim sadece 'Shared' katmanı veya 'Parent Component' üzerinden prop ile yapılır.

## Core Architecture Rules

1. Tüm iş mantığı `features/` içinde
2. Feature’lar birbirini doğrudan import etmez
3. Ortak kod global katmanda tutulur
4. Tekrar eden logic global’e taşınır
5. Feature dışa açılımı sadece `index.ts`
6. API layer UI’dan ayrıdır

## Architecture Type

* Feature-based
* Hybrid (global core + feature modules)
* Scalable for large teams
* Next.js & TanStack Start compatible
* AI-oriented structure
