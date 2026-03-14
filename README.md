# Slatesource ~ Slate as Code

Connect a GitHub repository to keep your slates in sync. Push a commit, your slate updates automatically.

---

## How It Works

1. Create an account and **Connect GitHub** at [slatesource.com](https://slatesource.com)
2. Any folder containing a `.slate` file is detected as a slate
3. `.chext` files inside the `pages/` subfolder become pages on that slate
4. Every push to the watched branch re-syncs automatically, slates sourced from GitHub are read-only in the UI

---

## Repository Structure

You can have slate folders at any depth, and multiple slates in a single repo.

```
my-project/
├── my-slate/
│   ├── .slate              ← slate metadata (name, description)
│   ├── pages/
│   │   ├── Home.chext      ← root page
│   │   └── Projects.chext  ← additional page titled "Projects"
│   └── assets/             ← local images referenced in .chext files (optional)
│       └── cover.png
└── another-slate/
    ├── .slate
    └── pages/
        └── Home.chext
```

- `pages/Home.chext` (any casing) maps to the root page of the slate
- Any other filename becomes a named page — `Projects.chext` → page titled "Projects"
- Images in `assets/` can be referenced by filename in chips that support images

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
- Long lines can be continued with a trailing `\` to improve readability — the newline is stripped

---

## Inline Attributes

Attributes are added on the chip header line inside `[key:value]` brackets.

```
:INFO [align:center]
:PROFILE [image:profile.png]
:TASK [status:in_progress, label:urgent, labelColor:#e74c3c, deadline:2026-03-31T00:00:00Z]
:GALLERY [displayStyle:travel]
:LINK [links:https://example.com]
```

Multiple attributes are comma-separated. Supported attributes vary by chip type.

---

## Half Layout

Append `.half` to any chip type to make it occupy half the width. Separate adjacent half chips with a `|` on its own
line.

```
:NOW.half
Current status text
|
:COUNTER.half
1200 Downloads
```

A `.half` chip without a `|` following it takes up the remaining space.

---

## Chip Reference

Full visual reference at [slatesource.com/chips](https://slatesource.com/chips).

| Chip        | Line 0                    | Line 1                   | Remaining lines                   |
|-------------|---------------------------|--------------------------|-----------------------------------|
| `INFO`      | title                     | body text                | more body text                    |
| `NOTE`      | body text                 | more body text           | more body text                    |
| `QUOTE`     | text (max 255 chars)      | —                        | —                                 |
| `TASK`      | `[ ]` / `[x]` / plain    | —                        | —                                 |
| `CHECKLIST` | title                     | `[ ] item` / `[x] item` | more items                        |
| `COUNTER`   | `{value} {label}`         | —                        | —                                 |
| `LINK`      | URL                       | optional title           | —                                 |
| `NOW`       | current status text       | —                        | —                                 |
| `PROFILE`   | name                      | location                 | bio (lines joined, max 100 chars) |
| `SOCIALS`   | title                     | URL                      | more URLs                         |
| `PATH`      | title                     | description              | —                                 |
| `CONTACT`   | title                     | subtitle / email         | —                                 |
| `DURATION`  | ISO 8601 start datetime   | ISO 8601 end datetime    | —                                 |
| `IMAGE`     | image filename or URL     | —                        | —                                 |
| `MOMENT`    | image filename or URL     | title                    | location                          |
| `GALLERY`   | image filename or URL     | more filenames or URLs   | more filenames or URLs            |

**`INFO` alignment** is set via inline attribute: `:INFO [align:left]`, `:INFO [align:center]`, `:INFO [align:right]`.
Defaults to left if omitted.

---

## Full Example — `pages/Home.chext`

```
// Root page for my portfolio slate

:PROFILE [image:profile.png]
Jane Doe
San Francisco, CA
Full-stack engineer and open source contributor. I build tools for developers.

:NOW
Currently focused on shipping v2.0 of my CLI tool.

:INFO [align:center]
About This Project
A long-running side project I've been building since 2021. The goal is to make \
developer portfolios as easy to maintain as a README.

:NOTE
This slate is generated automatically from my GitHub repo. Any push to main \
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

:COUNTER.half
4200 Downloads
|
:COUNTER.half
98.5% Uptime

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
