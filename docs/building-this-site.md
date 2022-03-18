---
title: Building This Site
---

## Getting Started

```bash
pip install mkdocs-material

cd ~/Sites
mkdir mk-test
cd mk-test
mkdocs new .

```

Add to `mkdocs.yml`:

```yaml
theme:
  name: material
```

## Git Stuff

```bash
# in the mk-test4 directory
git init
touch .gitignore
echo ".DS_Store" >> .gitignore
echo ".gitignore" >> .gitignore
git status
git add .
git commit -m "initial commit"
```

Go to GitHub and create a new repository `mk-test4`.

```bash
git remote add origin https://github.com/dgoppenheimer/mk-test4.git
git branch -M main
git push -u origin main
```

## Publishing the Site

From [Publishing your site](https://squidfunk.github.io/mkdocs-material/publishing-your-site/)

Create `.github/workflows/ci.yml`

```yaml
name: ci 
on:
  push:
    branches:
      - master 
      - main
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-python@v2
        with:
          python-version: 3.x
      - run: pip install mkdocs-material 
      - run: mkdocs gh-deploy --force
```

Go to GitHub &#8594; Settings &#8594; Pages &#8594; Source and choose `gh-pages` branch, and click `Save`.

!!! success

    So far, so good. I'll start changing a few things and see when the site breaks.

## Customize the Site

Add the following to `mkdocs.yml`.

```yaml
markdown_extensions:
  - admonition
  - pymdownx.details
  - pymdownx.superfences
```

- Create the `docs/overrides/assets/images` directories, and add the favicon and logo images:

```bash
docs/overrides/assets/images/favicon.png
docs/overrides/assets/logo.svg
docs/overrides/favicon.ico
```


