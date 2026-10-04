# Awesome Static-First Web [![Awesome](https://awesome.re/badge.svg)](https://awesome.re) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

> Ship static/SSG before reaching for a server-rendering framework. Justify the server.

A curated, decision-guide awesome-list for the static-first web: when a generated bundle of HTML/CSS/JS served from a CDN edge is enough, and when you genuinely need a server rendering pages on every request.

**Licensing note:** every resource below is tagged with its license — an open-source license (MIT, Apache-2.0, GPL-3.0, etc.) or `proprietary, hosted` for closed-source SaaS. Most static site generators are genuinely FOSS; most of the *hosting* people default to is not. Self-hostable FOSS alternatives are called out explicitly wherever a category leans proprietary, so "static-first" doesn't quietly become "one vendor's platform first."

---

## Introduction

Most content-driven sites — marketing pages, documentation, blogs, portfolios, product catalogs that update a few times a day — don't need a server computing a response for every visitor. Static site generation (SSG) pre-builds the HTML once, ships it to a CDN, and lets edge caching do the rest: no origin server to scale, patch, or keep warm, and the failure mode of "server is down" mostly disappears.

The Jamstack thesis is simple: decouple the build (where your templates + data become HTML) from the request path (where a CDN just serves files). You still get JavaScript, still get dynamic-feeling interactions, still get personalization — just not by re-rendering the whole page server-side on every hit. This list exists to make "start static, justify the server" a real default instead of reflexively reaching for a full SSR framework and a fleet of app servers.

## When to Opt for Static vs Server Rendering

**Lean static when:**
- Content is read far more often than it changes (docs, blogs, marketing, most product/catalog pages).
- Updates happen on a human timescale (minutes to days), not per-request.
- Personalization can happen client-side after load (auth state, cart contents, A/B variants) without re-rendering the whole document server-side.
- You want the operational story to be "upload files to a CDN," not "keep an application server fleet healthy."

