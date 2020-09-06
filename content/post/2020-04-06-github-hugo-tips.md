---
title: github에서 hugo 관련 팁
author: gglee
date: '2020-04-06'
categories:
  - Web
tags:
  - blogdown
  - hugo
  - github
slug: github-hugo-tips
draft: no
toc: yes
---

hugo와 github 관련해서 팁을 모아보았다.

## hugo 테마를 설치할 때

git submodule로 설치해야, 테마와 본인 내용을 분리해서 git 사용 가능

```bash
git submodule add https://github.com/yihui/hugo-xmag.git themes/hugo-xmag
```

## submodule 삭제시

git submodule로 설치된 내용은 아래의 순서대로 제거해야 사라짐

```bash
git submodule deinit -f themes/hugo-xmin
rm -rf .git/modules/themes/hugo-xmin
git rm -f themes/hugo-xmin
```

## hugo 테마를 포함한 리포지터리를 clone 할때

submodule로 테마가 설치되었을 경우 --recursive 옵션을 붙여야 한다.

```bash
git clone --recursive https://github.com/gglee/blog2md
```
