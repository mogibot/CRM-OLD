# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

# Karnaf CRM — Claude Code Project Context

## Project Overview
Karnaf Real Estate (קרנף נדל״ן) CRM — lead management + WhatsApp sales bot.
Tech: React 18 + TypeScript + Vite, Supabase (PostgreSQL + Auth + Realtime),
Tailwind CSS + shadcn/ui (RTL), WATI WhatsApp API, Claude API, Make.com.

## Core Products (affects bot classification)
- "הדרך לדירה" — digital training program, 4,000-5,000 ILS
- Premium investor coaching — 30,000 ILS, requires phone call
- Webinars — free lead-gen events
- Personal consulting — deprioritized

## Architecture
Instagram DM → WATI WhatsApp → Claude bot → Supabase CRM
Make.com orchestrates webhooks. Bot classifies leads, warms them,
escalates hot leads to human agent (2hr/day).

## Commands
- `npm run dev` — start dev server on port 8080
- `npm run build` — production build (type-checks via tsc -b then vite build)
- `npm run build:dev` — dev-mode build
- `npm run lint` — ESLint
- `npx supabase functions serve` — local edge functions
- `npx supabase functions deploy <name>` — deploy single edge function

## Path Aliases
`@/*` maps to `./src/*` (configured in tsconfig + vite)

## Token Efficiency Rules
- NEVER read entire files — use grep/ripgrep first to find relevant sections
- ALWAYS check what exists before creating new files
- Use batch operations: write multiple small files in one bash command
- Prefer editing existing components over creating new ones
- Use TodoWrite for tasks >3 steps; check off as you go
- If uncertain about a file's content, read only the first 50 lines first
- When fixing bugs, read only the failing function, not the whole file

## File Structure
src/components/   — UI components (leads/, dashboard/, layout/, workqueue/, bot/, ui/)
src/hooks/        — React Query hooks, one per domain
src/pages/        — Route pages
src/lib/          — Utilities, constants, types
src/integrations/ — Supabase client + generated types
supabase/functions/ — Edge functions (bot webhook, lead intake)

## Coding Standards
- TypeScript is NOT strict (noImplicitAny: false, strictNullChecks: false) — but avoid `any` where reasonable
- Components max 200 lines — extract if longer
- All text Hebrew, RTL (dir="rtl" on root)
- Tailwind only — no inline styles, no CSS modules
- shadcn/ui components preferred over custom
- Supabase RLS must be considered for all queries

## Supabase Schema (key tables)
leads — full_name, phone, email, source, stage, score, product_interest, bot_score, bot_stage
bot_sessions — lead_id, wati_phone, state (JSON), last_activity
work_queue — lead_id, reason, priority, status, agent_id
conversations — lead_id, channel, last_message_at
messages — conversation_id, content, sender_type (bot/agent/lead)
bot_config — key, value (JSON) — system_prompt, transfer_threshold, active_hours
integration_logs — source, status, request_data

## Routes (src/App.tsx)
/ → Dashboard, /leads → Leads, /work-queue → WorkQueue, /bot-config → BotConfig,
/calendar → Calendar, /reports → Reports, /settings → Settings, /auth → Auth
All except /auth wrapped in MainLayout.

## Do NOT
- Do not modify supabase/migrations without being asked
- Do not install packages without checking package.json first
- Do not create new pages without checking src/App.tsx routes
- Do not use ManyChat — it has been removed from scope
- Do not use Airtable — Google Sheets or Supabase only
- Do not hardcode phone numbers or API keys

## Environment Variables Required
VITE_SUPABASE_URL, VITE_SUPABASE_PUBLISHABLE_KEY
WATI_API_KEY, WATI_WEBHOOK_TOKEN
ANTHROPIC_API_KEY (for Supabase edge functions)
MAKE_WEBHOOK_SECRET
