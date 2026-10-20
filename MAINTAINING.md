# Maintaining the portfolio

## Preview and build

Install [Hugo 0.168.0](https://github.com/gohugoio/hugo/releases/tag/v0.168.0)
(standard edition), then run these commands from the repository root:

```sh
hugo server
hugo --minify --panicOnWarning
```

The second command is the production build used by CI. Its output goes to `public/`.

## Update content

- Edit the introduction, biography, experience, and contact copy in
  [`content/`](content/).
- Add or update projects in [`content/projects/`](content/projects/). Project
  `weight` sets display order; `draft = true` hides a project from production.
  Keep `[build] render = 'never'` so projects appear on the homepage without
  generating separate pages.
- Site settings, including the GitHub URL and résumé filename, are in
  [`hugo.toml`](hugo.toml). Keep the profile [`README.md`](README.md) aligned with
  the public biography and résumé when updating work history.

## Publish the résumé

The public PDF is checked in at [`static/resume.pdf`](static/resume.pdf).
Résumé generation stays in the existing Groundfloor/LaTeX tooling. To update it,
review a public-safe PDF and replace that file.

The homepage only shows the view and download links while the configured file
exists; set `params.resumeFile` in `hugo.toml` to use a different filename, and
update the résumé link in `README.md` to match.

## Publish the site

The [Pages workflow](.github/workflows/pages.yml) builds pull requests but deploys
only pushes to `main` (or manual runs against `main`). In the repository's
**Settings > Pages**, select **GitHub Actions** as the build and deployment source.
The repository includes [`static/CNAME`](static/CNAME); the Pages custom-domain
setting must also be configured in GitHub.

DNS records are managed separately at the domain provider, not in this repository.
Follow GitHub's [custom-domain DNS instructions](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site).
Domain ownership verification is an account setting; assigning the domain to this
Pages site and enabling HTTPS are repository Pages settings.
