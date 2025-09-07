# El Paso Mesh — Website (Docsy + Hugo)

This repository contains a Docsy/Hugo site for El Paso Mesh. It’s designed so you can **rebrand it for other mesh communities**, and you can **host it on Debian** with automatic pulls of the latest content.

---

## Repo layout (quick map)

```
.
├─ site/                      # Hugo project (source)
│  ├─ content/                # Markdown pages (home, docs, news, map, links)
│  ├─ assets/
│  │  ├─ icons/logo.svg       # Navbar logo (Docsy auto-detects)
│  │  └─ scss/                # _variables_project.scss, _styles_project.scss (theme overrides)
│  ├─ layouts/partials/       # Small customizations (footer, head hooks)
│  ├─ static/                 # Static files (favicons, images)
│  └─ hugo.toml               # Hugo config (baseURL, menus, Docsy params)
├─ docs/                      # Built site (used only for GitHub Pages "Deploy from branch")
└─ .github/workflows/         # CI (optional, for GitHub Pages auto-builds)
```

> On a Linux server with Apache you do **not** need `/docs/`. Build straight to your web root (e.g., `/var/www/meshsite`).

---

## 1) Rebrand for another mesh community

Follow these steps to adapt this repo for a new locality (e.g., “Your Mesh”).

### A. Fork or duplicate
- Fork this repo to your own GitHub account **or** create a new repo and copy the `/site` folder.

### B. Update branding & config
1. **Site config:** edit `site/hugo.toml`
   - `title` → `"Your Mesh"`
   - `baseURL` → your public URL (include trailing slash), e.g. `https://yourmesh.org/`
   - Menus under `[menu.main]` (Docs, News, Map, Links, Discord) — change names/URLs if needed.
   - Ensure Goldmark allows HTML in Markdown (already set):
     ```toml
     [markup.goldmark.renderer]
       unsafe = true
     ```
2. **Logo:** replace `site/assets/icons/logo.svg`
   - Prefer an SVG; Docsy inlines it and it works in dark/light modes.
   - If you only have a PNG, embed it inside an SVG wrapper.
3. **Colors:** edit `site/assets/scss/_variables_project.scss`
   - `$primary`, `$secondary`, `$warning` etc. to your palette.
4. **Footer tagline:** edit `site/layouts/partials/footer.html`
5. **Favicons & social images:**
   - `site/static/favicon.ico`, `favicon-32x32.png`, `favicon-16x16.png`, `apple-touch-icon.png`
   - Optional: add `site/static/og.png` (1200×630) and include it later in `head-extra.html`.

### C. Update content
- **Home:** `site/content/_index.md` (hero text, Discord link)
- **Docs:** `site/content/docs/` (Overview, Getting Started, Weekly Net, FAQ)  
  Prefer Hugo helpers for internal links, e.g. `{{< relref "overview.md" >}}`.
- **News:** keep section at `/news/` but use Docsy’s blog layout by setting:
  - `site/content/news/_index.md` → front matter: `type: "blog"`
  - Each post → include `type: "blog"` in front matter.
- **Map & Links:** edit `site/content/map/_index.md` and `site/content/links/_index.md`.

### D. Build locally (optional)
```bash
cd site
# Ensure "extended" Hugo; verify with: hugo version
hugo
# Outputs to ../docs/ by default (configured in hugo.toml)
```
If PostCSS is required locally:
```bash
npm i -D postcss postcss-cli autoprefixer
echo "module.exports = { plugins: { autoprefixer: {} } }" > postcss.config.cjs
```

---

## 2) Host on a Debian server (Apache) and auto-pull updates

These steps assume **Debian 12 (Bookworm)** or similar. You’ll build the site on the server and serve static files via **Apache**. The server will **periodically pull** from Git and rebuild automatically using **systemd**.

### A. Create a deploy user and directories
```bash
sudo adduser --system --group --home /opt/meshsite mesh
sudo mkdir -p /opt/meshsite/{repo,webroot,public}
sudo chown -R mesh:mesh /opt/meshsite
```

### B. Install prerequisites
```bash
sudo apt update
sudo apt install -y git apache2 nodejs npm

# Install Hugo (extended). If apt’s version isn’t “extended”, install from the official .deb
# Verify:
hugo version
# Look for "extended" in the output.
```

### C. Clone the repo
```bash
sudo -u mesh -H bash -lc '
  cd /opt/meshsite/repo
  git clone https://github.com/joeyjoey1234/elpmesh-prop2.git .
'
```

### D. Configure site for your domain
Edit `/opt/meshsite/repo/site/hugo.toml`:
```toml
baseURL = "https://yourdomain.example/"   # <-- include trailing slash
publishDir = "../public"                  # build into /opt/meshsite/public on the server
```
We’ll publish to `/opt/meshsite/public` and sync it into `webroot` for Apache.

### E. Minimal PostCSS setup (one-time on server)
```bash
sudo -u mesh -H bash -lc '
  cd /opt/meshsite/repo/site
  npm init -y
  npm i -D postcss postcss-cli autoprefixer
  echo "module.exports = { plugins: { autoprefixer: {} } }" > postcss.config.cjs
'
```

