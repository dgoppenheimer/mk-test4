# Building This Site

## Getting Started

!!! info

    Almost all of the information for building this site came from the [Material for MkDocs Documentation](https://squidfunk.github.io/mkdocs-material/) website.

- Run the following in Terminal.

```bash
pip install mkdocs-material

cd ~/Sites
mkdir mk-test
cd mk-test
mkdocs new .
```

- Add the following to `mkdocs.yml` to enable the Material theme:

```yaml
theme:
  name: material
```

## Git Stuff

- Create a repository and a `.gitignore` file, and commit the site.

```bash
# in the mk-test4 directory
git init
touch .gitignore
echo ".DS_Store" >> .gitignore
echo ".gitignore" >> .gitignore
echo "site/" >> .gitignore
git status
git add .
git commit -m "initial commit"
```

- Go to `GitHub.com` in a browser and create a new repository `mk-test4`.
- In terminal, set up the remote branch and push the site to the remote repository.

```bash
git remote add origin https://github.com/dgoppenheimer/mk-test4.git
git branch -M main
git push -u origin main
```

## Publishing the Site

- See [Publishing your site](https://squidfunk.github.io/mkdocs-material/publishing-your-site/) for instructions.
- Create `.github/workflows/ci.yml` in the project root that contains the following:

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

- Go to the `mk-test4` project on GitHub &#8594; Settings &#8594; Pages &#8594; Source and choose `gh-pages` branch, and click `Save`.

!!! success

    So far, so good. The site is visible on the web. I'll start changing a few things and see when the site breaks.

## Customize the Site

See the following pages for tips:

[How do I specify custom primary color for mkdocs-material?](https://stackoverflow.com/questions/63017898/how-do-i-specify-custom-primary-color-for-mkdocs-material)  
[Changing the colors](https://squidfunk.github.io/mkdocs-material/setup/changing-the-colors/)  
[Customization](https://squidfunk.github.io/mkdocs-material/customization/)

- Add the following to `mkdocs.yml`.

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

- Add the following to `mkdocs.yml` to tell mkdocs where to find theme overrides:

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

### Footnotes

Add the following to `mkdocs.yml`:

```yaml
markdown_extensions:
  - footnotes
```

### Change Colors on the Landing Page

The lower teal is okay, but I want to see how it looks with a lighter blue on top.

Add this to `mkdocs.yml`:

```yaml
theme:
  palette:
    primary: blue
```

Okay, I got most of the home page stuff shown on [Binbash Leverage??? Documentation](https://leverage.binbash.com.ar/), following their [GitHub repository](https://github.com/binbashar/le-ref-architecture-doc/) structure.

- To the `/docs/index.md` file, add the following at the top of the file:

```md
---
title: Molecular Dynamics
template: overrides/home.html
---
```

!!! success

    The site deployed properly and is still visible on GitHub.

I'll continue the customization of the site.

### Remove the LinkedIn link

- Open `material/overrides/main.html` and delete the announcement bar.

### Modify the Landing Page

- Open `material/overrides/home.html` and under `<!-- Hero image -->`, change `src="assets/images/illustrations/home.png"` to `src="assets/images/illustration.png"`.
- Also delete/change the text under `<!-- Hero content -->`.
- Remove the other sections.

### More Additions to `mkdocs.yml`

```yaml
site_author: David G Oppenheimer
site_description: >-
  My notes for preparing, running, and analyzing molecular dynamics simulations using Google Colab and Jupyter notebooks
site_url: https://dgoppenheimer.github.io/mk-test4/
repo_name: "dgoppenheimer/mk-test4"
repo_url: https://github.com/dgoppenheimer/mk-test4
copyright: Copyright &copy; 2022 David G Oppenheimer
```

### Test Deploy on GitHub

- Run the git stuff and push to GitHub.

!!! success

    Great. It works. I'll keep customizing the landing page and start setting up site navigation. It appears the previous failure to deploy had something to do with how I set up the landing page.

- In `material/overrides/main-styles.html` change the `#2a978d` color in `linear-gradient(to bottom, var(--md-primary-fg-color), #2a978d 99%, #fff 99%)` to `#BB8FCE`. This better matches the colors in the image better than the original teal. The code should look like this:

```css
.tx-container { 
        height: fit-content;
        padding-top: 0rem;
        background: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 1123 258'><path d='M1124,2c0,0 0,256 0,256l-1125,0l0,-48c0,0 16,5 55,5c116,0 197,-92 325,-92c121,0 114,46 254,46c140,0 214,-167 572,-166Z' style='fill: white' /></svg>") no-repeat bottom, linear-gradient(to bottom, var(--md-primary-fg-color), #BB8FCE 99%, #fff 99%)
}
```

## Navigation

### Navigation Features

See [Setting up navigation](https://squidfunk.github.io/mkdocs-material/setup/setting-up-navigation/) in the Material theme documentation for excellent instructions.

- Add the following to `mkdocs.yml`:

```yaml
theme:
  features:
    - navigation.tracking
    - navigation.tabs
    - navigation.tabs.sticky
    - navigation.top
```

The `nav` part of `mkdocs.yml` takes precedence when setting the page title. But I want a longer title on the page and a shorter one for the navigation links. I found this solution in the [mkdocs issues](https://github.com/mkdocs/mkdocs/issues/1795). 

- Put this code in your `/overrides/main.html` file.

```jinja
{% extends "base.html" %}
{% if page.toc|first is defined %}
{% set _title = page.toc.items[0].title %}
{% else %}
{% set _title = page.title|striptags %}
{% endif %}

{% block htmltitle %}
<title>{{ _title }}</title>
{% endblock %}
```

Now, just use an `<h1>` tag for any page title of any length. A shorter page title can be used in the `nav` section of `mkdocs.yml`.

## Video Plugin

This plugin lets you use standard Markdown to display videos. See the [mkdocs-video plugin GitHub repository](https://github.com/soulless-viewer/mkdocs-video) for more information.

### Usage

This markdown code

```markdown
![type:video](https://www.youtube.com/embed/LXb3EKWsInQ)
```

will render as

![type:video](https://www.youtube.com/embed/LXb3EKWsInQ)

## Images

With the [Attribute Lists](https://python-markdown.github.io/extensions/attr_list/) Markdown extension you can add height, width, and alignment to images.

- Add this to the `mkdocs.yml` file:

```yaml
markdown_extensions:
  - attr_list
```

- Align images as follows:

=== "aligned left"

    ```markdown
    ![Image title](https://dummyimage.com/300x300/eee/aaa){ align=left }
    ```

    ![Image title](https://dummyimage.com/300x300/eee/aaa){ align=left }

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum ornare dui vel dolor semper sagittis. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Aliquam in erat odio. Quisque aliquam odio id interdum bibendum. Praesent non magna non risus fermentum vulputate. Nunc tincidunt urna risus, a lacinia metus hendrerit semper. Sed imperdiet blandit ante vel tempus. Morbi maximus elit sapien, sed ullamcorper magna consequat accumsan. Sed quis lorem placerat, pretium velit quis, egestas elit.

=== "aligned right"

    ```mk
    ![Image title](https://dummyimage.com/300x300/eee/aaa){ align=right }
    ```

    ![Image title](https://dummyimage.com/300x300/eee/aaa){ align=right }

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum ornare dui vel dolor semper sagittis. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Aliquam in erat odio. Quisque aliquam odio id interdum bibendum. Praesent non magna non risus fermentum vulputate. Nunc tincidunt urna risus, a lacinia metus hendrerit semper. Sed imperdiet blandit ante vel tempus. Morbi maximus elit sapien, sed ullamcorper magna consequat accumsan. Sed quis lorem placerat, pretium velit quis, egestas elit.

You can also add dimensions to your images as well.

=== "image width=200px"

    ```mk
    ![Image title](https://dummyimage.com/400x400/eee/aaa){ width=200px }
    ```

    ![Image title](https://dummyimage.com/400x400/eee/aaa){ width=200px }

=== "image width=400px"

    ```mk
    ![Image title](https://dummyimage.com/800x400/eee/aaa){ width=300px }
    ```

    ![Image title](https://dummyimage.com/800x400/eee/aaa){ width=300px }

## MathJax

I want to use equations on this site and [MathJax](https://www.mathjax.org/) provides an easy way to do this.

!!! quote "from the [Material for MkDocs MathJax page](https://squidfunk.github.io/mkdocs-material/reference/mathjax/)"

    Create a configuration file and add the following lines to `mkdocs.yml`:


=== "docs/javascripts/mathjax.js"

    ```js
    window.MathJax = {
      tex: {
        inlineMath: [["\\(", "\\)"]],
        displayMath: [["\\[", "\\]"]],
        processEscapes: true,
        processEnvironments: true
      },
      options: {
        ignoreHtmlClass: ".*|",
        processHtmlClass: "arithmatex"
      }
    };

    document$.subscribe(() => { 
      MathJax.typesetPromise()
    })
    ```

=== "mkdocs.yml"

    ```yaml
    markdown_extensions:
      - pymdownx.arithmatex:
          generic: true

    extra_javascript:
      - javascripts/mathjax.js
      - https://polyfill.io/v3/polyfill.min.js?features=es6
      - https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js
    ```

Now you can use `$$...$$` to delimit a block for an equation:

```js
$$
\operatorname{ker} f=\{g\in G:f(g)=e_{H}\}{\mbox{.}}
$$
```

which renders to

$$
\operatorname{ker} f=\{g\in G:f(g)=e_{H}\}{\mbox{.}}
$$

Or use `$...$` for inline math like so: `$a\neq b$` renders to $a\neq b$.
