# GitHub Pages Deployment (auto-configured)

This repository has been prepared for GitHub Pages deployment of the **client** (front-end).

What I changed:
- `client/vite.config.ts` base set to `./` for relative asset paths (works on GitHub Pages).
- Added GitHub Actions workflow `.github/workflows/gh-pages.yml` which builds the `client` and deploys `client/dist` to the `gh-pages` branch on pushes to `main`.

How to use:
1. Push this repository to GitHub (remote `origin`, branch `main`).
2. On push to `main`, GitHub Actions will build the client and publish `client/dist` to the `gh-pages` branch automatically.
3. Enable GitHub Pages in repository Settings → Pages (branch: `gh-pages`, folder: `/`), or use the GitHub Pages URL:
   `https://<USERNAME>.github.io/<REPOSITORY_NAME>/`

Notes:
- The server folder is a Node.js backend and **cannot** run on GitHub Pages. If your client depends on API endpoints, deploy the server separately (Render, Railway, Vercel, etc.) and update the client API URLs.
- If you prefer to manually deploy, run `cd client && npm install && npm run build` locally, then push the contents of `client/dist` to the `gh-pages` branch.
