---
title: A plain markdown post
author: Yihui Xie
date: '2016-02-14'
categories:
  - Web
tags:
  - blogdown
  - hugo
  - markdown
  - mathjax
  - pandoc
  - rstudio
---

hugo-xmin 테마의 저자인 Yihui Xie의 [이 문서](https://xmin.yihui.org/post/2016/02/14/a-plain-markdown-post/)를 번역하였습니다.

-----------------------------

이 글은 주로 [**blogdown**](https://github.com/rstudio/blogdown) 사용자를 위하여 작성되었습니다. 만일 **blogdown**을 사용하지 않는다면 첫 번째 섹션은 건너 뛸 수 있습니다.

## 1. Markdown or R Markdown

1. 일반 Markdown 문서에서는 R 코드를 실행할 수 없지만 R Markdown 문서에서는 R 코드 이와 같은 형식으로 (```` ```{r} ````) 실행할 수 있습니다.
2. 일반 Markdown 게시물은 Blackfriday를 통해서 변환되지만[^1], R Markdown 문서는 [**rmarkdown**](http://rmarkdown.rstudio.com)과 [Pandoc](http://pandoc.org)에 의해서 변환됩니다.

Blackfriday의 Markdown과 Pandoc의 Markdown 사이에는 구문에 많은 차이가 있습니다. 예를 들어 Blackfriday는 작업 목록을 작성할 수 있으나, Pandoc은 작성할 수 없습니다.

- [x] Write an R package.
- [ ] Write a book.
- [ ] ...
- [ ] Profit!

마찬가지로 Pandoc은 LaTeX 수식을 지원하지만, Blackfriday는 지원하지 않습니다. 이 테마인 [hugo-xmin](https://github.com/yihui/hugo-xmin)은 MathJax 지원을 추가하였지만, 수식을 입력하기 위해서는 수식 앞뒤로 백틱(backtick)을 추가하여야 합니다. (인라인 사용시: `` `$ $` ``; 블럭형태로 사용시: `` `$$ $$` ``), 예를 들면 다음과 같습니다. `$S_n = \sum_{i=1}^n X_i$`.[^2] R Markdown 게시글의 경우 Pandoc이 수학 표현식을 식별하고 처리 할 수 ​​있으므로 백틱이 필요하지 않습니다.

새 게시글을 만들 때, 당신은 게시글 형식은 Markdown 또는 R Markdown 여부를 결정해야 하는데, 이는 아래와 같이 `blogdown::new_post()`함수와 `rmd` 인수를 이용하여 간단하게 선택할 수 있습니다.

```r
blogdown::new_post("Post Title", rmd = FALSE)
```

실제적으로는 RStudio를 사용시 addin에서 "New Post" 명령을 클릭하는 것을 추천합니다.

![RStudio addin New Post](https://bookdown.org/yihui/blogdown/images/new-post.png)

## 2. 텍스트 예제

## 2단계 제목

### 3단계 제목

#### 4단계 제목

단락 (각주 포함) :

**Lorem ipsum** dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore _magna aliqua_. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

인용구(blockquote) (왼쪽 및 밝은 회색 배경의 회색 막대):

> Quisque mattis volutpat lorem vitae feugiat. Praesent porta est quis porta imperdiet. Aenean porta, mi non cursus volutpat, mi est mollis libero, id suscipit orci urna a augue. In fringilla euismod lacus, vitae tristique massa ultricies vitae. Mauris accumsan ligula tristique, viverra nulla sed, porta sapien. Vestibulum facilisis nec nisl blandit convallis. Maecenas venenatis porta malesuada. Ut ac erat tortor. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Nulla sodales quam sit amet tincidunt egestas. In et turpis at orci vestibulum ullamcorper. Aliquam sed ante libero. Sed hendrerit arcu lacus.

컴퓨터 코드 (박스의 그림자 효과 포함):

```js
(function() {
  var quotes = document.getElementsByTagName('blockquote'), i, quote;
  for (i = 0; i < quotes.length; i++) {
    quote = quotes[i];
    var n = quote.children.length;
    if (n === 0) continue;
    var el = quote.children[n - 1];
    if (!el || el.nodeName !== 'P') continue;
    // right-align a quote footer if it starts with ---
    if (/^—/.test(el.textContent)) el.style.textAlign = 'right';
  }
})();
```

테이블 (기본으로 가운데 위치) :

| Sepal.Length| Sepal.Width| Petal.Length| Petal.Width|Species |
|------------:|-----------:|------------:|-----------:|:-------|
|          5.1|         3.5|          1.4|         0.2|setosa  |
|          4.9|         3.0|          1.4|         0.2|setosa  |
|          4.7|         3.2|          1.3|         0.2|setosa  |
|          4.6|         3.1|          1.5|         0.2|setosa  |
|          5.0|         3.6|          1.4|         0.2|setosa  |
|          5.4|         3.9|          1.7|         0.4|setosa  |

그림 (적절하게 중앙에 위치):

![Happy Elmo](https://slides.yihui.name/gif/happy-elmo.gif)

좋아 보이나요?

[^1]: 2020년 현재, hugo의 엔진은 Goldmark로 변경되었습니다. 그로 인하여 일부 기능이 달라졌습니다(예시: inline-note 미지원)
[^2]: 이는 math expression을 Markdown이 변환하는 것을 막기 위해서입니다.
