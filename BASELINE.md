# Texas Rugby — Site Baseline

Snapshot taken **2026-10-21** of the Texas Rugby site as it exists in Lovable. Use this as the
reference point for all future work tracked in this repository.

## Where the site lives

| Item | Value |
| --- | --- |
| Lovable project | **Texas Rugby Club** (`2042765e-cf62-4fe0-b7bb-3f14fea4c05e`) |
| Lovable workspace | Dustin's Lovable (`RMrGvbAhoS3o2FCf3PxW`), Pro plan |
| Editor | https://lovable.dev/projects/2042765e-cf62-4fe0-b7bb-3f14fea4c05e |
| Preview | https://id-preview--2042765e-cf62-4fe0-b7bb-3f14fea4c05e.lovable.app |
| Production | https://texasrugby.club (www redirects to apex; HTTP 200 at snapshot) |
| Published | Yes, public audience |
| Baseline commit (Lovable git) | `f9af1c0e4687c8c6f9c344af9b961844e79a77dd` |
| Last edited in Lovable | 2026-10-20 19:35 UTC |
| Project created | 2026-01-02 |
| Backend | Lovable Cloud (Supabase), project ref `mopscjemfjsojrchtwua` |
| GitHub sync | None. The Lovable project is not connected to any GitHub repo. |

Live page title: *Texas Rugby | Built on Brotherhood*. Homepage hero at snapshot: "Aggies, Alumni,
Austin" (UT vs A&M at the Rugby Ranch, 4100 Nixon Ln).

## Stack

- Vite 5 + React 19 + TypeScript, React Router 6, TanStack Query, react-helmet-async
- Tailwind CSS 3 + shadcn/ui (Radix), framer-motion, embla carousel, recharts
- Supabase JS client; Postgres with RLS, Deno Edge Functions, Storage, Auth
- Drizzle (schema + 26 migrations in `drizzle/`) alongside ~95 Supabase migrations in `supabase/migrations/`
- Email via Resend, called only from edge functions
- Tests: vitest (`src/lib/*.test.ts`: news, newsNewsletter, photoLibrary, teamPhotoRepo)
- Scripts: `dev`, `build`, `build:dev`, `lint`, `preview`

## Size of the codebase (~717 files)

| Area | Contents |
| --- | --- |
| `src/pages/` | 30 public pages, 54 admin pages, 10 alumni-portal pages, 8 player-area pages |
| `src/components/` | Feature components (admin, alumni, schedule, team, newsletter, photos, forms...) + shadcn `ui/` |
| `src/lib/` | Domain logic: `nameMatching`, `columnAliases`, `teamPhotoRepo`, `photoLibrary`, `standings`, `news`, CSV import/export |
| `supabase/functions/` | 34 edge functions (newsletter send/digests, Resend webhook, imports, notifications, YouTube sync, calendar feed, weekly reports) |
| `public/` + `src/assets/` | ~140 images (player/coach/HoF photos, hero slides, sponsor logos) |
| `docs/projects/` | 24 internal executive summaries, one per feature area |
| `.lovable/plan/` | 27 dated Lovable plan files (Sep–Oct 2026) |

## Public routes

`/`, `/team`, `/schedule`, `/recruiting`, `/alumni`, `/alumni/events`, `/donate`, `/scholarship`,
`/events`, `/hall-of-fame`, `/hof-vote`, `/media` (`/photos` → `/media#pictures`), `/sponsorships`
(= `/partnership`), `/advertising`, `/contact`, `/trophy-room`, `/program/history`,
`/program/highlights`, `/propose-event`, `/newsletters`, `/newsletters/:slug`,
`/newsletter-preferences`, `/subscribe`, `/auth`, `/auth/reset-password`, `/event-checkin`.

Hidden until launch (not in menus): `/40-40-40-challenge`, `/challenge-40`, `/support/40-40-40`.

## Signed-in areas

- **Player Area** `/player-area`: home, announcements, discussions, schedule, resources, directory (+ `/setup`)
- **Alumni Portal** `/alumni-portal`: home, announcements, discussions, events, resources, directory,
  profile (+ `/setup`, `/register`, `/pending`)
- **Admin** `/admin/*`: 54 screens covering roster, coaches, schedule, opponents, stats, standings,
  league results, people/alumni/team directories, newsletter + reports + mail log, news, media/photos,
  carousel and page headers/sections, site links, events/RSVPs/attendance, fundraising, 40-40-40
  challenge, HoF nominations, recruiting, permissions, analytics, admin guide.

## Ground rules already established in the project

From `AGENTS.md` and `docs/projects/README.md` in the Lovable repo:

- `people` is the single source of truth. DB triggers link every person-bearing row (players,
  coaches, signups, RSVPs, submissions, donors) to `people`; names flow only from `people`.
- All name matching goes through `src/lib/nameMatching.ts`; CSV headers must resolve via
  `src/lib/columnAliases.ts` so every export re-imports cleanly.
- Roles live in `user_roles`, checked with `has_role()`. Scoped roles: `super_admin`,
  `directory_manager`, `financial_manager`. Donor tables are readable only by super_admin and
  financial_manager.
- Public `/team` and Admin Roster share one current-squad rule (`get_current_roster_player_ids`).
- Newsletter digests send via Resend `/emails/batch`; only bulk newsletters use a temporary segment
  (plan cap is 3 segments).
- Page section headings are editable via `page_sections`, defaults in `src/lib/pageSectionsRegistry.json`.
- Brand rules: "TX" for match branding, "D1AA" competition level, say "Partnership"/"Advertising"
  (never "Sponsorship" in copy), zero alcohol content, no partner logos on team apparel.

## Open items at baseline (from `roadmap.md`)

- Odd email-only names (padjustin, jduke02, …): waiting on David to confirm real names/merges.
- September fix list remainder: live-stream capture from the UT Rec Sports YouTube channel, video
  season archive, public + admin conference results.
- Run the YouTube playlist sync on match days (kick-off + hourly) and add a manual "Sync now".
- Brand the auth emails. Blocked on an email sending domain (e.g. `mail.texasrugby.club`).
- Reconcile alumni directory fields with the directory export (14 orphaned `current_city` values,
  roster columns in export, legacy aliases). Deferred until directory cleanup.
- Enroll 22 directory members missing a subscription (user doing this via import UI).
- 40-40-40 Challenge: add Support menu link, waiting on David's go-ahead.
- News alerts/RSS: deferred by David.

## Notes for working from this repo

- Lovable is the system of record for code. Changes are made by prompting Lovable (or via the
  Lovable MCP `send_message`) and verified with `get_diff` against the commit above.
- The Lovable source tree has **not** been copied here yet. `docs/projects/README.md` states those
  docs are visible only to people with code access, and this GitHub repo is public, so mirroring the
  source should wait until the repo is private or the right connection is chosen.
