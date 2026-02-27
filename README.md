# Manifest — Humanoids for European Defence

Single-page marketing website for Manifest.

## Structure

```
├── index.html            # Main page
├── assets/
│   ├── images/           # Logo, partner logos, team photos
│   └── video/
│       └── hero.mp4      # Hero background video
├── vercel.json           # Cache & hosting config
└── .gitignore
```

## Deploy to Vercel

1. Push this repo to GitHub
2. Go to [vercel.com](https://vercel.com) → New Project → Import from GitHub
3. Select this repo — Vercel auto-detects static HTML, no build step needed
4. Deploy

## Local preview

Open `index.html` in a browser, or run:
```
npx serve .
```
