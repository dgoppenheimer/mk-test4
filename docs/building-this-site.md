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

See the following pages for tips:

- [How do I specify custom primary color for mkdocs-material?](https://stackoverflow.com/questions/63017898/how-do-i-specify-custom-primary-color-for-mkdocs-material)  
- [Changing the colors](https://squidfunk.github.io/mkdocs-material/setup/changing-the-colors/)  
- [Customization](https://squidfunk.github.io/mkdocs-material/customization/)

Add the following to `mkdocs.yml`.

```yaml
markdown_extensions:
  - admonition
  - pymdownx.details
  - pymdownx.superfences
```

- Create the `material/overrides/assets/images` directories, and add the favicon and logo images:

```bash
material/overrides/assets/images/favicon.png
material/overrides/assets/logo.svg
material/overrides/favicon.ico
```

Add the following to `mkdocs.yml` to tell mkdocs where to find theme overrides:

```yaml
theme:
  custom_dir: material
  logo: overrides/assets/logo.svg
  favicon: overrides/assets/images/favicon.png
```

!!! success

    The site builds and deploys fine on GitHub.

!!! info

    I think the failure of the previous site to deploy to GitHub has to do with my changes to `index.html` or one of the partials when I created my landing page.

### Navigation Features

See [Setting up navigation](https://squidfunk.github.io/mkdocs-material/setup/setting-up-navigation/) in the Material theme documentation for excellent instructions.

Add the following to `mkdocs.yml`:

```yaml
theme:
  features:
    - navigation.tracking
    - navigation.tabs
    - navigation.tabs.sticky
    - navigation.top
```

### Footnotes

Add the following to `mkdocs.yml`:

```yaml
markdown_extensions:
  - footnotes
```

### Change Colors on the Landing Page

The lower purple is okay, but I want to see how it looks with a lighter blue on top.

Add this to `mkdocs.yml`:

```yaml
theme:
  palette:
    primary: blue
```

Okay, I got most of the home page stuff shown on [Binbash Leverage™ Documentation](https://leverage.binbash.com.ar/), following their [GitHub repository](https://github.com/binbashar/le-ref-architecture-doc/) structure.

To the `/docs/index.md` file add the following at the top of the file:

```md
---
title: Molecular Dynamics
template: overrides/home.html
---
```

!!! success

    The site deployed properly and is visible on GitHub.

Continue customization.

### Remove the LinkedIn link

Open `material/overrides/main.html` and delete the announcement bar.

### Modify the Landing Page

- Open `material/overrides/home.html` and under `<!-- Hero image -->`, change `src="assets/images/illustrations/home.png"` to `src="assets/images/illustration.png"`.
- Also delete/change the text under `<!-- Hero content -->`.
- Remove the other sections.

### More Additions to `mkdocs.yml`

```yaml
site_author: David G Oppenheimer
site_description: >-
  My notes for preparing, running, and analyzing molecular dynamics simulations using Google Colab and Jupyter notebooks

# Copyright
copyright: Copyright &copy; 2022 David G Oppenheimer

repo_url: https://github.com/dgoppenheimer/mk-test4
```

### Test Deploy on GitHub

Run the git stuff and push to GitHub.

!!! success

    Great. It works. I'll keep customizing the landing page and start setting up site navigation. It appears the previous failure to deploy had something to do with how I set up the landing page. Also I do not have `site_url: https://dgoppenheimer.github.io/mk-test/` in my `mkdocs.yml` file.


In `material/overrides/main-styles.html` change the color for `linear-gradient(to bottom, var(--md-primary-fg-color), #2a978d 99%, #fff 99%)` to `#BB8FCE`. This better matches the colors in the image better than the original teal.

```css
.tx-container { 
        height: fit-content;
        padding-top: 0rem;
        background: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 1123 258'><path d='M1124,2c0,0 0,256 0,256l-1125,0l0,-48c0,0 16,5 55,5c116,0 197,-92 325,-92c121,0 114,46 254,46c140,0 214,-167 572,-166Z' style='fill: white' /></svg>") no-repeat bottom, linear-gradient(to bottom, var(--md-primary-fg-color), #BB8FCE 99%, #fff 99%)
}
```








