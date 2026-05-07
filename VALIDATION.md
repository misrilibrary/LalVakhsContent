# Content Validation

This repo uses a GitHub Actions workflow to validate content files on every push.

## What Gets Checked

| Check | What it catches | Can be skipped? |
|-------|----------------|-----------------|
| **Gzip integrity** | Corrupted or non-gzip files | No (always runs) |
| **Entry count match** | Mismatched entries between topic, title, subtitle, and body files | Yes |
| **File size change >50%** | Accidental truncation, wrong file pushed, or unexpected bulk changes | Yes |

## How It Works

- Runs automatically on every `git push` — no manual steps
- Results appear as ✅ or ❌ next to commits on GitHub
- If a check fails, you'll get an email notification from GitHub

## Overriding Secondary Checks

If the entry count or file size check fails intentionally (e.g., bulk content update), add `[skip-size-check]` to your commit message:

```bash
git commit -m "Bulk content update [skip-size-check]"
git push origin main
```

The gzip integrity check **cannot** be skipped — if a file isn't valid gzip, the app won't be able to read it.

## Updating Size Baselines

If content grows significantly over time, the file size check may start failing on normal updates. To update the baselines:

1. Open `.github/workflows/validate.yml`
2. Update the `expected_sizes` values in the "Check for drastic file size changes" step
3. Commit with the new sizes

Current baselines:

| File | Size |
|------|------|
| `custom_messaging.txt` | 52 B |
| `detail_page_body.txt` | 260,903 B |
| `detail_page_subtitle.txt` | 23,371 B |
| `detail_page_title.txt` | 25,048 B |
| `topic.txt` | 6,146 B |
| `youtubeUrlsHD.txt` | 3,277 B |
| `youtubeUrlsHdEnglish.txt` | 4,145 B |
| `youtubeUrlsHdKashmiri-English.txt` | 4,156 B |
| `recommended_topics_v2.txt` | 330 B |
