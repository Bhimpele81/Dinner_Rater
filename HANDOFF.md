# Handoff: Dinner_Rater (Himpele Family Favorites)

Read `CLAUDE.md` first (it loads automatically). `README.md` is the full feature and schema
reference and is current as of the last commit. Last updated: 2026-09-05.

## Why the Claude history for this project looked empty
This app was built largely on **Replit** (its agent wrote `replit.md`) and then maintained through
Claude Code sessions that were opened from other folders, so no transcripts were filed under this
project. Nothing is missing that the README does not cover. Open Claude Code directly on
`Dinner_Rater` from now on.

## State of the app
Complete and stable. Last commit: April 10, 2026 (comprehensive README rewrite). Working tree is
clean and matches `origin/main`. No known bugs.

## How it evolved (what shipped and why)
1. **Restaurant tracking**: name, cuisine category, description, dishes tried, attendees, visit date,
   photo. Individual **food item ratings (1 to 10)** with add/remove rows; the overall rating
   auto-averages them unless a manual rating is given.
2. **Recipes**: name, single description field for ingredients and instructions, rating, **multiple
   photos** with a responsive grid, delete individual photos from the edit page, delete recipe from
   the detail page.
3. **Map**: city, state, latitude, longitude columns were added; the home page shows every geocoded
   restaurant on a Leaflet map with popups linking to detail pages. Geocoding is by city/state via
   Nominatim at save time.
4. **Search** across restaurants (including food item names) and recipes, case-insensitive.
5. **Image handling**: dual storage (file plus Base64 in the DB) so images survive redeploys and path
   changes; filenames sanitized; display switched from `cover` to `contain` so photos are not cropped.
6. **Date display** fixes in three templates (visit dates are strings; formatting was normalized).
7. **Styling**: button classes unified, recipe cover images styled, favicon added to `base.html`.
8. Work flowed on `staging` and was merged to `main` through PRs #1, #2, #3.

## Operations
- Replit VM deployment (always on) running gunicorn from the `Restaurant Rater` folder.
  Production DB is PostgreSQL through `DATABASE_URL` with connection pooling
  (`pool_pre_ping`, `pool_recycle=3600`). Uploaded images persist under `static/uploads/`.
- Development uses SQLite; dev and prod data are completely separate. Publishing does not touch prod data.
- Health check: `/healthz`.

## Open items
- None requested. Candidates if Bill asks: a `.gitignore` so the committed `.db` snapshots and
  `uploads` stop changing; a home-page filter by cuisine; export of ratings to CSV.
- README title contains an em dash; replace with a colon on the next README edit.
