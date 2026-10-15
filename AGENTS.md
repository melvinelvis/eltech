# NetCashEmpire — Base44 Dev Environment

## What this project is

A static WordPress site export ("NetCashEmpire") packaged as a zip inside `NET CASH EMPIRE/`. The zip was extracted to `NET CASH EMPIRE/extracted/` and is served as static files by nginx.

## Structure

- `NET CASH EMPIRE/extracted/index.html` — the homepage HTML (copied from `mysocialpay.net/index.html.html`)
- `NET CASH EMPIRE/extracted/EkW2YWuBDN8u.net/` — WordPress assets (wp-content, wp-includes)
- `nginx.conf` — nginx server config (listens on port 3000)
- `docker-compose.base44.yml` — single nginx:alpine service, bind-mounts the extracted dir as the web root

## Theme name mismatches (fixed with symlinks)

The HTML references two theme names that map to one physical directory:

- `wp-content/themes/saaslauncher` → symlink to `streammanagement`
- `wp-content/themes/streammanagement/assets/js/streammanagement-scripts_ver=1.2.3.js` → symlink to `saaslauncher-scripts_ver=1.2.3.js`

## URL-encoded filenames

Asset filenames contain `=` and `&` (e.g. `style_ver=1.2.3.css`). The HTML references them URL-encoded (`%3D`, `%26`). Nginx decodes these automatically before file lookup, so no special config is needed.

## How to verify

```bash
docker compose -f docker-compose.base44.yml up -d
curl -s http://localhost:3000/ | grep '<title>'
```

No external credentials or secrets are required.
