---
title: git
date: 2018-03-30 08:36:16
tags:git
---

### create a new repository on the command line

```shell
echo "# leankotlin" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/username/project.git
git push -u origin master
```

### push an existing repository from the command line

```shell
git remote add origin https://github.com/username/project.git
git push -u origin master
```

