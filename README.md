# Job Description Library

A simple, searchable web page for standardized Job Descriptions, filterable by
**Position**, **Business Unit** and **Location**.

**Live site:** https://ant-jd-repository.github.io/

Job Descriptions are stored as `.docx` files in the **`jds/`** folder of this
repository. Whenever a file is added, updated, or removed there, the site
rebuilds and republishes itself automatically.

> **Why GitHub (and not Dropbox/Drive)?** The company environment blocks uploads
> to cloud-storage services (Dropbox, Google Drive, OneDrive) at the device
> level — on any network. GitHub uploads are allowed, so GitHub is the reliable
> channel for getting files in.

---

## How it works

```
Add a .docx to the           GitHub Action reads the         Site rebuilt and
jds/ folder on GitHub   →     template (build.py -> data.js)  →   republished
                                                                  (~1 min)
```

| Piece                | What it is                                                           |
| -------------------- | -------------------------------------------------------------------- |
| `jds/` folder        | The `.docx` files — **this is what gets updated.**                  |
| `index.html`         | The page itself (visual). Rarely needs editing.                     |
| `build.py`           | Reads the `.docx` files and generates `data.js`. Runs automatically.|
| `data.js`            | Auto-generated data the page reads. Never edited by hand.           |
| `.github/workflows/` | The automation (GitHub Action) that builds and deploys.             |

---

## Day-to-day: adding or updating a Job Description

You need a GitHub account with write access to this repository (ask the owner to
invite you to the `Ant-JD-Repository` organization).

**Add or update a JD**

1. Open the upload page (bookmark this):
   **https://github.com/Ant-JD-Repository/Ant-JD-Repository.github.io/upload/main/jds**
2. **Drag the `.docx` file** onto the page.
3. Scroll down and click **Commit changes**.
4. Wait ~1 minute, then open the site and reload with **Cmd+R**.

**Remove a JD**

1. Open the file in `jds/` on GitHub, click the **trash** icon, then **Commit
   changes**. It disappears from the site on the next build.

The filename does **not** matter: Position, Business Unit and Location are read
from inside the document's *Position Overview* section.

> On Safari, reload with **Cmd+R** — *not* Cmd+Shift+R (that toggles Reader
> view, which strips the styling).

---

## Check the build / force an update

- **Actions tab:** https://github.com/Ant-JD-Repository/Ant-JD-Repository.github.io/actions
- A commit (file upload) triggers a build automatically. You can also run it
  manually: open the workflow **"Build & Publish JD Library"** → **Run workflow**.

---

## Template requirements

`build.py` expects the Ant International JD template (v2.x). A file needs:

- A **Position Overview** table with the rows *Job Title*, *Department*,
  *Reports To*, *Location*, *Employment Type*.
- The standard section headings: *About \<Business Unit\>*, *About the Role*,
  *Key Responsibilities*, *Qualifications & Experience* (with *Required* /
  *Preferred*).

A file that doesn't match is simply skipped (the others still publish). The
builder reads `.docx`; PDFs must be saved as `.docx` first.

---

## Run locally (maintainers)

```bash
pip install python-docx
python build.py        # reads jds/, writes data.js
open index.html        # or just double-click it
```
