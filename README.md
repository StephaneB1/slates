# Slatesource ~ Slate as Code

Connect a GitHub repository to keep your slates in sync. Push a commit, your slate updates automatically.

---

## How It Works

1. Create an account and **Connect GitHub** at [slatesource.com](https://slatesource.com)
2. Any folder containing a `.slate` file is detected as a slate
3. `.chext` files inside that folder become pages on that slate
4. Every push to the watched branch re-syncs automatically, slates sourced from GitHub are read-only in the UI

---

## Repository Structure

You can have slate folders at any depth, and multiple slates in a single repo.

```
my-project/
├── portfolio/
│   ├── .slate           ← slate metadata (name, description)
│   ├── Home.chext       ← root page
│   └── Projects.chext   ← additional page named "Projects"
└── docs/
    ├── .slate
    └── Home.chext
```

- `Home.chext` (any casing) maps to the root page of the slate
- Any other filename becomes a named page — `Projects.chext` → page titled "Projects"

---

## The `.slate` Metadata File

Plain `key: value` pairs. Both `name` and `title` are accepted for the slate title. Surrounding quotes are stripped
automatically.

```
name: My Project
description: An overview of my open source project
```

YAML-style also works:

```yaml
title: "My Project"
description: "An overview of my open-source project"
```

---

## The `.chext` Page Format

Each `.chext` file defines one page. The filename without `.chext` becomes the page title.

**Syntax rules:**

- A chip block starts with `:CHIPTYPE` on its own line (case-insensitive)
- Lines starting with `//` are comments — ignored by the parser
- Each block ends where the next `:CHIPTYPE` begins (or at end of file)
- Lines within a block are positional — line 0 is the first line after the `:CHIPTYPE` header

---

## Chip Reference

Full visual reference at [slatesource.com/chips](https://slatesource.com/chips).

| Chip        | Line 0                               | Line 1                                          | Remaining lines                   |
|-------------|--------------------------------------|-------------------------------------------------|-----------------------------------|
| `INFO`      | title                                | optional alignment: `LEFT` / `CENTER` / `RIGHT` | body text (max 1 000 chars)       |
| `NOTE`      | —                                    | —                                               | all lines joined (max 500 chars)  |
| `QUOTE`     | text (max 255 chars)                 | —                                               | —                                 |
| `TASK`      | `[ ] text` / `[x] text` / plain text | —                                               | —                                 |
| `CHECKLIST` | title                                | `[ ] item` / `[x] item` / plain text            | more items…                       |
| `COUNTER`   | `{value} {label}`                    | —                                               | —                                 |
| `LINK`      | URL                                  | optional title                                  | —                                 |
| `NOW`       | current status text                  | —                                               | —                                 |
| `PROFILE`   | name                                 | location                                        | bio (lines joined, max 100 chars) |
| `SOCIALS`   | title                                | URL                                             | more URLs…                        |
| `PATH`      | title                                | subtitle                                        | —                                 |
| `CONTACT`   | title                                | subtitle / email                                | —                                 |

> `MOMENT` and `GALLERY` are reserved chip types but are not yet supported via `.chext` — they require file uploads and
> must be added directly in the UI.

---

## Full Example — `Home.chext`

```
// Root page for my portfolio slate

:PROFILE
Jane Doe
San Francisco, CA
Full-stack engineer and open source contributor. I build tools for developers.

:NOW
Currently focused on shipping v2.0 of my CLI tool.

:INFO
About This Project
LEFT
A long-running side project I've been building since 2021. The goal is to make
developer portfolios as easy to maintain as a README.

:NOTE
This slate is generated automatically from my GitHub repo. Any push to main
re-syncs the content here.

:QUOTE
The best documentation is the one developers actually keep up to date.

:SOCIALS
Find me online
https://github.com/janedoe
https://twitter.com/janedoe
https://linkedin.com/in/janedoe

:CHECKLIST
Project Milestones
[x] Initial release
[x] Add CLI installer
[ ] Publish to npm
[ ] Write full test suite

:COUNTER
4200 Downloads

:LINK
https://github.com/janedoe/my-cli-tool
View source on GitHub

:CONTACT
Get in touch
hello@janedoe.dev

:DURATION
2021-03-01T00:00:00Z
2024-12-31T00:00:00Z

:PATH
Open Source Journey
From first commit to 4k downloads
```

---

## Links

- Chip types visual reference — [slatesource.com/chips](https://slatesource.com/chips)
- `.chext` format docs — [slatesource.com/chips/chext](https://slatesource.com/chips/chext)
- Register — [slatesource.com/register](https://slatesource.com/register)