### F. Build script
Create `/opt/meshsite/deploy.sh`:
```bash
sudo tee /opt/meshsite/deploy.sh > /dev/null <<'EOF'
#!/usr/bin/env bash
set -euo pipefail

REPO_DIR="/opt/meshsite/repo"
SITE_DIR="$REPO_DIR/site"
PUB_DIR="/opt/meshsite/public"
WEBROOT="/opt/meshsite/webroot"   # Apache serves this path
BRANCH="main"

export HUGO_ENV=production
export PATH="$HOME/.local/bin:/usr/local/bin:/usr/bin:/bin"

cd "$REPO_DIR"
git fetch --all
git reset --hard "origin/${BRANCH}"

# Ensure Node deps for PostCSS (first run or after package changes)
if [ -f "$SITE_DIR/package.json" ]; then
  cd "$SITE_DIR"
  npm install --no-audit --no-fund
fi

# Build to /opt/meshsite/public
cd "$SITE_DIR"
hugo --minify -d "$PUB_DIR"

# Sync into webroot (atomic-ish)
rsync -a --delete "$PUB_DIR"/ "$WEBROOT"/

echo "[deploy] $(date -Iseconds): build complete."
EOF
sudo chmod +x /opt/meshsite/deploy.sh
sudo chown mesh:mesh /opt/meshsite/deploy.sh
```

### G. Systemd service & timer (auto-pull every 5 minutes)
Create `/etc/systemd/system/meshsite.service`:
```ini
[Unit]
Description=Build and deploy meshsite (Hugo)
Wants=network-online.target
After=network-online.target

[Service]
Type=oneshot
User=mesh
Group=mesh
ExecStart=/opt/meshsite/deploy.sh
Nice=10
```

Create `/etc/systemd/system/meshsite.timer`:
```ini
[Unit]
Description=Run meshsite deploy periodically

[Timer]
OnBootSec=1min
OnUnitActiveSec=5min
Unit=meshsite.service

[Install]
WantedBy=timers.target
```

Enable and start:
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now meshsite.timer
# Run once immediately:
sudo systemctl start meshsite.service
# Check logs:
journalctl -u meshsite.service -n 100 -f
```

### H. Apache VirtualHost
Create `/etc/apache2/sites-available/meshsite.conf`:
```apache
<VirtualHost *:80>
  ServerName yourdomain.example

  DocumentRoot /opt/meshsite/webroot
  <Directory /opt/meshsite/webroot>
    Options FollowSymLinks
    AllowOverride None
    Require all granted
    # Strong caching for static assets (optional)
    <FilesMatch "\.(css|js|png|jpg|jpeg|gif|svg|webp|ico)$">
      Header set Cache-Control "public, max-age=604800, immutable"
    </FilesMatch>
  </Directory>

  ErrorLog ${APACHE_LOG_DIR}/meshsite_error.log
  CustomLog ${APACHE_LOG_DIR}/meshsite_access.log combined
</VirtualHost>
```

Enable modules and site, then reload:
```bash
sudo a2enmod headers
sudo a2ensite meshsite
sudo systemctl reload apache2
```

Add HTTPS with Let’s Encrypt (recommended):
```bash
sudo apt install -y certbot python3-certbot-apache
sudo certbot --apache -d yourdomain.example
```

### I. Updating content
Push to `main` on GitHub. Within ~5 minutes the server will:
- `git reset --hard origin/main`
- rebuild the Hugo site with PostCSS
- sync the static output to `/opt/meshsite/webroot`

Force an immediate deploy any time:
```bash
sudo systemctl start meshsite.service
```

---

## 3) Useful: enable “Edit this page” links (contributions)

Make it easy for contributors to propose fixes directly from the site.

In `site/hugo.toml`:
```toml
[params]
  github_repo   = "https://github.com/joeyjoey1234/elpmesh-prop2"
  github_branch = "main"
  github_subdir = "site"
  # Docsy will add an "Edit this page" link when these are set.
```

You can also add a short **CONTRIBUTING.md** that explains how to open a PR and your content style guide.

---

## Tips & troubleshooting

- **Docs button 404 on subpaths**: use Hugo helpers for portability:
  ```html
  {{< relref "docs/_index.md" >}}
  ```
- **Navbar logo not showing**: ensure an SVG exists at `site/assets/icons/logo.svg`. Docsy auto-detects it.
- **Hero background or favicons not showing on GitHub Pages**: use `relURL` in the head hook so URLs include the project subpath.
- **Local PostCSS errors**: install `postcss postcss-cli autoprefixer` once and keep `postcss.config.cjs` in `site/`.
- **Switching to GitHub Pages**: set `publishDir` back to `../docs` and use the included workflow and Pages settings.

---

## License

Content: your own.  
Code & theme customizations: MIT unless otherwise noted in file headers.  
Docsy is licensed per its repository.
