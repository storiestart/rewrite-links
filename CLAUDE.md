# rewrite-links

A Cloudflare Worker that rewrites URLs in HTML responses using the built-in `HTMLRewriter` API.

## What It Does

This worker intercepts HTTP responses and rewrites occurrences of a configured old domain to a new domain within HTML content. It targets:
- `href` attributes on `<a>` tags
- `src` attributes on `<img>` tags

Non-HTML responses are passed through unchanged.

## Development Commands

```bash
npm run dev        # Start local development server (wrangler dev)
npm run test       # Run tests with Vitest
npm run deploy     # Deploy to Cloudflare Workers
npm run cf-typegen # Regenerate TypeScript types from wrangler config
```

## Project Structure

```
src/index.ts              # Worker entry point and all business logic
wrangler.jsonc            # Cloudflare Workers configuration
vitest.config.mts         # Vitest test runner configuration
worker-configuration.d.ts # Auto-generated Cloudflare environment types
```

## Key Implementation Details

- **Entry point**: `src/index.ts` — exports a single `fetch` handler
- **URL rewriting**: `OLD_URL` and `NEW_URL` are currently hardcoded constants inside the fetch handler; change them there to update which domains are swapped
- **HTMLRewriter**: Cloudflare's streaming HTML transformer — no external parsing dependencies needed
- **Content-type gate**: Only `text/html` responses are transformed; all others pass through as-is

## Testing

Tests run inside the Cloudflare Workers runtime via `@cloudflare/vitest-pool-workers`. Add test files under a `test/` directory (excluded from TypeScript compilation by `tsconfig.json`).

```bash
npm run test
```

## Deployment

Requires a Cloudflare account and Wrangler authenticated (`wrangler login`).

```bash
npm run deploy
```

The worker name is `rewrite-links` (set in `wrangler.jsonc`).
