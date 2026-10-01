---
sidebar_position: 3
title: Deploy
---

# Deploy Docusaurus to GitHub Pages

:::caution

Ensure that [docusaurus.config.js](../docusaurus.config.js) is correctly configured as outlined in the [Configuration](./config#configure) guide.

:::

## Configure Actions Permissions

In the corresponding GitHub repository, the `github-actions[bot]` user will need the appropriate **Actions** permissions. 

1. Navigate to `https://github.com/<org-or-user>/<repo>/settings/actions`.

2. In the **Workflow permissions** section, check *Read and write permissions* and click **Save**:

    ![actions-permissions](./actions-permissions.png)

## GitHub Actions Workflow

Create one of the following GitHub Actions workflows in `.github/workflows` based on your setup:

### Docusaurus in Root

```yml title=".github/workflows/deploy-docs.yml"
name: Deploy Docusaurus to GitHub Pages

on:
  push:
    paths-ignore:
      - 'README.md'
    branches:
      - main

jobs:
  deploy:
    name: Build Docusaurus to gh-pages
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
          cache: npm

      - name: Install dependencies
        run: npm ci
      - name: Build website
        run: npm run build

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: build
          user_name: github-actions[bot]
          user_email: 41898282+github-actions[bot]@users.noreply.github.com
```

### Docusaurus in Monorepo

:::info

The following example assumes that `docs` folder at the repository root contains the Docusaurus root. Of note, even with `defaults.run.working-directory: docs` specified, you still need to explicitly set:

* `cache-dependency-path` in *actions/setup-node@v3*
* `publish_dir: docs/build` in *Deploy to GitHub Pages*

:::

```yml title=".github/workflows/deploy-docs.yml"
name: Deploy Docusaurus to GitHub Pages

on:
  push:
    paths-ignore:
      - 'README.md'
    branches:
      - main

defaults:
  run:
    working-directory: docs

jobs:
  deploy:
    name: Build Docusaurus to gh-pages
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
          cache: npm
          cache-dependency-path: docs/package-lock.json

      - name: Install dependencies
        run: npm ci
      - name: Build website
        run: npm run build

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: docs/build
          user_name: github-actions[bot]
          user_email: 41898282+github-actions[bot]@users.noreply.github.com
```

Push the changes up to the remote repository, and verify that the action runs successfully at `https://github.com/<org-or-user>/<repo>/actions`:

![workflow-runs](./workflow-runs.png)

## Configure GitHub Pages

Once the workflow has successfully run, the repository needs to be configured to enable GitHub Pages.

1. Navigate to `https://github.com/<org-or-user>/<repo>/settings/pages`.

2. Set **Source** to *Deploy from a branch*.

3. Set **Branch** to `gh-pages` at `/(root)` and click **Save**:

    ![pages-config](./pages-config.png)
    
You should see the following in the GitHub Pages settings once the docs are deployed:

![pages-deployed](./pages-deployed.png)

Click **Visit Site** to see the hosted documentation:

![hosted-docs](./hosted-docs.png)
