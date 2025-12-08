## Project Overview

This is a static website generated using [Pelican](https://getpelican.com/). The project is configured to build a personal website for "Irina Beckel" with the site name "iRLS". The content is written in Markdown and is located in the `content` directory. The project includes configurations for development and production environments, as well as tasks for building, serving, and deploying the site.

## Key Files and Directories

*   `pelicanconf.py`: The main configuration file for Pelican. Contains settings for development, such as author, sitename, and plugins.
*   `publishconf.py`: The configuration file for publishing. It inherits settings from `pelicanconf.py` and overrides some for production, such as the `SITEURL`.
*   `Makefile`: Contains a set of `make` commands for managing the site, including building, serving, and deploying.
*   `tasks.py`: Defines a set of tasks using the `invoke` library, providing a Python-based alternative to the `Makefile`.
*   `content/`: The directory containing the website's content, such as Markdown files for pages and images.
*   `.github/workflows/`: Contains GitHub Actions workflows for continuous deployment to GitHub Pages.

## Building and Running

You can build and manage the site using either `make` or `invoke`.

### Using `make`

*   **`make html`**: Generates the website in the `output` directory.
*   **`make clean`**: Removes the `output` directory.
*   **`make serve`**: Starts a local webserver at `http://localhost:8000`.
*   **`make devserver`**: Starts a local webserver with auto-regeneration on content changes.
*   **`make publish`**: Generates the website using production settings.
*   **`make github`**: Publishes the website to GitHub Pages.

### Using `invoke`

*   **`invoke build`**: Builds the site.
*   **`invoke clean`**: Cleans the output directory.
*   **`invoke serve`**: Serves the site locally.
*   **`invoke livereload`**: Serves the site with live-reloading.
*   **`invoke gh-pages`**: Publishes the site to GitHub Pages.

## Development Conventions

*   **Content:** All website content is located in the `content` directory. Markdown (`.md`) files are used for pages and articles.
*   **Configuration:** The main configuration is in `pelicanconf.py`. For production-specific overrides, `publishconf.py` is used.
*   **Dependencies:** The project requires `pelican`, `ghp-import`, `invoke`, and `livereload`. These can be installed via `pip`.
*   **Internationalization (i18n):** The configuration files and the presence of `*-ru.md` files suggest that the site is multilingual, with English and Russian versions.

## Continuous Deployment

The project is set up for continuous deployment to GitHub Pages. The `.github/workflows/publish.yml` workflow is triggered on every push to the `main` branch, which in turn runs the `.github/workflows/github_pages.yml` workflow to build and deploy the site. This means that any changes merged into the `main` branch will be automatically published.