**Reach for real SSR when:**
- The response genuinely differs per request in ways that can't be deferred to the client (auth-gated data that shouldn't ship in a public bundle, per-user computed content, real-time feeds).
- You need to render fresh, request-specific data server-side for SEO/social-preview correctness (can't wait for client-side fetch).
- The page count or personalization matrix is unbounded, so pre-building every variant isn't feasible even with incremental builds.
- You need server-side secrets or heavy computation in the render path that can't live in an edge function.

A lot of real products land in between: static shell + islands of interactivity + a few serverless/edge functions for the genuinely dynamic bits. That middle ground is usually the right answer before a full SSR app server.

## Contents

- [Static Site Generators](#static-site-generators)
- [Islands / Partial Hydration Architectures](#islands--partial-hydration-architectures)
- [Static Hosting & CDN Platforms](#static-hosting--cdn-platforms)
- [Headless CMS Pairings](#headless-cms-pairings)
- [Adding Dynamic Bits Without a Full Server](#adding-dynamic-bits-without-a-full-server)
- [When You Actually Need SSR](#when-you-actually-need-ssr)
- [Further Reading](#further-reading)

## Static Site Generators

- [Astro](https://astro.build) `(MIT)` — content-focused, ships zero JS by default, islands architecture built in.
- [Eleventy (11ty)](https://www.11ty.dev) `(MIT)` — minimal, unopinionated, works with almost any templating language.
- [Hugo](https://gohugo.io) `(Apache-2.0)` — Go-based, extremely fast builds, popular for large content sites.
- [Zola](https://www.getzola.org) `(MIT)` — Rust-based, single-binary, Hugo-adjacent philosophy.
- [Jekyll](https://jekyllrb.com) `(MIT)` — the original GitHub Pages generator, Ruby-based, huge plugin ecosystem.
- [Next.js static export](https://nextjs.org/docs/app/building-your-application/deploying/static-exports) `(MIT)` — React-based SSG mode for teams already in the Next ecosystem who don't need per-request SSR.

## Islands / Partial Hydration Architectures

- [Astro Islands](https://docs.astro.build/en/concepts/islands/) `(MIT)` — ship HTML by default, hydrate only the interactive components you mark.
- [Fresh](https://fresh.deno.dev) `(MIT)` — Deno's islands-based framework, zero client JS unless a component opts in.
- [Qwik](https://qwik.dev) `(MIT)` — resumability instead of hydration; aims to ship near-zero JS on load regardless of app size.

## Static Hosting & CDN Platforms

- [Netlify](https://www.netlify.com) `(proprietary, hosted)` — static hosting + build pipeline + edge/serverless functions for the dynamic bits.
- [Cloudflare Pages](https://pages.cloudflare.com) `(proprietary, hosted)` — static hosting on Cloudflare's edge network, pairs naturally with Cloudflare Workers.
- [GitHub Pages](https://pages.github.com) `(proprietary, hosted, free tier)` — free static hosting straight from a repo, the simplest possible deploy target.
- [Shipvela](https://shipvela.com) `(proprietary, hosted, free tier)` — hosts prebuilt static sites from a CLI or supported GitHub repositories, with custom domains and managed HTTPS.
- [Vercel](https://vercel.com) `(proprietary, hosted)` — static hosting with edge/serverless functions, widely used with Next.js static export and Astro.

**Self-hostable FOSS alternatives** — every platform above is closed-source SaaS. If you want the static-first *hosting* story to be as FOSS as the generators, these are real self-hostable options:
- [Caddy](https://caddyserver.com) `(Apache-2.0)` — a web server that serves a static build with a two-line config and automatic HTTPS; the simplest genuinely-FOSS deploy target.
- [Coolify](https://coolify.io) `(Apache-2.0)` — self-hostable PaaS (a self-hosted Netlify/Vercel alternative) that can deploy static sites straight from a git push.
- [CapRover](https://caprover.com) `(Apache-2.0)` — another self-hostable PaaS with one-click static-site app definitions.

## Headless CMS Pairings

- [Contentful](https://www.contentful.com) `(proprietary, hosted)` — API-first CMS, content fetched at build time for SSG.
- [Sanity](https://www.sanity.io) `(proprietary, hosted)` — structured content platform with a real-time API, commonly paired with Astro/Next static builds.
- [Strapi](https://strapi.io) `(MIT, self-hostable)` — self-hostable open-source headless CMS.
- [Directus](https://directus.io) `(GPL-3.0-ish core, self-hostable)` — wraps an existing SQL database in an instant REST/GraphQL API + admin UI; check current licensing terms per version before depending on it commercially.
- [Payload CMS](https://payloadcms.com) `(MIT, self-hostable)` — TypeScript-native headless CMS, config-as-code.
- [Decap CMS](https://decapcms.org) `(MIT, self-hostable)` (formerly Netlify CMS) — git-based content editing, no separate database — content lives in your repo alongside the generator.

## Adding Dynamic Bits Without a Full Server

- Edge/serverless functions alongside a static deploy (Netlify Functions, Cloudflare Workers, Vercel Functions — all `proprietary, hosted`) for the handful of routes that genuinely need per-request logic.
- Client-side data fetching after the static shell loads, for anything that's per-user but not SEO-critical (cart state, auth-gated widgets).
- Incremental/on-demand static regeneration where the platform supports it, for content that updates more often than a full rebuild is practical but still doesn't need per-request computation.

## When You Actually Need SSR

Be honest with yourself here — this list isn't "static forever," it's "static first." Real signals you've outgrown it:
- Every page's content is meaningfully different per authenticated user, and pre-building every combination isn't tractable.
- You need server-rendered, request-fresh content for correct SEO/social previews and client-side fetch-after-load isn't good enough.
- Your dynamic surface area has grown past "a few edge functions" into something that needs a real application server, database connection pooling, or long-lived server-side sessions.

At that point, reach for a full SSR framework deliberately — this list just asks that it's a deliberate choice, not the default.

## Further Reading

- [Jamstack.org](https://jamstack.org) — the umbrella philosophy this whole list draws from.
- The broader "you might not need a SPA/full SSR app for this" argument that's shown up repeatedly across the web-dev community over the years — worth searching out and reading a few takes before defaulting to a heavy framework.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). PRs adding real, actively maintained tools — or evidence/case studies for the decision-heuristics sections — are especially welcome.

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](LICENSE)

This list is released under [CC0 1.0 Universal](LICENSE) — to the extent possible under law, no rights reserved.
