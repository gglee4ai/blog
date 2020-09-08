---
title: difference-between-two
author: gglee
date: '2020-04-19'
categories: [Statistics]
tags: [simulation]
slug: difference-between-two
draft: yes
---



## 소개

두 표본의 통계량 차이를 반복실험을 통해서 알아보자. 일반적으로 잘알려진 사실은 두 표본의 평균의 차는 변하지 않는다. 다만, 편차, 신뢰구간, 또는 표준오차는 몇개를 뽑느냐, 모분산이 있느냐 없느냐에 따라 여러 계산식으로 표현될 수 있다. 하지만, 간단하게 반복실험을 해보면 쉽게 결과를 확인할 수 있다.

정규분포를 가지는 두 집단에서의 차이를 계산하는 함수이다.




```r
simulation_diff <- function(n1, m1, s1, n2, m2, s2, n_total = 100000) {
  m <-list()
  for (i in 1:n_total) {
    x1 <- rnorm(n1, m1, s1)
    x2 <- rnorm(n2, m2, s2)
    x1_m <- mean(x1)
    x2_m <- mean(x2)
    m[[i]] <- x2_m - x1_m
  }
  unlist(m)
}
```

## 한번만 측정시 


```r
sim1 <- simulation_diff(n1 = 1, m1 = 10, s1 = 1, n2 = 1, m2 = 11, s2 = 1)
mean(sim1)  # = 11 - 10 
```

```
## [1] 0.9995378
```

```r
var(sim1)   # = 1^2 + 1^2
```

```
## [1] 1.993214
```

```r
sd(sim1)    # sqrt(var(sim1)
```

```
## [1] 1.411812
```

## 각각 2번씩 측정할 경우


```r
sim2 <- simulation_diff(n1 = 2, m1 = 10, s1 = 1, n2 = 2, m2 = 11, s2 = 1)
mean(sim2)  # = 11 - 10 
```

```
## [1] 1.001385
```

```r
var(sim2)   # = 1^2 / 2 + 1^2 / 2
```

```
## [1] 0.99754
```

## 두 표본의 sd가 다른 경우


```r
sim3 <- simulation_diff(n1 = 2, m1 = 10, s1 = 2, n2 = 2, m2 = 11, s2 = 1)
mean(sim3)  # = 11 - 10 
```

```
## [1] 0.9945362
```

```r
var(sim3)   # = 2^2 / 2 + 1^2 / 2
```

```
## [1] 2.47667
```

보다시피 어떤 경우에도 잘 대응된다. 즉 일반적으로,

$$ var(x_2 - x_1) = \frac{var(x_1)}{N_1} + \frac{var(x_2)}{N_2}  $$
$$ sd(x_2 - x_1) = \sqrt{ \frac{sd(x_1)^2}{N_1} + \frac{sd(x_2)^2}{N_2}  } $$


## 실전 적용

철판 두께 측정으로 다시 생각해보자.

```r
rnorm_simulation <- function(inspection, 
                             mu, 
                             sigma, 
                             n_sample, 
                             n_test = 10000, 
                             seed = NULL) {
  if (!is.null(seed)) set.seed(seed)
  m_list <- vector("double", n_test)
  s_list <- vector("double", n_test)
  v_list <- vector("double", n_test)
  for (i in 1:n_test) {
    a_sample <- rnorm(n_sample, mean = mu, sd = sigma)
    m_list[[i]] <- mean(a_sample)
    s_list[[i]] <- sd(a_sample)
    v_list[[i]] <- var(a_sample)
  }
  tibble(
    inspection = inspection,
    grid = 1:n_test,
    n_sample = n_sample, 
    sample_mean = m_list, 
    sample_sd = s_list, 
    sample_var = v_list)  
}
```


```r
df1 <- rnorm_simulation(
  inspection = "1st", mu = 10, sigma = 1, n_sample = 4, seed = 21
) %>% rename_all(~str_c(., '_1'))
df1
```

```
## # A tibble: 10,000 x 6
##    inspection_1 grid_1 n_sample_1 sample_mean_1 sample_sd_1 sample_var_1
##    <chr>         <int>      <dbl>         <dbl>       <dbl>        <dbl>
##  1 1st               1          4         10.4        1.26         1.59 
##  2 1st               2          4         10.0        1.67         2.78 
##  3 1st               3          4          9.64       1.32         1.74 
##  4 1st               4          4         10.4        0.859        0.737
##  5 1st               5          4         10.1        0.896        0.803
##  6 1st               6          4          9.53       0.806        0.650
##  7 1st               7          4          9.93       0.809        0.654
##  8 1st               8          4          9.08       0.627        0.394
##  9 1st               9          4         10.7        0.769        0.591
## 10 1st              10          4         11.2        0.772        0.596
## # … with 9,990 more rows
```


