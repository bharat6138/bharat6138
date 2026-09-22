# AURA Agent V1 — Security and Technology Dashboard

## Overview

AURA (Application Unified Runtime Analyzer) is a local, browser-powered application inspection tool. It opens a public HTTP or HTTPS target in bundled Chromium, follows same-origin links, and turns browser-visible evidence into a readable dashboard.

The V1 dashboard covers three areas:

1. Application health and quality findings.
2. Browser-visible security posture.
3. Technology, package, CDN, and asset inventory.

All inspection runs locally. AURA does not send scan results to an AURA cloud service.

## Dashboard output

### Health inspection

- HTTP response status and initial DOM load time.
- Missing titles, meta descriptions, and primary headings.
- Images missing `alt` attributes.
- Browser console errors.
- Same-origin crawling for up to 15 pages.
- A health score and prioritized recommendations.

### Security posture

- HTTP versus HTTPS transport.
- TLS protocol, certificate issuer, subject, and expiry when exposed by Chromium.
- Content Security Policy.
- HTTP Strict Transport Security.
- `X-Content-Type-Options`.
- `Referrer-Policy`.
- `Permissions-Policy`.
- Cross-Origin Opener and Resource policies.
- Clickjacking protection through `frame-ancestors` or `X-Frame-Options`.
- Mixed HTTP content on HTTPS pages.
- HTTP form targets and password fields on non-HTTPS pages.
- Cookie `Secure`, `HttpOnly`, and `SameSite` signals.
- Third-party scripts without Subresource Integrity.
- `Server` and `X-Powered-By` disclosure signals.
- A separate security score, grade, checks, evidence, and recommendations.

### Technology and asset inventory

- Framework and platform signals such as Next.js, React, Nuxt, Vue, Angular, Svelte, Astro, WordPress, Shopify, and jQuery.
- Public package names and versions exposed through npm CDN URLs or runtime markers.
- Version parsing for jsDelivr, UNPKG, esm.sh, Skypack, cdnjs, and Google Hosted Libraries.
- Recognized CDN providers, hosts, and resource counts.
- First-party and third-party external scripts.
- Script type, `async`, `defer`, and SRI status.
- Inline-script count.
- Stylesheet URLs.
- External resource hosts.
- Page-by-page script, stylesheet, and host totals.

### Report download

- **Download report** creates a self-contained HTML report with health scores, security checks, recommendations, technology inventory, package/version evidence, CDN providers, scripts, and page details.
- The HTML report works offline and can be converted to PDF with the browser's **Print → Save as PDF** option.
- **Download JSON** saves the complete machine-readable scan response for integrations or further analysis.

## Important detection limits

A browser can only report evidence delivered to the client. AURA cannot reliably enumerate dependencies that are bundled into anonymous chunks, renamed or minified, used only on the server, hidden behind authentication, or not loaded during the inspected journey.

An exposed package version is an inventory clue, not proof of a vulnerability. The security dashboard is a lightweight posture review, not a penetration test, source-code audit, CVE scanner, complete OWASP assessment, or compliance certification.

## Requirements

- Node.js 20 or newer.
- npm.
- Internet access during the first package and Chromium installation.

## Install

```bash
npm install
node scripts/install-browsers.mjs
```

The browser installer stores Chromium under `node_modules/playwright-core/.local-browsers` so it can also be included in packaged Electron builds. AURA automatically selects this path in local development.

## Run

Desktop development:

```bash
npm run dev
```

Web dashboard only:

```bash
npm run dev:web
```

Then open `http://127.0.0.1:3000`.

## Verify

```bash
npm test
npx tsc --noEmit
```

For a production build on a machine with limited Node.js heap, use:

```bash
NODE_OPTIONS=--max-old-space-size=4096 npm run build
```

Do not run `next dev` and `next build` at the same time because both write to `.next`.

## Windows package

Run on Windows for the most predictable Electron packaging result:

```powershell
npm install
npm run package:win
```

The artifacts are created in `dist`. The target PC does not need Node.js or npm.

## Main project files

- `app/page.tsx` — scan form and detailed dashboard.
- `app/styles.css` — dashboard presentation and responsive layout.
- `app/api/scan/route.ts` — scan API endpoint.
- `lib/scanner.ts` — crawler, security inspection, fingerprinting, and scoring.
- `lib/types.ts` — report data model.
- `electron/main.cjs` — Electron desktop bootstrap.
- `scripts/install-browsers.mjs` — local Chromium installation.
- `scripts/copy-standalone.mjs` — production UI asset preparation.

## Responsible use

Scan only applications you own or are authorized to test. AURA creates real browser traffic and may follow up to 15 same-origin pages. Avoid sensitive URLs and authenticated production workflows in V1.
