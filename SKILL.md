---
name: repo-ui-optimizer-skill
description: Analyze any repo end to end, inventory routes and risky global UI patterns, then ship safe conversion-first UI improvements without changing navigation structure, removing features, or breaking logic.
---

# Repo UI Optimizer Skill

Use this skill when the task is to analyze and improve an existing product UI while keeping the current product recognizable and functional.

## Core Intent

Optimize conversion, clarity, and responsiveness while preserving the existing product structure.

## Non-Negotiable Rules

- Preserve the product's structure, behavior, and existing core actions.
- Keep current navigation patterns unless the user explicitly asks to change them.
- Preserve roughly 80 percent of the existing visual language unless the user asks for a stronger redesign.
- Do not remove features, routes, inputs, buttons, or business logic just to make the UI look cleaner.
- Do not introduce risky global CSS overrides.
- Do not remove existing global overrides blindly either; first trace where they are used and replace them safely.
- Do not hardcode production URLs, screenshot APIs, or project-specific route lists into the workflow.
- Use the repo's own stack, components, and patterns instead of forcing a new system.

## Workflow

### 1. Read The Real Project Before Suggesting Anything

Read the minimum source of truth first:

- `AGENTS.md`, `README.md`, `package.json`
- app entry points and router files
- shared layout, navigation, shell, and global CSS files
- design tokens, Tailwind config, theme files, shared UI primitives
- the most relevant recent commits if there is evidence of recurring UI churn

State clearly:

- what the user really means
- which routes and flows are affected
- which files control the shell, navigation, page composition, tokens, and risky global styles
- what stack, UI library, and routing model are in use

### 2. Grep The Whole Codebase Before Editing

Use fast code search first. At minimum grep for:

- routes: `path`, `createBrowserRouter`, `Routes`, `Route`, `href`, `to`, `navigate`, `router.push`
- shells and nav: `Header`, `Footer`, `Sidebar`, `BottomNav`, `Tabs`, `Drawer`, `Dialog`
- conversion surfaces: `payment`, `checkout`, `billing`, `pricing`, `premium`, `promote`, `subscription`, `credits`, `upgrade`
- risky style drift: `!important`, `position: fixed`, `position: sticky`, `z-index`, `100vh`, `overflow: hidden`, `.modal`, `.dialog`, `[role="dialog"]`

Build a route and surface inventory before proposing UI changes.

### 3. Analyze The Whole Workflow, Not One Screen

Trace the affected experience across:

- home or landing
- listing and category/discovery views
- payment, promote, pricing, upgrade, and checkout flows
- auth gates and account dependencies
- mobile navigation and sticky/floating surfaces
- admin or merchant flows if they touch the same conversion path

Prioritize recurring issues like:

- too much scrolling on mobile
- fragmented payment or promote flows
- hidden or duplicated primary CTAs
- cluttered hierarchy and weak scanning
- inconsistent spacing, labels, states, or card patterns
- bottom-nav, banner, drawer, or FAB overlap
- risky global CSS overrides causing cross-page drift

### 4. Capture Visual Evidence When The App Is Runnable

If the app can run locally or there is a safe target environment:

- discover routes from code first
- capture multiple key pages, not just one
- prefer desktop and mobile screenshots at minimum
- include the main discovery and conversion surfaces
- inspect screenshots visually before deciding what to change
- use the available browser tools for the environment instead of inventing custom screenshot infrastructure

If the app is not runnable, continue with code analysis and say what visual verification remains blocked.

### 5. Plan Safe Changes

Default sequence:

1. remove or narrow risky global overrides
2. unify shared tokens and repeated action patterns
3. compact spacing and hierarchy on high-traffic pages
4. move rare actions into secondary surfaces without hiding critical actions
5. simplify payment and promote decision points without deleting existing capabilities

Prefer shared-component or token fixes before page-specific hacks.

### 6. Implement Conservatively

When editing code:

- preserve route behavior and business logic
- preserve all existing controls and core flows
- keep desktop and mobile navigation concepts intact
- avoid breaking auth, form validation, analytics, and API wiring
- do not turn cleanup into a redesign rewrite

### 7. Verify Before Claiming Success

After changes:

- re-check the edited routes visually if possible
- confirm primary CTAs and key summary info are still visible
- confirm there are no overlaps, clipped dialogs, or broken sticky elements
- check console errors and obvious workflow regressions
- mention any remaining risk explicitly

## Default Output Shape

Unless the user asks for something else, return:

1. analysis of what matters and what files/routes control it
2. step-by-step redesign or hardening plan
3. implementation summary if code changed
4. verification evidence
5. a final copy-paste prompt usable by a designer, spec writer, or coding agent

## Final Prompt Template

Use this template when the user wants a reusable prompt:

```text
Analyze this repo end to end before changing UI. First inspect the stack, router, layout shell, navigation, shared UI primitives, design tokens, global CSS, and recent relevant commits. Grep the full codebase for routes, conversion flows, payment/promote surfaces, and risky global overrides. Build a route inventory and identify which files control navigation, layout, dialogs, sticky elements, and shared styling.

Then improve the UI conservatively:
- preserve roughly 80% of the current visual style
- keep the current navigation structure
- keep all existing controls and core actions
- do not remove features or break logic
- reduce clutter, scroll depth, and duplicated actions
- keep primary CTAs and key info visible when possible
- move rare actions into secondary surfaces where appropriate
- remove or narrow risky global overrides safely
- unify spacing, hierarchy, labels, and CTA patterns across related pages

Scope the work across both desktop and responsive/mobile views, especially home, discovery/listing/category views, and payment/promote/upgrade flows.

If the app is runnable, take multiple screenshots of the key routes discovered from code and use them as evidence before and after changes. Use the environment's available browser tools rather than hardcoded external screenshot APIs.

Output:
1. What the user really means and the highest-value issues
2. Step-by-step redesign or hardening plan
3. Safe implementation approach tied to the actual files
4. Verification evidence and remaining risks
```
