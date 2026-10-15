# colezuckerman.com

Source for my portfolio site, https://colezuckerman.com. Built with [Hugo](https://gohugo.io) and the [Coder](https://github.com/luizdepra/hugo-coder) theme.

## Where things live

| What | File |
|---|---|
| Name, tagline, social links, menu | `hugo.toml` |
| Profile photo | `static/images/` (update `avatarURL` in `hugo.toml`) |
| About page | `content/about.md` |
| Contact page | `content/contact.md` |
| Projects (one file each) | `content/projects/*.md` |
| Project screenshots | `static/images/projects/` |

## Add a project

```
hugo new projects/my-new-project.md
```

Or copy an existing project file. The top section (between the `---` lines) controls the card:

- `title`, `summary`: shown on the card
- `tech`: list of tags, e.g. `["Python", "React"]`
- `image`: path to the main photo in `static/` (optional)
- `thumbnail`: a different photo for the card on the Projects page (optional)
- `github`, `demo`: links (optional)
- `weight`: order on the Projects page (lower = first)

Everything below the second `---` is the project page, written in Markdown.

## Preview locally

```
hugo server
```

Then open http://localhost:1313.

## Publish

Every push to `main` is built by GitHub Actions (`.github/workflows/hugo.yml`) and
published to GitHub Pages at https://colezuckerman.com within about a minute.
