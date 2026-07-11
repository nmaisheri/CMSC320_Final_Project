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

- `rows.json` **cannot be committed** if it is large (GitHub hard-limits files at 100 MB). Options:
  - Download it at notebook runtime via the Socrata API (see commented code block below).
  - Use [Git LFS](https://git-lfs.com/) if the file is under ~500 MB.
  - Host it on a public Google Drive / Dropbox and download with `gdown` in the notebook.

- **Rename the notebook** from `checkpoint3_skeleton.ipynb` to something descriptive before you submit. Update the `publish.yml` `--output index.html` line to match.

- **Test locally first:**
  ```bash
  jupyter nbconvert --to html --execute checkpoint3_skeleton.ipynb --output index.html
  open index.html
  ```

## Socrata API download (avoids committing a large JSON file)

Add this at the top of your notebook's data-loading cell instead of reading from a local file:

```python
import urllib.request, json

API_URL = "https://data.cityofnewyork.us/resource/h9gi-nx95.json?$limit=500000"
with urllib.request.urlopen(API_URL) as response:
    records = json.load(response)
df = pd.DataFrame(records)
```

Adjust `$limit` as needed (max is 1,000,000 per query without an app token).
