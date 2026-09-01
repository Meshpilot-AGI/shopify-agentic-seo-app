# Contributing to Shopify Agentic SEO App

## Quick Overview

**This is free software: you can redistribute it and/or modify it under the terms of the MIT License.** We welcome contributions! This document explains how to contribute effectively while keeping the codebase secure, maintainable, and aligned with the project's goals.

---

## License Notice

This repository is licensed under the **MIT License**. See [`LICENSE`](LICENSE) for the full text.

Key implications:

- **You may use, modify, distribute, and sublicense** — including in commercial products — with minimal restrictions
- **Attribution required** — retain the original copyright notice and license text in copies
- **No copyleft** — derivative works may be proprietary or open source, at your choice
- **No warranty** — software is provided "as is" without warranty of any kind

For questions about licensing or commercial use, contact: **help.nuraveda@gmail.com**

---

## How to Contribute

### 1. Fork and Clone

```bash
git clone https://github.com/your-username/shopify-agentic-seo-app.git
cd shopify-agentic-seo-app
git remote add upstream https://github.com/Meshpilot-AGI/shopify-agentic-seo-app.git
```

### 2. Create a Branch

```bash
git checkout -b feature/your-feature-name
```

Use descriptive branch names:
- `feature/` for new features (e.g., new SEO audit checks, JSON-LD types)
- `fix/` for bug fixes
- `docs/` for documentation improvements
- `refactor/` for code improvements without behavior changes

### 3. Make Your Changes

Ensure your changes:
- Are focused and atomic (one logical change per PR)
- Include tests if applicable (see `test/` or `__tests__/` directory)
- Update documentation if behavior changes (README, docs/)
- Do not break existing functionality

### 4. Test Locally

```bash
# Install dependencies
npm install  # or pnpm install

# Run linter (if configured)
npm run lint

# Run tests
npm test

# Run type checking (if using TypeScript)
npm run typecheck

# Test the app in development
npm run dev
```

### 5. Commit with Clear Messages

```bash
git add .
git commit -m "feat: add JSON-LD schema for product reviews

- Implements Review schema generation in src/jsonld/review.ts
- Adds unit tests for review schema output
- Updates README with JSON-LD feature list

Closes #42"
```

Follow [Conventional Commits](https://www.conventionalcommits.org/) where possible:
- `feat:` new features
- `fix:` bug fixes
- `docs:` documentation
- `style:` formatting, missing semi-colons, etc.
- `refactor:` code changes without behavior changes
- `test:` adding or updating tests
- `chore:` maintenance tasks, dependencies, config

### 6. Push and Open a Pull Request

```bash
git push origin feature/your-feature-name
```

Then open a PR on GitHub against `Meshpilot-AGI/shopify-agentic-seo-app:main`.

In your PR description, include:
- **What** this PR changes and why
- **How** to test it (steps, screenshots, expected behavior)
- **Related issues** (e.g., `Closes #42`)
- **Checklist**:
  - [ ] Tests added/updated
  - [ ] Documentation updated
  - [ ] No breaking changes (or clearly documented if breaking)

---

## What We're Looking For

### ✅ High-Value Contributions

- **New SEO audit checks** — additional Lighthouse-style audits for Shopify stores
- **JSON-LD schema types** — new schema types (FAQ, HowTo, Breadcrumb, Organization, etc.)
- **AI-search optimization** — improvements to meta tag generation, content suggestions for AI search engines (Perplexity, ChatGPT, Google SGE)
- **Bug fixes** — especially for Shopify API integration, OAuth flow, webhook handling
- **Performance improvements** — faster audits, reduced API calls, better caching
- **Documentation** — clearer README, setup guides, troubleshooting, SEO best practices
- **Tests** — unit tests, integration tests, E2E tests with Shopify dev stores
- **DevEx improvements** — better scripts, tooling, local development workflows
- **Security hardening** — secret scanning, input validation, safer defaults

### ⚠️ Contributions That Need Discussion First

- **Breaking changes** — API changes, env var renames, config schema modifications
- **Major refactors** — architecture changes, framework swaps (e.g., Next.js → Remix)
- **New dependencies** — especially heavy or controversial packages
- **Shopify API scope changes** — adding new OAuth scopes may affect app review

Open an Issue or Discussion before starting work on these.

### ❌ Unlikely to Be Accepted

- Cosmetic changes without functional improvements (e.g., renaming variables, reformatting)
- Features that duplicate existing functionality without clear benefit
- Vendor lock-in (e.g., hardcoding a specific LLM provider without abstraction)
- Changes that break Shopify App Store compliance (if planning to submit)

---

## Reporting Bugs

Before opening an issue:

1. Search existing Issues to confirm it hasn't been reported
2. Verify you're on the latest commit of `main`
3. Reproduce the issue in a clean environment (no production secrets)

When filing, include:

- **Environment**: Node.js version, OS, Shopify API version, relevant env vars (redacted)
- **Steps to reproduce**: Minimal, reproducible commands or screenshots
- **Expected vs actual behavior**
- **Logs or error traces** (sanitized — no API keys or store URLs)

Label your issue appropriately (e.g., `bug`, `enhancement`, `question`). Maintainers will respond and triage.

---

## Security Reports

**Do not file public Issues for security vulnerabilities.** See [`SECURITY.md`](SECURITY.md) for the private reporting process, expected timelines, and scope.

---

## Code Style & Conventions

- **Language**: JavaScript/TypeScript (Node.js 18+, Next.js 14+ if applicable)
- **Formatting**: Prettier config (if present), 2-space indentation, semicolons as per project config
- **Imports**: ES modules (`import`/`export`) preferred
- **Env vars**: Follow the naming in `.env.example`; never hardcode secrets
- **Tests**: Use Jest, Vitest, or Playwright; place tests in `test/` or `__tests__/` directory
- **TypeScript**: Strict mode enabled; no `any` types without justification

Run `npm run lint` and `npm run typecheck` (if configured) before committing to catch style issues.

---

## Architecture Notes

This app is an **open framework for building an agentic SEO app for Shopify** — it audits stores, generates JSON-LD schema, and prepares for AI-search visibility. Key components:
src/
├── app/ Next.js app router (if using Next.js)
├── lib/ Shared utilities (Shopify API client, JSON-LD generators)
├── routes/ API routes (webhooks, OAuth, audit endpoints)
├── components/ React components (if embedded app)
└── jsonld/ JSON-LD schema generators (Product, Review, FAQ, etc.)

test/ Unit and integration tests
docs/ Documentation (setup, SEO guides, API reference)

Key design principles:

- **Shopify-native** — uses official Shopify API, OAuth, webhooks, and App Bridge
- **Modular audits** — each SEO check is a self-contained module, easy to extend
- **JSON-LD first** — structured data is generated as pure JSON, rendered into `<script>` tags
- **AI-search ready** — meta tags, content suggestions optimized for AI search engines
- **Multi-tenant** — each Shopify store's data is isolated by `shop` domain

See the [README](README.md) and `docs/` for full details.

---

## Questions?

For general questions or discussion, open a GitHub Issue labeled `question` or start a Discussion.

For licensing or commercial inquiries:

📧 **help.nuraveda@gmail.com**

---

## Acknowledgements

This project is part of **Mesh Pilot** — the AI marketing-operations platform by **Nuraveda Labs**.

Built for Shopify merchants who want to automate SEO audits, generate structured data, and optimize for AI search visibility.

**This is free software: you can redistribute it and/or modify it under the terms of the MIT License.**
