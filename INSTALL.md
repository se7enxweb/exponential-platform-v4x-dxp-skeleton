# Exponential Platform v4.6.x DXP (Stable; Open Source; Starter Skeleton)
## Installation & Operations Guide

> **Platform v4 DXP** is the standard single-kernel release of Exponential Platform. It runs the **Exponential Platform v4 OSS** new-stack kernel on **Symfony 5.4 LTS** with full PHP 8.x support.
>
> This guide uses numbered **Git Save Points** throughout. Commit at each one so you can return to any working checkpoint without redoing completed work.

---

## Table of Contents

1. [Requirements](#1-requirements)
2. [Architecture Overview](#2-architecture-overview)
3. [First-Time Installation](#3-first-time-installation)
   - [3a. Composer create-project (recommended)](#3a-composer-create-project-recommended)
   - [3b. GitHub git clone (developers)](#3b-github-git-clone-developers)
4. [Environment Configuration (.env.local)](#4-environment-configuration-envlocal)
5. [Database Setup](#5-database-setup)
   - [5a. MySQL / MariaDB](#5-database-setup)
   - [5b. PostgreSQL](#5-database-setup)
   - [5c. SQLite (zero-config)](#5c-sqlite-zero-config-database)
6. [Web Server Setup](#6-web-server-setup)
   - [6a. Apache 2.4](#6a-apache-24)
   - [6b. Nginx](#6b-nginx)
   - [6c. Symfony CLI (development only)](#6c-symfony-cli-development-only)
7. [File & Directory Permissions](#7-file--directory-permissions)
8. [Frontend Assets (Site CSS/JS)](#8-frontend-assets-site-cssjs)
9. [Admin UI Assets (Platform v4 Admin UI)](#9-admin-ui-assets-platform-v4-admin-ui)
10. [JWT Authentication (REST API)](#10-jwt-authentication-rest-api)
11. [GraphQL Schema](#11-graphql-schema)
12. [Search Index](#12-search-index)
13. [Image Variations](#13-image-variations)
14. [Cache Management](#14-cache-management)
15. [Day-to-Day Operations: Start / Stop / Restart](#15-day-to-day-operations-start--stop--restart)
16. [Updating the Codebase](#16-updating-the-codebase)
17. [Cron Jobs](#17-cron-jobs)
18. [Solr Search Engine (optional)](#18-solr-search-engine-optional)
19. [Varnish HTTP Cache (optional)](#19-varnish-http-cache-optional)
20. [Troubleshooting](#20-troubleshooting)
21. [Database Conversion](#21-database-conversion)
22. [Complete CLI Reference](#22-complete-cli-reference)

---

## 1. Requirements

### PHP

- **PHP 8.0–8.5** (PHP 8.3 or 8.5 strongly recommended)
- Required extensions: `gd` or `imagick`, `curl`, `json`, `pdo_mysql` or `pdo_pgsql` or `pdo_sqlite`, `xsl`, `xml`, `intl`, `mbstring`, `opcache`, `ctype`, `iconv`
- For SQLite: `pdo_sqlite` + `sqlite3` PHP extensions
- `memory_limit` ≥ 256M
- `date.timezone` must be set in `php.ini`
- `max_execution_time` ≥ 120

### Web Server

- **Apache 2.4** with `mod_rewrite`, `mod_deflate`, `mod_headers`, `mod_expires` enabled _or_
- **Nginx 1.18+** with PHP-FPM

### Node.js & Yarn

- **Node.js 20 LTS** — managed via [nvm](https://github.com/nvm-sh/nvm) (recommended)
- **Yarn 1.22.x** — activated via `corepack enable` after `nvm use 20`
- Do **not** use Node.js 18 or 22 — only 20 LTS is tested

Installing nvm + Node.js 20 LTS (all UNIX / macOS / BSD / WSL):

```bash
# Universal installer — works on Linux (all distros), macOS, BSD, WSL
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.2/install.sh | bash
source ~/.nvm/nvm.sh           # or restart your shell
nvm install 20
nvm use 20
corepack enable                # activates Yarn 1.22.x
```

| OS | Alternative install |
|---|---|
| Debian / Ubuntu / Mint / Pop!_OS | `apt install nodejs npm` then `npm i -g yarn` |
| RHEL / CentOS / AlmaLinux / Rocky | `dnf module enable nodejs:20 && dnf install nodejs` |
| Fedora | `dnf install nodejs` |
| openSUSE / SUSE SLES | `zypper install nodejs20` |
| Arch / Manjaro | `pacman -S nodejs npm` |
| Slackware | SlackBuild at slackbuilds.org |
| FreeBSD | `pkg install node20` |
| OpenBSD | `pkg_add node` |
| macOS (Homebrew) | `brew install node@20` |
| macOS (MacPorts) | `port install nodejs20` |
| Generic binary | [nodejs.org/en/download](https://nodejs.org/en/download/) |

### Composer

- **Composer 2.x** — run `composer self-update` to ensure latest 2.x release

```bash
# Universal installer (all UNIX / macOS / BSD)
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php composer-setup.php --2          # install Composer v2
php -r "unlink('composer-setup.php');"
sudo mv composer.phar /usr/local/bin/composer
```

| OS | Package manager install |
|---|---|
| Debian / Ubuntu / Mint | `apt install composer` (may be older — prefer the installer above) |
| RHEL / AlmaLinux / Rocky | `dnf install composer` (EPEL required: `dnf install epel-release`) |
| Fedora | `dnf install composer` |
| openSUSE / SUSE | `zypper install php-composer2` |
| Arch / Manjaro | `pacman -S composer` |
| Slackware | SlackBuild at slackbuilds.org |
| FreeBSD | `pkg install php83-composer` (adjust PHP version) |
| macOS (Homebrew) | `brew install composer` |
| macOS (MacPorts) | `port install php-composer` |
| Generic | [getcomposer.org/download](https://getcomposer.org/download/) |

### Database

- **[MySQL 8.0+](https://dev.mysql.com/downloads/mysql/)** with `utf8mb4` / `utf8mb4_unicode_520_ci` _or_
- **[MariaDB 10.3+](https://mariadb.org/download/)** (10.6+ recommended) _or_
- **[PostgreSQL 14+](https://www.postgresql.org/download/)** _or_
- **[SQLite 3.35+](https://www.sqlite.org/download.html)** — no server required; dev/testing only. Requires the `pdo_sqlite` and `sqlite3` PHP extensions.

Installing MySQL / MariaDB by OS:

| OS | MySQL | MariaDB |
|---|---|---|
| Debian / Ubuntu / Mint | `apt install mysql-server` | `apt install mariadb-server` |
| RHEL / CentOS / AlmaLinux / Rocky | `dnf install mysql-server` | `dnf install mariadb-server` |
| Fedora | `dnf install community-mysql-server` | `dnf install mariadb-server` |
| openSUSE / SUSE SLES | `zypper install mysql-community-server` | `zypper install mariadb` |
| Arch / Manjaro | `pacman -S mysql` or `pacman -S mariadb` | same |
| Slackware | SlackBuilds.org/mariadb | — |
| FreeBSD | `pkg install mysql80-server` | `pkg install mariadb1011-server` |
| OpenBSD | `pkg_add mariadb-server` | same |
| macOS (Homebrew) | `brew install mysql` | `brew install mariadb` |
| macOS (MacPorts) | `port install mysql8` | `port install mariadb` |
| Generic binary | [dev.mysql.com/downloads](https://dev.mysql.com/downloads/mysql/) | [mariadb.org/download](https://mariadb.org/download/) |

Installing PostgreSQL by OS:

| OS | Install command |
|---|---|
| Debian / Ubuntu / Mint | `apt install postgresql` |
| RHEL / CentOS / AlmaLinux / Rocky | `dnf install postgresql-server && postgresql-setup --initdb` |
| Fedora | `dnf install postgresql-server` |
| openSUSE / SUSE SLES | `zypper install postgresql-server` |
| Arch / Manjaro | `pacman -S postgresql` |
| Slackware | SlackBuilds.org/postgresql |
| FreeBSD | `pkg install postgresql16-server` |
| OpenBSD | `pkg_add postgresql-server` |
| macOS (Homebrew) | `brew install postgresql@16` |
| macOS (MacPorts) | `port install postgresql16-server` |
| Generic | [postgresql.org/download](https://www.postgresql.org/download/) |

### Optional

- **[Redis 6+](https://redis.io/download/)** · recommended for production caching and sessions
- **[Solr 7.7 or 8.11.1+](https://solr.apache.org/downloads.html)** · for advanced full-text search (default engine is `legacy`)
- **[Varnish 6.0 or 7.1+](https://varnish-cache.org/releases/)** · for HTTP reverse-proxy caching
- **[ImageMagick](https://imagemagick.org/script/download.php)** · for advanced image processing (`IMAGEMAGICK_PATH` env var, default `/usr/bin`)

Installing optional services by OS:

| OS | Redis | Solr | Varnish | ImageMagick |
|---|---|---|---|---|
| Debian / Ubuntu / Mint | `apt install redis` | solr.apache.org tarball | `apt install varnish` | `apt install imagemagick` |
| RHEL / AlmaLinux / Rocky | `dnf install redis` (EPEL) | tarball | `dnf install varnish` (EPEL) | `dnf install ImageMagick` |
| Fedora | `dnf install redis` | tarball | `dnf install varnish` | `dnf install ImageMagick` |
| openSUSE / SUSE | `zypper install redis` | tarball | `zypper install varnish` | `zypper install ImageMagick` |
| Arch / Manjaro | `pacman -S redis` | AUR: solr | AUR: varnish | `pacman -S imagemagick` |
| Slackware | SlackBuilds | tarball | source | SlackBuilds |
| FreeBSD | `pkg install redis` | `pkg install solr` | `pkg install varnish` | `pkg install ImageMagick7` |
| OpenBSD | `pkg_add redis` | tarball | `pkg_add varnish` | `pkg_add ImageMagick` |
| macOS (Homebrew) | `brew install redis` | `brew install solr` | `brew install varnish` | `brew install imagemagick` |
| macOS (MacPorts) | `port install redis` | tarball | source | `port install ImageMagick` |
| Generic download | redis.io | solr.apache.org | varnish-cache.org | imagemagick.org |

### Full Requirements Summary

| Requirement | Minimum | Recommended |
|---|---|---|
| PHP | 8.0 | 8.3 or 8.5 |
| Composer | 2.x | latest 2.x |
| Node.js | 20 LTS | 20 LTS (via nvm) |
| Yarn | 1.x | 1.22.22 (corepack) |
| MySQL | 8.0 | 8.0+ (utf8mb4) |
| MariaDB | 10.3 | 10.6+ |
| PostgreSQL | 14 | 16+ |
| SQLite | 3.35 | 3.39+ (dev/testing) |
| Redis | 6.0 | 7.x (optional) |
| Solr | 7.7 | 8.11.x (optional) |
| Varnish | 6.0 | 7.1+ (optional) |
| Apache | 2.4 | 2.4 (event + PHP-FPM) |
| Nginx | 1.18 | 1.24+ |

---

## 2. Architecture Overview

```
Browser Request
      │
      ▼
   Web Server (Apache / Nginx)
      │
      ▼
  public/index.php (Symfony Entry Point)
      │
      └─── Symfony Kernel (Platform v4 OSS — Symfony 5.4 LTS)
                ├── Platform v4 Admin UI (/adminui/)
                ├── REST API v2 (/api/ezp/v2/)
                ├── GraphQL API (/graphql)
                └── Symfony/Twig site controllers
```

### Key Directories

```
project-root/
├── src/                   Your application code
├── templates/             Twig templates
├── config/                Symfony configuration
├── vendor/                PHP packages (composer-managed; not committed)
├── node_modules/          Node packages (yarn-managed; not committed)
├── public/                Web root
│   ├── assets/            Built frontend assets
│   └── bundles/           Symfony public assets (symlinked by assets:install)
└── var/                   Runtime cache, logs, sessions
```

---

## 3. First-Time Installation

### 3a. Composer create-project (recommended)

```bash
composer create-project se7enxweb/exponential-platform-v4x-dxp-skeleton:4.6.x-dev my-project
cd my-project
```

Composer will:
1. Download all PHP packages
2. Run Symfony Flex recipes
3. Execute `post-install-cmd` scripts:
   - `assets:install` — publishes bundle `public/` assets to `public/bundles/`
   - `cache:clear` — warms up the initial cache

> 💾 **Git Save Point 1 — Project created**
> ```bash
> git init && git add -A
> git commit -m "chore(init): composer create-project exponential-platform-v4x-dxp-skeleton 4.6.x-dev"
> ```

Continue from [Section 4](#4-environment-configuration-envlocal).

---

### 3b. GitHub git clone (developers)

```bash
git clone git@github.com:se7enxweb/exponential-platform-dxp-skeleton.git
cd exponential-platform-dxp-skeleton
git checkout 4.6
```

#### Step 1 — Install PHP dependencies

```bash
composer install --keep-vcs
```

> 💾 **Git Save Point 1 — Vendors installed**
> ```bash
> git add composer.lock && git commit -m "chore(install): lock vendor dependencies"
> ```

#### Step 2 — Configure environment

See [Section 4](#4-environment-configuration-envlocal).

#### Step 3 — Create the database

See [Section 5](#5-database-setup).

#### Step 4 — Set permissions

See [Section 7](#7-file--directory-permissions).

#### Step 5 — Build frontend assets

```bash
source ~/.nvm/nvm.sh && nvm use 20
corepack enable
yarn install
yarn dev
```

#### Step 6 — Build Admin UI assets

```bash
php bin/console assets:install --symlink --relative public
yarn ibexa:build
```

#### Step 7 — Generate JWT keypair

```bash
php bin/console lexik:jwt:generate-keypair
```

#### Step 8 — Generate GraphQL schema

```bash
php bin/console ibexa:graphql:generate-schema
```

#### Step 9 — Clear all caches

```bash
php bin/console cache:clear
```

#### Step 10 — Reindex search

```bash
php bin/console ezplatform:reindex
```

> 💾 **Git Save Point 2 — Installation complete**
> ```bash
> git add -A
> git commit -m "chore(install): platform v4 DXP install complete"
> ```

#### Step 11 — Start the dev server

```bash
symfony server:start
```

Access points after install:

| URL | What you get |
|---|---|
| `https://127.0.0.1:8000/` | Public site (Twig) |
| `https://127.0.0.1:8000/adminui/` | Platform v4 Admin UI (React) |
| `https://127.0.0.1:8000/api/ezp/v2/` | REST API v2 |
| `https://127.0.0.1:8000/graphql` | GraphQL endpoint |

---

## 4. Environment Configuration (.env.local)

**Never commit `.env.local`.** It overrides `.env` with host-specific secrets.

```bash
cp .env .env.local
$EDITOR .env.local
```

### Minimum required variables

```dotenv
# Application
APP_ENV=prod             # or dev
APP_SECRET=<random-32-char-hex-string>

# Database — MySQL / MariaDB
DATABASE_DRIVER=pdo_mysql
DATABASE_HOST=127.0.0.1
DATABASE_PORT=3306
DATABASE_NAME=your_db_name
DATABASE_USER=your_db_user
DATABASE_PASSWORD=your_db_password
DATABASE_CHARSET=utf8mb4
DATABASE_COLLATION=utf8mb4_unicode_520_ci
DATABASE_VERSION=mariadb-10.6.0

# JWT (REST API authentication)
JWT_SECRET_KEY=%kernel.project_dir%/config/jwt/private.pem
JWT_PUBLIC_KEY=%kernel.project_dir%/config/jwt/public.pem
JWT_PASSPHRASE=<random-64-char-hex-string>
```

### PostgreSQL (alternative to MySQL)

```dotenv
DATABASE_DRIVER=pdo_pgsql
DATABASE_HOST=127.0.0.1
DATABASE_PORT=5432
DATABASE_NAME=your_db_name
DATABASE_USER=your_db_user
DATABASE_PASSWORD=your_db_password
DATABASE_CHARSET=utf8
DATABASE_VERSION=16
```

### SQLite (zero-config alternative — dev / testing)

```dotenv
DATABASE_URL="sqlite:///%kernel.project_dir%/var/data_%kernel.environment%.db"
MESSENGER_TRANSPORT_DSN=sync://
```

### Search engine

```dotenv
SEARCH_ENGINE=legacy       # default
# SEARCH_ENGINE=solr
```

### HTTP cache

```dotenv
HTTPCACHE_PURGE_TYPE=local
HTTPCACHE_DEFAULT_TTL=86400
HTTPCACHE_PURGE_SERVER=http://localhost:80
```

### Application cache backend

```dotenv
CACHE_POOL=cache.tagaware.filesystem
# CACHE_POOL=cache.redis
# CACHE_DSN=redis://localhost:6379
```

### Mail

```dotenv
MAILER_DSN=null://null
```

### Other

```dotenv
IMAGEMAGICK_PATH=/usr/bin
CORS_ALLOW_ORIGIN='^https?://(localhost|127\.0\.0\.1)(:[0-9]+)?$'
SESSION_HANDLER_ID=session.handler.native_file
SESSION_SAVE_PATH=%kernel.project_dir%/var/sessions/%kernel.environment%
```

---

## 5. Database Setup

### Create the database

```sql
-- MySQL / MariaDB
CREATE DATABASE exponential
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_520_ci;

GRANT ALL PRIVILEGES ON exponential.* TO 'your_db_user'@'localhost'
  IDENTIFIED BY 'your_db_password';
FLUSH PRIVILEGES;
```

```bash
# PostgreSQL
psql -U postgres -c "CREATE DATABASE exponential ENCODING 'UTF8';"
```

### Import schema and demo data

```bash
php bin/console ibexa:install exponential-oss
```

The demo data creates an administrator user:
- **Username:** `admin`
- **Password:** `publish`

**Change the admin password immediately** after installation.

> 💾 **Git Save Point — Database provisioned**
> ```bash
> git commit --allow-empty -m "chore(install): database created and demo data imported"
> ```

### Run Doctrine migrations (on updates)

```bash
php bin/console doctrine:migration:migrate --allow-no-migration
```

---

### 5c. SQLite (zero-config database)

#### Step 1 — Verify PHP extensions

```bash
php -m | grep -i sqlite
# Expected: SQLite3 and pdo_sqlite
```

#### Step 2 — Configure `.env.local`

```dotenv
DATABASE_URL="sqlite:///%kernel.project_dir%/var/data_%kernel.environment%.db"
MESSENGER_TRANSPORT_DSN=sync://
```

#### Step 3 — Run the install command

```bash
php bin/console ibexa:install exponential-oss
```

#### Step 4 — Fix file permissions

```bash
chmod 664 var/data_dev.db
chown $USER:www-data var/data_dev.db
```

#### Step 5 — Clear caches

```bash
php bin/console cache:clear
```

> 💾 **Git Save Point — SQLite install complete**
> ```bash
> git commit --allow-empty -m "chore(install): sqlite database provisioned for dev"
> ```

---

## 6. Web Server Setup

### 6a. Apache 2.4

```bash
a2enmod rewrite deflate headers expires
```

Example virtual host:

```apache
<VirtualHost *:80>
    ServerName exponential.local
    DocumentRoot /var/www/exponential/public
    DirectoryIndex index.php

    <Directory /var/www/exponential/public>
        AllowOverride None
        Require all granted
        FallbackResource /index.php
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/exponential_error.log
    CustomLog ${APACHE_LOG_DIR}/exponential_access.log combined
</VirtualHost>
```

### 6b. Nginx

```nginx
server {
    listen 80;
    server_name exponential.local;
    root /var/www/exponential/public;
    index index.php;

    location / {
        try_files $uri /index.php$is_args$args;
    }

    location ~ ^/index\.php(/|$) {
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        fastcgi_param DOCUMENT_ROOT $realpath_root;
        fastcgi_param APP_ENV prod;
        fastcgi_param APP_DEBUG 0;
        internal;
    }

    location ~ \.php$ { return 404; }

    error_log /var/log/nginx/exponential_error.log;
    access_log /var/log/nginx/exponential_access.log;
}
```

### 6c. Symfony CLI (development only)

```bash
symfony server:start               # HTTPS dev server on https://127.0.0.1:8000
symfony server:start -d            # run in background
symfony server:stop                # stop background server
symfony server:log                 # tail server log
```

---

## 7. File & Directory Permissions

Replace `www-data` with your actual web server user.

```bash
# Symfony runtime directories
setfacl -R  -m u:www-data:rwX -m g:www-data:rwX var/
setfacl -dR -m u:www-data:rwX -m g:www-data:rwX var/

# Platform v4 new-stack public var directory
setfacl -R  -m u:www-data:rwX -m g:www-data:rwX public/var/
setfacl -dR -m u:www-data:rwX -m g:www-data:rwX public/var/
```

If [setfacl](https://savannah.nongnu.org/projects/acl/) is unavailable, install the `acl` package first:

| OS | Install command |
|---|---|
| Debian / Ubuntu / Mint / Pop!_OS | `apt install acl` |
| RHEL / CentOS / AlmaLinux / Rocky | `dnf install acl` |
| Fedora | `dnf install acl` |
| openSUSE / SUSE SLES | `zypper install acl` |
| Arch / Manjaro | `pacman -S acl` |
| FreeBSD | built in — mount filesystem with `-o acls` |
| macOS | ACLs are enabled by default; use `chmod +a` syntax instead |
| Slackware | included in core — ensure filesystem is mounted with `acl` option |

If ACLs are not available on your filesystem (NFS, some BSD mounts, macOS APFS):

```bash
chown -R www-data:www-data var/ public/var/
chmod -R 775 var/ public/var/
```

> **Note for development:** If your CLI user and web server user differ, the ACL approach lets both write simultaneously. This avoids `Permission denied` errors when alternating between `php bin/console` (CLI) and web requests (`www-data`).

---

## 8. Frontend Assets (Site CSS/JS)

```bash
source ~/.nvm/nvm.sh && nvm use 20
corepack enable
```

### Install Node dependencies

```bash
yarn install
```

### Build for development (with source maps)

```bash
yarn dev
```

### Build for production (minified)

```bash
yarn build
```

### Watch mode

```bash
yarn watch
```

---

## 9. Admin UI Assets (Platform v4 Admin UI)

### Prerequisites

```bash
php bin/console assets:install --symlink --relative public
```

### Build Admin UI assets — production

```bash
yarn ibexa:build
```

### Build Admin UI assets — development

```bash
yarn ibexa:dev
```

### Watch mode

```bash
yarn ibexa:watch
```

### Dump JS translation assets

```bash
php bin/console bazinga:js-translation:dump public/assets --merge-domains
```

### What changes require an Admin UI asset rebuild

| Changed | Rebuild needed |
|---|---|
| `composer update` pulled a new `se7enxweb/admin-ui` version | Yes — `yarn ibexa:build` |
| Any bundle's `Resources/public/` JS or SCSS | Yes — `yarn ibexa:build` |
| `webpack.config.js` or `ibexa.webpack.config.manager.js` | Yes — `yarn ibexa:build` |
| Admin richtext editor configuration | Yes — `yarn ibexa:build` |
| Translation strings changed | Yes — dump translations |

---

## 10. JWT Authentication (REST API)

```bash
php bin/console lexik:jwt:generate-keypair
# Writes:
#   config/jwt/private.pem
#   config/jwt/public.pem
```

On key rotation:

```bash
php bin/console lexik:jwt:generate-keypair --overwrite
php bin/console cache:clear
```

---

## 11. GraphQL Schema

Run after any content type or field type changes:

```bash
php bin/console ibexa:graphql:generate-schema
```

---

## 12. Search Index

### Full reindex

```bash
php bin/console ezplatform:reindex
```

### Incremental reindex

```bash
php bin/console ezplatform:reindex --iteration-count=100
```

### Reindex a specific content type

```bash
php bin/console ezplatform:reindex --content-type=article
```

---

## 13. Image Variations

### Clear generated variation cache

```bash
php bin/console liip:imagine:cache:remove
php bin/console cache:clear
```

### Example variation configuration

```yaml
# config/packages/ibexa.yaml
ibexa:
    system:
        site_group:
            image_variations:
                small:
                    reference: ~
                    filters:
                        - { name: geometry/scaledownonly, params: [160, 120] }
                medium:
                    reference: ~
                    filters:
                        - { name: geometry/scaledownonly, params: [480, 360] }
                large:
                    reference: ~
                    filters:
                        - { name: geometry/scaledownonly, params: [960, 720] }
```

---

## 14. Cache Management

### Clear Symfony application cache

```bash
php bin/console cache:clear
php bin/console cache:clear --env=prod
```

### Warm up cache (production)

```bash
php bin/console cache:warmup --env=prod
```

### Clear a specific cache pool

```bash
php bin/console cache:pool:clear cache.redis
php bin/console cache:pool:clear cache.tagaware.filesystem
```

### Purge HTTP cache

```bash
php bin/console fos:httpcache:invalidate:path / --all
php bin/console fos:httpcache:invalidate:tag <tag>
```

### Nuclear option (development)

```bash
rm -rf var/cache/dev var/cache/prod
php bin/console cache:warmup --env=prod
```

---

## 15. Day-to-Day Operations: Start / Stop / Restart

### Apache

```bash
systemctl start apache2
systemctl stop apache2
systemctl restart apache2
systemctl reload apache2
```

### Nginx

```bash
systemctl start nginx
systemctl stop nginx
systemctl reload nginx
```

### PHP-FPM

```bash
systemctl restart php8.3-fpm
systemctl reload php8.3-fpm
```

### Redis (if used)

```bash
systemctl start redis
systemctl restart redis
```

### Symfony CLI dev server

```bash
symfony server:start -d
symfony server:stop
symfony server:log
symfony server:status
```

### After deploying code changes (production checklist)

```bash
# 1. Pull code
git pull --rebase

# 2. Install/update vendors
composer install --no-dev -o

# 3. Run Doctrine migrations
php bin/console doctrine:migration:migrate --allow-no-migration --env=prod

# 4. Publish bundle public assets
php bin/console assets:install --symlink --relative public --env=prod

# 5. Rebuild Platform v4 Admin UI assets (if admin-ui bundle updated)
source ~/.nvm/nvm.sh && nvm use 20 && yarn ibexa:build

# 6. Rebuild frontend site assets (if theme/JS/CSS changed)
yarn build

# 7. Dump JS translations
php bin/console bazinga:js-translation:dump public/assets --merge-domains --env=prod

# 8. Clear & warm up caches
php bin/console cache:clear --env=prod
php bin/console cache:warmup --env=prod

# 9. Reindex search (if content model changed)
# php bin/console ezplatform:reindex --env=prod
```

---

## 16. Updating the Codebase

### Pull latest code and rebuild

```bash
git pull --rebase
composer install
php bin/console doctrine:migration:migrate --allow-no-migration
php bin/console cache:clear
```

### Update Composer packages

```bash
composer update
composer update se7enxweb/exponential-platform-dxp

# After update, always run:
php bin/console doctrine:migration:migrate --allow-no-migration
php bin/console cache:clear
php bin/console ezplatform:reindex
```

### Update Node packages

```bash
yarn upgrade
yarn dev
```

---

## 17. Cron Jobs

Add to crontab (`crontab -e -u www-data`):

```cron
# Platform v4 cron runner (every 5 minutes)
*/5 * * * * /usr/bin/php /var/www/exponential/bin/console ezplatform:cron:run --env=prod >> /var/log/exponential-cron.log 2>&1
```

---

## 18. Solr Search Engine (optional)

### Switch from legacy to Solr

1. Set `SEARCH_ENGINE=solr` and `SOLR_DSN`/`SOLR_CORE` in `.env.local`
2. Clear cache: `php bin/console cache:clear`
3. Provision the Solr core:
   ```bash
   php bin/console ezplatform:solr:create-core --cores=default
   ```
4. Reindex all content:
   ```bash
   php bin/console ezplatform:reindex
   ```

### Switch back to legacy search

```dotenv
SEARCH_ENGINE=legacy
```

```bash
php bin/console cache:clear
```

---

## 19. Varnish HTTP Cache (optional)

1. Set env vars in `.env.local`:
   ```dotenv
   HTTPCACHE_PURGE_TYPE=varnish
   HTTPCACHE_PURGE_SERVER=http://127.0.0.1:6081
   HTTPCACHE_VARNISH_INVALIDATE_TOKEN=<your-secret>
   TRUSTED_PROXIES=127.0.0.1
   ```
2. Set `APP_HTTP_CACHE=0` in your web server vhost.
3. Clear cache after any VCL change:
   ```bash
   php bin/console cache:clear
   php bin/console fos:httpcache:invalidate:path / --all
   ```

---

## 20. Troubleshooting

### White screen / 500 error

```bash
tail -f var/log/dev.log
tail -f var/log/prod.log
APP_ENV=dev php bin/console cache:clear
```

### "Class not found" after composer update

```bash
composer dump-autoload -o
php bin/console cache:clear
```

### Assets not loading (404 on /bundles/ or /assets/)

```bash
php bin/console assets:install --symlink --relative public
yarn dev
yarn ibexa:build
```

### `yarn ibexa:build` fails with "Module not found"

```bash
php bin/console assets:install --symlink --relative public
yarn ibexa:build
```

### Cache not clearing / stale content

```bash
php bin/console cache:clear
rm -rf var/cache/dev var/cache/prod
php bin/console cache:warmup --env=prod
```

### Image variations missing

```bash
php bin/console liip:imagine:cache:remove
php bin/console cache:clear
```

### Search results outdated

```bash
php bin/console ezplatform:reindex
```

### Permission denied on var/ or public/var/

```bash
setfacl -R  -m u:www-data:rwX -m g:www-data:rwX var/ public/var/
setfacl -dR -m u:www-data:rwX -m g:www-data:rwX var/ public/var/
```

### JWT authentication errors (REST API)

```bash
php bin/console lexik:jwt:generate-keypair --overwrite
php bin/console cache:clear
```

---

## 21. Database Conversion

This section covers converting an existing, running Exponential Platform DXP application from one database engine to another using free and open-source tools only.

All tools listed below are either:
- distributed under OSI-approved open-source licences (MIT, GPL, BSD, Apache 2.0), or
- free CLI utilities included with the database server packages.

> **Before you start — backup everything.**
> ```bash
> # MySQL / MariaDB
> mysqldump -u "$DATABASE_USER" -p"$DATABASE_PASSWORD" "$DATABASE_NAME" > backup_$(date +%Y%m%d).sql
> # PostgreSQL
> pg_dump -U "$DATABASE_USER" "$DATABASE_NAME" > backup_$(date +%Y%m%d).sql
> # SQLite
> cp var/data_dev.db var/data_dev.db.bak
> # Also backup var/ and your .env.local
> cp .env.local .env.local.bak
> ```

### Tool inventory

All tools are free and open-source. Download links and cross-platform install commands are provided for every tool.

#### `mysqldump` / `mysql` CLI

Download: [dev.mysql.com/downloads/mysql](https://dev.mysql.com/downloads/mysql/) — bundled with every MySQL and MariaDB server package.

| OS | Install command |
|---|---|
| Debian / Ubuntu / Mint | `apt install default-mysql-client` |
| RHEL / CentOS / AlmaLinux / Rocky | `dnf install mysql` |
| Fedora | `dnf install community-mysql` |
| openSUSE / SUSE | `zypper install mysql-client` |
| Arch / Manjaro | `pacman -S mysql-clients` |
| Slackware | included with MariaDB SlackBuild |
| FreeBSD | `pkg install mysql80-client` |
| OpenBSD | `pkg_add mariadb-client` |
| macOS (Homebrew) | `brew install mysql-client` — then `echo 'export PATH="$(brew --prefix mysql-client)/bin:$PATH"' >> ~/.zshrc` |
| macOS (MacPorts) | `port install mysql8` |
| Generic | dev.mysql.com/downloads/mysql |

#### `pg_dump` / `psql`

Download: [postgresql.org/download](https://www.postgresql.org/download/) — bundled with PostgreSQL server packages.

| OS | Install command |
|---|---|
| Debian / Ubuntu / Mint | `apt install postgresql-client` |
| RHEL / CentOS / AlmaLinux / Rocky | `dnf install postgresql` |
| Fedora | `dnf install postgresql` |
| openSUSE / SUSE | `zypper install postgresql-client` |
| Arch / Manjaro | `pacman -S postgresql-libs` |
| Slackware | included with PostgreSQL SlackBuild |
| FreeBSD | `pkg install postgresql16-client` |
| OpenBSD | `pkg_add postgresql-client` |
| macOS (Homebrew) | `brew install libpq` — then `echo 'export PATH="$(brew --prefix libpq)/bin:$PATH"' >> ~/.zshrc` |
| macOS (MacPorts) | `port install postgresql16` |
| Generic | postgresql.org/download |

#### `sqlite3` CLI

Download: [sqlite.org/download.html](https://www.sqlite.org/download.html) — pre-built binaries for Linux, macOS, Windows.

| OS | Install command |
|---|---|
| Debian / Ubuntu / Mint | `apt install sqlite3` |
| RHEL / CentOS / AlmaLinux / Rocky | `dnf install sqlite` |
| Fedora | `dnf install sqlite` |
| openSUSE / SUSE | `zypper install sqlite3` |
| Arch / Manjaro | `pacman -S sqlite` |
| Slackware | included in core |
| FreeBSD | `pkg install sqlite3` |
| OpenBSD | `pkg_add sqlite3` |
| macOS | pre-installed on all versions |
| Generic | sqlite.org/download.html |

#### pgloader

Download / docs: [pgloader.io](https://pgloader.io/) · source: [github.com/dimitri/pgloader](https://github.com/dimitri/pgloader) · Licence: PostgreSQL (BSD-like)

| OS | Install command |
|---|---|
| Debian / Ubuntu / Mint | `apt install pgloader` |
| RHEL / AlmaLinux / Rocky | build from source (see below) or use the Docker image |
| Fedora | `dnf install pgloader` |
| openSUSE / SUSE | build from source |
| Arch / Manjaro | AUR: `yay -S pgloader` |
| Slackware | build from source |
| FreeBSD | `pkg install pgloader` |
| macOS (Homebrew) | `brew install pgloader` |
| Docker (any OS) | `docker run --rm -it dimitri/pgloader:latest pgloader <args>` |
| Generic / source | `git clone https://github.com/dimitri/pgloader && cd pgloader && make pgloader` (requires SBCL) |

#### pgcopydb

Download: [github.com/dimitri/pgcopydb/releases](https://github.com/dimitri/pgcopydb/releases) · Licence: PostgreSQL (BSD-like)

| OS | Install command |
|---|---|
| Debian / Ubuntu / Mint | `apt install pgcopydb` (Debian 12+ / Ubuntu 22.04+) |
| RHEL / AlmaLinux / Rocky | pre-built RPM at github.com/dimitri/pgcopydb/releases |
| Fedora | pre-built RPM from releases page |
| openSUSE / SUSE | build from source |
| Arch / Manjaro | AUR: `yay -S pgcopydb` |
| FreeBSD | build from source |
| macOS (Homebrew) | `brew install pgcopydb` |
| Docker (any OS) | `docker run --rm -it dimitri/pgcopydb pgcopydb <args>` |
| Generic binary | github.com/dimitri/pgcopydb/releases |

#### mysql2sqlite

Download: [github.com/dumblob/mysql2sqlite](https://github.com/dumblob/mysql2sqlite) · Licence: MIT · single shell script, no compiled dependencies beyond `bash` and `sqlite3`.

```bash
# Works on any UNIX / macOS / BSD with bash + sqlite3
curl -LO https://raw.githubusercontent.com/dumblob/mysql2sqlite/master/mysql2sqlite
chmod +x mysql2sqlite
```

#### sqlite3-to-mysql

Download / docs: [github.com/techouse/sqlite3-to-mysql](https://github.com/techouse/sqlite3-to-mysql) · Licence: MIT · Python package, requires Python 3.8+.

| OS | Install command |
|---|---|
| Debian / Ubuntu / Mint | `apt install python3 python3-pip && pip3 install sqlite3-to-mysql` |
| RHEL / AlmaLinux / Rocky | `dnf install python3 python3-pip && pip3 install sqlite3-to-mysql` |
| Fedora | `dnf install python3 && pip3 install sqlite3-to-mysql` |
| openSUSE / SUSE | `zypper install python3 python3-pip && pip3 install sqlite3-to-mysql` |
| Arch / Manjaro | `pacman -S python && pip install sqlite3-to-mysql` |
| Slackware | Python included in full install; `pip3 install sqlite3-to-mysql` |
| FreeBSD | `pkg install python3 && pip3 install sqlite3-to-mysql` |
| OpenBSD | `pkg_add python3 && pip3 install sqlite3-to-mysql` |
| macOS | Python pre-installed on macOS 12+; `pip3 install sqlite3-to-mysql` |
| Generic | pypi.org/project/sqlite3-to-mysql |

#### pgslice (optional — large table partitioning)

Download: [github.com/ankane/pgslice](https://github.com/ankane/pgslice) · Licence: MIT · Ruby gem.

| OS | Install command |
|---|---|
| Debian / Ubuntu / Mint | `apt install ruby && gem install pgslice` |
| RHEL / AlmaLinux / Rocky | `dnf install ruby && gem install pgslice` |
| Fedora | `dnf install ruby && gem install pgslice` |
| openSUSE / SUSE | `zypper install ruby && gem install pgslice` |
| Arch / Manjaro | `pacman -S ruby && gem install pgslice` |
| FreeBSD | `pkg install ruby && gem install pgslice` |
| macOS | Ruby pre-installed; `gem install pgslice` |
| Generic | rubygems.org/gems/pgslice |

[pgloader](https://pgloader.io/) is the most capable single tool: it can load MySQL → PostgreSQL in one command and has a SQLite → PostgreSQL mode. Install it first; it covers most conversion paths.

Quick install summary by OS family:

```bash
# Debian / Ubuntu / Mint / Pop!_OS
apt install default-mysql-client postgresql-client sqlite3 pgloader pgcopydb python3-pip ruby
pip3 install sqlite3-to-mysql
gem install pgslice
curl -LO https://raw.githubusercontent.com/dumblob/mysql2sqlite/master/mysql2sqlite && chmod +x mysql2sqlite

# RHEL / CentOS / AlmaLinux / Rocky Linux (EPEL required)
dnf install epel-release
dnf install mysql postgresql sqlite pgloader python3-pip ruby
pip3 install sqlite3-to-mysql
gem install pgslice
curl -LO https://raw.githubusercontent.com/dumblob/mysql2sqlite/master/mysql2sqlite && chmod +x mysql2sqlite

# Fedora
dnf install community-mysql postgresql sqlite pgloader python3-pip ruby
pip3 install sqlite3-to-mysql
gem install pgslice
curl -LO https://raw.githubusercontent.com/dumblob/mysql2sqlite/master/mysql2sqlite && chmod +x mysql2sqlite

# openSUSE / SUSE SLES
zypper install mysql-client postgresql-client sqlite3 python3-pip ruby
pip3 install sqlite3-to-mysql
gem install pgslice
curl -LO https://raw.githubusercontent.com/dumblob/mysql2sqlite/master/mysql2sqlite && chmod +x mysql2sqlite
# pgloader: brew or Docker

# Arch Linux / Manjaro
pacman -S mysql-clients postgresql sqlite python ruby
pip install sqlite3-to-mysql
gem install pgslice
yay -S pgloader pgcopydb
curl -LO https://raw.githubusercontent.com/dumblob/mysql2sqlite/master/mysql2sqlite && chmod +x mysql2sqlite

# macOS (Homebrew)
brew install mysql-client libpq pgloader pgcopydb python3 ruby sqlite3
pip3 install sqlite3-to-mysql
gem install pgslice
curl -LO https://raw.githubusercontent.com/dumblob/mysql2sqlite/master/mysql2sqlite && chmod +x mysql2sqlite

# FreeBSD
pkg install mysql80-client postgresql16-client sqlite3 pgloader pgcopydb python3 ruby
pip3 install sqlite3-to-mysql
gem install pgslice
curl -LO https://raw.githubusercontent.com/dumblob/mysql2sqlite/master/mysql2sqlite && chmod +x mysql2sqlite

# Docker (distro-agnostic)
docker pull dimitri/pgloader:latest
docker pull dimitri/pgcopydb:latest
```

---

### 21a. Any → SQLite (go to SQLite)

#### From MySQL / MariaDB → SQLite

Use the [mysql2sqlite](https://github.com/dumblob/mysql2sqlite) shell script (MIT licence, no dependencies beyond `bash` and `sqlite3`):

```bash
# 1. Get the script
curl -LO https://raw.githubusercontent.com/dumblob/mysql2sqlite/master/mysql2sqlite
chmod +x mysql2sqlite

# 2. Dump the MySQL database through the converter and pipe into SQLite
mysqldump --no-tablespaces --skip-extended-insert --compact \
  -u "$DATABASE_USER" -p"$DATABASE_PASSWORD" \
  -h "$DATABASE_HOST" "$DATABASE_NAME" \
  | ./mysql2sqlite - | sqlite3 var/data_dev.db
```

`--skip-extended-insert` produces one `INSERT` per row — slower but required by the converter. For large databases, export table-by-table to avoid memory pressure:

```bash
# list tables
TABLES=$(mysql -u "$DB_USER" -p"$DB_PASS" "$DB_NAME" -e 'SHOW TABLES;' --batch --skip-column-names)

for TABLE in $TABLES; do
  mysqldump --no-tablespaces --skip-extended-insert --compact \
    -u "$DB_USER" -p"$DB_PASS" "$DB_NAME" "$TABLE" \
    | ./mysql2sqlite - >> /tmp/dump.sql
done

sqlite3 var/data_dev.db < /tmp/dump.sql
```

#### From PostgreSQL → SQLite

Use [pgloader](https://pgloader.io/) (PostgreSQL-licenced):

```bash
apt install pgloader          # Debian/Ubuntu
# or: brew install pgloader  # macOS

# Create an empty target file
touch var/data_dev.db

# Write a pgloader command file
cat > /tmp/pg_to_sqlite.load <<'EOF'
LOAD DATABASE
  FROM postgresql://db_user:db_pass@127.0.0.1/db_name
  INTO sqlite:///{{ project_dir }}/var/data_dev.db

WITH include no drop, create tables, create indexes, reset sequences

SET work_mem TO '128MB', maintenance_work_mem TO '512MB';
EOF

# Replace {{ project_dir }} with the actual absolute path
sed -i "s|{{ project_dir }}|$(pwd)|g" /tmp/pg_to_sqlite.load

pgloader /tmp/pg_to_sqlite.load
```

#### After migrating to SQLite — update .env.local

```dotenv
DATABASE_URL="sqlite:///%kernel.project_dir%/var/data_dev.db"
MESSENGER_TRANSPORT_DSN=sync://
```

Fix permissions and clear caches (see Section 5c steps 4–5).

---

### 21b. SQLite → MySQL / MariaDB

Create the target database first (see Section 5 — Database Setup).

#### Method 1 — [sqlite3-to-mysql](https://github.com/techouse/sqlite3-to-mysql) (Python, MIT)

```bash
pip install sqlite3-to-mysql

sqlite3mysql \
  --sqlite-file var/data_dev.db \
  --mysql-database "$DATABASE_NAME" \
  --mysql-user "$DATABASE_USER" \
  --mysql-password "$DATABASE_PASSWORD" \
  --mysql-host "$DATABASE_HOST" \
  --mysql-port 3306 \
  --chunk 1000
# --chunk 1000 inserts 1000 rows per batch — tune for your server
```

Verify row counts match:

```bash
# SQLite side
sqlite3 var/data_dev.db "SELECT name, COUNT(*) FROM sqlite_master WHERE type='table' GROUP BY name ORDER BY name;"

# MySQL side
mysql -u "$DATABASE_USER" -p"$DATABASE_PASSWORD" "$DATABASE_NAME" \
  -e "SELECT table_name, table_rows FROM information_schema.tables
      WHERE table_schema='$DATABASE_NAME' ORDER BY table_name;"
```

#### Method 2 — manual SQL dump + sed fixes

SQLite's `.dump` produces ANSI SQL that MySQL almost accepts. The main differences are quoting style and type names:

```bash
# 1. Dump from SQLite
sqlite3 var/data_dev.db .dump > /tmp/sqlite_dump.sql

# 2. Strip SQLite-specific preamble that MySQL rejects
sed -i '/^PRAGMA/d; /^BEGIN TRANSACTION/d; /^COMMIT/d; /^CREATE UNIQUE INDEX/d' /tmp/sqlite_dump.sql

# 3. Convert double-quoted identifiers to backtick-quoted
sed -i 's/"\([a-zA-Z_][a-zA-Z0-9_]*\)"/`\1`/g' /tmp/sqlite_dump.sql

# 4. Map SQLite types to MySQL equivalents
sed -i 's/INTEGER PRIMARY KEY AUTOINCREMENT/INT NOT NULL AUTO_INCREMENT PRIMARY KEY/g' /tmp/sqlite_dump.sql
sed -i 's/ BOOLEAN/ TINYINT(1)/g; s/ DATETIME/ DATETIME/g' /tmp/sqlite_dump.sql

# 5. Import
mysql -u "$DATABASE_USER" -p"$DATABASE_PASSWORD" "$DATABASE_NAME" < /tmp/sqlite_dump.sql
```

> The sed approach is fragile on complex schemas. Prefer `sqlite3-to-mysql` for production data migrations.

#### After migrating — update .env.local

```dotenv
DATABASE_DRIVER=pdo_mysql
DATABASE_HOST=127.0.0.1
DATABASE_PORT=3306
DATABASE_NAME=your_db_name
DATABASE_USER=your_db_user
DATABASE_PASSWORD=your_db_password
DATABASE_CHARSET=utf8mb4
DATABASE_COLLATION=utf8mb4_unicode_520_ci
DATABASE_VERSION=mariadb-10.6.0   # or MySQL version e.g. 8.0
# Remove or comment out DATABASE_URL and MESSENGER_TRANSPORT_DSN=sync://
```

---

### 21c. SQLite → PostgreSQL

Use [pgloader](https://pgloader.io/) — it has native SQLite source support:

```bash
apt install pgloader

# Create the target database
psql -U postgres -c "CREATE DATABASE exponential ENCODING 'UTF8';"

# Write a pgloader command file
cat > /tmp/sqlite_to_pg.load <<EOF
LOAD DATABASE
  FROM sqlite:///$(pwd)/var/data_dev.db
  INTO postgresql://pg_user:pg_pass@127.0.0.1/exponential

WITH include no drop, create tables, create indexes, reset sequences;
EOF

pgloader /tmp/sqlite_to_pg.load
```

pgloader handles:
- Type mapping (`INTEGER` → `bigint`, `TEXT` → `text`, `REAL` → `double precision`)
- Index creation
- Sequence reset so `nextval()` picks up after the highest existing ID

#### After migrating — update .env.local

```dotenv
DATABASE_DRIVER=pdo_pgsql
DATABASE_HOST=127.0.0.1
DATABASE_PORT=5432
DATABASE_NAME=exponential
DATABASE_USER=pg_user
DATABASE_PASSWORD=pg_pass
DATABASE_CHARSET=utf8
DATABASE_VERSION=16
# Remove DATABASE_URL=sqlite:// and MESSENGER_TRANSPORT_DSN=sync://
```

---

### 21d. MySQL / MariaDB → PostgreSQL

Use [pgloader](https://pgloader.io/) — this is its primary, most mature use-case:

```bash
apt install pgloader

# Create target DB
psql -U postgres -c "CREATE DATABASE exponential ENCODING 'UTF8';"

# pgloader command file
cat > /tmp/mysql_to_pg.load <<'EOF'
LOAD DATABASE
  FROM      mysql://db_user:db_pass@127.0.0.1/source_db
  INTO      postgresql://pg_user:pg_pass@127.0.0.1/exponential

WITH include no drop,
     create tables,
     create indexes,
     reset sequences,
     foreign keys

SET work_mem TO '128MB'

EXCLUDING TABLE NAMES MATCHING ~/session/  -- optional: skip session tables

CAST
  -- MySQL enums → text (PostgreSQL enum DDL is cumbersome)
  column type matching ~/enum/ to text,
  -- tinyint(1) → boolean
  type tinyint to boolean using tinyint-to-boolean,
  -- longtext / mediumtext → text
  type longtext to text, type mediumtext to text,
  -- unsigned int
  type int with unsigned to bigint;
EOF

pgloader /tmp/mysql_to_pg.load
```

Verify row counts:

```bash
# MySQL total rows
mysql -u db_user -pdb_pass source_db \
  -e "SELECT SUM(table_rows) FROM information_schema.tables WHERE table_schema='source_db';"

# PostgreSQL total rows (approximate)
psql -U pg_user exponential \
  -c "SELECT schemaname, tablename, n_live_tup AS rows FROM pg_stat_user_tables ORDER BY tablename;"
```

#### After migrating — update .env.local

```dotenv
DATABASE_DRIVER=pdo_pgsql
DATABASE_HOST=127.0.0.1
DATABASE_PORT=5432
DATABASE_NAME=exponential
DATABASE_USER=pg_user
DATABASE_PASSWORD=pg_pass
DATABASE_CHARSET=utf8
DATABASE_VERSION=16
```

---

### 21e. PostgreSQL → MySQL / MariaDB

There is no single widely-adopted tool for this direction; the most reliable free approach is a `pg_dump` CSV export + MySQL `LOAD DATA` pipeline.

#### Step 1 — Export each table as CSV from PostgreSQL

```bash
TARGET_DIR=/tmp/pg_csv_export
mkdir -p "$TARGET_DIR"

# Get table list
TABLES=$(psql -U pg_user -d exponential -t \
  -c "SELECT tablename FROM pg_tables WHERE schemaname='public' ORDER BY tablename;")

for TABLE in $TABLES; do
  TABLE=$(echo "$TABLE" | xargs)  # trim whitespace
  psql -U pg_user -d exponential \
    -c "\COPY \"$TABLE\" TO '$TARGET_DIR/$TABLE.csv' WITH (FORMAT csv, HEADER true, NULL '\\N');"
done
```

#### Step 2 — DDL: dump structure from PostgreSQL, convert to MySQL

```bash
# pgloader can convert schema only (no data) into MySQL dialect:
cat > /tmp/schema_only.load <<'EOF'
LOAD DATABASE
  FROM      postgresql://pg_user:pg_pass@127.0.0.1/exponential
  INTO      mysql://db_user:db_pass@127.0.0.1/target_db

WITH include no drop, create tables, no data;
EOF

pgloader /tmp/schema_only.load
```

> **Note:** pgloader's MySQL target support is less mature than its PostgreSQL target. Inspect the generated schema and fix any `bytea`, `jsonb`, `array`, or custom-type columns manually.

#### Step 3 — Import CSVs into MySQL

```bash
for CSV in "$TARGET_DIR"/*.csv; do
  TABLE=$(basename "$CSV" .csv)
  # MySQL LOAD DATA requires the file to be on the server or use LOCAL keyword
  mysql --local-infile=1 \
    -u db_user -pdb_pass target_db \
    -e "LOAD DATA LOCAL INFILE '$CSV'
        INTO TABLE \`$TABLE\`
        FIELDS TERMINATED BY ','
        OPTIONALLY ENCLOSED BY '\"'
        LINES TERMINATED BY '\n'
        IGNORE 1 ROWS;"
done
```

Grant `local_infile` on the server if needed:

```sql
SET GLOBAL local_infile = 1;
```

#### After migrating — update .env.local

```dotenv
DATABASE_DRIVER=pdo_mysql
DATABASE_HOST=127.0.0.1
DATABASE_PORT=3306
DATABASE_NAME=target_db
DATABASE_USER=db_user
DATABASE_PASSWORD=db_pass
DATABASE_CHARSET=utf8mb4
DATABASE_COLLATION=utf8mb4_unicode_520_ci
DATABASE_VERSION=mariadb-10.6.0
```

---

### 21f. Any → Oracle (export only)

Oracle XE (Express Edition) is free to use but not open-source. The recommended free/open-source path for Oracle targets is [ora2pg](https://github.com/darold/ora2pg) (GPL v3) — the industry-standard open-source Oracle-to-PostgreSQL migration tool. Full Oracle migration is outside the scope of this guide.

For teams moving from Oracle to MySQL/PostgreSQL the recommended approach is:
1. Export via [ora2pg](https://github.com/darold/ora2pg) (GPL v3)
2. Then follow the PostgreSQL → MySQL path above if MySQL is the target.

```bash
apt install ora2pg

# Minimal ora2pg config (~/.ora2pg.conf or /etc/ora2pg/ora2pg.conf)
cat > /tmp/ora2pg.conf <<'EOF'
ORACLE_DSN  dbi:Oracle:host=oracle_host;sid=ORCL
ORACLE_USER system
ORACLE_PWD  password
SCHEMA      YOUR_SCHEMA
TYPE        TABLE, INSERT, SEQUENCE, INDEX, CONSTRAINT
OUT_FILE    /tmp/ora_export.sql
QUOTE_STRING_WITH_DOLLAR 0
EOF

ora2pg -c /tmp/ora2pg.conf
psql -U pg_user exponential < /tmp/ora_export.sql
```

---

### 21g. Post-conversion checklist

After any database engine switch, run through every item:

```bash
# 1. Update .env.local with the new DATABASE_URL or database vars
$EDITOR .env.local

# 2. Clear the Symfony container and cache (it caches the DBAL connection)
php bin/console cache:clear

# 3. Validate Doctrine entity mappings against the new DB
php bin/console doctrine:schema:validate

# 4. Run any pending Doctrine migrations
php bin/console doctrine:migration:migrate --allow-no-migration

# 5. Regenerate the GraphQL schema
php bin/console ibexa:graphql:generate-schema

# 6. Regenerate the search index against the new DB
php bin/console ezplatform:reindex

# 7. Smoke-test the site
curl -I http://localhost/
curl -I http://localhost/adminui/

# 8. If using SQLite as target — fix file permissions
# (skip for MySQL/PostgreSQL)
chmod 664 var/data_dev.db
chown "$USER":www-data var/data_dev.db
```

#### Common post-conversion issues

| Symptom | Likely cause | Fix |
|---|---|---|
| `SQLSTATE[42S02]: Base table not found` | Table created in source but not migrated | Run `doctrine:schema:validate` and check pgloader/mysql2sqlite log for errors |
| Binary/blob content garbled | Charset mismatch during export | Re-export with explicit `--default-character-set=utf8mb4` (mysqldump) or `CLIENT_ENCODING=UTF8` (psql) |
| Serialization failure (PostgreSQL) | Concurrent access during import | Import with `APP_ENV=dev` and no web traffic; use a maintenance window |
| Image variation 404s | `ezcontentobject_attribute` row count mismatch | Verify row counts between source and target; re-run data transfer for that table |
| `SQLite: attempt to write a readonly database` | Web server user cannot write the `.db` file | `chmod 664 var/data_*.db && chown $USER:www-data var/data_*.db` |

> 💾 **Git Save Point — database conversion complete**
> ```bash
> git add .env.local.bak   # keep the backup of the old config as a reference
> git commit -m "chore(db): convert database from <source> to <target>"
> ```

---

## 22. Complete CLI Reference

### 22.1 Symfony Core

```bash
php bin/console list                                          # list all commands
php bin/console help <command>                                # help for any command
php bin/console cache:clear                                   # clear current APP_ENV cache
php bin/console cache:clear --env=prod                        # clear production cache
php bin/console cache:warmup --env=prod                       # warm up production cache
php bin/console cache:pool:clear cache.redis                  # clear a named cache pool
php bin/console assets:install --symlink --relative public    # publish bundle assets
php bin/console debug:router                                  # list all routes
php bin/console debug:container                               # list all service IDs
php bin/console debug:config <bundle>                         # dump resolved bundle config
php bin/console debug:event-dispatcher                        # list all registered listeners
php bin/console messenger:consume                             # consume messages from queue
php bin/console lexik:jwt:generate-keypair                    # generate RSA keypair
php bin/console lexik:jwt:generate-keypair --overwrite        # rotate keypair
```

### 22.2 Doctrine / Migrations

```bash
php bin/console doctrine:migration:migrate --allow-no-migration   # run pending migrations
php bin/console doctrine:migration:migrate --dry-run              # preview SQL only
php bin/console doctrine:migration:status                         # show pending/applied status
php bin/console doctrine:migration:diff                           # generate migration from entity diff
php bin/console doctrine:schema:validate                          # validate entity ↔ DB schema
php bin/console doctrine:database:create                          # create the database
php bin/console doctrine:database:drop --force                    # drop the database (DESTRUCTIVE)
```

### 22.3 Platform v4 New Stack

```bash
php bin/console ibexa:install exponential-oss                 # schema + demo data
php bin/console ibexa:install ibexa-oss                       # upstream install type
php bin/console ezplatform:reindex                            # full reindex
php bin/console ezplatform:reindex --iteration-count=100      # incremental
php bin/console ezplatform:reindex --content-type=article     # one content type
php bin/console ezplatform:solr:create-core --cores=default   # provision Solr core
php bin/console ezplatform:content:cleanup-drafts             # remove stale drafts
php bin/console ezplatform:content:cleanup-versions --keep=3  # keep last N per content
php bin/console ezplatform:cron:run                           # run Platform v4 cron scheduler
php bin/console ibexa:graphql:generate-schema                 # regenerate from content model
php bin/console fos:httpcache:invalidate:path / --all         # purge all HTTP cache paths
php bin/console fos:httpcache:invalidate:tag <tag>            # purge by cache tag
php bin/console bazinga:js-translation:dump public/assets --merge-domains  # Admin UI i18n
php bin/console liip:imagine:cache:remove                     # remove all cached variations
php bin/console liip:imagine:cache:remove --filter=small      # remove one variation alias
php bin/console debug:config ibexa                            # dump full resolved platform config
```

### 22.4 Frontend / Asset Build (Yarn / Webpack Encore)

```bash
source ~/.nvm/nvm.sh && nvm use 20     # activate Node.js 20 LTS
corepack enable                        # activates yarn 1.22.x
yarn install                           # install / sync all Node dependencies
yarn upgrade                           # upgrade packages within semver constraints
yarn dev                               # build with source maps (development)
yarn build                             # build minified (production)
yarn watch                             # watch mode — auto-rebuild on source change
yarn ibexa:dev                         # build Platform v4 Admin UI — dev mode
yarn ibexa:build                       # build Platform v4 Admin UI — production
yarn ibexa:watch                       # watch Admin UI assets for changes
```

### 22.5 Composer Maintenance

```bash
composer install                       # install from composer.lock
composer install --no-dev -o           # production + optimised autoloader
composer update                        # update all within constraints
composer update se7enxweb/exponential-platform-dxp  # update one package
composer dump-autoload -o              # optimised (production) autoloader
composer show                          # list all installed packages
composer outdated                      # list outdated packages
composer audit                         # check for security advisories
composer validate                      # validate composer.json / composer.lock
```

### 22.6 Symfony CLI

```bash
symfony server:start                   # start HTTPS dev server (https://127.0.0.1:8000)
symfony server:start -d                # start in background daemon mode
symfony server:stop                    # stop background server
symfony server:log                     # tail server access/error log
symfony server:status                  # show server status + URL
symfony check:requirements             # verify PHP + extension requirements
symfony check:security                 # audit composer.lock for known CVEs
```

---

*For web server configuration templates see `doc/apache2/` and `doc/nginx/` (if present).*
*For Docker-based development see `doc/docker/` and `compose.override.yaml` (if present).*

---

*Copyright &copy; 1998 – 2026 7x (se7enx.com). All rights reserved unless otherwise noted.*
*Exponential Platform DXP is Open Source software released under the GNU GPL v2 or any later version.*