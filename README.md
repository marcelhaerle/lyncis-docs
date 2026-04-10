# Lyncis Security Platform - Documentation

[![Deploy MkDocs](https://github.com/marcelhaerle/lyncis-docs/actions/workflows/deploy.yml/badge.svg)](https://github.com/marcelhaerle/lyncis-docs/actions)
[![Live Docs](https://img.shields.io/badge/Docs-Live-brightgreen.svg)](https://marcelhaerle.github.io/lyncis-docs/)

This repository contains the central documentation, architecture blueprints, and deployment guides for the **Lyncis Security Platform**.

Lyncis is a distributed, on-premise client/server platform designed to centralize and automate system security audits using `lynis`.

👉 **[Read the official documentation here](https://marcelhaerle.github.io/lyncis-docs/)**

---

## Related Repositories

Lyncis is built with a microservice-like architecture. The source code is divided into the following repositories:

* **[lyncis-backend](https://github.com/marcelhaerle/lyncis-backend):** Go / Fiber REST API & Task Orchestration
* **[lyncis-agent](https://github.com/marcelhaerle/lyncis-agent):** Go standalone binary for host auditing
* **[lyncis-ui](https://github.com/marcelhaerle/lyncis-ui):** React / Vite / Tailwind UI Dashboard

---

## Contributing & Local Development

This documentation is built using [MkDocs](https://www.mkdocs.org/) and the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme.

If you want to edit the documentation or preview changes locally before pushing, follow these steps:

### Prerequisites

Make sure you have Python 3.x installed.

### Setup

#### Clone this repository

```bash
git clone [https://github.com/marcelhaerle/lyncis-docs.git](https://github.com/marcelhaerle/lyncis-docs.git)
cd lyncis-docs
```

#### Install the required dependencies

```bash
pip install -r requirements.txt
```

#### Running the Live Server

```bash
mkdocs serve
```

The documentation will be available at `http://localhost:8000`. Any changes you make to the `.md` files in the `docs/` folder will automatically refresh the browser.

## Deployment

This repository uses GitHub Actions to automatically build and deploy the documentation to GitHub Pages whenever a
change is pushed to the `main` branch.

You do not need to build the static files manually.
