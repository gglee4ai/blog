---
title: Visual studio code setting tips
author: gglee
date: '2020-04-05'
categories: [Web]
tags: [vscode, tips]
slug: vscode-setting-tips
draft: no
---

VScode를 사용하면서 필요한 옵션에 대해서 정리해 보았다.

## 자동 줄바꿈 켜기

가끔식 Rmarkdown 파일을 열 경우, 줄바꿈이 안될때가 있다. 이때에는,
**File > Preferences > Settings** 에서 아래 옵션을 켠다.

```json
{ "editor.wordWrap": "on" }
```

## tab 설정

항상 스페이스, 사이즈는 2, 다른 파일에도 강제 적용

```json
{
  "editor.insertSpaces": true,
  "editor.tabSize": 2,
  "editor.detectIndentation": false,
}
```
