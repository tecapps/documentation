# Frontend Onboarding

Hello, frontend person. Welcome to **Affirm**. You have been chosen. I'm `syn` and you're about to learn a thing.

> "I'm not a frontend developer."
> _-- syn, `README.md`_

I do ops. I do not do frontend. So if you've been wondering why a project that runs on the edge, hits a SQLite database in twenty-something data centres, and lints itself with three different tools also looks like someone glued a potato to Vue and pushed, then congratulations. That's now your job.

We are very pleased to have you. Please don't leave.

---

## The thirty-second tl;dr

- Nuxt 4 + Vue 3 (Composition API, `<script setup lang="ts">`)
- Tailwind CSS v4 + DaisyUI v5 + Catppuccin Mocha
- Cloudflare Workers + D1 (SQLite) + Drizzle ORM
- Bun for everything (install, run, scripts)
- Trunk + ESLint + Prettier for the lint/format mess
- Vitest (unit) + Playwright (e2e)
- No React. We do not speak of React.

If any of that scares you: it shouldn't. If all of it scares you: same, honestly, but it works.

---

## The stack, with feelings

### Nuxt 4

This is Nuxt 4. Not Nuxt 3. Most of the differences are subtle but the most visible one is that the app source lives in `app/` instead of the project root. If you find yourself looking at a Nuxt 3 Stack Overflow answer that contradicts what you're seeing, that's why. Trust the file tree over the internet.

Nuxt 4 is what Next.js wishes it was when it grows up. Don't tell the Vercel people I said that.

### Vue 3 with `<script setup>`

We use the Composition API with `<script setup lang="ts">` exclusively. The Options API exists, philosophically. We just don't use it. If a PR shows up with `data()` and `methods:`, we gently suggest the author retire to a quiet room and reflect on their choices.

### Tailwind v4 + DaisyUI v5

> Want to change any of this? Go ahead. This is frontend dev territory. If you need any help yanking stuff out and replacing it with your preferred options, syn is here.

- **Tailwind** gives you utility classes (`flex`, `p-6`, `text-3xl`, etc.).
- **DaisyUI** gives you semantic component classes (`btn`, `card`, `navbar`, `hero`, `badge`).
- Both can be mixed freely. DaisyUI is built on top of Tailwind; they're friends.

There is **no `tailwind.config.js`**. Tailwind v4 moved the configuration into CSS. The entire stylesheet entry point is five lines:

```css
@import "tailwindcss";
@plugin "daisyui" {
  themes: false;
}
@plugin "./catppuccin-mocha.ts";
```

That `themes: false` is deliberate - DaisyUI's default themes are turned off so the Catppuccin Mocha plugin can be the only theme in town.

#### Why DaisyUI and not NuxtUI?

DaisyUI doesn't have a "pro" version. The open-source one is the whole thing. That's a rarity in UI kits.

Shadcn Vue is a compelling option, but involves extra tooling and other wonk whereas DaisyUI is just plain CSS.

### Cloudflare Workers + D1

Your Vue code runs on Cloudflare's edge. Yes, your Vue code runs on the edge. Yes, it's wild. No, don't think about cold starts. Workers don't really have those. Blink twice if you're impressed.

The database is **D1** - SQLite that Cloudflare runs for us and pretends it's a networked database. You'll mostly interact with it via the API in `server/`, but if you ever need to know more, the schema lives in `server/database/schema.ts` and is managed with Drizzle.

### Bun

If you've been a Node person all your life, Bun is what Node would be if it stopped being haunted. Use `bun` and `bun run` where you'd normally type `npm` or `pnpm`. Install dependencies with `bun install`. Don't fight it.

Also, `bun update` has saved me an inordinate amount of time lately.

---

## Get the machine going

You will need:

