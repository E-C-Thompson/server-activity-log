# Server Activity Log

A static site for the mod team showing channel/thread message activity —
built to be hosted for free on GitHub Pages.

## What's in here

```
discord-stats-site/
  index.html                        <- the whole site (one file)
  data/
    channel_stats_summary.csv       <- bundled with real data already
    channel_stats_nested.csv        <- add this (see below) for the
                                        "Channels & Threads" tab
    channel_stats_monthly.csv       <- add this for the "Activity Over
                                        Time" trend chart
```

The site reads its data from the CSV files in `/data` at page load time —
nothing is hardcoded into the HTML. That means updating the site later is
just: re-run your stats script, copy the new CSVs into `/data`, and push.
No code changes needed.

## Putting this on GitHub Pages (one-time setup)

1. Go to github.com and create a new repository (can be private if you only
   want the mod team to see it — GitHub Pages works with private repos too,
   just note viewers will need repo access; use public if the goal is a
   share-with-anyone link).
2. Upload this whole `discord-stats-site` folder's contents to that
   repository (via the GitHub web upload button, GitHub Desktop, or `git
   push` if you're comfortable with git — all work the same).
3. In the repo, go to **Settings → Pages**.
4. Under "Build and deployment", set Source to **Deploy from a branch**,
   branch = `main` (or whichever branch you pushed to), folder = `/ (root)`.
5. Save. GitHub will give you a URL like
   `https://yourusername.github.io/discord-stats-site/` — that's the link
   to share with the mod team. It can take a minute or two to go live the
   first time.

## Keeping the data current

Whenever you want to refresh the numbers:

1. Re-run `discord_channel_stats.py` (or just `rebuild_stats_from_raw.py`
   if you already have an up-to-date `channel_stats_raw.csv`).
2. Run `nested_channel_summary.py` against the raw file to regenerate
   `channel_stats_nested.csv`.
3. Copy the fresh `channel_stats_summary.csv`, `channel_stats_monthly.csv`,
   and `channel_stats_nested.csv` into this folder's `/data` directory,
   overwriting the old ones.
4. Push the change to GitHub. The live site updates automatically once
   Pages rebuilds (usually under a minute).

## What each tab shows

- **Overview** — total messages logged, busiest/quietest channel, a top-20
  bar chart, and a full sortable/searchable table of every channel and
  thread.
- **Channels & Threads** — the nested view where thread activity (like a
  long-running counting thread) is shown rolled into its parent channel's
  total, so you can see a channel's *real* activity level. Needs
  `channel_stats_nested.csv` to be present.
- **Activity Over Time** — a month-by-month line chart of total server
  activity (needs `channel_stats_monthly.csv`), plus a channel lifespan
  view (first message → last message) that works off the summary file
  alone, so it's there even before you add monthly data.

## Testing locally before pushing

Don't just double-click `index.html` — browsers block the CSV-loading step
for files opened directly from disk. Instead, open a terminal in this
folder and run:

```
python -m http.server 8000
```

Then visit `http://localhost:8000` in your browser. This is exactly how
GitHub Pages serves it too, so if it works here, it'll work live.
