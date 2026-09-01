# Security Policy

## Overview

This is an open-source Shopify app that audits store SEO, generates JSON-LD schema, and optimizes for AI search visibility. While it does not handle sensitive customer PII like phone numbers or call recordings, it does interact with Shopify store data, API credentials, and may process product/collection metadata. We take security seriously and appreciate responsible disclosure.

---

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| Latest commit on `main` | ✅ Actively supported |
| Older commits | ❌ No patches |

We recommend always running the latest commit from `main` in production.

---

## Scope

The following components and data surfaces are in scope:

- **Shopify API authentication** — OAuth flow, access token storage, session management
- **Shopify webhook handlers** — `products/update`, `collections/update`, `shop/update` (HMAC verification)
- **SEO audit engine** — product/collection metadata scanning, JSON-LD generation
- **AI-search optimization** — LLM API calls for meta tag generation, content suggestions
- **Database access** — Prisma/PostgreSQL (if used) for audit history, job queues
- **Environment secrets** — `SHOPIFY_API_KEY`, `SHOPIFY_API_SECRET`, `DATABASE_URL`, LLM API keys, webhook secrets
- **App bridge / embedded app** — Shopify App Bridge authentication, iframe security

Out of scope: third-party services (Shopify platform, LLM providers like OpenAI/Anthropic, Vercel/Netlify hosting), infrastructure-level issues outside this codebase.

---

## Reporting a Vulnerability

**Please do not file public GitHub issues for security vulnerabilities.**

Report security issues privately to:

📧 **help.nuraveda@gmail.com**  
Subject line: `[SECURITY] Shopify Agentic SEO — <brief description>`

### What to include

- A clear description of the vulnerability and affected component
- Steps to reproduce or a proof-of-concept (sanitized — no real store data)
- The potential impact (e.g., unauthorized store access, API token leakage, audit data exposure)
- Your suggested severity (Critical / High / Medium / Low)

### What to expect

| Stage | Timeline |
| ----- | -------- |
| Acknowledgement | Within **48 hours** |
| Initial triage & severity assessment | Within **5 business days** |
| Fix or mitigation shipped | Within **14 days** for Critical/High; best-effort for lower severity |
| Disclosure coordination | We will notify you before any public disclosure |

We do not currently offer a bug bounty program, but we will credit researchers in the changelog (with your permission).

---

## Security Design Notes

These are the primary controls in place — useful context when evaluating findings:

- **Shopify OAuth 2.0** — app uses Shopify's official OAuth flow; access tokens are stored encrypted at rest (via environment-level encryption or secrets manager)
- **Webhook HMAC verification** — all incoming Shopify webhooks are verified with `crypto.timingSafeEqual` HMAC comparison using the app's webhook secret
- **Session isolation** — each Shopify store's session is isolated by `shop` domain; no cross-store data leakage
- **Environment-only secrets** — API keys, database URLs, and webhook secrets are loaded from environment variables (`.env`), never committed to git
- **Rate limiting** — Shopify API calls respect rate limits; audit jobs are queued to avoid burst requests
- **No PII storage** — the app does not store customer phone numbers, emails, or addresses — only product/collection metadata and SEO audit results
- **Embedded app security** — if using Shopify App Bridge, iframe is sandboxed and authenticated via Shopify's session token

---

## Sensitive Data Handling

The app processes the following categories of data:

| Data type | Storage location | Retention |
| --------- | ---------------- | --------- |
| Shopify access tokens | Environment variables / secrets manager | Indefinite (until app uninstall) |
| Product/collection metadata | PostgreSQL (if configured) | Operator-managed |
| SEO audit results | PostgreSQL (if configured) | Operator-managed |
| LLM API keys | Environment variables | Indefinite |

Operators are responsible for configuring appropriate access controls on their database and environment secrets.

---

## Dependency & Supply Chain

- Dependencies are managed via `npm` or `pnpm` with a lockfile (`package-lock.json` or `pnpm-lock.yaml`). Audit regularly with `npm audit` or `pnpm audit`.
- Pre-commit hooks (if configured via `.pre-commit-config.yaml`) should include secret-scanning checks. Never commit real API keys or webhook secrets.
- The `.env.example` file must never contain real values — it is a template only.

---

## License

This repository is licensed under the **MIT License**. See [`LICENSE`](LICENSE) for the full text.

Security research on your own lawfully-obtained instance is welcomed; unauthorized access to production systems is not.
