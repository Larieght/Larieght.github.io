# Larieght.github.io

**Redirect only.** This repo used to be my GitHub Pages site. Now every
address on https://larieght.github.io sends visitors to **webie**, my
personal website:

- Live site: https://tydtr0026.dpdns.org (hosted on Cloudflare Workers)
- Source: the private repo [`Larieght/webie`](https://github.com/Larieght/webie)

## How the redirect works

GitHub Pages only hosts static files, so it can't send a real server-side
"301 Moved Permanently". Instead every page (including the 404 page used for
any unknown address) is a tiny HTML page that:

1. runs a line of JavaScript first, which jumps to the same path on the new
   site, keeping the query and `#hash`
   (`/about?x=1#top` → `https://tydtr0026.dpdns.org/about?x=1#top`);
2. for visitors with JavaScript off, falls back to a meta refresh to the new
   site's front page;
3. tells search engines the new address with `<link rel="canonical">`;
4. shows a plain link, in case nothing else works.

Unknown addresses still return status 404 (GitHub decides that), but the
visitor is redirected all the same.

## Files

| File | What it is |
| --- | --- |
| `_config.yml` | Holds `new_site`, the redirect target. |
| `_layouts/redirect.html` | The redirect page itself. |
| `index.html`, `404.html` | Two-line stubs that use that layout. |
| `.github/workflows/check-redirect.yml` | Builds the site on every pull request and checks the redirect is intact. Never publishes. |

GitHub Pages builds the site with Jekyll (setting: *Deploy from a branch*,
`main`, `/`), which fills `new_site` into the layout.

## Changing the domain

Edit one line in `_config.yml`:

```yaml
new_site: https://new-domain.example
```

Do it on a branch, open a pull request, wait for the **Check redirect** check
to pass, then merge. GitHub Pages republishes within a minute or two.
