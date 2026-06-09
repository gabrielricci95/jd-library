# Job Description Library

A simple, searchable web page for standardized Job Descriptions, filterable by
**Position**, **Business Unit** and **Location**.

**Live site:** https://ant-jd-repository.github.io/

The manager just drops `.docx` files into a **Dropbox** folder; the page
rebuilds and republishes itself automatically.

---

## How it works

```
Manager adds a .docx          GitHub pulls from Dropbox          Site rebuilt and
to the Dropbox folder    →    (rclone) + reads the template  →   republished
"Job Descriptions"            (build.py -> data.js)              (~1 min)
```

| Piece                | What it is                                                              |
| -------------------- | ----------------------------------------------------------------------- |
| Dropbox folder       | `Job Descriptions` — **this is what the manager updates.**              |
| `index.html`         | The page itself (visual). Rarely needs editing.                         |
| `build.py`           | Reads the `.docx` files and generates `data.js`. Runs automatically.    |
| `data.js`            | Auto-generated data the page reads. Never edited by hand.               |
| `.github/workflows/` | The automation (GitHub Action) that pulls from Dropbox, builds, deploys.|

The Action runs **every hour** automatically, and can also be **triggered on
demand** (see below) so you don't have to wait for the hourly cycle.

---

## Day-to-day: adding or updating a Job Description (for the manager)

1. Open the shared **Dropbox** folder **`Job Descriptions`**.
2. Add (or replace) the `.docx` file(s) there — drag & drop on the Dropbox
   website, or into the Dropbox folder on your computer.
3. Within ~1 hour the site updates automatically — or trigger it now (below).

To **remove** a JD: delete the file from the Dropbox folder. It disappears from
the site on the next update.

The filename does **not** matter: Position, Business Unit and Location are read
from inside the document's *Position Overview* section.

---

## Update now (skip the 1-hour wait)

1. Go to the **Actions** tab:
   https://github.com/Ant-JD-Repository/Ant-JD-Repository.github.io/actions
2. Click the workflow **“Build & Publish JD Library”** (left side).
3. Click **Run workflow** → **Run workflow** (green button).
4. Wait ~1 minute (it turns green), then reload the site with **Cmd+R**.

> On Safari, use **Cmd+R** to reload — *not* Cmd+Shift+R (that toggles Reader
> view, which strips the styling).

---

## Template requirements

`build.py` expects the Ant International JD template (v2.x). A file needs:

- A **Position Overview** table with the rows *Job Title*, *Department*,
  *Reports To*, *Location*, *Employment Type*.
- The standard section headings: *About \<Business Unit\>*, *About the Role*,
  *Key Responsibilities*, *Qualifications & Experience* (with *Required* /
  *Preferred*).

A file that doesn't match is simply skipped (the others still publish). The
builder reads `.docx`; PDFs would need to be saved as `.docx` first.

---

## Maintainer notes (technical)

- **Source of truth:** the Dropbox folder `Job Descriptions`. The `jds/` folder
  in this repo is just a sample and is overwritten from Dropbox on each run.
- **Dropbox access:** stored as the encrypted Actions secret `RCLONE_CONF`
  (read-only). Regenerate with `rclone authorize "dropbox"` if it ever needs
  rotating, then `gh secret set RCLONE_CONF`.
- **Safety guard:** if the Dropbox folder is empty, the deploy is skipped so the
  live site is not wiped.
- **Run locally:** `pip install python-docx` then `python build.py` (uses the
  files in `jds/`), then open `index.html`.
