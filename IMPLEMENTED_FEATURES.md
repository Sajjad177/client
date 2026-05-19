# Implemented Features — Grocery App (initial scaffolding)

This file lists the UI scaffolding, placeholder routes, and shared components that were added as part of the initial project setup. It also documents the current authentication/protected-route situation and recommended next steps.

1. App Router route groups and layouts

- `app/(root)/layout.tsx` — Public website layout (Navbar + Footer). Uses shared components from `components/ui` and wraps public pages.
- `app/(customer)/layout.tsx` — Customer dashboard layout. Uses `components/ui/dashboard-shell` to render a sidebar + content area.

2. Public placeholder pages (file paths)

- `app/(root)/page.tsx` -> /
- `app/(root)/shop/page.tsx` -> /shop
- `app/(root)/categories/page.tsx` -> /categories
- `app/(root)/product/[slug]/page.tsx` -> /product/[slug]
- `app/(root)/cart/page.tsx` -> /cart
- `app/(root)/checkout/page.tsx` -> /checkout
- `app/(root)/wishlist/page.tsx` -> /wishlist
- `app/(root)/about/page.tsx` -> /about
- `app/(root)/contact/page.tsx` -> /contact
- `app/(root)/auth/login/page.tsx` -> /auth/login
- `app/(root)/auth/register/page.tsx` -> /auth/register

3. Customer dashboard placeholder pages

- `app/(customer)/dashboard/page.tsx` -> /dashboard
- `app/(customer)/dashboard/profile/page.tsx` -> /dashboard/profile
- `app/(customer)/dashboard/orders/page.tsx` -> /dashboard/orders
- `app/(customer)/dashboard/addresses/page.tsx` -> /dashboard/addresses
- `app/(customer)/dashboard/wishlist/page.tsx` -> /dashboard/wishlist
- `app/(customer)/dashboard/settings/page.tsx` -> /dashboard/settings

4. Shared UI primitives (presentational)

- `components/ui/navbar.tsx`
- `components/ui/footer.tsx`
- `components/ui/logo.tsx`
- `components/ui/container.tsx`
- `components/ui/dashboard-shell.tsx`
- `components/ui/dashboard-sidebar.tsx`

5. Other files added

- `client/ARCHITECTURE.md` — short architecture notes & conventions

6. Existing react-router (legacy/spa) files
   NOTE: There is also a React SPA routing implementation under `client/src` that uses `react-router-dom`.

- `client/src/App.tsx` — react-router-dom routes and nested `ProtectedRoute` wrapper
- `client/src/components/ProtectedRoute.tsx` — currently this component simply returns `<Outlet />` (no auth logic yet). This means the routes nested inside it are not actually protected.

Current auth/protected-route status

- The `ProtectedRoute` component in `client/src/components/ProtectedRoute.tsx` currently DOES NOT perform any authentication checks. It returns `<Outlet />` which lets nested routes render for all users.
- In the Next.js App Router code (under `app/`), no server-side or middleware-based protection has been added yet.

Important notes & explanations

- Dual-routing present: The repo currently contains both Next.js App Router pages (`app/`) and a client-side SPA using `react-router-dom` (`src/`). These are different routing paradigms and should be consolidated. For a Next.js App Router app, prefer using `app/` routes and remove `react-router-dom` routing to avoid confusion.
- Client-side ProtectedRoute is only a UX layer — server/API routes must validate authentication on every request.
- For a secure production-ready approach, protect the dashboard using Next.js middleware or server-side checks in the dashboard layout (see recommended next steps below).

Recommended next steps (pick one)

1. Remove `react-router-dom` and migrate SPA routes into Next.js App Router under `app/` (recommended). Then protect dashboard routes using one of:
   - Middleware (`middleware.ts`) to redirect unauthenticated visitors early.
   - Server-side check in `app/(customer)/layout.tsx` using `cookies()` + `redirect()`.
2. If you want to keep `react-router-dom` for now, implement a proper `useAuth()` hook and replace `ProtectedRoute` with logic that checks auth state and returns `<Navigate to="/login" replace />` when unauthenticated.
3. Implement secure session management (httpOnly cookies, refresh token flow) and enforce auth checks in API routes.

If you want, I can implement one of the recommended protections for you now:

- Add `middleware.ts` that protects `/dashboard`.
- Add server-side check to `app/(customer)/layout.tsx`.
- Update `client/src/components/ProtectedRoute.tsx` to a real react-router guard.

File location

- Document created at: `client/IMPLEMENTED_FEATURES.md`

If you'd like this exported or presented differently (README section, PR-ready patch, or a short in-app banner showing the routes), tell me which option and I'll prepare it.
