+++
title = "Build a blog"
author = ["Grant"]
lastmod = 2023-06-22T18:12:23+08:00
tags = ["hugo", "emacs"]
categories = ["cs"]
draft = false
+++

## How to build your own blog? {#how-to-build-your-own-blog}

I wanna build my own blog for a long time. Long ago, I have heard about Hugo, which is a convenient tool to build a static website quickly.

Following the [course](https://www.youtube.com/watch?v=hjD9jTi_DQ4&list=PLeiDFxcsdhUrzkK5Jg9IZyiTsIMvXxKZP&index=2) of Youtube, I have probably finished it, and take a record for it now.


### Install Hugo {#install-hugo}

Easy for macos. Make sure that you have installed brew.

```bash
brew install hugo
```


### Create local website {#create-local-website}

Be familiar with the commands of Hugo.

```bash
hugo new site GrantSite
```

Now you can open VSCode or Emacs to edit your own blogs. Use

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
name: Publish to GH Pages
on:
  push:
    branches:
      - main
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout source
	uses: actions/checkout@v3
	with:
	  submodules: true

      - name: Checkout destination
	uses: actions/checkout@v3
	if: github.ref == 'refs/heads/main'
	with:
	  ref: gh-pages
	  path: built-site

      - name: Setup Hugo
	run: |
	  curl -L -o /tmp/hugo.tar.gz 'https://github.com/gohugoio/hugo/releases/download/v0.110.0/hugo_extended_0.110.0_linux-amd64.tar.gz'
	  tar -C ${RUNNER_TEMP} -zxvf /tmp/hugo.tar.gz hugo
      - name: Build
	run: ${RUNNER_TEMP}/hugo

      - name: Deploy
	if: github.ref == 'refs/heads/main'
	run: |
	  cp -R public/* ${GITHUB_WORKSPACE}/built-site/
	  cd ${GITHUB_WORKSPACE}/built-site
	  git add .
	  git config user.name 'dhij'
	  git config user.email 'davidhwang.ij@gmail.com'
	  git commit -m 'Updated site'
	  git push
```

After that, you can see your website. For me, here's [it](https://grant-s-z.github.io/GrantSite/).


## How to use Emacs to write blogs? {#how-to-use-emacs-to-write-blogs}

You need two packages: easy-hugo and ox-hugo.

Just use Emacs to install them, and it is easy to manage your blogs now.
