# Job Description Library

A simple, searchable web page for standardized Job Descriptions, filterable by
**Position**, **Business Unit** and **Location**. Everything lives on GitHub —
no other service required.

The manager just drops `.docx` files into the `jds/` folder; the page rebuilds
and republishes itself automatically.

---

## How it works

```
Manager uploads a .docx     GitHub runs the          Site is rebuilt and
   into /jds (web)     →     Action automatically  →  republished (~1 min)
                              (build.py → data.js)
```

| File / folder        | What it is                                                        |
| -------------------- | ----------------------------------------------------------------- |
| `index.html`         | The page itself (visual). Rarely needs editing.                   |
| `jds/`               | The Job Description source files. **This is what the manager updates.** |
| `build.py`           | Reads the `.docx` files and generates `data.js`. Runs automatically. |
| `data.js`            | Auto-generated data the page reads. Never edited by hand.         |
| `.github/workflows/` | The automation (GitHub Action) that rebuilds + publishes.         |

---

## One-time setup

1. **Create a GitHub account** (if you don't have one) at <https://github.com>.
2. **Create a new repository** — give it a name like `jd-library`. It can be
   public or private (Pages works with both).
3. **Upload these files** to the repository: on the repo page click
   *Add file → Upload files*, drag in everything from this folder
   (including the `jds/` folder), and click *Commit changes*.
4. **Turn on GitHub Pages:** go to *Settings → Pages → Build and deployment →
   Source* and choose **GitHub Actions**.
5. Wait ~1 minute. Your live link appears under *Settings → Pages* — something like
   `https://<your-username>.github.io/jd-library/`.

That's it. The site is live and will update itself from now on.

---

## Day-to-day: adding or updating a Job Description

No code, no commands — just the GitHub website:

1. Open the repository and click into the **`jds`** folder.
2. Click **Add file → Upload files**.
3. Drag in the new/updated `.docx` file(s) and click **Commit changes**.
4. Wait ~1 minute — the page updates automatically. Done.

The filename does **not** matter: Position, Business Unit and Location are read
from inside the document's *Position Overview* section, so the data is always
correct even if a file is named differently.

To **remove** a JD: open the `jds` folder, click the file, then the trash icon →
*Commit changes*.

---

## Template requirements

`build.py` expects the Ant International JD template (v2.x). For a file to be
read correctly it needs:

- A **Position Overview** table with the rows *Job Title*, *Department*,
  *Reports To*, *Location*, *Employment Type*.
- The standard section headings: *About \<Business Unit\>*, *About the Role*,
  *Key Responsibilities*, *Qualifications & Experience* (with *Required* /
  *Preferred*).

A file that doesn't match is simply skipped (the others still publish).

> **Note:** the builder reads `.docx` files. If the source files are PDFs,
> save/export them as `.docx` first (or ask the developer to add PDF support).

---

## Running locally (for developers)

```bash
pip install python-docx
python build.py        # regenerates data.js from /jds
open index.html        # view the page
```
