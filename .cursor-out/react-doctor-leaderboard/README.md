# Leaderboard patch for `millionco/react-doctor`

This branch is a temporary holder. The cloud agent that produced it was scoped
to `millionco/react-doctor-benchmarks` and could not push directly to
`millionco/react-doctor`, so the work lives here as a patch series.

## What it does

1. Adds `scripts/sync-leaderboard.ts` to `react-doctor`. The script fetches
   `https://raw.githubusercontent.com/millionco/react-doctor-benchmarks/main/results/leaderboard.json`
   and:
   - writes the full JSON to
     `packages/website/src/app/leaderboard/leaderboard-data.json`
   - splices a top-10 markdown table into
     `packages/react-doctor/README.md` between
     `<!-- LEADERBOARD:START -->` and `<!-- LEADERBOARD:END -->`
2. Adds the README markers + surrounding `## Leaderboard` section.
3. Adds `packages/website/src/app/leaderboard/page.tsx`, restoring the
   `react.doctor/leaderboard` page (matches the existing `/share` page style).
4. Adds `.github/workflows/refresh-leaderboard.yml`, a daily
   (plus manual + repository_dispatch) workflow that re-runs the sync and
   commits when anything changed.

Tested locally against the benchmarks repo's current `leaderboard.json`:
`pnpm typecheck`, `pnpm lint`, `pnpm format:check`, `pnpm test`, and
`pnpm --filter website build` all pass.

## How to apply

```bash
git clone https://github.com/millionco/react-doctor.git
cd react-doctor
git checkout -b cursor/leaderboard-from-benchmarks
git am /path/to/0001-leaderboard.patch
git push -u origin cursor/leaderboard-from-benchmarks
```

The patch is a `git format-patch` series with five logical commits:

1. `feat(scripts): sync leaderboard data from react-doctor-benchmarks`
2. `docs(react-doctor): add leaderboard section synced via CI`
3. `feat(website): add /leaderboard page`
4. `ci: refresh leaderboard from react-doctor-benchmarks`
5. `chore: drop bench:scores in favor of the synced leaderboard`

After pushing, open a PR against `millionco/react-doctor:main`.
