---
name: redesign-ui
description: Redesigns a Vue 3 application's UI into a modern SaaS-style interface with a vertical navigation sidebar on the left, consistent spacing, and a polished professional look. Replaces horizontal top nav with a fixed dark sidebar containing icons, nav links, and a footer with profile/language controls.
---

# UI Redesign: Vertical Sidebar Layout

This skill transforms a Vue 3 app from a horizontal top-nav layout into a modern SaaS-style vertical sidebar layout. Follow every step precisely. All `.vue` file edits **must be delegated to the vue-expert subagent** per CLAUDE.md rules.

---

## Overview of Changes

| Area | Before | After |
|------|--------|-------|
| Layout direction | `flex-column` (stacked) | `flex-row` (side-by-side) |
| Navigation | Horizontal tabs in top bar | Vertical links in left sidebar |
| Sidebar | None | 240px fixed, dark (#0f172a) |
| FilterBar position | Sticky below top nav (top: 70px) | Sticky at top of main wrapper (top: 0) |
| Profile / Language | Top-right of nav bar | Sidebar footer |
| CSS tokens | Hardcoded hex values | CSS custom properties on `:root` |

---

## Pre-flight Checklist

Before making any changes, confirm:
- [ ] `client/src/App.vue` exists and uses the horizontal `.top-nav` pattern
- [ ] `client/src/components/FilterBar.vue` exists with `position: sticky; top: 70px`
- [ ] The app runs cleanly on `npm run dev`
- [ ] You have identified all nav routes from `client/src/main.js`

---

## Step 1 — Redesign `App.vue` (delegate to vue-expert)

**Delegate this entire step to the vue-expert subagent.**

### 1a. Replace the `<template>` block

Replace the existing template with this structure. Preserve all existing `<script>` imports, logic, and modal components exactly — only the structural HTML changes:

```html
<template>
  <div class="app">

    <!-- Sidebar -->
    <aside class="sidebar">
      <div class="sidebar-brand">
        <span class="sidebar-brand-name">{{ t('nav.companyName') }}</span>
        <span class="sidebar-brand-sub">{{ t('nav.subtitle') }}</span>
      </div>

      <nav class="sidebar-nav">
        <router-link to="/" :class="{ active: $route.path === '/' }">
          <!-- Overview icon -->
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <rect x="3" y="3" width="7" height="7"/><rect x="14" y="3" width="7" height="7"/>
            <rect x="3" y="14" width="7" height="7"/><rect x="14" y="14" width="7" height="7"/>
          </svg>
          <span>{{ t('nav.overview') }}</span>
        </router-link>

        <router-link to="/inventory" :class="{ active: $route.path === '/inventory' }">
          <!-- Inventory / box icon -->
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M21 8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16Z"/>
            <path d="m3.3 7 8.7 5 8.7-5"/><path d="M12 22V12"/>
          </svg>
          <span>{{ t('nav.inventory') }}</span>
        </router-link>

        <router-link to="/orders" :class="{ active: $route.path === '/orders' }">
          <!-- Orders / clipboard icon -->
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <rect width="8" height="4" x="8" y="2" rx="1" ry="1"/>
            <path d="M16 4h2a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2V6a2 2 0 0 1 2-2h2"/>
            <path d="M12 11h4"/><path d="M12 16h4"/><path d="M8 11h.01"/><path d="M8 16h.01"/>
          </svg>
          <span>{{ t('nav.orders') }}</span>
        </router-link>

        <router-link to="/spending" :class="{ active: $route.path === '/spending' }">
          <!-- Finance / credit card icon -->
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <rect width="20" height="14" x="2" y="5" rx="2"/>
            <line x1="2" x2="22" y1="10" y2="10"/>
          </svg>
          <span>{{ t('nav.finance') }}</span>
        </router-link>

        <router-link to="/demand" :class="{ active: $route.path === '/demand' }">
          <!-- Demand / trending up icon -->
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <polyline points="22 7 13.5 15.5 8.5 10.5 2 17"/>
            <polyline points="16 7 22 7 22 13"/>
          </svg>
          <span>{{ t('nav.demandForecast') }}</span>
        </router-link>

        <router-link to="/reports" :class="{ active: $route.path === '/reports' }">
          <!-- Reports / bar chart icon -->
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="18" x2="18" y1="20" y2="10"/><line x1="12" x2="12" y1="20" y2="4"/>
            <line x1="6" x2="6" y1="20" y2="14"/>
          </svg>
          <span>Reports</span>
        </router-link>

        <router-link to="/backlog" :class="{ active: $route.path === '/backlog' }">
          <!-- Backlog / clock-alert icon -->
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <circle cx="12" cy="12" r="10"/>
            <polyline points="12 6 12 12 16 14"/>
          </svg>
          <span>Backlog</span>
        </router-link>
      </nav>

      <div class="sidebar-footer">
        <LanguageSwitcher />
        <ProfileMenu
          @show-profile-details="showProfileDetails = true"
          @show-tasks="showTasks = true"
        />
      </div>
    </aside>

    <!-- Main area -->
    <div class="main-wrapper">
      <FilterBar />
      <main class="main-content">
        <router-view />
      </main>
    </div>

    <!-- Modals (unchanged) -->
    <ProfileDetailsModal
      :is-open="showProfileDetails"
      @close="showProfileDetails = false"
    />
    <TasksModal
      :is-open="showTasks"
      :tasks="tasks"
      @close="showTasks = false"
      @add-task="addTask"
      @delete-task="deleteTask"
      @toggle-task="toggleTask"
    />

  </div>
</template>
```

> **Note:** If the app has a `/backlog` route, include the Backlog nav link as shown above. If not, omit it.

### 1b. Replace the global `<style>` block

Keep all existing utility classes (`.card`, `.badge`, `.stat-card`, `.loading`, `.error`, table styles, etc.) unchanged. Only replace the layout/nav styles at the top. The full replacement for the layout portion (everything before `.page-header`) is:

```css
/* ── Reset ── */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* ── Design tokens ── */
:root {
  --sidebar-width: 240px;
  --sidebar-bg: #0f172a;
  --sidebar-text: #94a3b8;
  --sidebar-text-active: #f1f5f9;
  --sidebar-active-bg: rgba(255, 255, 255, 0.08);
  --sidebar-hover-bg: rgba(255, 255, 255, 0.05);
  --sidebar-border: rgba(255, 255, 255, 0.07);
  --content-bg: #f8fafc;
  --nav-link-radius: 7px;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
    Oxygen, Ubuntu, Cantarell, sans-serif;
  background: var(--content-bg);
  color: #1e293b;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* ── Root layout ── */
.app {
  display: flex;
  flex-direction: row;
  height: 100vh;
  overflow: hidden;
}

/* ── Sidebar ── */
.sidebar {
  width: var(--sidebar-width);
  min-width: var(--sidebar-width);
  background: var(--sidebar-bg);
  display: flex;
  flex-direction: column;
  height: 100vh;
  overflow: hidden;
  border-right: 1px solid var(--sidebar-border);
}

/* Brand / logo area */
.sidebar-brand {
  padding: 1.5rem 1.25rem 1.25rem;
  border-bottom: 1px solid var(--sidebar-border);
  flex-shrink: 0;
}

.sidebar-brand-name {
  display: block;
  font-size: 0.9375rem;
  font-weight: 700;
  color: #f1f5f9;
  letter-spacing: -0.02em;
  line-height: 1.3;
}

.sidebar-brand-sub {
  display: block;
  font-size: 0.75rem;
  color: var(--sidebar-text);
  margin-top: 0.25rem;
  font-weight: 400;
}

/* Nav links */
.sidebar-nav {
  flex: 1;
  overflow-y: auto;
  padding: 0.75rem 0.75rem;
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.sidebar-nav a {
  display: flex;
  align-items: center;
  gap: 0.625rem;
  padding: 0.5625rem 0.75rem;
  border-radius: var(--nav-link-radius);
  color: var(--sidebar-text);
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 500;
  transition: background 0.15s ease, color 0.15s ease;
  white-space: nowrap;
  overflow: hidden;
}

.sidebar-nav a svg {
  flex-shrink: 0;
  opacity: 0.7;
  transition: opacity 0.15s ease;
}

.sidebar-nav a:hover {
  background: var(--sidebar-hover-bg);
  color: var(--sidebar-text-active);
}

.sidebar-nav a:hover svg {
  opacity: 1;
}

.sidebar-nav a.active {
  background: var(--sidebar-active-bg);
  color: var(--sidebar-text-active);
}

.sidebar-nav a.active svg {
  opacity: 1;
}

/* Sidebar footer */
.sidebar-footer {
  flex-shrink: 0;
  padding: 0.875rem 0.75rem;
  border-top: 1px solid var(--sidebar-border);
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 0.5rem;
}

/* ── Main wrapper ── */
.main-wrapper {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-width: 0;
  overflow: hidden;
}

/* ── Main content ── */
.main-content {
  flex: 1;
  overflow-y: auto;
  padding: 1.5rem 2rem;
}
```

> **Important:** After this block, keep **all existing utility classes unchanged** starting from `.page-header` through the end of the file. Do not remove `.card`, `.badge`, `.stat-card`, table styles, `.loading`, `.error`, or any other utility class.

---

## Step 2 — Update `FilterBar.vue` (delegate to vue-expert)

**Delegate this step to the vue-expert subagent.**

The FilterBar is now inside `.main-wrapper` which is an `overflow: hidden` flex column, so it needs `top: 0` instead of `top: 70px`.

Find and replace in the scoped `<style>` block:

```css
/* BEFORE */
position: sticky;
top: 70px;

/* AFTER */
position: sticky;
top: 0;
```

Also, if FilterBar has `z-index: 90`, keep it. If it references the nav height anywhere else, remove those references.

---

## Step 3 — Verify ProfileMenu and LanguageSwitcher still work

The `ProfileMenu` and `LanguageSwitcher` components are now inside `.sidebar-footer` instead of the old `.nav-container`. Confirm:

1. Both components still receive any required props
2. `ProfileMenu` still emits `@show-profile-details` and `@show-tasks` (wired in the template above)
3. No CSS in those components hardcodes positioning relative to `top: 0` or a top nav height

If either component has styles that assume top-right positioning (e.g., `position: fixed; top: 0; right: 0`), update them to work in the sidebar footer context.

---

## CSS Design Tokens Reference

| Token | Value | Usage |
|-------|-------|-------|
| `--sidebar-width` | `240px` | Sidebar fixed width |
| `--sidebar-bg` | `#0f172a` | Sidebar background (slate-900) |
| `--sidebar-text` | `#94a3b8` | Inactive link color (slate-400) |
| `--sidebar-text-active` | `#f1f5f9` | Active/hover link color (slate-100) |
| `--sidebar-active-bg` | `rgba(255,255,255,0.08)` | Active link background |
| `--sidebar-hover-bg` | `rgba(255,255,255,0.05)` | Hover link background |
| `--sidebar-border` | `rgba(255,255,255,0.07)` | Internal dividers |
| `--content-bg` | `#f8fafc` | Main content background |
| `--nav-link-radius` | `7px` | Nav link border radius |

---

## Nav Icon Reference

Each route gets a Lucide-compatible inline SVG (18×18, `stroke="currentColor"`, `stroke-width="2"`):

| Route | Icon style |
|-------|-----------|
| `/` Overview | 4 small rectangles (grid) |
| `/inventory` | 3D box / package |
| `/orders` | Clipboard with lines |
| `/spending` | Credit card with line |
| `/demand` | Trending-up polyline |
| `/reports` | Three vertical bars |
| `/backlog` | Clock with hands |

---

## Common Pitfalls

- **Scrolling broken**: If page content doesn't scroll, ensure `overflow-y: auto` is on `.main-content` and `overflow: hidden` is on `.app` and `.main-wrapper`. Do NOT put `overflow: auto` on `.app`.
- **FilterBar not sticky**: If FilterBar doesn't stick, confirm `.main-wrapper` has `overflow: hidden` (not `overflow: auto`). The scroll context must be on `.main-content`, not its parent.
- **Sidebar overflows**: If sidebar content overflows vertically, ensure `.sidebar-nav` has `overflow-y: auto` and `.sidebar` has `overflow: hidden`.
- **ProfileMenu renders off-screen**: If the dropdown renders outside the viewport, the component may use `position: fixed`. This is fine — fixed positioning is relative to the viewport, not the sidebar.
- **Active state not highlighting**: Confirm router-links use `:class="{ active: $route.path === '/path' }"`. Vue Router's `router-link-active` class may also work if CSS is adjusted.

---

## Verification Checklist

After implementation, verify each item:

- [ ] `cd client && npm run dev` — zero console errors
- [ ] Sidebar renders on the left (240px dark panel)
- [ ] All 7 nav links visible with icons
- [ ] Clicking each nav link changes route and highlights the active link
- [ ] FilterBar renders at top of main area and stays sticky on scroll
- [ ] Changing a filter reloads data in the active view
- [ ] ProfileMenu opens correctly from sidebar footer
- [ ] LanguageSwitcher toggles locale from sidebar footer
- [ ] All 7 views (Dashboard, Inventory, Orders, Spending, Demand, Reports, Backlog) render without layout breakage
- [ ] Cards, tables, badges, and stat cards retain their original appearance
- [ ] No horizontal scrollbar on the main content area at 1280px+ viewport width
