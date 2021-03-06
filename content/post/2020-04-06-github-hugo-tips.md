---
title: github에서 hugo 관련 팁
author: gglee
date: '2020-04-06'
categories: [Web]
tags: [github, hugo, tips]
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

## git cache 삭제 실행

hugo와 blogdown을 동시에 사용하다 보면, 가끔씩 docs 폴더가 꼬이는 수가 있다. 이럴 때는 cache를 제거하면 해결되기도 한다.

```bash
git rm -r --cached .
git add .
git commit -m "cache clear"
```
