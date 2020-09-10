---
title: ggpubr 예제
author: gglee
date: '2020-04-04'
slug: ggpubr-example
categories: [Statistics]
tags: 
  - ggplot
draft: no
---





ggplot이 참 좋은 패키지인 것은 맞지만, 간단한 그림을 그리는데 타이핑해야 하는 정보가 너무 많이 필요하다. 그래서 알아보니, ggpubr이 과거 plot과 유사한 간단한 사용법에 그림 퀄리티도 뛰어난 것 같아 사용법을 간단하게 정리한다. 



## Histogram


```r
p1 <- ggdensity(
  data = df, x = "weight",
  add = "mean", rug = TRUE,
  color = "sex", fill = "sex",
  palette = c("#00AFBB", "#E7B800")
)

p2 <- gghistogram(
  data = df, x = "weight",
  add = "mean", rug = TRUE,
  color = "sex", fill = "sex",
  palette = c("#00AFBB", "#E7B800")
)

p3 <- ggdensity(
  data = df, x = "weight",
  add = "mean", rug = TRUE,
  fill = "lightgray"
)

p4 <- gghistogram(
  data = df, x = "weight",
  add = "mean",
  color = "sex",
  fill = "sex",
  palette = c("#00AFBB", "#E7B800"),
  add_density = TRUE
)

ggarrange(
  p1, p2, p3, p4, 
  labels = c("a)", "b)", "c)", "d)"), 
  ncol = 2, nrow = 2)
```

<div class="figure">
<img src="/post/2020-04-04-ggpubr-example_files/figure-html/example-histogram-1.png" alt="히스토그램 예제" width="672" />
<p class="caption">Figure 1: 히스토그램 예제</p>
</div>
