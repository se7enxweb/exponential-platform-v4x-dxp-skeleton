# Exponential Platform v4.6.x DXP (Stable; Open Source; Starter Skeleton)

[![PHP](https://img.shields.io/badge/PHP-8.0%20→%208.5-8892BF?logo=php&logoColor=white)](https://php.net)
[![Symfony](https://img.shields.io/badge/Symfony-5.4%20LTS-000000?logo=symfony&logoColor=white)](https://symfony.com)
[![Platform](https://img.shields.io/badge/Platform-4.6%20OSS-orange)](https://github.com/se7enxweb)
[![License: GPL v2](https://img.shields.io/badge/License-GPL%20v2-blue.svg)](https://www.gnu.org/licenses/gpl-2.0)
[![GitHub issues](https://img.shields.io/github/issues/se7enxweb/exponential-platform-dxp-skeleton)](https://github.com/se7enxweb/exponential-platform-dxp-skeleton/issues)
[![GitHub stars](https://img.shields.io/github/stars/se7enxweb/exponential-platform-dxp-skeleton?style=social)](https://github.com/se7enxweb/exponential-platform-dxp-skeleton)

> **Exponential Platform DXP** is an open-source Digital Experience Platform (DXP/CMS) built on **Symfony 5.4 LTS** with full PHP 8.x compatibility. This is the **DXP skeleton** — the standard, single-kernel Exponential Platform v4 project template.

This is a website skeleton for Exponential Platform DXP (Stable; Open Source).

---

## Table of Contents

1. [Project Notice](#exponential-platform-dxp-project-notice)
2. [Project Status](#exponential-platform-dxp-project-status)
3. [Who is 7x](#who-is-7x)
4. [What is Exponential Platform DXP?](#what-is-exponential-platform-dxp)
5. [Technology Stack](#technology-stack)
6. [Requirements](#requirements)
7. [Quick Start](#quick-start)
8. [Main Features](#main-exponential-platform-dxp-features)
9. [Installation](#installation)
10. [Key CLI Commands Reference](#key-cli-commands-reference)
11. [Issue Tracker](#issue-tracker)
12. [Where to Get More Help](#where-to-get-more-help)
13. [How to Contribute](#how-to-contribute-new-features-and-bugfixes-into-exponential-platform-dxp)
14. [Donate & Support](#donate-and-make-a-support-subscription)
15. [Copyright](#copyright)
16. [License](#license)

---

## Exponential Platform DXP Project Notice

> "Please Note: This project is not associated with the original eZ Publish software or its original developer, eZ Systems."

This is an independent, 7x + community-driven continuation of the platform. The Exponential Platform DXP codebase is stewarded and evolved by [7x (se7enx.com)](https://se7enx.com) and the open-source community of developers and integrators who have relied on it for decades.

---

## Exponential Platform DXP Project Status

**Exponential Platform DXP has made it beyond its end of life in 2021 and survived. Current releases are primarily aimed at easing the requirements to support current versions of the PHP language like PHP 8.2, 8.3, 8.4, 8.5, 8.6 and beyond.**

The platform is under active maintenance and targeted improvement. The **4.6.x (Platform v4)** release line is the current stable version series of the Platform v4 Distribution. This is a standard single-kernel release running Exponential Platform v4 OSS on Symfony 5.4 LTS with full PHP 8.x support. Ongoing work focuses on:

- Continued PHP 8.x compatibility (8.2, 8.3, 8.4, 8.5 tested and supported)
- Exponential Platform v4 kernel patches for PHP 8.x runtime compatibility
- Symfony 5.4 LTS alignment and dependency upgrades
- Dependency upgrades across Composer and Yarn package ecosystems
- Security patches and vulnerability triage
- Documentation and developer experience improvements

---

## Who is 7x

[7x](https://se7enx.com) is the North American corporation driving the continued general use, support, development, hosting, and design of Exponential Platform DXP Enterprise Open Source Content Management System.

7x has been in business supporting Exponential Platform website customers and projects for over 24 years. 7x took over leadership of the project and its development, support, adoption and community growth in 2023.

7x represents a serious company leading the open source community-based effort to improve Exponential Platform and its available community resources to help users continue to adopt and use the platform to deliver the very best in web applications, websites and headless applications in the cloud.

Previously before 2022, 7x was called Brookins Consulting — the outspoken leader in the active Exponential Platform Community and its portals for over 24 years.

**7x offers:**
- Commercial support subscriptions for Exponential Platform DXP deployments
- Hosting on the Exponential Platform cloud infrastructure (`exponential.earth`)
- Custom development, migrations, upgrades, and training
- Community stewardship via [share.exponential.earth](https://share.exponential.earth)

---

## What is Exponential Platform DXP?

Exponential Platform DXP runs a **single, modern Symfony-native content kernel** — Exponential Platform v4 OSS (Symfony 5.4 LTS). This is the standard release for new projects or projects that do not require the classic legacy kernel.

### Recent Improvements to Exponential Platform DXP

Exponential Platform DXP 4.6.x (Platform v4) releases run **Exponential Platform v4 OSS + Symfony 5.4 LTS** — providing the modern eZ / Ibexa-lineage CMS experience with full PHP 8.x runtime support.

### What Does Exponential Platform DXP Provide for End Users Building Websites?

Exponential Platform DXP is a professional PHP application framework with advanced CMS (content management system) functionality. As a CMS, its most notable feature is its fully customizable and extendable content model. It is also suitable as a platform for general PHP development, allowing you to develop professional Internet applications, fast.

Standard CMS functionality, like news publishing and forums, is built in and ready for you to use. Its stand-alone libraries can be used for cross-platform, secure, database independent PHP projects.

Exponential Platform DXP is database, platform and browser independent. Because it is browser based it can be used and updated from anywhere as long as you have access to the Internet.

---

## Technology Stack

| Layer | Technology |
|---|---|
| **Language** | PHP 8.0 → 8.5 |
| **Framework** | Symfony 5.4 LTS |
| **CMS Core** | Exponential Platform v4 OSS |
| **ORM** | Doctrine ORM 2.x |
| **Template Engine** | Twig 3.x |
| **Frontend Build** | Webpack Encore + Yarn 1.x + Node.js 20 LTS |
| **Search** | Legacy search (default) · Solr 7.7 / 8.x (optional) |
| **HTTP Cache** | Symfony HttpCache (default) · Varnish 6/7 (optional) |
| **App Cache** | Filesystem (default) · Redis 6+ (optional) |
| **Database** | MySQL 8.0+ · MariaDB 10.3+ · PostgreSQL 14+ · SQLite 3.x (dev / testing) |
| **API** | REST API v2 · GraphQL (schema auto-generated) · JWT auth |
| **Admin UI** | Platform v4 Admin UI (`/adminui/`) |
| **Dependency Mgmt** | Composer 2.x · Yarn 1.x |

---

## Requirements

- PHP 8.0+ (8.2, 8.3, or 8.5 recommended)
- A web server: Apache 2.4 or Nginx 1.18+
- A database server: MySQL 8.0+, MariaDB 10.3+, PostgreSQL 14+, or SQLite 3.x (dev/testing)
- Composer 2.x
- Node.js 20 LTS (via nvm recommended)
- Yarn 1.22.x

### Full Requirements Summary

| Requirement | Minimum | Recommended |
|---|---|---|
| PHP | 8.0 | 8.3 or 8.5 |
| Composer | 2.x | latest 2.x |
| Node.js | 20 LTS | 20 LTS (via nvm) |
| Yarn | 1.x | 1.22.x |
| MySQL | 8.0 | 8.0+ (utf8mb4) |
| MariaDB | 10.3 | 10.6+ |
| PostgreSQL | 14 | 16+ |
| SQLite | 3.x | 3.35+ (dev/testing only) |
| Redis | 6.0 | 7.x (optional) |
| Solr | 7.7 | 8.11.x (optional) |
| Varnish | 6.0 | 7.1+ (optional) |
| Apache | 2.4 | 2.4 (event + PHP-FPM) |
| Nginx | 1.18 | 1.24+ |

---

## Quick Start

```bash
# 1. Create project
composer create-project se7enxweb/exponential-platform-v4x-dxp-skeleton:4.6.x-dev exponential_website
cd exponential_website

# 2. Configure environment
cp .env .env.local
# MySQL/MariaDB: edit DATABASE_HOST, DATABASE_NAME, DATABASE_USER, DATABASE_PASSWORD, APP_SECRET, APP_ENV
# SQLite (zero-config): set DATABASE_URL="sqlite:///%kernel.project_dir%/var/data_%kernel.environment%.db"
#                       and MESSENGER_TRANSPORT_DSN=sync://

# 3. Create database and import demo data
# MySQL/MariaDB first:
mysql -u root -p -e "CREATE DATABASE exponential CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_520_ci;"
# Then install:
php bin/console ibexa:install --no-interaction

# 4. Set permissions
setfacl -R -m u:www-data:rwX -m g:www-data:rwX var public/var
setfacl -dR -m u:www-data:rwX -m g:www-data:rwX var public/var

# 5. Build frontend assets
source ~/.nvm/nvm.sh && nvm use 20 && corepack enable
yarn install && yarn build

# 6. Build Admin UI assets
php bin/console assets:install --symlink --relative public
yarn ibexa:build

# 7. Generate JWT keypair
php bin/console lexik:jwt:generate-keypair

# 8. Generate GraphQL schema
php bin/console ibexa:graphql:generate-schema

# 9. Clear all caches
php bin/console cache:clear

# 10. Start
symfony server:start
# → https://127.0.0.1:8000           (site frontend)
# → https://127.0.0.1:8000/adminui/  (Platform v4 Admin UI — admin / publish)
# → https://127.0.0.1:8000/api/ezp/v2/  (REST API v2)
# → https://127.0.0.1:8000/graphql   (GraphQL endpoint)
```

> See [INSTALL.md](INSTALL.md) for the complete step-by-step guide with server configuration, Solr, Varnish, and production deployment.

---

## Main Exponential Platform DXP Features

- User defined content classes and objects
- Version control
- Advanced multi-lingual support
- Built in search engine
- Separation of content and presentation layer
- Fine grained role based permissions system
- Content approval and scheduled publication
- Multi-site support
- Multimedia support with automatic image conversion and scaling
- RSS feeds
- Contact forms
- Flexible workflow management system
- Full support for Unicode
- Twig 3.x template engine
- A headless CRUD REST API
- Database abstraction layer supporting MySQL, MariaDB, SQLite 3.x, PostgreSQL, and Oracle
- MVC architecture
- Support for the latest image and video file formats (webp, webm, png, jpeg, etc)
- Support for highly available and scalable configurations (multi-server clusters)
- XML handling and parsing library
- SOAP communication library
- Localisation and internationalisation libraries
- Several other reusable libraries
- SDK (software development kit) and full documentation
- Plugin API with thousands of open-source extensions available

### Additional Capabilities in the 4.6.x (Platform v4) Series

- **GraphQL API** — auto-generated schema per content model via `ibexa:graphql:generate-schema`
- **JWT Authentication** — REST API secured by RSA keypairs (`lexik/jwt-authentication-bundle`)
- **Platform v4 Admin UI** — React-powered editorial interface at `/adminui/`
- **Webpack Encore** — modern asset pipeline with HMR dev server and production minification
- **Design Engine** — `@ezdesign` Twig namespace with theme fallback chain for clean template inheritance
- **Multi-siteaccess** — run multiple sites, languages, or environments from a single codebase and database
- **SQLite database support** — zero-config alternative to MySQL/MariaDB for local development, testing, air-gapped deployments, and demo environments

---

## Installation

Create a new project using Composer:

```bash
composer create-project se7enxweb/exponential-platform-v4x-dxp-skeleton:4.6.x-dev exponential_website
```

The installation guide covers:
- First-time install (`composer create-project`)
- Environment configuration (`.env.local` reference)
- Database creation and demo data import
- Web server setup (Apache 2.4, Nginx, Symfony CLI)
- File & directory permissions
- Frontend asset build (Webpack Encore / Yarn)
- Admin UI asset build
- JWT authentication setup
- Search index initialisation
- Cache management
- Day-to-day operations (start / stop / restart / deploy)
- Cron job setup
- Solr search engine integration
- Varnish HTTP cache integration
- Troubleshooting
- Database Conversion

Learn more about our open source products — [Exponential Platform DXP](https://platform.exponential.earth).

---

## Key CLI Commands Reference

A quick reference for the most frequently used Symfony, Platform v4, and Exponential console commands.

### Symfony Core

```bash
php bin/console list                                          # list all registered commands
php bin/console help <command>                                # help for a specific command
php bin/console cache:clear                                   # clear application cache
php bin/console cache:clear --env=prod                        # clear production cache
php bin/console cache:warmup --env=prod                       # warm up prod cache after deploy
php bin/console cache:pool:clear cache.redis                  # clear a specific cache pool
php bin/console debug:router                                  # list all routes
php bin/console debug:container                               # list all service IDs
php bin/console debug:config <bundle>                         # dump resolved bundle config
php bin/console debug:event-dispatcher                        # list all event listeners
php bin/console assets:install --symlink --relative public    # publish bundle public/ assets
php bin/console messenger:consume                             # consume async message queue
```

### Doctrine / Migrations

```bash
php bin/console doctrine:migration:migrate --allow-no-migration   # run pending migrations
php bin/console doctrine:migration:status                         # show migration status
php bin/console doctrine:migration:diff                           # generate a new migration
php bin/console doctrine:schema:validate                          # validate entity mappings
```

### Exponential Platform v4

```bash
php bin/console ibexa:install exponential-oss                 # fresh install with demo data
php bin/console ibexa:install ibexa-oss                       # upstream install type
php bin/console ezplatform:reindex                            # rebuild search index (full)
php bin/console ezplatform:reindex --iteration-count=50       # incremental reindex
php bin/console ezplatform:cron:run                           # run the Platform v4 cron scheduler
php bin/console ibexa:graphql:generate-schema                 # regenerate GraphQL schema
php bin/console ezplatform:solr:create-core --cores=default   # set up Solr core
php bin/console bazinga:js-translation:dump public/assets --merge-domains   # JS i18n
php bin/console fos:httpcache:invalidate:path / --all         # purge HTTP cache paths
php bin/console lexik:jwt:generate-keypair                    # generate RSA keypair for REST API auth
```

### Admin & Site URLs

| URL | Purpose |
|---|---|
| `/adminui/` | Platform v4 Admin UI (React) |
| `/` | Public site (Twig) |
| `/api/ezp/v2/` | REST API v2 |
| `/graphql` | GraphQL endpoint |

### Frontend / Asset Build (Yarn / Webpack)

Activate Node.js 20 LTS via nvm before running any Yarn commands:

```bash
source ~/.nvm/nvm.sh && nvm use 20    # activate Node.js 20 LTS (required)
corepack enable                        # activates yarn 1.22.x
```

```bash
yarn install            # install / update Node dependencies
yarn dev                # build all assets with source maps — dev mode
yarn build              # build all assets minified for production
yarn watch              # watch mode — auto-rebuild site assets on change
yarn ibexa:dev          # build Platform v4 Admin UI assets — dev mode
yarn ibexa:watch        # watch mode — auto-rebuild Admin UI assets on change
yarn ibexa:build        # build Platform v4 Admin UI assets — production
```

---

## Issue Tracker

Submitting bugs, improvements and stories is possible on
[https://github.com/se7enxweb/exponential-platform-dxp-skeleton/issues](https://github.com/se7enxweb/exponential-platform-dxp-skeleton/issues)

If you discover a [security issue](SECURITY.md), please responsibly report such issues via email to security@exponential.one

---

## Where to Get More Help

| Resource | URL |
|---|---|
| Platform Website | [platform.exponential.earth](https://platform.exponential.earth) |
| Documentation Hub | [doc.exponential.earth](https://doc.exponential.earth) |
| Community Forums | [share.exponential.earth](https://share.exponential.earth) |
| GitHub Organisation | [github.com/se7enxweb](https://github.com/se7enxweb) |
| This Repository | [github.com/se7enxweb/exponential-platform-dxp-skeleton](https://github.com/se7enxweb/exponential-platform-dxp-skeleton) |
| Issue Tracker | [Issues](https://github.com/se7enxweb/exponential-platform-dxp-skeleton/issues) |
| Discussions | [Discussions](https://github.com/se7enxweb/exponential-platform-dxp-skeleton/discussions) |
| Telegram Chat | [t.me/exponentialcms](https://t.me/exponentialcms) |
| Discord | [discord.gg/exponential](https://discord.gg/exponential) |
| 7x Corporate | [se7enx.com](https://se7enx.com) |
| Support Subscriptions | [support.exponential.earth](https://support.exponential.earth) |
| Sponsor 7x | [sponsor.se7enx.com](https://sponsor.se7enx.com) |

---

## How to Contribute New Features and Bugfixes into Exponential Platform DXP

Everyone is encouraged to [contribute](CONTRIBUTING.md) to the development of new features and bugfixes for Exponential Platform DXP.

**Getting started as a contributor:**

1. **Fork** the repository on GitHub: [github.com/se7enxweb/exponential-platform-dxp-skeleton](https://github.com/se7enxweb/exponential-platform-dxp-skeleton)
2. **Clone** your fork and create a feature branch: `git checkout -b feature/my-improvement`
3. **Install** the full dev stack per [INSTALL.md](INSTALL.md) (`APP_ENV=dev`)
4. **Make** your changes — follow coding standards in [CONTRIBUTING.md](CONTRIBUTING.md)
5. **Test** with `php bin/phpunit` and verify no regressions
6. **Push** your branch and open a **Pull Request** against the `4.6` branch
7. **Participate** in the review — maintainers will give feedback promptly

Bug reports, feature requests, and discussion are all welcome via the [issue tracker](https://github.com/se7enxweb/exponential-platform-dxp-skeleton/issues) and [GitHub Discussions](https://github.com/se7enxweb/exponential-platform-dxp-skeleton/discussions).

---

## Donate and Make a Support Subscription

### Help Fund Exponential Platform DXP!

You can support this project and its community by making a donation of whatever size you feel willing to give to the project.

If we have helped you and you would like to support the project with a subscription of financial support you may. This is what helps us deliver more new features and improvements to the software. Support Exponential Platform DXP with a subscription today!

A wide range of donation options available at [sponsor.se7enx.com](https://sponsor.se7enx.com), [paypal.com/paypalme/7xweb](https://www.paypal.com/paypalme/7xweb) and [github.com/sponsors/se7enxweb](https://github.com/sponsors/se7enxweb)

Every contribution — from a one-time thank-you donation to an ongoing support subscription — goes directly toward:
- Maintaining PHP compatibility as new versions release
- Patching the Exponential Platform for PHP 8.x and beyond
- Writing documentation and tutorials
- Running the community infrastructure (forums, chat, docs portal)
- Triaging and fixing security vulnerabilities
- Funding new features voted on by the community

---

## COPYRIGHT

Copyright (C) 1998 - 2026 7x. All rights reserved.

Copyright (C) 1999-2025 Ibexa AS (formerly eZ Systems AS). All rights reserved.

---

## LICENSE

This source code is available separately under the following licenses:

A - Ibexa Business Use License Agreement (Ibexa BUL),
version 2.4 or later versions (as license terms may be updated from time to time)
Ibexa BUL is granted by having a valid Ibexa DXP (formerly eZ Platform Enterprise) subscription,
as described at: https://www.ibexa.co/product
For the full Ibexa BUL license text, please see:
https://www.ibexa.co/software-information/licenses-and-agreements (latest version applies)

AND

B - GNU General Public License, version 2
Grants an copyleft open source license with ABSOLUTELY NO WARRANTY. For the full GPL license text, please see:
https://www.gnu.org/licenses/old-licenses/gpl-2.0.html

---

*Copyright &copy; 1998 – 2026 7x (se7enx.com). All rights reserved unless otherwise noted.*
*Exponential Platform DXP is Open Source software released under the GNU GPL v2 or any later version.*
