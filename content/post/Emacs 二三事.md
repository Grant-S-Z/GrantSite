+++
title = "Emacs 二三事"
author = ["Grant"]
lastmod = 2023-12-24T04:24:21+08:00
tags = ["Emacs"]
categories = ["cs"]
draft = false
+++

## 安装 {#安装}

在 MacOS 上安装 Emacs：

```sh
brew tap railwaycat/emacsmacport
brew install emacs-mac
```

安装后 Emacs 位于路径 `/opt/homebrew/opt/emacs-mac`, 可以直接拖动 app 至程序坞。

由于 Emacs 安装时参数对使用有影响，下面陈述一些参数的作用：

-   `--with-modules`

    支持动态模块。使用 Emacs rime 等功能时需要启用。


## 常见问题记录 {#常见问题记录}

使用 Emacs 时遇到问题是很正常的，以下是我的一些记录，包括配置、问题与解决方式。


### Rime {#rime}

首先需要安装并配置鼠须管：

```sh
brew install --cask squirrel
```

配置方式可以参考[帮助文档](https://rime.im/docs/)。我直接使用了雾凇拼音的[安装脚本](https://github.com/Mark24Code/rime-auto-deploy)，对配置文件仅做些许改动。现在可以在 MacOS 上使用 Rime 输入法了。

为了在 Emacs 上使用 Rime，需要下载编译好的 librime：

```sh
curl -L -O https://github.com/rime/librime/releases/download/1.7.1/rime-1.7.1-osx.zip
unzip rime-1.7.1-osx.zip -d ~/.emacs.d/librime
rm -rf rime-1.7.1-osx.zip
```

下面需要在 Emacs 上安装 rime. 我的配置如下：

```emacs-lisp
(use-package rime ;; 输入法
  :custom
  (default-input-method "rime")
  (rime-librime-root "~/.emacs.d/librime/dist")
  (rime-emacs-module-header-root "/opt/homebrew/opt/emacs-mac/include/")
  (rime-user-data-dir "~/Library/Rime/")
  (rime-cursor ".")
  (rime-show-candidate 'posframe)
  (rime-commit1-forall t)
  (rime-posframe-properties
   (list :internal-border-width 10
         :font "LXGW WenKai"
         :color 'rime-default-face))
  ;; 在 modeline 显示输入法指示标志
  (mode-line-mule-info '((:eval (rime-lighter))))
  ;; 在 minibuffer 使用后自动关闭输入法
  (rime-deactivate-when-exit-minibuffer t))
```

需要注意：

-   rime-librime-root: `librime/dist` 位置。
-   rime-emacs-module-header-root

    使用 brew 安装的 Emacs 位置与系统默认位置不一致，需要指定其头文件位置。
-   rime-user-data-dir: Rime 配置位置。
-   rime-show-candidate

    选择输入法显示方式，我选择 posframe, 需要额外安装。

-   rime-commit1-forall: 在输入处显示首个备选项。
