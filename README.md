# Website

This website is built using [Docusaurus](https://docusaurus.io/), a modern static website generator.

## Installation

```bash
yarn
```

## Local Development

```bash
yarn start
```

This command starts a local development server and opens up a browser window. Most changes are reflected live without having to restart the server.

## Build

```bash
yarn build
```

This command generates static content into the `build` directory and can be served using any static contents hosting service.

## Deployment

The site is deployed to GitHub Pages by the [Deploy to GitHub Pages](.github/workflows/deploy.yml)
workflow, which builds the site and publishes it on every push to the `Github-Pages` branch (it can
also be run manually from the Actions tab).

For this to work, the repository's **Settings → Pages → Build and deployment → Source** must be set
to **GitHub Actions**.

The published site lives at <https://jepvid.github.io/the-harbour/>.
