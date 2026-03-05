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

## Local preview

Open `index.html` in a browser, or run:
```
npx serve .
```

## Debugging

**Reset the language popup** (forces it to reappear on next page load):
```js
localStorage.removeItem('manifest-lang'); location.reload();
```
Run this in the browser DevTools Console (F12 → Console tab).
