# Dinner_Rater: project notes for Claude Code

This file loads automatically at the start of every Claude Code session in this repo.
`README.md` is the complete feature and schema reference. `replit.md` describes the Replit
runtime. `HANDOFF.md` has the history and open items.

## What this is
"**Himpele Family Favorites**": a family app for rating restaurants and dishes, saving recipes with
photos, and seeing every restaurant on a map. Flask 3 + SQLAlchemy 2 + Jinja2 templates, SQLite in
development and PostgreSQL in production, Leaflet.js map with OpenStreetMap tiles, geocoding
through the Nominatim API. Hosted on **Replit** (VM deployment running gunicorn).

## Where things live
- Everything is under **`Restaurant Rater/`** (folder name has a space; quote it in shells).
  `app.py` is the whole backend: models, routes, image handling, geocoding.
- `Restaurant Rater/templates/`: `base.html`, `index.html` (home with map), `restaurants.html`,
  `restaurant_detail.html`, `add_restaurant.html`, `edit_restaurant.html`, `recipes.html`,
  `recipe_detail.html`, `add_recipe.html`, `edit_recipe.html`. `static/style.css` for styling.
- Routes: `/`, `/restaurants`, `/recipes`, `/add_restaurant`, `/restaurant/<id>`,
  `/edit_restaurant/<id>`, `/delete_restaurant/<id>`, `/add_recipe`, `/recipe/<id>`,
  `/edit_recipe/<id>`, `/delete_recipe/<id>`, `/delete_recipe_photo/<photo_id>`, `/healthz`.
- Images are stored **twice**: as a file under `static/uploads/` and as Base64 in the database
  (`image_data`), so they survive path changes. Deleting a restaurant or recipe cascades.
- Overall restaurant rating auto-calculates from the food-item ratings unless a manual rating is entered.
- Several SQLite files are committed (`restaurant_rater.db`, `restaurant_rater (copy).db`,
  `restaurant_rater_prod.db`, `restaurants.db`). They are dev snapshots; **production data is in
  PostgreSQL** (`DATABASE_URL`) and is not in the repo.

## Deploy and run
- GitHub `Bhimpele81/Dinner_Rater`. Work happened on **`staging`** and was merged to **`main`** via
  pull requests (three merges so far). Keep that pattern unless Bill says otherwise.
- Replit runs `gunicorn --bind=0.0.0.0:5000 --reuse-port --chdir="Restaurant Rater" app:app`; the
  Replit workflow pulls from GitHub. Health check `/healthz`.
- Local: `pip install -r "Restaurant Rater/requirements.txt"`, then `cd "Restaurant Rater" && python app.py`
  (SQLite database is created on first run at `Restaurant Rater/restaurant_rater.db`).

## Gotchas
- Dates are stored as strings (`visit_date`); several commits fixed display formatting. Do not switch
  the column type casually; existing rows are strings.
- Geocoding runs on save from city/state (not street address) and uses Nominatim, which rate-limits.
  Include a User-Agent and expect occasional misses; `latitude`/`longitude` may be null.
- Images use `object-fit: contain` (changed from `cover` at Bill's request). Leave it.

## Standing rules from Bill (follow without being asked)
- **Never use em dashes** anywhere (UI text, README, comments, commits). Use commas, colons,
  parentheses, or separate sentences. The README title has one; fix it only when already editing the README.
- **Do not change layout or formatting** beyond what was asked.
- Keep `README.md` in sync when a feature ships.
- **Never use the word "corpus."**
- Commit messages end with `Co-Authored-By: Claude <noreply@anthropic.com>`.

## Machines
- Windows: `C:\Users\bhimpele\Desktop\GitHub\Dinner_Rater`. Mac: `/Users/billhimpele/Documents/GitHub/Dinner_Rater`.
- **Open Claude Code directly on this folder** so its history lands here.
