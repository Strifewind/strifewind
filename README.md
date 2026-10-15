# Strifewind

A personal workbench for software, infrastructure, research, and electronics.

This repository contains my single-page portfolio for **https://strifewind.sbs/**,
built with Hugo. Its dark circuit-board design uses local assets and system fonts;
there are no JavaScript dependencies, tracking scripts, or external font requests.

## Preview locally

Install **Hugo 0.168.0** (the standard edition is sufficient), then run:

```sh
hugo server --source /home/runner/work/strifewind/strifewind
```

Open the local URL printed by Hugo. For other checkouts, substitute your absolute
repository path. To make the same production build used in CI:

```sh
hugo --source /home/runner/work/strifewind/strifewind --minify --panicOnWarning
```

Generated output goes into the ignored `public` directory. Hugo warnings fail CI.
The repository has no separate test or lint framework.

## Update content

All paths below are relative to this repository's root.

| Content | File |
| --- | --- |
| Introduction, headline | `content/_index.md` |
| Biography | `content/about.md` |
| Experience and résumé introduction | `content/experience.md` |
| Contact copy | `content/contact.md` |
| GitHub URL, résumé filename, site description | `hugo.toml` |
| Projects | `content/projects/*.md` |
| Colors, layout, reduced-motion styles | `assets/css/site.css` |
| Circuit-board illustration | `layouts/partials/board.html` |

Each content file starts with TOML front matter between `+++` delimiters, followed
by Markdown prose. Project `weight` controls ordering (smaller comes first);
`category`, `status`, `code`, and `tags` control card labels. The initial copy and
project statuses are starting points to review before launch, not a verified
employment history. Add professional highlights to `content/experience.md` when
ready.

To add a project, copy an existing project file, choose a unique filename and code,
and edit its metadata and description. Keep `[build] render = 'never'` so content
appears on the homepage without generating a separate page. Delete its file to
remove a project. Alternatively, `draft = true` before `[build]` hides it from
production.

Projects without public URLs deliberately have no fabricated or dead links.
Replace `links = []` in their front matter to publish real links:

```toml
links = [
  { label = "Source", url = "https://github.com/Strifewind/your-project" },
  { label = "Demo", url = "https://your-demo.example" }
]
```

Only add public HTTPS URLs. Use Markdown links in the contact copy to add other
public contact methods. Raw HTML in Markdown is disabled.

## Publish the résumé

Continue using the existing Groundfloor/LaTeX tooling; this site does not compile
LaTeX or require your résumé's TOML source.

1. Generate and review a **public-safe** PDF with the existing builder.
2. Copy the PDF to `static/resume.pdf` and commit it.
3. The next build automatically enables the homepage's view/download links at
   `https://strifewind.sbs/resume.pdf`.

Until that file exists, the page shows “Résumé PDF not published yet” rather than
a broken download. Removing it disables the links again. Set `params.resumeFile`
in `hugo.toml` to change the filename (relative to `static`, without a leading
slash). Do not add private résumé source, personal addresses, or credentials.

## GitHub Pages and domain setup

Implementation lives in this existing repository; no separate repository has
been created. This profile repository can host a Pages project site using the
custom domain, while its README remains visible on the GitHub profile.

1. Merge the site into `main`.
2. In **Settings → Pages → Build and deployment**, select **GitHub Actions**.
3. Set the Pages custom domain to **strifewind.sbs**. The matching
   `static/CNAME` is included in build output, but does not replace the Pages
   setting when using Actions.
4. At your DNS provider, configure the apex domain using GitHub Pages' current
   [DNS instructions](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site).
   Remove conflicting records. If using `www`, configure it following the same
   guide and choose the primary domain in GitHub.
5. Verify domain ownership in account settings if not already done. Domain
   verification is separate from assigning the domain to this Pages site.
6. Once DNS and the certificate are ready, enable **Enforce HTTPS** and check
   the site and résumé URL.

The workflow builds pull requests without deploying them. Pushes to `main` deploy
the site; manual runs deploy only when run against `main`. Deployment requires
Pages to be enabled and any `github-pages` environment approvals to be satisfied.
No DNS or account settings are changed by this repository.

Actions are pinned to commit SHAs. The Hugo archive is version-pinned and checked
against its release SHA-256; update both version and checksum together.
