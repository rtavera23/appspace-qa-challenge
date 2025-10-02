# Appspace QA Challenge

Manual & API testing for the Appspace challenge.

- **Part 1:** Manual test plan and bug reports for the registration form.
- **Part 2:** Postman collection + CLI (Newman) tests for the To-Do API.

---

## Repository Structure

```
.
├─ docs/
│  ├─ bug-report-template.md
│  ├─ bugs-found.md
│  └─ testcases.md
├─ postman/
│  ├─ Appspace – QA.postman_environment.json
│  ├─ Appspace – ToDo API.postman_collection.json
│  ├─ appspace postman issue.png
│  └─ bugs-found.md            
├─ rest/
│  └─ todo.http                # VS Code REST Client requests
└─ README.md
```

> Some filenames contain **spaces** and an **en dash (–)**. Wrap paths in **quotes** in terminal commands.

---

## Prerequisites

- **Node.js ≥ 16** and **npm**
- **Postman** (for GUI runs)
- **Newman** (Postman CLI) + **htmlextra** HTML reporter
  ```bash
  npm install -g newman newman-reporter-htmlextra
  ```
- **VS Code** and the **REST Client** extension (to run `rest/todo.http`)
  - VS Code → Extensions → search `humao.rest-client` → Install
- (Optional) **Glow** to render Markdown in terminal
  ```bash
  # macOS (Homebrew)
  brew install glow
  # Ubuntu/Debian (Snap)
  sudo snap install glow
  ```

---

## Quick Links

- Test cases: [`docs/testcases.md`](docs/testcases.md)  
- Bug list (Form): [`docs/bugs-found.md`](docs/bugs-found.md)  
- API bug (404 contract): [`postman/bugs-found.md`](postman/bugs-found.md)
- Postman **environment**: [`postman/Appspace – QA.postman_environment.json`](postman/Appspace%20–%20QA.postman_environment.json)  
- Postman **collection**: [`postman/Appspace – ToDo API.postman_collection.json`](postman/Appspace%20–%20ToDo%20API.postman_collection.json)

---

## Running the Postman Collection with Newman (CLI)

> Because filenames include spaces and an en dash, wrap paths in **quotes**.

### 1) Basic run
```bash
newman run "postman/Appspace – ToDo API.postman_collection.json"   -e "postman/Appspace – QA.postman_environment.json"
```

### 2) Run with HTML report
```bash
mkdir -p reports
newman run "postman/Appspace – ToDo API.postman_collection.json"   -e "postman/Appspace – QA.postman_environment.json"   --reporters cli,htmlextra   --reporter-htmlextra-export "reports/newman-report.html"
```

Open the report:
- macOS: `open reports/newman-report.html`  
- Linux: `xdg-open reports/newman-report.html`  
- Windows: `start reports\newman-report.html`

### 3) Run a single folder (e.g., “Negative”)
```bash
newman run "postman/Appspace – ToDo API.postman_collection.json"   -e "postman/Appspace – QA.postman_environment.json"   --folder Negative
```

---

## Running Requests with VS Code REST Client

There is a sample file at `rest/todo.http`.

1. Install **REST Client** (Huachao Mao) in VS Code.  
2. Open `rest/todo.http`.  
3. Click **“Send Request”** above any request block.  
4. Inspect the response panel (status, headers, body).

---

## Viewing Markdown Files

### VS Code (GUI)
- Open the repo: `code .`  
- Open any `.md` file and press:
  - macOS: **⌘⇧V**
  - Windows/Linux: **Ctrl+Shift+V**
- Split preview: **⌘K V** (or **Ctrl+K V**)

### Terminal
```bash
glow docs/testcases.md
glow docs/bugs-found.md
glow docs/bug-report-template.md
```

---

## Troubleshooting

- **“Cannot find reporter htmlextra”** → `npm install -g newman-reporter-htmlextra`
- **Paths with spaces or en dash (–)** → always wrap paths in quotes `"..."`.
- **Negative `GET /tasks/{id}` 404 test** → if a non-existing ID returns **200**, capture status/body and attach to `docs/BUG-20251002-API-001.md`.
- **macOS open command** → don’t include inline comments on the same line; run:
  ```bash
  open reports/newman-report.html
  ```

---

## Notes

This repository contains:
- **Part 1:** Manual test plan & bug reports for the registration form.
- **Part 2:** Postman collection and Newman runs for the To-Do API.
