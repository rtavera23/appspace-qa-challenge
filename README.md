# Appspace QA Challenge

Manual & API testing for the Appspace challenge.

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

````

> Note: some filenames contain **spaces** and an **en dash (–)**. Use quotes in your terminal commands.

---

## Prerequisites

- **Node.js ≥ 16** and **npm**
- **Newman** (Postman CLI) + **htmlextra** HTML reporter
  ```bash
  npm install -g newman newman-reporter-htmlextra
````

* (Optional) **Glow** to render Markdown in terminal

  ```bash
  # macOS (Homebrew)
  brew install glow
  # Ubuntu/Debian (Snap)
  sudo snap install glow
  ```

---

## Viewing Markdown files

### VS Code (GUI)

* Open the repo: `code .`
* Open any `.md` file and press:

  * macOS: **⌘⇧V**
  * Windows/Linux: **Ctrl+Shift+V**
* Split preview: **⌘K V** (or **Ctrl+K V**)

### Terminal (no GUI)

```bash
glow docs/testcases.md
glow docs/bugs-found.md
glow docs/bug-report-template.md
```

(Use `bat` or `cat` if you prefer.)

---

## Run the Postman collection with **Newman**

> Because filenames include spaces and an en dash, wrap paths in **quotes**.

### 1) Basic run (CLI)

```bash
newman run "postman/Appspace – ToDo API.postman_collection.json" \
  -e "postman/Appspace – QA.postman_environment.json"
```

### 2) Run with **HTML report**

```bash
mkdir -p reports
newman run "postman/Appspace – ToDo API.postman_collection.json" \
  -e "postman/Appspace – QA.postman_environment.json" \
  --reporters cli,htmlextra \
  --reporter-htmlextra-export reports/newman-report.html
```

Open the report:

* macOS: `open reports/newman-report.html`
* Linux: `xdg-open reports/newman-report.html`
* Windows: `start reports\newman-report.html`

### 3) Run a single **folder** (e.g., “Negative”)

```bash
newman run "postman/Appspace – ToDo API.postman_collection.json" \
  -e "postman/Appspace – QA.postman_environment.json" \
  --folder Negative
```

---

## Run requests with **VS Code REST Client**

There is a sample file at `rest/todo.http`.

Requirements: VS Code extension **REST Client** (`humao.rest-client`).

Steps:

1. Open `rest/todo.http` in VS Code.
2. Click **“Send Request”** above the request you want to run.
3. Inspect the response panel (status, headers, body).

> Tip: duplicate the file and parameterize `baseUrl` using REST Client variables if you prefer.

---

## Evidence & Bug Reports

* Keep screenshots (e.g., `postman/appspace postman issue.png`) in `postman/` or `docs/`.
* Use `docs/bug-report-template.md` for new tickets.
* Summarize findings in `docs/bugs-found.md`.

---

## Troubleshooting

* **“Cannot find reporter htmlextra”**
  Install globally: `npm install -g newman-reporter-htmlextra`
* **Paths with spaces or en dash (–)**
  Always wrap the path in quotes `"..."`.
* **Negative `GET /tasks/{id}` 404 test**
  If a non-existing ID does not return **404**, capture the actual status/body and attach to `docs/BUG-20251002-API-001.md`.

---

## Useful one-liners

```bash
# Full run + HTML report
newman run "postman/Appspace – ToDo API.postman_collection.json" \
  -e "postman/Appspace – QA.postman_environment.json" \
  --reporters cli,htmlextra \
  --reporter-htmlextra-export reports/newman-report.html

# Run only Smoke tests
newman run "postman/Appspace – ToDo API.postman_collection.json" \
  -e "postman/Appspace – QA.postman_environment.json" \
  --folder Smoke

# Preview Markdown in terminal
glow docs/bugs-found.md
```

---

### Required VS Code Extension

To run `rest/todo.http` requests:

- Install **REST Client** by Huachao Mao  
  - Open VS Code → Extensions → search `humao.rest-client` → Install.

Then open `rest/todo.http` and click **“Send Request”** above any block.

### Notes for Terminal Use

- When opening the Newman HTML report on macOS, run **without** inline comments:
  ```bash
  open reports/newman-report.html

---

## Notes

This repository contains:

* **Part 1:** Manual test plan and bug reports for the registration form.
* **Part 2:** Postman collection and CLI runs (Newman) for the To-Do API.

```
```
