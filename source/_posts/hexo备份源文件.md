---
title: hexo备份源文件
date: 2018-03-06 00:15:03
tags: hexo
---

## 背景

- hexo deploy发布默认只有public文件夹，即生成的html网页文件，并不包括md源文件。
- 这样就需要自己保存好源文件，并且还要保存主题等文件，避免文件丢失和可以随时发布。

## 方案

- 使用[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)插件，发布同时同步源文件到GitHub。
- 在github.io上创建分支src，将源文件一同上传。
- 本人使用了如下配置

```yaml
  # _config.yaml
  deploy:
    - type: git
      repo: git@github.com:<username>/<username>.github.io.git
      branch: master
    - type: git
      repo: git@github.com:<username>/<username>.github.io.git
      branch: src
      extend_dirs: /
      ignore_hidden: false
      ignore_pattern:
          public: .
```

## hexo-deployer-git官方文档

### Installation

``` bash
$ npm install hexo-deployer-git --save
```

If you want to use the latest features of hexo-deployer-git, you may install it from github:

* for npm version under 4
``` bash
$ npm install git+git@github.com:hexojs/hexo-deployer-git.git --save
```

* for npm version 5
```bash
$ npm install git+ssh://git@github.com:hexojs/hexo-deployer-git.git --save
```

### Options

You can configure this plugin in `_config.yml`.

``` yaml
# You can use this:
deploy:
  type: git
  repo: <repository url>
  branch: [branch]
  message: [message]
  name: [git user]
  email: [git email]
  extend_dirs: [extend directory]
  ignore_hidden: false # default is true
  ignore_pattern: regexp  # whatever file that matches the regexp will be ignored when deploying

# or this:
deploy:
  type: git
  message: [message]
  repo:
    github: <repository url>,[branch]
    coding: <repository url>,[branch]
  extend_dirs:
    - [extend directory]
    - [another extend directory]
  ignore_hidden:
    public: false
    [extend directory]: true
    [another extend directory]: false
  ignore_pattern:
    [folder]: regexp  # or you could specify the ignore_pattern under a certain directory
```

- **repo**: Repository URL
- **branch**: Git branch to deploy the static site to
- **message**: Commit message. 
- **name** and **email**: User info for committing the change, overrides global config. This info is independent of git login.
- **extend_dirs**: Add some extensions directory to publish. e.g `demo`, `examples`
- **ignore_hidden** (Boolean|Object): whether ignore hidden files to publish. the github requires the `.nojekyll` in root.
  * Boolean: for all dirs.
  * Object: for public dir and extend dir:
    * `public`: the public dir defaults.
    * [extend directory]
- **ignore_pattern** (Object|RegExp): Choose the ignore pattern when deploying
  * RegExp: for all dirs.
  * Object: specify the ignore pattern under certain directory. For example, if you want to push the source files and generated files at the same time to two different branches. The option should be like
  ```yaml
  # _config.yaml
  deploy:
    - type: git
      repo: git@github.com:<username>/<username>.github.io.git
      branch: master
    - type: git
      repo: git@github.com:<username>/<username>.github.io.git
      branch: src
      extend_dirs: /
      ignore_hidden: false
      ignore_pattern:
          public: .
  ```

### How it works

`hexo-deployer-git` works by generating the site in `.deploy_git` and *force pushing* to the repo(es) in config.
If `.deploy_git` does not exist, a repo will initialized (`git init`).
Otherwise the curent repo (with its commit history) will be used.

Users can clone the deployed repo to `.deploy_git` to keep the commit history.
```
git clone <gh-pages repo> .deploy_git
```

### Reset

Remove `.deploy_git` folder.

``` bash
$ rm -rf .deploy_git
```

### License

MIT

[Hexo]: http://hexo.io/

