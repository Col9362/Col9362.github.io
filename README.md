# My Portfolio

Built with [Hugo](https://gohugo.io) and the [Coder](https://github.com/luizdepra/hugo-coder) theme.

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
- `image`: path to a screenshot in `static/` (optional)
- `github`, `demo`: links (optional)
- `weight`: order on the Projects page (lower = first)

Everything below the second `---` is the project page, written in Markdown.

## Preview locally

```
hugo server
```

Then open http://localhost:1313.

## Publish

Push to a GitHub repo named `YOUR-USERNAME.github.io`, then in the repo go to
**Settings → Pages → Source** and pick **GitHub Actions**. Every push to `main` redeploys the site.
