# Deploying the portfolio

The site is hosted on **GitHub Pages** via the repo `yannuli/yannuli.github.io`.
Pushing to `main` is all it takes — GitHub Pages deploys automatically, usually within 1–2 minutes.

## Step-by-step

```bash
# 1. Stage the files you changed
git add <file1> <file2> ...

# 2. Commit
git commit -m "Short description of what changed"

# 3. Push
git push origin main
```

## What to stage

| File type | Include? |
|---|---|
| `index.html`, case study pages, about page | Yes |
| `assets/` images | Yes |
| `fonts/`, `styles/` | Yes |
| `.claude/` | No — internal Claude Code files, never commit |
| `*.jsx` scratch files | Only if intentionally part of the site |

## Live URL

`https://yannuli.github.io`

Changes are live once the GitHub Pages build finishes (check the **Actions** tab on the repo if you're unsure).
