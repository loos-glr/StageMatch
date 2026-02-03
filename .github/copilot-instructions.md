# StageMatch Copilot Instructions

## Project Overview
StageMatch is a Tinder-style web application for matching students with internships. It is a Vue 3 application built with Vite and Tailwind CSS.
- **Root Directory**: `app/` (Run all commands here)
- **Deployment**: Netlify (Static build to `dist`)
- **Key Documentation**: `docs/project.md` contains the functional flow (Swipe logic).

## Architecture & Conventions
- **Framework**: Vue 3 (Options API currently used in `App.vue`, prefer consistency if extending).
- **Styling**: Tailwind CSS v4 with `@tailwindcss/vite`.
  - Custom colors: `lime` defined in `tailwind.config.cjs`.
  - The app mimics a mobile device interface (`max-w-md mx-auto`).
- **Routing**: Minimalistic custom routing via dynamic components (`<component :is="currentView" />`) in `App.vue`. No `vue-router` is currently installed.
- **State Management**: Local state passed via props (`active-tab`) and events.

## Key Files
- `app/src/App.vue`: Main layout and "router" logic. Handles bottom navigation.
- `app/src/views/`: Contains main page views (`HomeView.vue` for swiping, `MessagesView.vue`, `ProfileView.vue`).
- `app/views/HomeView.vue`: Likely contains the core "Swipe" logic (referenced in project specs).
- `app/src/styles.css`: Global styles imported in `main.js`.

## Developer Workflow
- **Development**: `npm run dev` (Runs Vite server on port 5173).
- **Build**: `npm run build` (Outputs to `app/dist`).
- **Package Management**: usage of `npm` in `app/` directory.

## Coding Guidelines
- **Vue Components**:
  - Use PascalCase for component filenames (`HeaderComp.vue`).
  - Keep styling utility-first (Tailwind).
- **Icons**: SVG icons are currently inline in `App.vue`.
- **Match Logic**: Follow the flow defined in `docs/project.md` (Swipe Left/Right logic).

## Git Workflow & Interaction
- **Commit Policy**: Never commit changes automatically. Only commit on explicit user demand.
- **Workflow**:
  1. Propose a plan for changes.
  2. Implement changes upon approval.
  3. Ask the user to review the implementation.
  4. On approval, ask if changes should be committed.
- **Push Policy**: Never push changes. The user will always push manually.
