# Front End Developer

> [!IMPORTANT]
> If this file is up, with the contact details at the end, we're still looking. I'll modify it to remove the contact info as soon as the place gets filled.

## Brief

We're hiring a front end developer to build and maintain **Affirm**, a modern web app running on Cloudflare's edge platform. You'll work across the UI - from page layouts to reusable components - in a small, close-knit team where the front end and backend sit side by side.

### Who we are

Trans Equity Coalition is a 501(c)(3) nonprofit New Jersey state-wide organization led exclusively by transgender, nonbinary and gender nonconforming people. You don't need to be New Jersey based (or, indeed, even US-based - one of our colleagues works out of the UK) to apply.

### What you'll do

Build responsive, accessible UI features in Vue and Nuxt, turn designs into clean themed interfaces, wire the front end up to our data layer, and ship through an automated Cloudflare pipeline. You'll write tests, keep the codebase tidy and type-safe, and take features from idea to production.

The application is built with **Nuxt 4** and **Vue 3** (Composition API, `<script setup>`), styled with **Tailwind CSS v4** and **DaisyUI**, and deployed to **Cloudflare Workers**. Data lives in **Cloudflare D1** (SQLite) accessed through **Drizzle ORM**. We use **Bun** as the package manager and runtime, **TypeScript** throughout, **Vitest** and **Playwright** for testing, and **Trunk** plus **ESLint** and **Prettier** for linting and formatting.

You don't need deep experience in every one of these, but you should be strong in the core (Vue, modern CSS, TypeScript) and comfortable picking up the rest quickly.

### What we're looking for

- **Vue 3 and the Composition API.** You can build components with `<script setup>`, manage reactive state with `ref`/`reactive`/`computed`, write reusable composables, and reason about component lifecycle and props/emits. Equivalent strong React or Svelte experience plus a clear willingness to work in Vue can also work for the right candidate.
- **Modern JavaScript and TypeScript.** You write typed code by default, understand generics and type inference well enough to keep the codebase type-safe, and are comfortable with ES modules and modern async patterns. The project runs `tsc --noEmit` in CI, so type correctness is part of the job, not an afterthought.
- **CSS with a utility-first framework.** You're fluent in Tailwind (v4 preferred) or can transfer quickly from another utility-first framework, and you understand responsive design, layout (flexbox/grid), and theming. We use DaisyUI components and a Catppuccin theme, so an eye for consistent, accessible UI matters.
- **A meta-framework for SSR/SSG.** Hands-on experience with Nuxt is ideal; equivalent experience with Next.js or SvelteKit is a strong substitute. You should understand file-based routing, the difference between server and client rendering, data fetching patterns, and how layouts and pages fit together.
- **Git and collaborative workflow.** You work confidently with branches and pull requests, write clear commits, and respond to code review constructively.
- **Testing.** You write and maintain unit and component tests (we use Vitest) and understand the value of end-to-end coverage (we use Playwright). You treat tests as part of delivering a feature.

### Nice to have

Experience with **Cloudflare Workers** and the edge/serverless model, including the constraints it places on front end code. Familiarity with **Drizzle ORM** or other type-safe query builders, and comfort reading and writing simple **SQL** against SQLite/**D1**. Experience with **Bun** as a package manager and test runner. Exposure to **Wrangler** for local development and deployment. Familiarity with linting/formatting toolchains such as **Trunk**, ESLint and Prettier, and with keeping CI green. An eye for **accessibility** (WCAG basics, semantic HTML, keyboard navigation) and **performance** (bundle size, Core Web Vitals, edge caching).

### Ways of working

We value people who write clear, maintainable code, communicate well in writing, and take ownership of features end to end. You should be comfortable in a small team where the front end and the edge backend sit close together, and happy to learn the parts of the stack you haven't used before.

### Our stack

Nuxt 4 · Vue 3 · TypeScript · Tailwind CSS v4 · DaisyUI · Cloudflare Workers · D1 (SQLite) · Drizzle ORM · Bun · Vitest · Playwright

### Level

This is flexible. We're open to mid-level through senior candidates. A mid-level developer should be strong in Vue/TypeScript/CSS and able to learn the Cloudflare and Drizzle pieces on the job. A senior candidate would additionally bring architectural judgement, edge/serverless experience, and the ability to set front end direction and mentor others.

In an ideal world, you'll own the frontend and make architectural decisions in collaboration with backend and devops.

### Queerness and neurodiversity friendly

This isn't a position where you're a grey-faced converter of caffeine to code. We're all a bit of a mess, and we're expecting you to be a bit of a mess too. We value **total honesty** over **total compliance**.

We're here to make a thing and have a good time doing it. We're not here to work you to death through unadjustable expectations. You need to be good at what you do, but we're here to be told how we support you, not to tell you how to support us.

### Remuneration

This is a paid position, but we're not going to be able to match business numbers. Get involved with us because you want to make a difference - _that_ we can promise - with the payment just a bonus, rather than making this your day job.

### Applying

Sound good? Fantastic. Drop an email to [contact@transequitycoalition.org](mailto:contact@transequitycoalition.org), mention that this is about the frontend position for Affirm, and we can take it from there.
