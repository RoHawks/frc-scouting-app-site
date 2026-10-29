# 3419 Scout

The scouting app for FRC Team 3419, Hunter Robotics (the RoHawks). Scouts record matches from the stands; managers plan assignments, track coverage and build the picklist.

Live at **https://scout.rohawks.org**

## How to use

**Scouts**
- Ask a scouting manager for an invite code, then create your account with the code they provide. 
- Sign in, pick your event, and record each match from **Home** page.
- Scouting works even with no WiFi. Your entries are saved locally then sync whenever possible. 

**Managers**
- **Plan**: pull the schedule from The Blue Alliance (TBA) and manage and create scouting assignments.
- **Run**: see each match with its assigned coverage and conflicts. also see how many entries each scout has recorded so far.
- **Pick**: individual team stats, side-by-side comparisons, picklist and CSV export.
- **Accounts**: create accounts, send invite codes and give permissions

## How it works

- SvelteKit 2 with Svelte 5, built as a static site and hosted on GitHub Pages.
- Offline first: every entry is first written to IndexedDB, then synced to Supabase (Postgres) in the background whenever possible.
- Accounts and permissions are enforced in the database with RLS. Scouts, managers and supers each only see and change what their role allows.

## Developing

You need Node 22+ and, for a local database, Docker plus the Supabase CLI. 

You're also going to need access to the original repo, contact a captain for more information.

```bash
npm install
npm run dev
```

By default the dev server talks to the **production** database. To work against a **local** one instead:

```bash
supabase start
node scripts/seed_demo.mjs
```

Then create a `.env.local` with `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` from `supabase status`, and restart `npm run dev`. Delete `.env.local` before building; otherwise the build points at your laptop.

## Tests

```bash
npm test
```

This runs the unit tests plus two checkers: one for component rules (tap-target size, design tokens, nav labels) and one for colour contrast in every theme. `npm run test:rls` tests database permissions against a running local Supabase.

## Deploying

Pushing to `main` on the original repo initiates an automated Github workflow that migrates all necessary files to `frc-scouting-app-site`, this repo. The website is deployed to [Github Pages](https://scout.rohawks.org "visit our site") from here. 

Database changes go through migration files in `supabase/migrations/` and must reach production **before** any client code that depends on them.

## Contact

Report bugs by messaging a captain (for now!)

Questions: scouting@rohawks.org

Created by Haolun Zhang (haolunzhang@hunterschools.org)
