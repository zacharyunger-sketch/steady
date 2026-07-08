# Steady - Recovery Companion PWA

Everything is included. No build step, no dependencies.

```
steady/
├── index.html      # the whole app
├── manifest.json   # PWA install metadata
├── sw.js           # offline support (bump CACHE version after editing index.html)
├── icons/          # app icons (180/192/512)
├── CLAUDE.md       # project context for Claude Code
└── README.md
```

## 1. Open in VS Code

Open the `steady` folder in VS Code. To preview locally, either:

- Install the **Live Server** extension, right-click `index.html`, "Open with Live Server", or
- Terminal: `python3 -m http.server 8000`, open http://localhost:8000

(Don't just double-click the file - service workers need localhost or HTTPS.)

## 2. Connect git + GitHub

In the VS Code terminal, inside the `steady` folder:

```bash
git init
git add .
git commit -m "Steady v1 - recovery PWA"
```

Then create the GitHub repo. Easiest with GitHub CLI (`brew install gh`, then `gh auth login`):

```bash
gh repo create steady --private --source=. --push
```

No GitHub CLI? Create an empty repo named `steady` at github.com/new, then:

```bash
git remote add origin https://github.com/YOUR_USERNAME/steady.git
git branch -M main
git push -u origin main
```

**Note:** GitHub Pages on a *private* repo requires GitHub Pro. On a free account make the repo public - fine here, since the app contains no personal data (everything lives in localStorage on your phone, never in the code).

### Claude Code prompt for git (if you'd rather delegate)

> Initialize git in this folder, make an initial commit, create a GitHub repo called "steady" using the gh CLI, push to main, and enable GitHub Pages serving from the main branch root. Give me the final Pages URL.

## 3. Get it on your iPhone

1. Push to GitHub (step 2), then on github.com: repo, **Settings, Pages**, Source: "Deploy from a branch", Branch: `main`, folder `/ (root)`, Save
2. Wait ~1 minute, your app is live at `https://YOUR_USERNAME.github.io/steady/`
3. On your iPhone, open that URL **in Safari**
4. Tap **Share, Add to Home Screen, Add**
5. Launch from the home screen icon - it opens full-screen like a native app and works fully offline

**Alternative without GitHub:** drag the `steady` folder onto https://app.netlify.com/drop - instant HTTPS URL, same Add to Home Screen steps.

## Updating the app later

1. Edit `index.html` (or ask Claude Code to)
2. Bump the version in `sw.js`: `const CACHE = 'steady-v2';`
3. `git add . && git commit -m "describe change" && git push`
4. On your phone, open the app and pull-close/reopen once - the new version installs

## Your data

Everything stays in localStorage on your phone. Use **Settings, Export my data** inside the app for backups. Deleting the home-screen app or clearing Safari website data erases it - export first.
