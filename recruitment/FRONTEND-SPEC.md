# Front End Developer

## Brief

We're hiring a front end developer to build and maintain **Affirm**, a modern web app running on Cloudflare's edge platform. You'll work across the UI — from page layouts to reusable components — in a small, close-knit team where the front end and backend sit side by side.

### What you'll do

Build responsive, accessible UI features in Vue and Nuxt, turn designs into clean themed interfaces, wire the front end up to our data layer, and ship through an automated Cloudflare pipeline. You'll write tests, keep the codebase tidy and type-safe, and take features from idea to production.

### What we're looking for

- Strong **Vue 3** (Composition API), or equivalent React/Svelte experience and a willingness to work in Vue
- Confident **JavaScript** and **TypeScript**
- Fluent in modern **CSS** and a utility-first framework like **Tailwind**
- Experience with an SSR/SSG framework (**Nuxt**, Next.js or SvelteKit)
- Comfortable with **Git**, pull requests and code review
- Writes and maintains **tests** as part of shipping

### Nice to have

Cloudflare Workers or other edge/serverless experience, type-safe ORMs (we use **Drizzle**) and basic SQL, **Bun**, and a good eye for accessibility and performance. Happy to teach these on the job.

### Our stack

Nuxt 4 · Vue 3 · TypeScript · Tailwind CSS v4 · DaisyUI · Cloudflare Workers · D1 (SQLite) · Drizzle ORM · Bun · Vitest · Playwright

### Level

Open to **mid-level through senior**. We'll match the role and responsibilities to your experience.

## Detail

We're looking for a front end developer to work on **Affirm**, a Nuxt 4 application running on Cloudflare Workers. This document lists the skills and experience we need so candidates and recruiters know what the role actually involves. It's written around the stack we use today, so the right person can be productive without a long ramp-up.

### The stack you'll work in

The application is built with **Nuxt 4** and **Vue 3** (Composition API, `<script setup>`), styled with **Tailwind CSS v4** and **DaisyUI**, and deployed to **Cloudflare Workers**. Data lives in **Cloudflare D1** (SQLite) accessed through **Drizzle ORM**. We use **Bun** as the package manager and runtime, **TypeScript** throughout, **Vitest** and **Playwright** for testing, and **Trunk** plus **ESLint** and **Prettier** for linting and formatting.

You don't need deep experience in every one of these, but you should be strong in the core (Vue, modern CSS, TypeScript) and comfortable picking up the rest quickly.

### Essential skills

You should have solid, demonstrable experience with the following.

**Vue 3 and the Composition API.** You can build components with `<script setup>`, manage reactive state with `ref`/`reactive`/`computed`, write reusable composables, and reason about component lifecycle and props/emits. Equivalent strong React or Svelte experience plus a clear willingness to work in Vue can also work for the right candidate.

**Modern JavaScript and TypeScript.** You write typed code by default, understand generics and type inference well enough to keep the codebase type-safe, and are comfortable with ES modules and modern async patterns. The project runs `tsc --noEmit` in CI, so type correctness is part of the job, not an afterthought.

**CSS with a utility-first framework.** You're fluent in Tailwind (v4 preferred) or can transfer quickly from another utility-first framework, and you understand responsive design, layout (flexbox/grid), and theming. We use DaisyUI components and a Catppuccin theme, so an eye for consistent, accessible UI matters.

**A meta-framework for SSR/SSG.** Hands-on experience with Nuxt is ideal; equivalent experience with Next.js or SvelteKit is a strong substitute. You should understand file-based routing, the difference between server and client rendering, data fetching patterns, and how layouts and pages fit together.

**Git and collaborative workflow.** You work confidently with branches and pull requests, write clear commits, and respond to code review constructively.

**Testing.** You write and maintain unit and component tests (we use Vitest) and understand the value of end-to-end coverage (we use Playwright). You treat tests as part of delivering a feature.

### Desirable / nice to have

These would let you hit the ground running, but we're happy to help you learn them on the job.

Experience with **Cloudflare Workers** and the edge/serverless model, including the constraints it places on front end code. Familiarity with **Drizzle ORM** or other type-safe query builders, and comfort reading and writing simple **SQL** against SQLite/**D1**. Experience with **Bun** as a package manager and test runner. Exposure to **Wrangler** for local development and deployment. Familiarity with linting/formatting toolchains such as **Trunk**, ESLint and Prettier, and with keeping CI green. An eye for **accessibility** (WCAG basics, semantic HTML, keyboard navigation) and **performance** (bundle size, Core Web Vitals, edge caching).

### What you'll be doing

Building and maintaining UI features in Vue and Nuxt, from page layouts down to reusable components and composables. Translating designs into responsive, accessible, themed interfaces with Tailwind and DaisyUI. Wiring the front end to server routes and the D1 database via Drizzle. Writing tests, keeping types and linting clean, and shipping through our Cloudflare Workers pipeline. Collaborating on code review and helping keep the codebase consistent and maintainable.

### Ways of working

We value people who write clear, maintainable code, communicate well in writing, and take ownership of features end to end. You should be comfortable in a small team where the front end and the edge backend sit close together, and happy to learn the parts of the stack you haven't used before.

### Skill Level

This is flexible. We're open to mid-level through senior candidates. A mid-level developer should be strong in Vue/TypeScript/CSS and able to learn the Cloudflare and Drizzle pieces on the job. A senior candidate would additionally bring architectural judgement, edge/serverless experience, and the ability to set front end direction and mentor others.
