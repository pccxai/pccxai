# pccxai static site

This repository contains the main public website for the `pccxai` open-source
ecosystem.

The site is static HTML and CSS intended for Cloudflare Pages deployment at:

<https://pccxai.pages.dev>

## Site map

| Path | Purpose |
| --- | --- |
| `/` | Ecosystem overview and primary entry points |
| `/contribute/` | Contributor guidance, review expectations, and evidence rules |
| `/assistant/` | Static assistant preview with scripted local content only |
| `/security/` | Current static-site posture and future protection plan |

## Files

```text
public/
|-- _headers
|-- _redirects
|-- index.html
|-- assets/
|   `-- styles.css
|-- assistant/
|   `-- index.html
|-- contribute/
|   `-- index.html
`-- security/
    `-- index.html
```

## Cloudflare Pages settings

Use these settings for the `pccxai` Pages project:

| Setting | Value |
| --- | --- |
| Project name | `pccxai` |
| Production URL | `pccxai.pages.dev` |
| Root directory | repository root |
| Build command | `exit 0` |
| Build output directory | `public` |

No build step, package manager, framework adapter, environment variable, API key,
or Pages Function is required for the current site.

## Assistant boundary

`/assistant/` is a static preview. It does not call a model provider, does not
contain secrets, and does not include a Cloudflare Function.

A future live assistant should be added only with a reviewed server-side
boundary, Turnstile, rate limiting, Cloudflare AI Gateway, provider budget
guards, and daily or monthly spend limits.

## Security posture

The current deployed surface is static content served from `public/`. There is
no login, no form submission, no paid provider endpoint, and no runtime secret
in this repository.

Basic static-site headers are configured in `public/_headers`.

Security disclosures for code under the `pccxai` organization should follow the
organization security policy:

<https://github.com/pccxai/.github/blob/main/SECURITY.md>

## Local validation

```sh
git status --short --branch
find public -maxdepth 3 -type f | sort
python3 -m http.server 8000 --directory public
```

Smoke test from another shell while the local server is running:

```sh
curl -I http://localhost:8000/
curl -I http://localhost:8000/contribute/
curl -I http://localhost:8000/assistant/
curl -I http://localhost:8000/security/
```
