---
title: RStudio에서 Rmd 파일 세팅
author: gglee
date: '2020-04-11'
categories: [Statistics]
tags: [rstudio, tips]
slug: rstudio-rmd-setting
draft: no
---

RStudio로 데이터 분석을 하다 보면 기본적으로 rmarkdown, 또는 rnotebook 문서를 생성하게 된다. 하지만, client에게 전달해 줄때에는 pdf로 주는 것이 가장 확실할 때가 있다. R은 기본적으로 한글 처리가 완벽하지 않아서 처음에 설치된 상태에서는 한글 pdf를 만들수 없다. 따라서 아래의 과정을 이용하여 pdf 출력을 한다.

## TinyTex 패키지 설치

pdf 출력에 필수인 패키지로, 예전에는 texlive 등 엄청 무거운 패키지를 깔았어야 했지만, R에서는 단지 TinyTex 만으로도 충분히 글을 쓸만큼 깔아준다. RStudio의 개발자인 Yihui Xie에 중국인이라서 다행인 점.

```r
install.packages('tinytex')
tinytex::install_tinytex()
```

## Rmd 파일의 설정 추가

출력하고자 하는 문서에 두 줄이 반드시 추가되어야 한다. xelatex은 CJK 문자 처리를 잘 해는 latex_engine으로, RStudio의 기본인 pdflatex에서 반드시 변경해 주어야 한다.

기본 pdf 문서 폰트는 한글을 내장하고 있지 않기 때문에 main 폰트를 반드시 바꿔줘야 한다. 맥에서는 font book에서 한글 폰트를 찾아 Postscript Name을 info 창에서 확인하고 이름을 넣으면 된다. 이것 저것 확인해 본 결과 나는 나눔고딕이 가장 무난했다.

```yaml
---
title: "PDF 출력"
date: "2020-04-11"
author: gglee
output:
  pdf_document:
    latex_engine: xelatex  # mandatory
mainfont: NanumGothic      # mandatory
---
```

## 나의 Rnotebook 파일의 기본 정보

일단 분석을 위해서는 속도가 중요하기 때문에 html_notebook으로 작성을 하고, 어느정도 내용이 잘 정리가 되면, 외부인에게 제공할 목적으로 pdf_document를 작성한다.

```yaml
---
title: "PDF 출력"
date: "2020-04-11"
author: gglee
output:
  html_notebook:
    code_folding: none
    number_section: yes
    toc: yes
    toc_depth: 3
  bookdown::pdf_document2:
    fig_caption: yes
    latex_engine: xelatex
    number_sections: yes
    toc: yes
mainfont: NanumGothic
---
```

헤더에 이어서 출력을 위한 chunk option을 아래와 같이 설정

```r
knitr::opts_chunk$set(
  echo = FALSE,
  include = FALSE,
  message = FALSE,
  warning = FALSE
)
```

## 부가정보

- 그림안에서 한글을 쓰기 위해서는 또다른 방법이 필요하다. 하지만 기본적으로 그림 및 table에서는 영어로 쓰는 편이 좋다.
- 폰트 추가 설치를 위해서는 웹에서 otf 파일을 구해서 font book에 던져 넣으면 된다. 아마 그럴 일은 많지 않지만...
