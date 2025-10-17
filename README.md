
# Personal GitHub Pages

This repository contains the source code for my personal GitHub Pages website, which showcases my portfolio, projects, and blog posts.

## Getting Started


To run the website locally for development and testing purposes, follow these steps:

1. Clone the repository.

2. Install the required dependencies:

    ```bash
    brew install ruby-dev ruby-bundler nodejs
    ```

3. Install the Ruby dependencies:

    ```bash
    bundle install
    ```

    If you encounter any errors, try deleting the `Gemfile.lock` file and running the command again.

4. Start the local development server:

    ```bash
    jekyll serve -l -H localhost
    ```

    This will generate the HTML files and serve the website at `http://localhost:4000`. The local server will automatically rebuild and refresh the pages whenever you make changes to the source files.

## Pages Structure

The project follows the standard Jekyll directory structure:

```bash
├── _bibliography     # Bibliography files
├── _config.yml       # Main configuration file
├── _data             # Data files (e.g., CV, repositories)
├── _includes         # Reusable components and partials
├── _layouts          # Layout templates
├── _news             # News and announcements
├── _pages            # Top-level pages
├── _plugins          # Jekyll plugins
├── _posts            # Blog posts
├── _projects         # Project listings
├── _sass             # Sass stylesheets
├── _site             # Generated site (not committed to the repository)
├── assets            # Static assets (images, PDFs, etc.)
├── bin               # Utility scripts
├── blog              # Blog directory
├── docker-compose.yml # Docker Compose configuration
├── docker-local.yml  # Docker configuration for local development
├── news.html         # News page
└── robots.txt        # Robots exclusion rules

```

## Deployment

The website is automatically deployed to GitHub Pages whenever changes are pushed to the main branch of this repository.

## License

This project is licensed under the [MIT License](https://github.com/kobeHub/kobehub.github.io/blob/main/LICENSE).

