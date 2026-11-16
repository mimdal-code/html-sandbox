# Project Name

A collection of simple HTML pages and mini-projects. Each project lives in its own subdirectory, with a single root `index.html` acting as a directory/landing page linking out to each one.

## 📁 Structure

```
/
├── index.html              # Landing page (auto-generated links section)
├── project-one/
│   └── index.html
├── project-two/
│   └── index.html
└── project-three/
    └── index.html
```

Each project is self-contained in its own folder. The root `index.html` contains a **links section** where entries are manually maintained:

| Field          | Description                                              |
| -------------- | -------------------------------------------------------- |
| **Link**       | Relative path to the project (e.g. `./project-one/`)     |
| **Query**      | Optional query string (e.g. `?mode=demo&theme=dark`)     |
| **Description**| A short manual note about what the project does          |

## ➕ Adding a New Project

1. Create a new subdirectory:
   ```bash
   mkdir my-new-project
   ```
2. Add your HTML (and any assets) inside it — entry point should be `index.html`:
   ```
   my-new-project/
   ├── index.html
   └── style.css
   ```
3. Add an entry to the links section in the root `index.html`:

   ```html
   <li>
     <a href="./my-new-project/?example=1">My New Project</a>
     <p>A short description of what this project does.</p>
   </li>
   ```

   Adjust the `href` to include any query parameters the project expects.

## 🔗 Query Parameters

Some projects require specific query parameters to render correctly (e.g. a demo mode, a preset theme, or a selected item). These are set explicitly in the link's `href` in the root `index.html`. If a project needs new parameters, simply edit its link entry.

## 🧭 Notes

- All links are **relative** — no build step, no server required beyond static hosting (GitHub Pages works out of the box).
- Descriptions are written manually; there is no auto-detection or metadata parsing.
- Keep each project's assets inside its own folder to avoid path collisions.

## 🚀 Usage

Open `index.html` in a browser, or serve the repository root with any static file server:

```bash
python3 -m http.server
# or
npx serve .
```

Then visit `http://localhost:8000`.

## 📄 License

MIT.