1. **[`mise`](https://github.com/jdx/mise)** for tool management. The repo has a `mise.toml` that pins the right versions of Bun, Node, etc.
2. **[`bun`](https://bun.sh)** - `mise` will install it.
3. **A Cloudflare account** isn't required for local dev, just for deploying. Probably worth having anyway, gives you access to logs and observability. syn can send an invite to your email address.

Then:

```bash
# trust mise so it'll actually read the config
mise trust

# install the tools mise.toml asks for
mise install

# install npm-ish dependencies
bun install

# start the dev server on http://localhost:3000
bun run dev # or just bun dev, I'm not your dad
```

The dev server runs Nuxt with proxied Cloudflare bindings via `wrangler.dev.jsonc`, so D1 and friends Just Work locally. If you see a request fail because `DB` isn't bound, you probably ran a bare `nuxt dev` instead of `bun run dev`. Don't do that.

> **Pro tip:** Nuxt Devtools are enabled (the floating Nuxt logo at the bottom of the page). It's actually useful. Don't dismiss it for three months only to discover it has a route inspector, like SOME PEOPLE. ps: me.

---

## The directory map

```plaintext
app/                     ← all your frontend code lives here
├── app.vue              ← root component (basically just renders NuxtLayout > NuxtPage)
├── layouts/             ← page wrappers (default.vue gives you the header + footer + main)
├── pages/               ← file-based routing. each .vue file is a route.
├── components/          ← auto-imported. no manual imports needed.
├── composables/         ← auto-imported reactive logic (useAppInfo, etc.)
├── middleware/          ← route middleware. `.global.ts` runs on every nav.
├── plugins/             ← runs once at app startup, both server and client.
└── assets/css/          ← the five-line tailwind.css entry point

server/                  ← Nitro backend (you'll touch this less)
├── api/                 ← API endpoints. /server/api/hello.get.ts → GET /api/hello
├── database/            ← Drizzle schema + auto-generated migrations
└── utils/               ← server-only helpers (useDB)

shared/                  ← code that both client and server import
└── utils/               ← format.ts and friends

public/                  ← static files served verbatim (favicon, robots.txt)

wrangler.dev.jsonc       ← local dev Cloudflare config
wrangler.staging.jsonc   ← staging worker (affirm-staging) config
wrangler.jsonc           ← production worker (affirm) config
nuxt.config.ts           ← Nuxt config + per-env D1 bindings
```

The `~` alias points at the project root, so `~/server/utils/db` works from anywhere. Use it. Relative `../../../` paths are a war crime.

---

## How a page becomes a page

You create a file. That's it. That's the whole feature.

- `app/pages/index.vue` → `/`
- `app/pages/about.vue` → `/about`
- `app/pages/blog/[slug].vue` → `/blog/:slug` (dynamic segment)
- `app/pages/blog/index.vue` → `/blog`

No router config. No `router.ts`. No exporting of route objects. Nuxt reads the filesystem at build time and generates the route table itself.

Use `<NuxtLink to="/about">` for internal links - it does client-side navigation. Use `<a href="...">` only for external links or you'll get sad full-page reloads and your colleagues will judge you silently. I'm also pretty sure you can use NuxtLink for external links? A question for [the
docs](https://nuxt.com/docs/4.x/api/components/nuxt-link) I guess.

---

## Auto-imports: a tale of "where did `ref` come from?"

Nuxt auto-imports a bunch of things so you can use them without `import` statements:

- **Vue reactivity:** `ref`, `reactive`, `computed`, `watch`, `watchEffect`
- **Nuxt composables:** `useFetch`, `useAsyncData`, `useRoute`, `useRouter`, `useNuxtApp`, `useState`, `useHead`
- **Components in `app/components/`:** all of them, by filename
- **Composables in `app/composables/`:** by exported name

This means a component like this is valid, despite looking like it shouldn't be:

```vue
<script setup lang="ts">
const count = ref(0);
const doubled = computed(() => count.value * 2);
const { appName } = useAppInfo();
</script>

<template>
  <FeatureCard :title="appName" :description="`Count is ${count}`" />
</template>
```

Where did `ref`, `computed`, `useAppInfo`, and `FeatureCard` come from? They came from the auto-import fairy. Don't argue with the fairy. Don't try to manually import them either - ESLint will tell on you.

If your editor's autocomplete is confused about auto-imports, run `bun run postinstall` (or just `bun run nuxt prepare`) to regenerate the type definitions.

---

## Components: existing ones and adding new ones

Currently in `app/components/`:

- **`AppHeader.vue`** - the top navbar (DaisyUI `navbar` class).
- **`AppFooter.vue`** - the bottom thing.
- **`FeatureCard.vue`** - a typed reusable card with props for title, description, optional badge, and a slot.

To add a new component, drop a `.vue` file in `app/components/` and use it by filename. `MyThing.vue` becomes `<MyThing />` everywhere, no imports. Nested folders get prefixed: `app/components/forms/InputField.vue` becomes `<FormsInputField />`.

Props get typed via `defineProps<{ ... }>()`. Defaults via `withDefaults()`. See `FeatureCard.vue` for the canonical example - it's well-commented and worth reading.

---

## Styling: the Catppuccin Mocha experience

> Again; you want to yank this out and replace it, you go ahead. This just describes how it is _now_.

The colour palette is Catppuccin Mocha. It's a dark theme that looks like someone made a sunset out of dessert. You'll get used to it.

Useful DaisyUI semantic colours (these all map to Catppuccin Mocha shades automatically):

- **Base layers:** `bg-base-100`, `bg-base-200`, `bg-base-300` (light → darker)
- **Accents:** `primary`, `secondary`, `accent`
- **Status:** `info`, `success`, `warning`, `error`

Used as:

```vue
<button class="btn btn-primary">Affirm</button>
<div class="card bg-base-100 shadow-xl">...</div>
<span class="badge badge-secondary">v0.0.0</span>
```

For layout, reach for Tailwind: `flex`, `grid`, `gap-4`, `p-6`, `mx-auto`, etc. The `app/pages/about.vue` file is a good kitchen-sink reference for what good mixing of DaisyUI components + Tailwind utilities looks like in this codebase.

---

## State, composables, and the question of "where do I put this logic"

If a piece of reactive state needs to be shared between components, write a composable in `app/composables/`. See `useAppInfo.ts`:

```ts
export function useAppInfo() {
  const appName = ref("Affirm");
  const version = ref(pkg.version);
  const greeting = computed(
    () => `Welcome to ${appName.value} ${version.value}`,
  );
  return { appName, version, greeting };
}
```

Auto-imported. Just call it from any component.

For genuinely global singletons (config, an SDK instance, a feature flag client), use a plugin in `app/plugins/` and `nuxtApp.provide('whatever', ...)`. See `app/plugins/app-init.ts` for the simplest possible example. Then access via `const { $whatever } = useNuxtApp()`.

For state that needs to survive SSR hydration correctly, use `useState('key', () => initial)` instead of `ref()`. This is the Nuxt-blessed way to avoid hydration mismatch.

We don't use Pinia, but only because we don't need it yet. If you want it, grab it. It's the state management system which plays best with Nuxt.

---

## Calling our own API

```ts
const { data, error, pending, refresh } = await useFetch("/api/hello");
```

`useFetch` does the right thing on SSR (runs server-side, hydrates the result, doesn't double-fetch on the client). Use it. Don't reach for `$fetch` unless you have a specific reason - and you probably don't. `$fetch` is for the backend.

The endpoint above (`/api/hello`) is defined by the file `server/api/hello.get.ts`. The `.get.ts` suffix makes it GET-only. `.post.ts`, `.put.ts`, `.delete.ts` work too. No-suffix files (`hello.ts`) accept any method.

---

## Icons

We use `@nuxt/icon` with Iconify icon sets. The currently-installed sets are:

- **`heroicons`** - the workhorse UI icon set
- **`logos`** - brand logos (Vue, Nuxt, Tailwind, etc.)

Use them like this:

```vue
<Icon name="heroicons:arrow-left" size="1.2rem" />
<Icon name="logos:vue" size="2rem" />
```

`@nuxt/icon` will let you use any of the iconsets it supports, and it has a utility in the Nuxt devtools to make your life easier. If the package for an icon suite is not installed, it'll still work; it'll be fetched remotely, and startup will whinge about it. Just add the package and it'll shut up.

---

## Dev workflow commands

```bash
bun run dev              # dev server on http://localhost:3000
bun run lint             # eslint + trunk + tsc (run before pushing)
bun run lint:fix         # same but with --fix
bun run format           # prettier + trunk fmt
bun run lint:types       # just tsc --noEmit
bun run test             # vitest
bun run test:e2e         # playwright
bun run test:e2e:ui      # playwright with the nice UI runner
bun run build            # production build (don't forget --envName, the script handles it)
bun run preview          # build + wrangler dev to preview production locally
```

Our formatting task is a bit of a cheat; first Prettier runs, to catch files Trunk misses, then Trunk runs to bring the important stuff into line. `bun format` does it all for you.

A `bun run lint:fix && bun run lint:types` is the minimum you should run before pushing. CI will reject you otherwise. CI is not your friend; CI is your strict and disappointed parent.

---

## Browser / E2E testing

We haven't got any tests yet but we're set up for **Playwright** for e2e. Tests live in… well, wherever Playwright is configured to look (check `playwright.config.ts` when it exists). For frontend dev work, the UI mode is invaluable:

```bash
bun run test:e2e:ui
```

It gives you a step-by-step replay of each test with DOM snapshots. Use it when your test is flaky and you don't know why. (Spoiler: it's almost always an `await` you forgot.)

---

## Branching, PRs, commits

### Branches

- **`production`** - what's live. Don't push here directly. You can't anyway. It's protected.
- **`staging`** - what's about to be live. Also protected.
- **Your branches:** `yourname/short-purpose`, e.g. `synmux/fix-header`. Yes, this is bikeshedding. Yes, we do it anyway.

PRs target `staging`. Two approvals required to merge. syn will try to review yours; you can be one of the approvers on someone else's. Two-pairs-of-eyes is the whole point.

Every push to a PR-targeting-staging branch gets a **preview deployment** on the `affirm-staging` Worker. That URL is the easiest way to show someone what you've actually built. Use it.

### Commits

We use **Conventional Commits with GitMoji**. Yes, we use emoji in commit titles like it's 2017. No, we will not be taking questions.

Format:

```plaintext
<emoji> <type>(<scope>)<!>: <description>

<longer body explaining the why>
```

Examples:

```plaintext
✨ feat(header): add user avatar dropdown
🐛 fix(api): handle empty results from /api/hello
💄 style(about): reduce hero padding on mobile
♻️ refactor(composables): split useAppInfo into smaller pieces
📝 docs: explain the catppuccin choice in the README
```

Run `bun x gitmoji-cli list` to see the full emoji catalogue, or look at the recent git log to see what we've been using.

I truly do not care if you don't want to do this.

### Signing commits

**Please sign your commits.** You don't need GPG any more - Git supports SSH-key signing, and you're already using one of those to push. There's a guide [in GitHub's docs](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits). It takes ten minutes. Do it. please. Need help, ask syn.

---

## The gotchas (read this twice)

1. **Always `bun run dev`, never `nuxt dev`.** The first one wires up the Cloudflare bindings via `nitro-cloudflare-dev`. The second one gives you a half-broken app that fails the moment any code tries to use D1.
2. **Auto-imports are real.** If your editor whines about `ref` being undefined, run `bun run postinstall` to regenerate types. Do not "fix" it by adding manual imports.
3. **Nuxt 4, not 3.** App source is in `app/`, not the project root. Internet answers may be wrong.
4. **`useFetch` is not `fetch`.** It's reactive, SSR-aware, and runs server-side during the initial render. Treat its return value as a `Ref`, not a plain value.
5. **CSS lives in five lines.** Don't go hunting for `tailwind.config.js` - it isn't there. Tailwind v4 configures via CSS. The whole stylesheet entry is `app/assets/css/tailwind.css`.
6. **Catppuccin Mocha is the only theme.** DaisyUI's defaults are explicitly disabled. Change that if you want to.
7. **American English** in user-facing copy. "Color", not "colour". "Organization", not "organisation". It makes me very happy to be working with someone else who feels the pain about that.
8. **No React.** I cannot stress this enough. If you find yourself wanting React, take a walk.
9. The others are **not fans of AI**. Reasonable enough position; not mine. Still, it's the position which applies to this project because I'm outvoted (and fine with that). We don't integrate AI into PR review or anything like that. If you want to run it client-side, go for it. The `AGENTS.md` is the sole concession to AI in the code or tooling.

---

## When you're lost

- **`README.md`** - the existing human-facing docs, more general than this one.
- **`AGENTS.md`** (= `CLAUDE.md`) - the version of the docs that AI assistants read. Sometimes has things the README doesn't.
- **`GETTING-STARTED.md`** - long-form walkthrough of the stack. I wrote it many moons ago and it is basically this document.
  - You will note that I've spotted it 17,000 characters in. Fuck my life.
- **Existing files** - `app/pages/about.vue` is a deliberately well-commented kitchen-sink of frontend patterns. Steal from it.
- **syn** - when all else fails, ask them. They're friendly. They said so themself at the bottom of the README.

---

## Welcome aboard

That's the lot. Make pages, build components, mix DaisyUI with Tailwind utilities until something nice happens, and remember to push to `yourname/something` and PR to `staging`.

If something in this document is wrong, please fix it. If something is missing, please add it. Onboarding docs that rot are worse than no onboarding docs at all, and you, dear new frontend person, are the most qualified person in the building to spot what was missing.

Now go build something nice.

And here's a cat to reward you for reading all the way through this bullshit.

![Catputer](catputer.jpg)
