# REACT > 

## THE MENTAL MODEL: LAYERS AND FLOW

- Shared → Features → App (one-way flow)
- Shared: reusable, domain-agnostic building blocks (components, hooks, lib, utils, types)
- Features: domain-specific slices (auth, listings, cart, checkout, payments, messaging)
- App: composition layer (routing, providers, app shell)

This keeps domains isolated, improves tree-shaking, and prevents tight coupling.

## TOP-LEVEL SRC LAYOUT (WHAT EACH FOLDER IS FOR)

- `app/` — Composition layer
  - `routes/` or `pages/`: your route modules
  - `app.tsx`: main shell
  - `provider.tsx`: wraps global providers (router, query, Zustand persist, theming, etc.)
  - `router.tsx`: route configuration
- `assets/` — Static assets shared app-wide
- `components/` — Shared UI components (buttons, modals, form inputs)
- `config/` — App-wide config and env exposure (e.g., safe `VITE_` env exports)
- `features/` — Domain modules (auth, listings, cart, checkout, payments, messaging)
- `hooks/` — Shared hooks (e.g., `useDebounce`, `useIntersection`)
- `lib/` — Preconfigured libraries (e.g., axios instance, query client, i18n)
- `stores/` — Global state stores (Zustand) that are truly cross-feature
- `testing/` — Test utils/mocks
- `types/` — Global types
- `utils/` — Pure utility functions

Your current repo already has: `api/ assets/ components/ hooks/ stores/ types/ utils/`. You’ll add `app/`, `features/`, and optionally `config/`, `lib/`, `testing/`.

## INSIDE A FEATURE FOLDER (PICK ONLY WHAT YOU NEED)

Example: `src/features/auth`
- `api/` — API declarations/hooks specific to auth (e.g., `login`, `signup`, `refreshToken`)
- `assets/` — Images/icons for auth
- `components/` — Screens and subcomponents (LoginForm, SignupForm)
- `hooks/` — Feature-scoped hooks (`useLogin`, `useAuthGuard`)
- `stores/` — Feature stores (e.g., `useAuthStore` if not global)
- `types/` — Auth-specific types (Tokens, UserCredentials)
- `utils/` — Helpers (token parsing, storage keys)

You don’t need every folder for every feature; add only what’s needed.

## API PLACEMENT: PER-FEATURE VS CENTRALIZED `api/`

- Prefer per-feature when endpoints map cleanly to one domain (auth, listings, cart).
- Centralize in api if endpoints are shared by many features or when your HTTP client setup is complex and shared. You can still keep per-feature hooks that wrap the shared client functions.

For your app:
- Auth (JWT, Gmail API for notifications/verification): per-feature (`features/auth/api`)
- Listings/catalog/search: per-feature (`features/catalog/api`)
- Cart: per-feature (`features/cart/api`)
- Checkout (Stripe/PayPal): per-feature (`features/checkout/api`), can reuse a shared `lib/payments` if you need shared payment wiring
- Messaging (seller-buyer): per-feature (`features/messaging/api`)

## AVOID BARREL FILES IN FEATURES

Barrels (`index.ts` re-exporting everything) can break Vite tree-shaking and create accidental coupling. Import what you need directly from concrete modules (e.g., `import { LoginForm } from '@/features/auth/components/login-form'`).

## PREVENT CROSS-FEATURE IMPORTS

Don’t let `features/comments` import from `features/discussions`. Compose them in `app/`. Enforce with ESLint’s `import/no-restricted-paths`.

Flat-config example for your eslint.config.js (ESLint v9+):

```js
// eslint.config.js
import ts from 'typescript-eslint';
import importPlugin from 'eslint-plugin-import';

export default [
  ...ts.configs.recommendedTypeChecked, // or your TS setup
  {
    files: ['src/**/*.{ts,tsx}'],
    plugins: { import: importPlugin },
    rules: {
      'import/no-restricted-paths': ['error', {
        zones: [
          // disable cross-feature imports
          { target: './src/features/auth', from: './src/features', except: ['./auth'] },
          { target: './src/features/catalog', from: './src/features', except: ['./catalog'] },
          { target: './src/features/cart', from: './src/features', except: ['./cart'] },
          { target: './src/features/checkout', from: './src/features', except: ['./checkout'] },
          { target: './src/features/payments', from: './src/features', except: ['./payments'] },
          { target: './src/features/messaging', from: './src/features', except: ['./messaging'] },
          // add other features as you create them
        ],
      }],
    },
  },
];
```

Ensure `eslint-plugin-import` is installed and resolvers set if needed.

## ENFORCE UNIDIRECTIONAL ARCHITECTURE

App can import from features and shared; features can import only from shared; shared imports from nothing app/feature-specific.

Add zones:

```js
// inside the same rules block as above
'import/no-restricted-paths': ['error', {
  zones: [
    // ... previous cross-feature restrictions

    // features cannot import from app
    { target: './src/features', from: './src/app' },

    // shared can’t import upward from features/app
    {
      target: [
        './src/components',
        './src/hooks',
        './src/lib',
        './src/types',
        './src/utils',
      ],
      from: ['./src/features', './src/app'],
    },
  ],
}],
```

This nudges code to flow: shared → features → app.

## APPLY THIS TO YOUR EBAY BUILD

Suggest creating:
- `src/app/`
  - `app.tsx` — Layout shell (header, footer, outlet)
  - `provider.tsx` — React Router, Zustand persist, React Query, theming
  - `router.tsx` — Routes for listings, product detail, cart, checkout, auth, orders
  - `routes/` — Route modules if you prefer co-located routes
- `src/features/`
  - `auth/` — login/signup, JWT handling, email verification (Gmail API if applicable)
  - `catalog/` — search, categories, product detail
  - `cart/` — cart state & UI
  - `checkout/` — address, shipping, Stripe/PayPal flows
  - `payments/` — payment-specific integrations if you want a separate module (or keep in `checkout`)
  - `orders/` — order history, order detail
  - `messaging/` — buyer–seller messages/notifications
  - `profile/` — account settings, addresses, payment methods
- `src/lib/` — axios client, Stripe/PayPal SDK wrappers, JWT helpers
- `src/config/` — env exposure (e.g., `VITE_API_URL`, `VITE_STRIPE_PK`, `VITE_PAYPAL_CLIENT_ID`)
- Keep your existing `components/`, `hooks/`, `utils/`, `types/`, `assets/`, `stores/` for truly shared building blocks.

Tip: If a Zustand store is domain-specific (e.g., cart), put it in that feature’s `stores/`. Reserve root stores for global/shared stores only.

## OPTIONAL: PATH ALIASES FOR CLEAN IMPORTS

In tsconfig.json:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

Then import like `import { LoginForm } from '@/features/auth/components/login-form'`.

## QUICK NEXT STEPS

- Add `src/app/` and `src/features/*` folders and move route composition into `app/`.
- Add ESLint `import/no-restricted-paths` zones for cross-feature and unidirectional rules.
- Decide per-feature vs centralized `api/` per domain; start per-feature for auth, cart, checkout.
- Create `lib/axios.ts`, `lib/stripe.ts`, `lib/paypal.ts` wrappers and use them in feature APIs.
- Keep auth and payments logic inside features; only compose at `app/`.

If you want, I can scaffold the `app/` files and a few feature skeletons (auth, catalog, cart, checkout) in your repo to get you moving.