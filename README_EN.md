[简体中文](README.md) | English

# BXI Robotics Wiki

Developer documentation for BXI Robotics, built with **MkDocs Material**, supporting Chinese and English, and automatically deployed via GitHub Actions.

- Live site: [wiki.bxirobotics.cn](https://wiki.bxirobotics.cn)

---

## Deployment

This repository uses a GitHub Actions pipeline — **no local `mkdocs build` required**.

1. Edit or create Markdown files locally
2. `git push` to the `master` branch

After pushing, GitHub Actions syncs `docs/`, `mkdocs.yml`, and `requirements.txt` to the server via SCP, then runs `docker compose restart` to restart the MkDocs container. The container installs dependencies from `requirements.txt` and renders pages on startup.

---

## Local Preview

```bash
# Create and activate virtual environment (first time only)
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate

# Install dependencies (first time only)
pip install -r requirements.txt

# Start local preview server
mkdocs serve
```

Visit `http://127.0.0.1:8000` for a live preview.

---

## Project Structure

This project uses **i18n suffix mode**: Chinese and English files share the same directory, distinguished by filename suffix.

```
docs/
├── assets/                        # Shared static assets (images, etc.)
│   ├── elf3/
│   │   ├── quick_start/           # Images organized by their owning md file
│   │   ├── overview/
│   │   └── developer/
│   │       ├── motioncontrol/
│   │       └── overview/
│   ├── control/
│   │   ├── appguide/
│   │   └── toolguide/
│   ├── actuators/
│   │   └── Introduction/
│   │       ├── BXI5014-19/
│   │       ├── BXI5018-19/
│   │       ├── BXI7010-19/
│   │       └── BXI8515-19/
│   └── ...
├── .nav.yml                       # Root navigation config (includes use_index_title: true)
├── index.zh.md                    # Chinese homepage
├── index.en.md                    # English homepage
│
├── elf3/
│   ├── .nav.yml
│   ├── index.zh.md                # Title frontmatter only — provides section name
│   ├── index.en.md
│   ├── overview.zh.md
│   ├── overview.en.md
│   ├── quick_start.zh.md
│   ├── quick_start.en.md
│   └── developer/
│       ├── .nav.yml
│       ├── index.zh.md
│       ├── index.en.md
│       ├── overview.zh.md
│       ├── motioncontrol.zh.md
│       ├── navigation.zh.md
│       └── manipulation.zh.md     # (each with a matching .en.md)
│
├── control/
│   ├── .nav.yml
│   ├── index.zh.md
│   ├── index.en.md
│   ├── app_guide.zh.md            # BXI Control App user guide
│   ├── app_guide.en.md
│   ├── tool_guide.zh.md           # BXI Tool motor software guide
│   └── tool_guide.en.md
│
├── actuators/
│   ├── .nav.yml
│   ├── index.zh.md
│   ├── index.en.md
│   ├── can_communication.zh.md
│   └── Introduction/
│       └── ...                    # Per-model actuator spec pages
└── ...
```

### The index file convention

Each section directory has a pair of `index.zh.md` / `index.en.md` files containing **only a frontmatter title, no body content**:

```markdown
---
title: ELF3 Robot
---
```

These files **do not appear in the navigation menu** — they exist only to provide the section's localized display name (read by `use_index_title: true` in the root `.nav.yml`).

### Naming Conventions

| Rule | Details |
|------|---------|
| Folder / file names | Lowercase letters, digits, `_`, `-` only — **no Chinese characters or spaces** |
| Chinese documents | `filename.zh.md` |
| English documents | `filename.en.md` |
| Shared assets | Place under `docs/assets/<section>/<md-filename>/` — organized by document, never duplicated |

---

## Adding Content

### Adding a new page

Create both language files in the target directory — no config changes needed:

```
docs/elf3/new_page.zh.md
docs/elf3/new_page.en.md
```

### Adding a new section

1. Create the directory structure:

```
docs/half_robot/
├── .nav.yml             # Controls item order, does NOT include index.md
├── index.zh.md          # Title only: 半人形机器人
├── index.en.md          # Title only: Half Robot
├── overview.zh.md       # First content page
├── overview.en.md
└── pnd_adam_u_sdk/
    ├── .nav.yml
    ├── index.zh.md      # Title only: PND Adam U SDK
    ├── index.en.md
    └── ...
```

2. Add the directory name to the parent `.nav.yml`:

```yaml
# docs/.nav.yml
use_index_title: true

nav:
  - index.md
  - elf3
  - control
  - half_robot      # add this line
  - actuators
  - ...
```

---

## Navigation Config (`.nav.yml`)

Each directory has a `.nav.yml` to control the order of nav items. **Do not list `index.md` in `nav:`** — index files are not shown in the menu.

```yaml
# docs/elf3/.nav.yml
nav:
  - overview.md       # Use logical names without locale suffix
  - quick_start.md
  - developer
```

---

## Image References

All images go under `docs/assets/`, **in a subdirectory named after the owning md file**. Reference them with relative paths:

```markdown
![Description](../assets/elf3/quick_start/demo.png)
```

Convention: `docs/assets/<section>/<md-filename>/image.png` — for example, images for `docs/elf3/quick_start.zh.md` go in `docs/assets/elf3/quick_start/`.

**Never** store duplicate copies of the same image under separate language directories — Chinese and English pages share the same image files.
