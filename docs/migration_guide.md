# Migration Guide — Add Governance Hubs without touching `_posts/`

_Last updated: 2026-03-03

## Goal
Add a governance-first information architecture around existing posts (no renames, no moves, no rewrites).

## What this pack adds
- `/governance/` hub pages (Jekyll pages)
- reusable includes (`_includes/tealtiger-lens.html`, `_includes/governance-callout.html`)
- editorial/corrections policy docs

## Steps
1. Unzip this pack into the root of your blog repository.
2. Commit the new folders: `governance/` and `docs/` and new includes.
3. Update `index.md` (optional) to link to `/governance/` as the new “Start here”.
4. (Optional) Add this near the top of older posts:

```liquid
{% include governance-callout.html %}
```

5. (Optional) Add this at the bottom of posts where TealTiger fits naturally:

```liquid
{% include tealtiger-lens.html %}
```

## Notes about links
This pack assumes standard Jekyll permalink output for posts: `/YYYY/MM/DD/slug.html`.
If your `_config.yml` changes permalinks, update links in hub pages accordingly.

