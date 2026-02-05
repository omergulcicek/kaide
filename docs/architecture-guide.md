# architecture.md

## Tech Stack

- **Server State:** TanStack Query  
- **Client State:** Zustand  
- **Validation:** Zod  
- **UI:** shadcn/ui + TailwindCSS  
- **i18n:**  
  - Next.js → next-intl  
  - TanStack Start → Paraglide JS  
- **API Layer:** Feature-based `features/[feature]/api` yapısı  
- **SSR Server Calls:**  
  - Next.js → Server Actions / Route Handlers  
  - TanStack Start → serverFn  

## Overview

Bu proje **feature-based (domain odaklı)** mimari kullanır.
Yapı hem **Next.js** hem **TanStack Start** ile uyumludur ve AI araçlarının hızlı anlaması için sade tutulmuştur.

**Framework farkı sadece routing katmanındadır:**

* **Next.js:** `src/app/`
* **TanStack Start:** `src/routes/`

Diğer tüm klasör yapısı aynıdır.

## Folder Structure

```text
src/
├── app/ | routes/        # Routing layer (framework'e göre değişir)
│
├── features/             # Tüm business & domain logic
│   └── [feature-name]/
│       ├── api/          # Server/client API fonksiyonları
│       ├── components/   # Feature’a özel UI
│       ├── hooks/        # Feature logic
│       ├── schemas/      # Zod validation
│       ├── stores/       # Feature state
│       ├── types/        # TS tipleri
│       └── index.ts      # Public API
│
├── components/           # Global UI layer
│   ├── ui/               # Atomic (shadcn/ui)
│   ├── shared/           # Functional components
│   └── layout/           # Header, Sidebar, Footer
│
├── hooks/                # Generic reusable hooks
├── lib/                  # SDK, utils, axios, configs
├── providers/            # Global context (Theme, Query, Auth)
├── stores/               # Global state
├── schemas/              # Shared validation models
├── types/                # Global TypeScript types
├── constants/            # Enums & static values
├── config/               # App & SEO config
├── styles/               # Tailwind & base CSS
├── i18n/                 # Localization setup
├── messages/             # Translation files
└── env.ts                # Env validation
```

## Next.js Client / Server Component Kuralı

Next.js App Router kullanılırken:

* Varsayılan yaklaşım: **Server Component**
* Tarayıcı API, event handler veya local state gerekiyorsa → `"use client"`
* Data fetching mümkün olduğunca server tarafında yapılır
* Feature içindeki `components/` klasörü hem server hem client bileşen barındırabilir
* Client’a geçiş mümkün olan en dar noktada yapılır

## API & Server Boundary Kuralı

* API çağrıları doğrudan component içinde yazılmaz
* Tüm istekler `features/[feature]/api` altında tanımlanır
* Component’ler sadece import ederek kullanır

**SSR çalışan sayfalarda:**

* Server tarafı işlemler **serverFn** olarak yazılır
* Supabase, DB, secret erişimi sadece server tarafında yapılır
* Client/Server ayrımı API katmanında çözülür, UI katmanı sadece tüketir

## State Yerleşim Kuralı

* Sadece tek bir feature’ı ilgilendiriyorsa →
  `features/[feature]/stores`

* Birden fazla feature arasında paylaşılıyorsa →
  `src/stores`

## Temel Mimari Kurallar

1. Tüm iş mantığı `features/` içinde yaşar
2. Feature’lar birbirini doğrudan import etmez
3. Ortak/generic kodlar global klasörlerde tutulur
4. Tekrar kullanılan logic global katmana taşınır
5. Her feature dışarıya sadece `index.ts` üzerinden açılır
6. API katmanı UI’dan ayrıdır, doğrudan component içine yazılmaz

## Architecture Type

* **Feature-based**
* **Hybrid (global core + feature modules)**
* **Scalable for large teams**
* **Next.js & TanStack uyumlu**
* **AI-friendly structure**
