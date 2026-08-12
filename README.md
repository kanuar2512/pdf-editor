# PDF Editor

A PDF editor that runs entirely in your browser. Fill in forms, write on flat or
scanned documents, merge several PDFs, reorder and rotate pages, and pull pages
out into a new file.

**No installation, no server, no upload.** It is one HTML file. Your documents
never leave your computer.

👉 **[Open the editor](https://kanuar2512.github.io/pdf-editor/)**

---

## What it does

### Filling forms

* Detects interactive **AcroForm** fields automatically — text boxes, multi-line
  boxes, checkboxes, radio buttons and dropdowns. Existing values are read in.
* Fill either **straight on the page** or from the **Fields panel**, which is
  handy when fields are tiny or unlabelled. The two stay in sync.
* **Highlight fields** tints every fillable box so nothing gets missed.

### Flat and scanned documents

Not every PDF has real fields. For those:

| Tool | Use |
|---|---|
| **Text** | Click anywhere and type |
| **✔ / ✘** | Drop a tick or a cross into a box |
| **White‑out** | Drag a white rectangle over something, then type over it |
| **Signature** | Draw with the mouse, trackpad or finger, then place it |
| **Image** | Insert a PNG or JPEG (stamp, letterhead, scanned signature) |

Drag items to move, use the corner grip to resize, arrow keys to nudge
(hold <kbd>Shift</kbd> for bigger steps) and <kbd>Delete</kbd> to remove.

### Merging and organising pages

The **Pages** panel is a page organiser:

* **+ Add PDF** — merge one or more further PDFs into the document
* **Drag thumbnails** to reorder pages, across files as well as within one
* **⟲ ⟳** rotate — annotations already placed rotate with the page
* **Delete** unwanted pages
* **Extract** — tick some pages and save just those as a new PDF (split)

Nothing is re-encoded while you work. The document is only rebuilt when you save,
so reordering a 200-page merge is instant.

### Saving

**Save PDF** writes the file to your downloads folder.

**Flatten** (checkbox in the toolbar) bakes the values into the page so they can
no longer be edited — use it when sending a completed form to someone else, and
whenever you merge two PDFs that both contain form fields.

---

## Running it

**Online:** open the GitHub Pages link above.

**Offline / locally:** download `index.html` and double-click it. It needs an
internet connection the first time so the browser can fetch pdf.js and pdf-lib
from a CDN; after that the browser cache normally covers it. To make it fully
offline, download the two library files and point the `<script src="…">` tags at
local copies.

Tested in Chrome and Edge. Firefox and Safari work too.

---

## Try it

Two sample files are in [`samples/`](samples/):

* `sample-form.pdf` — has interactive fields, for testing form filling
* `sample-flat.pdf` — no fields at all, for testing the drawing tools

---

## Deploying your own copy

1. Push this folder to a GitHub repository.
2. **Settings → Pages → Build and deployment → Source: GitHub Actions.**
3. The workflow in `.github/workflows/pages.yml` publishes the site on every
   push to `main`.

Updating later, once the remote is set up:

```bash
git add .
git commit -m "describe the change"
git push
```

Prefer no workflow? Choose **Deploy from a branch → `main` → `/ (root)`** instead
and delete the workflow file — `index.html` at the repository root is all Pages
needs.

---

## How it works

| Library | Job |
|---|---|
| [pdf.js](https://mozilla.github.io/pdf.js/) | Renders pages to canvas and reads the form-field definitions |
| [pdf-lib](https://pdf-lib.js.org/) | Writes the finished PDF: field values, drawn annotations, page assembly |

Internally the app keeps a list of *source documents* plus an *order* of page
references. Merging appends references; reordering moves them. On save each
source is reloaded from its original bytes, its field values are written in and
turned into appearance streams, the pages are copied into a fresh document in
the chosen order, and the annotations are drawn on top. Because the values are
baked before the copy, they survive the merge even in viewers that ignore the
rebuilt AcroForm.

### Known limits

* Text uses the PDF standard fonts (WinAnsi), so characters outside Western
  European — Chinese, Japanese, Arabic, Thai — are written as `?`. Supporting
  them means embedding a font file with `fontkit`.
* Existing text in a PDF cannot be re-typed. White‑out plus a text box is the
  workaround, and is how most simple PDF editors handle it.
* Encrypted PDFs open read-only where the encryption allows it; password-protected
  files will not open.
* Merging two PDFs that use the same field names renames the duplicates
  (`nama` → `nama_2`). Ticking **Flatten** avoids the issue entirely.

---

## Licence

MIT — see [LICENSE](LICENSE).