```r
df2 <- rnorm_simulation(
  inspection = "2nd", mu = 9, sigma = 1, n_sample = 4, seed = 23
) %>% rename_all(~str_c(., '_2'))
df2
```

```
## # A tibble: 10,000 x 6
##    inspection_2 grid_2 n_sample_2 sample_mean_2 sample_sd_2 sample_var_2
##    <chr>         <int>      <dbl>         <dbl>       <dbl>        <dbl>
##  1 2nd               1          4          9.62       0.959        0.919
##  2 2nd               2          4          9.71       0.661        0.437
##  3 2nd               3          4          9.20       1.08         1.16 
##  4 2nd               4          4          8.82       0.766        0.587
##  5 2nd               5          4          8.93       0.910        0.828
##  6 2nd               6          4          8.97       0.999        0.999
##  7 2nd               7          4          9.05       0.833        0.693
##  8 2nd               8          4          8.76       0.430        0.185
##  9 2nd               9          4          8.39       0.703        0.495
## 10 2nd              10          4          9.26       1.16         1.34 
## # … with 9,990 more rows
```


```r
dd <- 
  bind_cols(df1, df2)
dd
```

```
## # A tibble: 10,000 x 12
##    inspection_1 grid_1 n_sample_1 sample_mean_1 sample_sd_1 sample_var_1
##    <chr>         <int>      <dbl>         <dbl>       <dbl>        <dbl>
##  1 1st               1          4         10.4        1.26         1.59 
##  2 1st               2          4         10.0        1.67         2.78 
##  3 1st               3          4          9.64       1.32         1.74 
##  4 1st               4          4         10.4        0.859        0.737
##  5 1st               5          4         10.1        0.896        0.803
##  6 1st               6          4          9.53       0.806        0.650
##  7 1st               7          4          9.93       0.809        0.654
##  8 1st               8          4          9.08       0.627        0.394
##  9 1st               9          4         10.7        0.769        0.591
## 10 1st              10          4         11.2        0.772        0.596
## # … with 9,990 more rows, and 6 more variables: inspection_2 <chr>,
## #   grid_2 <int>, n_sample_2 <dbl>, sample_mean_2 <dbl>, sample_sd_2 <dbl>,
## #   sample_var_2 <dbl>
```


```r
dd <- dd %>% 
  mutate(
    diff_mean = sample_mean_2 - sample_mean_1,
    diff_sd = sqrt(sample_sd_2^2 / n_sample_2 + sample_sd_1^2 / n_sample_1),
    diff_var = sample_var_2 / n_sample_2 + sample_var_1 / n_sample_1
  )
dd
```

```
## # A tibble: 10,000 x 15
##    inspection_1 grid_1 n_sample_1 sample_mean_1 sample_sd_1 sample_var_1
##    <chr>         <int>      <dbl>         <dbl>       <dbl>        <dbl>
##  1 1st               1          4         10.4        1.26         1.59 
##  2 1st               2          4         10.0        1.67         2.78 
##  3 1st               3          4          9.64       1.32         1.74 
##  4 1st               4          4         10.4        0.859        0.737
##  5 1st               5          4         10.1        0.896        0.803
##  6 1st               6          4          9.53       0.806        0.650
##  7 1st               7          4          9.93       0.809        0.654
##  8 1st               8          4          9.08       0.627        0.394
##  9 1st               9          4         10.7        0.769        0.591
## 10 1st              10          4         11.2        0.772        0.596
## # … with 9,990 more rows, and 9 more variables: inspection_2 <chr>,
## #   grid_2 <int>, n_sample_2 <dbl>, sample_mean_2 <dbl>, sample_sd_2 <dbl>,
## #   sample_var_2 <dbl>, diff_mean <dbl>, diff_sd <dbl>, diff_var <dbl>
```


```r
dd %>% summarize(
  average_diff_mean = mean(diff_mean),
  average_diff_sd = mean(diff_sd),  # caution: not sqrt(diff_var)
  average_diff_var = mean(diff_var)
)
```

```
## # A tibble: 1 x 3
##   average_diff_mean average_diff_sd average_diff_var
##               <dbl>           <dbl>            <dbl>
## 1             -1.01           0.680            0.503
```


```r
dd %>% 
  pivot_longer(
    cols = c("diff_mean", "diff_var"), 
    names_to = "keys", values_to = "values"
  ) %>%
  ggplot(aes(values)) +
  geom_histogram(color = "black", fill = "white") +
    facet_wrap(~keys, scales = "free")
```

<img src="/post/2020-04-19-difference-between-two_files/figure-html/unnamed-chunk-13-1.png" width="672" />






