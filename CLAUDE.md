# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
pnpm dev          # Start dev server on port 3000
pnpm build        # Production build
pnpm test         # Run tests with Vitest (vitest run)
pnpm lint         # Run ESLint
pnpm format       # Check formatting with Prettier
pnpm check        # Auto-fix formatting and lint issues (prettier --write + eslint --fix)
pnpm deploy       # Build and deploy to Cloudflare Workers
```

## Architecture

This is a **TanStack Start** full-stack React app deployed to **Cloudflare Workers** via the `@cloudflare/vite-plugin`. The SSR environment runs in the Worker (configured with `nodejs_compat`).

**Routing** is file-based via TanStack Router. Routes live in `src/routes/`. Adding a file there auto-generates the route tree in `src/routeTree.gen.ts` — never edit that file manually. The root layout is `src/routes/__root.tsx`, which wraps all routes in the HTML shell.

**Path aliases**: `@/*` maps to `src/*` (configured in both `tsconfig.json` and `vite-tsconfig-paths`). The `package.json` also defines `#/*` → `./src/*` for Node subpath imports.

**Server functions** use `createServerFn` from `@tanstack/react-start` — they run on the Worker and are callable from client components with full type safety.

**Styling** uses Tailwind CSS v4 (via `@tailwindcss/vite` plugin, not PostCSS). Global styles are in `src/styles.css`.

**Navigation** uses `<Link>` from `@tanstack/react-router` for SPA navigation. The `Header` component (`src/components/Header.tsx`) contains a slide-out drawer nav — add new route links there between the `{/* Demo Links Start */}` and `{/* Demo Links End */}` comments.

**Testing** uses Vitest with jsdom and `@testing-library/react`.

**TypeScript** is strict with `noUnusedLocals` and `noUnusedParameters` enforced.
