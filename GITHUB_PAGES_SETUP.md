# GitHub Pages Setup — Checkpoint 3

## Option A: Username Site (per assignment instructions)

The assignment says to create a repo named `<username>.github.io`. This is the simplest option — GitHub auto-publishes the `main` branch's root as a website.

### Steps

1. **Create the repo**
   - Go to github.com → New repository
   - Name it exactly: `<your-github-username>.github.io`
   - Set to **Public**
   - Do NOT initialize with a README (you'll push from here)

2. **Add this project as the remote and push**
   ```bash
   cd /Users/nmaisheri/CMSC320_Final_Project

   git init
   git add .
   git commit -m "Initial checkpoint 3 skeleton"

   git remote add origin https://github.com/<YOUR-USERNAME>/<YOUR-USERNAME>.github.io.git
   git branch -M main
   git push -u origin main
   ```

3. **Convert the notebook to HTML before pushing**
   ```bash
   # Run this after you finish filling in the notebook:
   jupyter nbconvert --to html --execute checkpoint3_skeleton.ipynb --output index.html
   git add index.html
   git commit -m "Add final tutorial HTML export"
   git push
   ```

4. **Your site will be live at:**
   ```
   https://<YOUR-USERNAME>.github.io/
   ```
   It usually takes 1–2 minutes to go live after the first push.

---

## Option B: Project Site with GitHub Actions (automated, re-runs on every push)

If you already have a `<username>.github.io` repo for something else, use a separate project repo instead.

### Steps

1. Create a **new public repo** on GitHub (any name, e.g. `cmsc320-final`).

2. Push this directory to it:
   ```bash
   cd /Users/nmaisheri/CMSC320_Final_Project
   git init
   git add .
   git commit -m "Initial skeleton"
   git remote add origin https://github.com/<USERNAME>/cmsc320-final.git
   git branch -M main
   git push -u origin main
   ```

3. **Enable GitHub Actions Pages** in the repo:
   - Repo → Settings → Pages
   - Source: **GitHub Actions**
   - Save

4. The `.github/workflows/publish.yml` workflow included here will automatically:
   - Execute your notebook
   - Convert it to `index.html`
   - Deploy to Pages

5. **Your URL will be:**
   ```
   https://<USERNAME>.github.io/cmsc320-final/
   ```

---

## Important Notes

- `rows.json` is **not committed** — it's ~1 GB, far over GitHub's 100 MB file limit. The notebook downloads it directly from the NYC Open Data API on first run and caches it locally (see the load cell + `.gitignore`). The GitHub Actions workflow does the same download on every CI run before executing the notebook.

- **Rename the notebook** from `checkpoint3_skeleton.ipynb` to something descriptive before you submit. Update the `publish.yml` `--output index.html` line to match.

- **Test locally first:**
  ```bash
  jupyter nbconvert --to html --execute checkpoint3_skeleton.ipynb --output index.html
  open index.html
  ```

