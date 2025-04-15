
# Teamdroid-Docs

This repository is focused on storing and managing the official technical documentation and blogs of *Teamdroid*. 
The platform used for content generation and publication is [Docusaurus](https://docusaurus.io/), a modern static site generator maintained by Meta, designed specifically for technical documentation.

## Purpose

- Centralize technical documentation of Teamdroid processes, architectures, standards, and best practices.
- Publish technical articles, blogs, and usage guides for developers and internal collaborators.
- Maintain version control of all documented content.
- Automate the generation and deployment of the static site for both development and production environments.
- Serve the generated site via HTTP servers (Docusaurus Serve or Caddy) within Docker containers.

## Technical Features

- Framework: Docusaurus v2
- Dependency management: NPM
- Multi-stage Docker architecture to support:
  - Development mode (live-reload enabled)
  - Static build generation for production
  - Serving the site in production using either Docusaurus Serve or Caddy
- Environment configuration using `.env` files
- Ready for Continuous Integration and Continuous Deployment (CI/CD) using GitHub Pages or any static hosting platform
- Content structure:
  - `/docs` → Technical Documentation
  - `/blog` → Articles and Blog Posts
  - `/src` → Site Customizations
  - `/static` → Static Resources

### Installation

```
$ npm install
```

### Local Development

```
$ npm start
```

This command starts a local development server and opens up a browser window. Most changes are reflected live without having to restart the server.

### Build

```
$ npm run build
```

This command generates static content into the `build` directory and can be served using any static contents hosting service.

### Deployment

Using SSH:

```
$ USE_SSH=true npm run deploy
```

Not using SSH:

```
$ GIT_USER=<Your GitHub username> npm run deploy
```

If you are using GitHub pages for hosting, this command is a convenient way to build the website and push to the `gh-pages` branch.

# Using npm 
Inside that directory, you can run several commands:

  `npm start`
    Starts the development server.

  `npm run build`
    Bundles your website into static files for production.

  `npm run serve`
    Serves the built website locally.

  `npm run deploy`
    Publishes the website to GitHub pages.

We recommend that you begin by typing:

  `cd teamdroid-docs`
  
  `npm start`

## Dockerfile Explanation

This project uses a multi-stage Dockerfile to optimize the build and execution of the Docusaurus site. Below are the different stages and their purpose:

### Stages

| Stage   | Name   | Purpose                                                                 |
|---------|--------|-------------------------------------------------------------------------|
| Stage 1 | base   | Provides a Node.js environment and prepares the workspace for builds.   |
| Stage 2 | dev    | Runs the Docusaurus development server with hot reload.                 |
| Stage 2 | prod   | Installs dependencies and generates a production build of the site.     |
| Stage 3 | serve  | Serves the static build using `docusaurus serve`.                       |
| Stage 3 | caddy  | Serves the static build using [Caddy](https://caddyserver.com/) server.|

## Running the Docker Image

Depending on your need, you can run the image using different stages:

### Development Mode (Hot Reload)

To run Docusaurus in development mode (port 3000):

```
docker build -t teamdroid-docs-dev --target dev .
docker run -it --rm -p 3000:3000 teamdroid-docs-dev
```

### Production Build

To build the static site only:

```
docker build -t teamdroid-docs-prod --target prod .
```

### Serve Production Build with Docusaurus

```
docker build -t teamdroid-docs-serve --target serve .
docker run -it --rm -p 3000:3000 teamdroid-docs-serve
```

### Serve Production Build with Caddy

```
docker build -t teamdroid-docs-caddy --target caddy .
docker run -it --rm -p 80:80 -p 443:443 teamdroid-docs-caddy
```

Make sure to configure domain and certificates properly in the `Caddyfile` for HTTPS support.
