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

## Framework Boundaries (Next.js vs TanStack Start)

Bu proje framework-agnostic değildir. Next.js App Router ve TanStack Start farklı rendering ve data-fetching semantiklerine sahiptir.
Ortaklaştırma sadece aşağıdaki katmanlarda yapılır:

* Domain + validation: `shared/schemas` (Zod), saf TS utils (framework import yok)
* API contracts: request/response shape, error mapping, typed clients
* UI primitives: shadcn/ui + design tokens (routing/rendering bağımlılığı yok)

Framework-spesifik kabul edilen ve ortak wrapper YAZILMAYACAK alanlar:

* Rendering modeli:
  * Next: RSC, Server Components, streaming, segment config
  * Start: client/server component ayrımı farklı lifecycle ile çalışır
* Data fetching + caching:
  * Next: server prefetch, RSC cache, revalidation davranışı
  * Start: loader/serverFn lifecycle, hydration modeli
* Mutations:
  * Next: Server Actions
  * Start: serverFn
* Routing/layout:
  * Next: App Router layouts/segments
  * Start: file-based routing + loaders

Kural: Bu başlıktaki alanlar Shared katmana taşınmaz ve tek bir abstraction altında birleştirilmez.
Framework davranışı ilgili feature içinde, native pattern ile uygulanır.

## Rule Index (Canonical Refs)

Bu doküman sadece “mimari harita”dır. Uygulanabilir kurallar aşağıdaki MDC dosyalarındadır.
Her kural ifadesi **Ref:** ile buraya bağlanmak zorundadır. Ref olmayan kural = geçersiz.

* Ref: `.cursor/rules/core-principles.mdc`
* Ref: `.cursor/rules/frontend/api.mdc`
* Ref: `.cursor/rules/frontend/forms.mdc`
* Ref: `.cursor/rules/frontend/i18n.mdc`
* Ref: `.cursor/rules/frontend/react-best-practices.mdc`
* Ref: `.cursor/rules/frontend/state-management.mdc`
* Ref: `.cursor/rules/frontend/tanstack-query.mdc`
* Ref: `.cursor/rules/frontend/typescript.mdc`
* Ref: `.cursor/rules/frontend/ui-components.mdc`

Ref formatı: `Ref: <path>` (tekil). Birden fazla kural varsa her biri ayrı Ref satırıyla verilir.

## TanStack Query default policy (SSOT)

Varsayılan değerler tek kaynak; override noktası aşağıda.

| Policy   | Liste | Detail | Not |
|----------|-------|--------|-----|
| staleTime | ~60s | ~5m | İhtiyaca göre feature hook içinde override edilebilir. |
| gcTime    | ~10m | ~10m | |
| Retry/delay | Merkezi | — | 4xx retry yok. |

**Override:** `src/providers/query-provider.tsx` (QueryClient defaultOptions). Feature düzeyi override: ilgili custom hook içinde `useQuery`/`useMutation` seçenekleri.

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

## API & Server Boundary (High-Level Contract)

Bu bölüm sadece sorumluluk ve boundary tanımıdır. Implementasyon kuralları Ref: `.cursor/rules/frontend/api.mdc`.

* **Component’te HTTP yok:** Bileşen içinde HTTP çağrısı yazılmaz; UI yalnızca typed API fonksiyonlarını tüketir.
* **Server-only işlemler:** Veritabanı, secret, headers/cookies erişimi yalnızca server runtime’da yapılır (Server Actions, Route Handlers, serverFn). Bu işlemler client’a taşınmaz.
* **API layer framework’ü soyutlamaz:** API katmanı framework’ün data-fetching/mutation davranışını gizlemez; her framework kendi native pattern’i ile kullanır.
* **Client/Server ayrımı:** Hangi kodun client’ta, hangisinin server’da çalışacağı framework runtime tarafından belirlenir; API layer bu ayrımı yapmaz.

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
