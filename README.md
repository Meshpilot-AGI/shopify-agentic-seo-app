<div align="center">

# 🛍️ Shopify Agentic SEO App

### Open framework for building an agentic SEO app for Shopify — audits, JSON-LD automation, AI-search readiness.

A **self-hostable Shopify app** that audits store SEO, generates JSON-LD schema, and
optimizes for AI search visibility (ChatGPT, Perplexity, Google SGE, Gemini). Built
as an open framework — extend it with your own audit checks, schema types, and
AI-search strategies. Runs headless on your infrastructure or as an embedded
Shopify app.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Node.js 18+](https://img.shields.io/badge/node-18%2B-blue.svg)](https://nodejs.org/)
[![Shopify App](https://img.shields.io/badge/shopify-app-008060.svg)](https://shopify.dev/)
[![CI](https://img.shields.io/github/actions/workflow/status/Meshpilot-AGI/shopify-agentic-seo-app/ci.yml?branch=main&label=CI)](../../actions)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Stars](https://img.shields.io/github/stars/Meshpilot-AGI/shopify-agentic-seo-app?style=social)](../../stargazers)

**[Features](#features)** · **[Architecture](#how-it-works-the-architecture)** · **[Quickstart](#quickstart)** · **[Audit Checks](#audit-checks)** · **[JSON-LD Types](#jsonld-types)** · **[Security](SECURITY.md)** · **[Contributing](CONTRIBUTING.md)**

</div>

---

> **🟢 Open source · MIT.** This app is free and open source — read it, run it,
> extend it. You may use, modify, distribute, and sublicense, including in commercial
> products, with minimal restrictions. See [`LICENSE`](LICENSE).

Most "SEO apps" are static checklists or rigid rule engines. **Shopify Agentic SEO
is an open framework** — modular audit checks, pluggable JSON-LD generators, and
AI-search optimization strategies you can extend. Run it as a standalone service,
embed it in Shopify, or integrate it into your own tooling. The framework handles
the hard parts (Shopify API, OAuth, webhooks, database); you add audit logic and
schema types.

## Contents

- [Features](#features)
- [What it does](#what-it-does)
- [How it works — architecture](#how-it-works-the-architecture)
- [Tech stack](#tech-stack)
- [Audit checks](#audit-checks)
- [JSON-LD types](#jsonld-types)
- [AI-search optimization](#ai-search-optimization)
- [Quickstart](#quickstart)
- [Deploy](#deploy)
- [Shopify integration](#shopify-integration)
- [Configuration](#configuration)
- [Extending the framework](#extending-the-framework)
- [License](#license) · [Security](#security) · [Contributing](#contributing)

---

## Features

- **SEO audits** — automated checks for product pages, collections, blog posts, and
  site-wide SEO (meta tags, headings, alt text, internal linking, structured data)
- **JSON-LD generation** — auto-generate schema markup for products, reviews, FAQs,
  breadcrumbs, organizations, and more
- **AI-search optimization** — optimize meta tags, content, and structured data for
  AI search engines (ChatGPT, Perplexity, Google SGE, Gemini)
- **Shopify-native** — OAuth 2.0, webhooks (`products/update`, `collections/update`),
  embedded app via Shopify App Bridge
- **Extensible framework** — add your own audit checks, JSON-LD types, and AI-search
  strategies as modules
- **Multi-tenant** — serve multiple Shopify stores from one deployment; each store's
  data is isolated
- **Headless or embedded** — run as a standalone API or embed in Shopify Admin

---

## What it does
Shopify store install (OAuth 2.0)
│
▼ (webhook: products/update, collections/update)
SEO audit engine scans store
│
▼ (per-product, per-collection, site-wide)
┌─────────────┬─────────────┐
▼ ▼ ▼
Meta tags JSON-LD AI-search
headings schema optimization
alt text audits suggestions
│ │ │
└─────────────┼─────────────┘
▼
Audit report + fix suggestions
(priority-scored, actionable)
│
▼
Auto-apply fixes (optional)
(meta tags, JSON-LD injection)
│
▼
Persist audit history (database)
Track improvements over time

**Key capabilities:**

1. **Automated audits** — scans products, collections, blog posts, and site structure
   for SEO issues (missing meta tags, broken links, duplicate content, slow pages)
2. **JSON-LD injection** — generates and injects structured data into product pages,
   collection pages, and blog posts
3. **AI-search readiness** — optimizes content for AI search engines (natural
   language meta tags, FAQ schema, brand knowledge base)
4. **Actionable fixes** — priority-scored recommendations with one-click apply
   (where possible)
5. **Audit history** — tracks SEO improvements over time, measures impact of changes

---

## How it works (the architecture)

```mermaid
flowchart TB
    subgraph Shopify["Shopify"]
        OAUTH["OAuth 2.0 install"]
        WEBHOOK["Webhooks\nproducts/update · collections/update"]
    end
    subgraph App["Shopify Agentic SEO App"]
        API["API routes\n/audit · /jsonld · /ai-optimize"]
        AUDIT["Audit engine\nmodular checks"]
        JSONLD["JSON-LD generator\npluggable schema types"]
        AIOPT["AI-search optimizer\nmeta tags · content suggestions"]
    end
    subgraph Data["Data layer"]
        DB[("Database\naudit_results · jsonld_cache · store_config")]
    end
    
    OAUTH --> API
    WEBHOOK --> API
    API --> AUDIT
    API --> JSONLD
    API --> AIOPT
    AUDIT --> DB
    JSONLD --> DB
    AIOPT --> DB
```

### The audit loop

This is the heart — what runs per store, on-demand or scheduled:

```mermaid
flowchart LR
    TRIGGER["Webhook or manual scan"] --> FETCH["Fetch store data\n(Shopify API)"]
    FETCH --> SCAN["Run audit checks\n(modular, parallel)"]
    SCAN --> SCORE["Priority scoring\n(impact × effort)"]
    SCORE --> REPORT["Generate report\n(issues + fixes)"]
    REPORT --> APPLY{"Auto-apply?"}
    APPLY -->|yes| FIX["Apply fixes\n(meta tags · JSON-LD)"]
    APPLY -->|no| STORE["Store in database"]
    FIX --> STORE
    STORE --> TRACK["Track over time\n(audit history)"]
```

Three things make it scalable:

1. **Modular audits** — each check is a self-contained module; add new checks without
   touching the core engine
2. **Parallel execution** — audits run in parallel per product/collection; scales to
   large catalogs
3. **Incremental scans** — webhooks trigger targeted audits (only changed products),
   not full-store rescans

---

## Tech stack

| Layer | Technology | Notes |
|---|---|---|
| Language / runtime | **Node.js ≥ 18** | JavaScript/TypeScript, ES modules |
| Framework | **Next.js** or **Express** | API routes, embedded app UI |
| Shopify integration | **Shopify App Router** (`@shopify/shopify-api`) | OAuth, webhooks, App Bridge |
| Database | **PostgreSQL** or **SQLite** | `audit_results` · `jsonld_cache` · `store_config` |
| Caching | **Redis** (optional) | Cache audit results, rate-limit API calls |
| AI/LLM | **OpenAI** / **Anthropic** (optional) | Meta tag generation, content suggestions |
| Structured data | **JSON-LD** (schema.org) | Product, Review, FAQ, Breadcrumb, Organization |
| Hosting | **Vercel** / **Railway** / **Fly.io** / **Docker** | Serverless or containerized |
| CI/CD | **GitHub Actions** | Lint, test, deploy on push to `main` |
| Observability | **Sentry** · **Logtail** (optional) | Error tracking, structured logging |
| Secrets / auth | Environment variables · **Shopify HMAC** (webhooks) | Per-store tokens encrypted at rest |

---

## Quickstart

### Prerequisites

- Node.js 18+ · npm or pnpm
- A Shopify Partner account (for app development)
- A development store (for testing)
- Database (PostgreSQL, SQLite, or cloud DB like Supabase/Neon)

### Install

```bash
git clone https://github.com/Meshpilot-AGI/shopify-agentic-seo-app.git
cd shopify-agentic-seo-app
npm install  # or pnpm install
cp .env.example .env
# Edit .env (see "Configuration" below)
```

### Run locally

```bash
# Development server
npm run dev

# This starts the app at http://localhost:3000
# Visit /api/health to verify it's running
```

### Install on a development store

1. In your Shopify Partner dashboard, create a new app
2. Set the app URL to `http://localhost:3000` (or your tunnel URL via ngrok)
3. Copy the `SHOPIFY_API_KEY` and `SHOPIFY_API_SECRET` to your `.env`
4. Visit `http://localhost:3000/api/auth` to start OAuth install
5. After install, the app will scan the store and show audit results

---

## Deploy

### Vercel (serverless)

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel --prod
```

Set environment variables in Vercel dashboard:
- `SHOPIFY_API_KEY`
- `SHOPIFY_API_SECRET`
- `DATABASE_URL`
- `NEXT_PUBLIC_APP_URL`

### Railway (containerized)

```bash
# Install Railway CLI
npm i -g @railway/cli

# Deploy
railway up
```

Railway auto-detects Next.js and deploys as a container.

### Docker (self-hosted)

```bash
# Build image
docker build -t shopify-seo-app .

# Run container
docker run -p 3000:3000 --env-file .env shopify-seo-app
```

---

## Shopify integration

### OAuth 2.0 install flow

1. User clicks "Install app" in Shopify Admin
2. App redirects to Shopify OAuth URL with scopes (`read_products`, `write_products`,
   `read_themes`, `write_themes`)
3. Shopify redirects back with `code` param
4. App exchanges `code` for access token
5. Token is encrypted and stored in database
6. App triggers initial audit scan

### Webhook handlers

The app subscribes to these webhooks on install:

| Webhook | Trigger | Action |
|---|---|---|
| `products/update` | Product updated | Re-audit that product |
| `collections/update` | Collection updated | Re-audit that collection |
| `shop/update` | Store settings changed | Re-audit site-wide SEO |

Webhooks are HMAC-verified for security.

### Embedded app (App Bridge)

The app can run embedded in Shopify Admin via App Bridge:

```tsx
// app/embedded/page.tsx
import { AppProvider } from '@shopify/app-bridge-react';

export default function EmbeddedApp() {
  return (
    <AppProvider>
      {/* Your audit UI here */}
    </AppProvider>
  );
}
```

---

## Configuration

### Core environment variables

```bash
# Shopify
SHOPIFY_API_KEY=your_api_key
SHOPIFY_API_SECRET=your_api_secret
SHOPIFY_SCOPES=read_products,write_products,read_themes,write_themes

# Database
DATABASE_URL=postgresql://user:pass@localhost:5432/shopify_seo?schema=public

# App
NEXT_PUBLIC_APP_URL=http://localhost:3000
SESSION_SECRET=your_session_secret_32_chars

# Optional: AI/LLM
OPENAI_API_KEY=sk-...  # for meta tag generation
ANTHROPIC_API_KEY=sk-ant-...  # alternative LLM

# Optional: Redis (caching)
REDIS_URL=redis://localhost:6379
```

### Per-store configuration

Each store has its own config row in the database:
