# Learn how this site is made

At `86e0b7dcb72e40cbc93da645fba8eabb378d72cc`, this checkout is the React/Vite personal site. It also contains a Rust **post-build** generator and verifier. [`src/App.tsx`](../src/App.tsx) maps paths to page components; [`src/data`](../src/data) holds authored content; [`src/entry-server.tsx`](../src/entry-server.tsx) renders React for static routes; [`scripts/spa-fallback.mjs`](../scripts/spa-fallback.mjs) writes route files. [`crates/site-builder/src/main.rs`](../crates/site-builder/src/main.rs) checks those files and emits sitemap and robots data in build mode. It does not replace the React site. The independent `aien-spark/drakestapleton.com` repository should be read at its own pin; its Rust generator is not the implementation in this checkout.

[The Pages workflow](../.github/workflows/pages.yml) runs Node 22 `npm ci`, `npm run build:portfolio`, then the Rust site builder and uploads `dist`. This is the deployment configuration, not proof of current live bytes. The [waitlist API](../server/waitlist_api.py) and [telemetry helper](../src/lib/tracking.ts) are separate from the static route build; a local build does not exercise consent storage, ads or live hosting. I kept content, routing, rendering and final verification separate so missing routes and metadata fail before Pages publication.

## Fresh checkout exercise (offline, no deployment)

Prerequisites: Git and Node 22. Checkout `86e0b7dcb72e40cbc93da645fba8eabb378d72cc`; run from repository root:

```sh
node --check scripts/spa-fallback.mjs
node --check scripts/verify-build.mjs
node scripts/check-copy.mjs
```

The two syntax checks return 0 with no stdout. The existing copy guard currently returns 1 and lists nine source lines, beginning `src/pages/AienPage.tsx:97: no, no`. This is a real failing baseline on the pinned checkout, not a passing build. It demonstrates the negative case: the guard refuses forbidden copy in pages. The overall command exits 1. The full release route requires Node 22, npm lockfile, Rust toolchain, `npm ci`, `npm run build:portfolio`, and `cargo run --release --manifest-path crates/site-builder/Cargo.toml -- dist` on the CI runner; that heavier route is recorded as pending here. [Replay receipt](teaching-replay-2026-10-22.txt).
