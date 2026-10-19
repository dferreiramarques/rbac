# RBAC Manager

A zero-dependency, no-build web tool to design and document **role-based access control (RBAC)**: define roles, register screens and features, assign permissions, and generate documentation as a matrix, table, diagram or PDF.

The workflow follows the steps described in IBM's guide to [implementing RBAC](https://www.ibm.com/think/topics/role-based-access-control-implementation): define roles and resources, assign permissions following least privilege, use role hierarchy, and enforce separation of duties.

<!-- Add a screenshot here: ![RBAC Manager](docs/screenshot.png) -->

## Features

| Step | Tab | What you can do |
|---|---|---|
| 1 | **Roles** | Create roles with name, description, scope (global or domain), identity type (human, service account, AI agent) and an optional parent role (hierarchy). Define mutually exclusive role pairs (separation of duties) with conflict warnings. |
| 2 | **Resources** | Register screens and features, group them by module and set a risk level (Low, Medium, High). |
| 3 | **Matrix** | Click to grant View, Create, Edit, Delete, Approve and Export on each resource per role. Assigned permissions are green, inherited ones are violet. |
| 4 | **Table** | Read-only list of effective permissions, filterable by role and module. |
| 5 | **Diagram** | SVG showing role inheritance and role-to-resource permissions, with high-risk resources highlighted. |
| 6 | **Export** | Markdown, CSV, Mermaid, SVG and JSON (backup/import), plus a PDF report (roles, permissions matrix, separation of duties). |

### Role inheritance

A child role receives every permission of its parent in addition to its own. Circular inheritance is blocked.

Do not link roles through inheritance when they must stay isolated. For example, if a "Reporting Manager" role should see consolidated data but not raw data, do not make it inherit from a "Data Analyst" role. Create a shared base role for what both need and add a separation-of-duties constraint between them.

### Projects

Each product or system is a project, shown as a tab above the steps (colour-coded; drag a tab to reorder it, right-click a tab or use **⋯** to rename, recolour, move, duplicate or delete it, **+** to add one). Projects are stored only in this browser's `localStorage`. **Load project from JSON file…** (in the New project dialog and in Export) adds a project from a file; a file exported with **JSON (all projects)** adds one tab per project.

## Usage

No build step and no server needed.

1. Download or clone the repository.
2. Open `index.html` (product page) or `app.html` (the app) in a modern browser.

### Deploy with GitHub Pages

1. Keep `index.html` (product page) and `app.html` (the app) in the repository root.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, then select your main branch and the `/ (root)` folder.
4. Your tool will be live at `https://<user>.github.io/<repo>/`.

## Data and privacy

- Everything runs in your browser. No data is sent to any server.
- Your projects are stored in the browser's `localStorage`, so it is private to that browser on that device.
- Use **Export → JSON** to back up your model or move it to another device, and **Import pasted JSON** to restore it.
- Clearing site data deletes your model. Export a backup first.

## PDF export

The PDF is generated client-side with [jsPDF](https://github.com/parallax/jsPDF) and the [jsPDF-AutoTable](https://github.com/simonbengtsson/jsPDF-AutoTable) plugin, loaded from cdnjs. It is an A4 landscape document with the roles table, the permissions matrix (including inherited permissions) and the separation-of-duties constraints. An internet connection is needed the first time so the libraries can load.

The diagram is not included in the PDF. Copy it from **Export → SVG** or **Export → Mermaid** instead.

## Tech

- Plain HTML, CSS and JavaScript: `index.html` is the product page, `app.html` the app
- `product-page.css` is the design system shared with the Roadmaps, Taskboards and Diagramico product pages (copy of the Roadmaps reference). RBAC only overrides the accent tokens (violet); Inter and JetBrains Mono are self-hosted in `assets/fonts`
- jsPDF 2.5.1 and jsPDF-AutoTable 3.8.2 (CDN)
- Light and dark theme follow the system setting until you pick one; dark/light only change colours

## Limitations

- Documents roles and permissions only. It does not assign real users to roles or enforce access.
- The six permission types (View, Create, Edit, Delete, Approve, Export) are fixed.
- Data lives in one browser. There is no sync or multi-user editing.

## License

MIT
