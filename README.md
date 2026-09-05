# Deploy and Host WordPress on Railway – Self-Hosted CMS with MariaDB

WordPress powers a large share of the web. This template self-hosts WordPress on Railway with MariaDB, giving you a full CMS install — no traffic caps, no plugin restrictions, and none of the renewal price jumps that make cheap shared hosting expensive after the first year.

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/deploy/wordpress-cms?referralCode=zxcgoT&utm_medium=integration&utm_source=template&utm_campaign=generic)

## 🚀 Quick Start Deployment Guide

### Step 1: Deploy on Railway
1. Click **Deploy on Railway** above
2. Wait for both services — WordPress and MariaDB — to finish building

### Step 2: Mount both volumes
1. Add a Railway Volume mounted at `/var/www/html` on the WordPress service — this holds core files, themes, plugins and every upload
2. Add a Railway Volume mounted at `/var/lib/mysql` on the MariaDB service — this is the database itself
3. Skipping either means losing your site on the next redeploy

### Step 3: Check the generated credentials
1. Open the **Variables** tab and confirm `WORDPRESS_DB_PASSWORD` and `MARIADB_ROOT_PASSWORD` were generated
2. Confirm the eight `WORDPRESS_*_KEY` and `WORDPRESS_*_SALT` values are present — they sign auth cookies
3. Leave the salts alone once the site is live; changing them logs everyone out

### Step 4: Run the WordPress installer
1. Open your Railway public domain
2. Choose a language, then set the site title, admin username, a strong password and your email
3. Avoid `admin` as the username — it is the first thing brute-force bots try

### Step 5: Secure the new site
1. Log in at `/wp-admin`
2. Under **Settings → General**, confirm the WordPress Address and Site Address match your Railway domain
3. Under **Settings → Discussion**, require approval for comments if the site is public
4. Delete unused default themes and plugins

### Step 6: Set up a backup routine
1. The Railway volumes hold everything, but a volume is not a backup
2. Install a backup plugin, or export the database and `/var/www/html` on a schedule
3. Test a restore before you need one

## About Hosting WordPress

This Railway template deploys two services: the official `wordpress` image on Apache, serving the site and admin on port 80 with a public Railway domain, and `mariadb:latest` reachable over Railway's private network for posts, pages, users, settings and plugin data.

Two volumes matter and both are required — `/var/www/html` for WordPress core, themes, plugins and uploads, and `/var/lib/mysql` for the MariaDB data directory. Database credentials and the eight WordPress security salts are generated automatically at deploy time.

The template exists so you get standard, unrestricted self-hosted WordPress — full plugin and theme freedom, flat Railway compute for two services with no traffic caps, and none of the promotional-pricing traps that make shared hosting cheap in year one and expensive afterward.

## Common Use Cases

- **Content publishing**: a blog or content site with no visit caps and no renewal price jump
- **Business sites**: full plugin and theme freedom for sites that need customization beyond managed tiers
- **E-commerce and membership**: WooCommerce or membership sites that outgrow shared hosting limits
- **Disposable environments**: staging or client sites that need to be spun up and torn down quickly

## Dependencies for WordPress Hosting

### Deployment Dependencies

- [Official WordPress Docker image](https://github.com/docker-library/wordpress)
- [WordPress documentation](https://wordpress.org/documentation/)
- [MariaDB](https://mariadb.org/)

## ⚙️ Configuration

| Variable | Required | Description |
|---|---|---|
| `WORDPRESS_DB_HOST` | Yes | MariaDB host, wired to the database service over Railway's private network |
| `WORDPRESS_DB_NAME` | Yes | Database name — `railway` in this template |
| `WORDPRESS_DB_USER` | Yes | Database user — `railway` in this template |
| `WORDPRESS_DB_PASSWORD` | Yes | Database password, auto-generated as a 32-character secret |
| `MARIADB_ROOT_PASSWORD` | Yes | MariaDB root password, auto-generated |
| `WORDPRESS_CONFIG_EXTRA` | No | Extra PHP appended to wp-config.php. This template uses it to set the site and home URL from the Railway domain |
| `WORDPRESS_AUTH_KEY` and the other seven salts | Yes | WordPress security salts (AUTH_KEY, SECURE_AUTH_KEY, LOGGED_IN_KEY, NONCE_KEY and their SALT counterparts), auto-generated. Changing them logs every user out |
| `WORDPRESS_TABLE_PREFIX` | No | Table prefix, defaulting to `wp_` |
| `PORT` | No | Container port — 80 |

## 🐳 Self-Host with Docker Compose

```bash
git clone https://github.com/sahilrupani/wordpress-railway-template
cd wordpress-railway-template
cp .env.example .env
```

Generate the required secrets (database password, root password, and the eight WordPress security salts) and add them to `.env`, then bring the stack up:

```bash
docker compose up -d
```

Open `http://localhost` (or the port configured in your compose file) to run the WordPress installer.

## ❓ Frequently Asked Questions (FAQ)

### How much does it cost to run WordPress on Railway?
Flat compute for two services — WordPress and MariaDB. There are no traffic caps and no promotional rate that jumps at renewal, which is the usual sting with cheap shared hosting.

### Can I install any plugin or theme?
Yes. This is standard self-hosted WordPress with no marketplace restrictions, unlike managed tiers that block certain plugins.

### How do I back it up?
Volumes keep data across redeploys but are not backups. Use a backup plugin or export the database along with `/var/www/html` on a schedule, and test a restore.

### Can I use my own domain?
Yes — add a custom domain to the WordPress service in Railway, then make sure the WordPress Address and Site Address match it or you will hit redirect loops.

### Is this the official WordPress image?
Yes, the official image maintained by Docker Library, paired with MariaDB.

### Can I migrate an existing site in?
Yes. Import a database dump into MariaDB and copy `wp-content` into `/var/www/html`, then fix the site URLs in the database to the new domain.

### Why do I see "Error establishing a database connection"?
`WORDPRESS_DB_HOST`, `_USER`, `_NAME` or `_PASSWORD` does not match the MariaDB service, or MariaDB has not finished starting. Check the database service is healthy and the values line up.

### Why does the whole site disappear after a redeploy?
There is no volume at `/var/www/html` (loses themes, plugins and uploads) or none at `/var/lib/mysql` (loses the database). Both are required.

### Why is my CSS broken, or why am I stuck in a redirect loop?
The WordPress Address and Site Address do not match the domain you are browsing. This template sets them through `WORDPRESS_CONFIG_EXTRA`; correct it there or in Settings → General.

### Why was every user logged out unexpectedly?
One of the eight security salts changed. They sign auth cookies, so rotating them invalidates all sessions.

### Why do uploads fail on larger media files?
PHP upload limits. Raise `upload_max_filesize` and `post_max_size` for the container, or upload large media directly to object storage.

## 🛠️ Support & Issues

If you run into problems with this template, open an issue at [github.com/sahilrupani/wordpress-railway-template/issues](https://github.com/sahilrupani/wordpress-railway-template/issues) with a description of the problem, steps to reproduce it, and any relevant logs from the WordPress or MariaDB service.

---

*This is a community-maintained Railway template built around the [official WordPress Docker image](https://github.com/docker-library/wordpress). It is not affiliated with WordPress, Automattic, MariaDB, or Railway.*