+++
title = "Build a blog"
author = ["Grant"]
lastmod = 2023-11-21T14:12:38+08:00
tags = ["Emacs", "hugo"]
categories = ["cs"]
draft = false
+++

## How to build your own blog? {#how-to-build-your-own-blog}

I wanna build my own blog for a long time. Long ago, I have heard about Hugo, which is a convenient tool to build a static website quickly.

Following the [course](https://www.youtube.com/watch?v=hjD9jTi_DQ4&list=PLeiDFxcsdhUrzkK5Jg9IZyiTsIMvXxKZP&index=2) of Youtube, I have probably finished it, and take a record for it now.


### Install Hugo {#install-hugo}

Easy for Macos. Make sure that you have installed brew.

```bash
brew install hugo
```


### Create local website {#create-local-website}

Be familiar with the commands of Hugo.

```bash
hugo new site GrantSite
```

Now you can open VS Code or Emacs to edit your own blogs. Use

```bash
hugo server
```

to preview your website.

By the way, you can use Git to download a theme you like.

```bash
git init
git clone https://github.com/adityatelange/hugo-PaperMod themes/PaperMod --depth=1
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

You also need to set your theme in your hugo.yml like this.

```nil
theme: PaperMod
```


### Upload your website {#upload-your-website}

You need to configure github actions to publish the website to github pages. Before that, you should create a repository and upload it first.

```bash
git add README.md
git commit -m "init the website"
git branch -M main
git remote add origin git@github.com:Grant-S-Z/GrantSite.git
git push -u origin main
```

After creating it, to make github do the jobs automatically, you need to establish the workflow. First of all, you should go to the repo actions and change the workflow permissions into "Read and write permissions". Then, add a .github/workflows/deploy.yml file under the project root directory like this.

```nil
name: GitHub Pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-22.04
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public

```

After that, you can see your website. For me, here's [it](https://grant-s-z.github.io/GrantSite/).


## How to use Emacs to write blogs? {#how-to-use-emacs-to-write-blogs}

You need two packages: easy-hugo and ox-hugo.

Just use Emacs to install them, and it is easy to manage your blogs now.

Here are my settings:

```emacs-lisp
 ;; hugo
(use-package easy-hugo
  :init
  (setq easy-hugo-basedir "~/Code/Site/GrantSite/") ;; 网站本地文件根目录
  (setq easy-hugo-url "https://Grant-S-Z.github.io/GrantSite") ;; url 路径
  (setq easy-hugo-sshdomain "Grant-S-Z.github.io")
  (setq easy-hugo-previewtime "300")
  (setq easy-hugo-default-ext ".org")
  :bind ("C-c b" . easy-hugo))

(use-package ox-hugo
  :init
  (setq org-hugo-base-dir "~/Code/Site/GrantSite/")
  (setq org-hugo-section "post")
  :config
  (ox-hugo))
```
