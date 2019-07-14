작고 유연한 데이터 요약 팩키지 - skimr
================

skimr은 최소한의 놀라움의 원칙에 부합하는 요약 통계에 대한 마찰없는 접근법을 제공하여 사용자가 자신의 데이터를 빨리 이해할
수있는 요약 통계를 표시합니다. 다른 데이터 유형을 처리하고 파이프 라인에 포함되거나 인간 판독기에 대해 멋지게 표시 될
수있는 skim\_df 객체를 반환합니다.

## 설치

skimr의 버전 2는 매우 적극적으로 개발 중이며 출시 될 예정입니다. 버전 1은 중요한 문제에 대한 업데이트 만 받고
있습니다. 개발 버전에 관심있는 신규 사용자는 v2 분기 설치를 고려하는 것이 좋습니다.

현재 출시 된 skimr 버전은 CRAN에서 설치할 수 있습니다. 다음 릴리스의 현재 빌드를 설치하려면 다음을 사용하십시오.

``` r
#devtools::install_github("ropensci/skimr")
library(skimr)
```

## Skim statistics in the console

`skimr` 패키지

  - 누락, 완료, n 및 sd를 포함하여 summary ()보다 많은 통계 세트를 제공합니다.
  - 각 데이터 유형을 별도로보고합니다.
  - 날짜, 논리 및 기타 다양한 유형을 처리합니다.
  - `[pillar 패키지](https://github.com/r-lib/pillar)`를 기반으로하는 스파크 바
    (spark-bar) 및 스파크 라인 (spark-line)을 지원합니다. 사용자가 데이터 유형에 포함 된 통계를 사용자
    정의하고 추가 클래스에 대한 스키밍을 구현할 수 있습니다. 이와 더불어, 많은 Tidyverse 기능과 함께
    작동합니다.

### 클래스별로 변수 분리

``` r
skim(chickwts)
```

    ## Skim summary statistics
    ##  n obs: 71 
    ##  n variables: 2 
    ## 
    ## ── Variable type:factor ───────────────────────────────────────────────────────────────────────────
    ##  variable missing complete  n n_unique                         top_counts
    ##      feed       0       71 71        6 soy: 14, cas: 12, lin: 12, sun: 12
    ##  ordered
    ##    FALSE
    ## 
    ## ── Variable type:numeric ──────────────────────────────────────────────────────────────────────────
    ##  variable missing complete  n   mean    sd  p0   p25 p50   p75 p100
    ##    weight       0       71 71 261.31 78.07 108 204.5 258 323.5  423
    ##      hist
    ##  ▃▅▅▇▃▇▂▂

### 프리젠테이션은 컴팩트한 수평 형식

``` r
skim(iris)
```

    ## Skim summary statistics
    ##  n obs: 150 
    ##  n variables: 5 
    ## 
    ## ── Variable type:factor ───────────────────────────────────────────────────────────────────────────
    ##  variable missing complete   n n_unique                       top_counts
    ##   Species       0      150 150        3 set: 50, ver: 50, vir: 50, NA: 0
    ##  ordered
    ##    FALSE
    ## 
    ## ── Variable type:numeric ──────────────────────────────────────────────────────────────────────────
    ##      variable missing complete   n mean   sd  p0 p25  p50 p75 p100
    ##  Petal.Length       0      150 150 3.76 1.77 1   1.6 4.35 5.1  6.9
    ##   Petal.Width       0      150 150 1.2  0.76 0.1 0.3 1.3  1.8  2.5
    ##  Sepal.Length       0      150 150 5.84 0.83 4.3 5.1 5.8  6.4  7.9
    ##   Sepal.Width       0      150 150 3.06 0.44 2   2.8 3    3.3  4.4
    ##      hist
    ##  ▇▁▁▂▅▅▃▁
    ##  ▇▁▁▅▃▃▂▂
    ##  ▂▇▅▇▆▅▂▂
    ##  ▁▂▅▇▃▂▁▁

### 문자열, 목록 및 기타 열 클래스 지원 기능 내장

``` r
skim(dplyr::starwars)
```

    ## Skim summary statistics
    ##  n obs: 87 
    ##  n variables: 13 
    ## 
    ## ── Variable type:character ────────────────────────────────────────────────────────────────────────
    ##    variable missing complete  n min max empty n_unique
    ##   eye_color       0       87 87   3  13     0       15
    ##      gender       3       84 87   4  13     0        4
    ##  hair_color       5       82 87   4  13     0       12
    ##   homeworld      10       77 87   4  14     0       48
    ##        name       0       87 87   3  21     0       87
    ##  skin_color       0       87 87   3  19     0       31
    ##     species       5       82 87   3  14     0       37
    ## 
    ## ── Variable type:integer ──────────────────────────────────────────────────────────────────────────
    ##  variable missing complete  n   mean    sd p0 p25 p50 p75 p100     hist
    ##    height       6       81 87 174.36 34.77 66 167 180 191  264 ▁▁▁▂▇▃▁▁
    ## 
    ## ── Variable type:list ─────────────────────────────────────────────────────────────────────────────
    ##   variable missing complete  n n_unique min_length median_length
    ##      films       0       87 87       24          1             1
    ##  starships       0       87 87       17          0             0
    ##   vehicles       0       87 87       11          0             0
    ##  max_length
    ##           7
    ##           5
    ##           2
    ## 
    ## ── Variable type:numeric ──────────────────────────────────────────────────────────────────────────
    ##    variable missing complete  n  mean     sd p0  p25 p50  p75 p100
    ##  birth_year      44       43 87 87.57 154.69  8 35    52 72    896
    ##        mass      28       59 87 97.31 169.46 15 55.6  79 84.5 1358
    ##      hist
    ##  ▇▁▁▁▁▁▁▁
    ##  ▇▁▁▁▁▁▁▁

### 유용한 요약 기능이 있습니다.

``` r
skim(iris) %>% summary()
```

    ## A skim object    
    ## 
    ## Name: iris   
    ## Number of Rows: 150   
    ## Number of Columns: 5    
    ##     
    ## Column type frequency    
    ## factor: 1   
    ## numeric: 4

### Tidyverse 스타일 선택자를 사용하여 개별 열을 선택할 수 있습니다

``` r
skim(iris, Sepal.Length, Petal.Length)
```

    ## Skim summary statistics
    ##  n obs: 150 
    ##  n variables: 5 
    ## 
    ## ── Variable type:numeric ──────────────────────────────────────────────────────────────────────────
    ##      variable missing complete   n mean   sd  p0 p25  p50 p75 p100
    ##  Petal.Length       0      150 150 3.76 1.77 1   1.6 4.35 5.1  6.9
    ##  Sepal.Length       0      150 150 5.84 0.83 4.3 5.1 5.8  6.4  7.9
    ##      hist
    ##  ▇▁▁▂▅▅▃▁
    ##  ▂▇▅▇▆▅▂▂

### Handles grouped data

`skim()`은 dplyr::group\_by를 사용하여 그룹화 된 데이터를 처리 할 수 있습니다.

``` r
iris %>% dplyr::group_by(Species) %>% skim()
```

    ## Skim summary statistics
    ##  n obs: 150 
    ##  n variables: 5 
    ##  group variables: Species 
    ## 
    ## ── Variable type:numeric ──────────────────────────────────────────────────────────────────────────
    ##     Species     variable missing complete  n mean   sd  p0  p25  p50  p75
    ##      setosa Petal.Length       0       50 50 1.46 0.17 1   1.4  1.5  1.58
    ##      setosa  Petal.Width       0       50 50 0.25 0.11 0.1 0.2  0.2  0.3 
    ##      setosa Sepal.Length       0       50 50 5.01 0.35 4.3 4.8  5    5.2 
    ##      setosa  Sepal.Width       0       50 50 3.43 0.38 2.3 3.2  3.4  3.68
    ##  versicolor Petal.Length       0       50 50 4.26 0.47 3   4    4.35 4.6 
    ##  versicolor  Petal.Width       0       50 50 1.33 0.2  1   1.2  1.3  1.5 
    ##  versicolor Sepal.Length       0       50 50 5.94 0.52 4.9 5.6  5.9  6.3 
    ##  versicolor  Sepal.Width       0       50 50 2.77 0.31 2   2.52 2.8  3   
    ##   virginica Petal.Length       0       50 50 5.55 0.55 4.5 5.1  5.55 5.88
    ##   virginica  Petal.Width       0       50 50 2.03 0.27 1.4 1.8  2    2.3 
    ##   virginica Sepal.Length       0       50 50 6.59 0.64 4.9 6.23 6.5  6.9 
    ##   virginica  Sepal.Width       0       50 50 2.97 0.32 2.2 2.8  3    3.18
    ##  p100     hist
    ##   1.9 ▁▁▅▇▇▅▂▁
    ##   0.6 ▂▇▁▂▂▁▁▁
    ##   5.8 ▂▃▅▇▇▃▁▂
    ##   4.4 ▁▁▃▅▇▃▂▁
    ##   5.1 ▁▃▂▆▆▇▇▃
    ##   1.8 ▆▃▇▅▆▂▁▁
    ##   7   ▃▂▇▇▇▃▅▂
    ##   3.4 ▁▂▃▅▃▇▃▁
    ##   6.9 ▂▇▃▇▅▂▁▂
    ##   2.5 ▂▁▇▃▃▆▅▃
    ##   7.9 ▁▁▃▇▅▃▂▃
    ##   3.8 ▁▃▇▇▅▃▁▂

## 니트(Knitted) 결과

단순히 데이터 프레임을 스키밍하면 위에 표시된 가로 인쇄 레이아웃이 생성됩니다. Kniiting 할 때 kable 및 pander
구현으로 향상된 렌더링을 사용할 수도 있습니다 (v2에 대한 pander 지원은 더 이상 사용되지 않습니다).

### kable 및 pander 옵션

향상된 인쇄 옵션은 kable() 또는 pander()에 파이핑하여 사용할 수 있습니다. pander 패키지와 knitr 패키지의
kable 함수를 기반으로합니다.이 예제는 knitting 후에 향상된 옵션이 어떻게 나타나는지 보여줍니다. 그러나 결과는 다를
수 있습니다 (자세한 내용은 비네트를 참조하십시오).

패키지 내의 pander 지원은 버전 2에서 더이상 사용되지 않습니다.

### kable을위한 옵션

results = ’asis’청크 옵션이 사용되고 skimr :: namespace가 knitr::kable로 대체되는 것을
방지하기 위해 skimr::namespace가 사용됩니다 (결과적으로 긴 skim\_df 객체가 인쇄 됨).

``` r
skim(iris) %>% skimr::kable()
```

    ## Skim summary statistics  
    ##  n obs: 150    
    ##  n variables: 5    
    ## 
    ## Variable type: factor
    ## 
    ##  variable    missing    complete     n     n_unique               top_counts               ordered 
    ## ----------  ---------  ----------  -----  ----------  ----------------------------------  ---------
    ##  Species        0         150       150       3        set: 50, ver: 50, vir: 50, NA: 0     FALSE  
    ## 
    ## Variable type: numeric
    ## 
    ##    variable      missing    complete     n     mean     sd     p0     p25    p50     p75    p100      hist   
    ## --------------  ---------  ----------  -----  ------  ------  -----  -----  ------  -----  ------  ----------
    ##  Petal.Length       0         150       150    3.76    1.77     1     1.6    4.35    5.1    6.9     ▇▁▁▂▅▅▃▁ 
    ##  Petal.Width        0         150       150    1.2     0.76    0.1    0.3    1.3     1.8    2.5     ▇▁▁▅▃▃▂▂ 
    ##  Sepal.Length       0         150       150    5.84    0.83    4.3    5.1    5.8     6.4    7.9     ▂▇▅▇▆▅▂▂ 
    ##  Sepal.Width        0         150       150    3.06    0.44     2     2.8     3      3.3    4.4     ▁▂▅▇▃▂▁▁

### pander를 위한 옵션

때때로 panderOptions( ‘knitr.auto.asis’, FALSE)가 필요할 수 있습니다.

``` r
skim(iris) %>% pander()
```

Skim summary statistics  
n obs: 150  
n variables: 5

| variable | missing | complete |  n  | n\_unique |
| :------: | :-----: | :------: | :-: | :-------: |
| Species  |    0    |   150    | 150 |     3     |

Table continues below

|           top\_counts            | ordered |
| :------------------------------: | :-----: |
| set: 50, ver: 50, vir: 50, NA: 0 |  FALSE  |

|   variable   | missing | complete |  n  | mean |  sd  | p0  | p25 | p50  | p75 |
| :----------: | :-----: | :------: | :-: | :--: | :--: | :-: | :-: | :--: | :-: |
| Petal.Length |    0    |   150    | 150 | 3.76 | 1.77 |  1  | 1.6 | 4.35 | 5.1 |
| Petal.Width  |    0    |   150    | 150 | 1.2  | 0.76 | 0.1 | 0.3 | 1.3  | 1.8 |
| Sepal.Length |    0    |   150    | 150 | 5.84 | 0.83 | 4.3 | 5.1 | 5.8  | 6.4 |
| Sepal.Width  |    0    |   150    | 150 | 3.06 | 0.44 |  2  | 2.8 |  3   | 3.3 |

Table continues below

| p100 |   hist   |
| :--: | :------: |
| 6.9  | ▇▁▁▂▅▅▃▁ |
| 2.5  | ▇▁▁▅▃▃▂▂ |
| 7.9  | ▂▇▅▇▆▅▂▂ |
| 4.4  | ▁▂▅▇▃▂▁▁ |

## `skim_df object` (긴 서식)

기본적으로 `skim()`은 콘솔에서 미려하게 인쇄하지만 계산할 수 있는 길고 깔끔한 형식의 skim\_df 객체도 생성합니다.

``` r
a <-  skim(chickwts)
dim(a)
```

    ## [1] 23  6

``` r
print.data.frame(skim(chickwts))
```

    ##    variable    type       stat     level    value formatted
    ## 1    weight numeric    missing      .all   0.0000         0
    ## 2    weight numeric   complete      .all  71.0000        71
    ## 3    weight numeric          n      .all  71.0000        71
    ## 4    weight numeric       mean      .all 261.3099    261.31
    ## 5    weight numeric         sd      .all  78.0737     78.07
    ## 6    weight numeric         p0      .all 108.0000       108
    ## 7    weight numeric        p25      .all 204.5000     204.5
    ## 8    weight numeric        p50      .all 258.0000       258
    ## 9    weight numeric        p75      .all 323.5000     323.5
    ## 10   weight numeric       p100      .all 423.0000       423
    ## 11   weight numeric       hist      .all       NA  ▃▅▅▇▃▇▂▂
    ## 12     feed  factor    missing      .all   0.0000         0
    ## 13     feed  factor   complete      .all  71.0000        71
    ## 14     feed  factor          n      .all  71.0000        71
    ## 15     feed  factor   n_unique      .all   6.0000         6
    ## 16     feed  factor top_counts   soybean  14.0000   soy: 14
    ## 17     feed  factor top_counts    casein  12.0000   cas: 12
    ## 18     feed  factor top_counts   linseed  12.0000   lin: 12
    ## 19     feed  factor top_counts sunflower  12.0000   sun: 12
    ## 20     feed  factor top_counts  meatmeal  11.0000   mea: 11
    ## 21     feed  factor top_counts horsebean  10.0000   hor: 10
    ## 22     feed  factor top_counts      <NA>   0.0000     NA: 0
    ## 23     feed  factor    ordered      .all   0.0000     FALSE

> 버전 2에서는 긴 skimr 객체가 지원되지 않습니다.

### 전체 skim\_df 개체에 대해 계산

``` r
skim(mtcars) %>% dplyr::filter(stat=="hist")
```

    ## # A tibble: 11 x 6
    ##    variable type    stat  level value formatted
    ##    <chr>    <chr>   <chr> <chr> <dbl> <chr>    
    ##  1 mpg      numeric hist  .all     NA ▃▇▇▇▃▂▂▂ 
    ##  2 cyl      numeric hist  .all     NA ▆▁▁▃▁▁▁▇ 
    ##  3 disp     numeric hist  .all     NA ▇▆▁▂▅▃▁▂ 
    ##  4 hp       numeric hist  .all     NA ▃▇▃▅▂▃▁▁ 
    ##  5 drat     numeric hist  .all     NA ▃▇▁▅▇▂▁▁ 
    ##  6 wt       numeric hist  .all     NA ▃▃▃▇▆▁▁▂ 
    ##  7 qsec     numeric hist  .all     NA ▃▂▇▆▃▃▁▁ 
    ##  8 vs       numeric hist  .all     NA ▇▁▁▁▁▁▁▆ 
    ##  9 am       numeric hist  .all     NA ▇▁▁▁▁▁▁▆ 
    ## 10 gear     numeric hist  .all     NA ▇▁▁▆▁▁▁▂ 
    ## 11 carb     numeric hist  .all     NA ▆▇▂▇▁▁▁▁

### `skimr` 사용자 정의

`skimr`은 유념된 기본값을 제공하지만 고도의 맞춤 설정이 가능합니다. 사용자는 자신의 통계를 지정하고, 결과 형식을
변경하고, 새 클래스에 대한 통계를 만들고, 데이터 프레임이 아닌 데이터 구조에 대한 `skimr`를 개발할 수
있습니다.

### 자체 통계 및 클래스 지정

사용자는 `skim_with()` 함수와 결합 된 목록을 사용하여 자체 통계를 지정할 수 있습니다. 이렇게하면 데이터에있는 명명된
클래스를 지원할 수 있습니다.

``` r
funs <- list(
  iqr = IQR,
  quantile = purrr::partial(quantile, probs = .99)
)

skim_with(numeric = funs, append = FALSE)
skim(iris, Sepal.Length)
```

    ## Skim summary statistics
    ##  n obs: 150 
    ##  n variables: 5 
    ## 
    ## ── Variable type:numeric ──────────────────────────────────────────────────────────────────────────
    ##      variable iqr quantile
    ##  Sepal.Length 1.3      7.7

``` r
# Restore defaults
skim_with_defaults()
```

### 서식 변경

skimr은 열의 십진수를 정렬 할 수있는 기본 형식 세트, 숫자 데이터의 적절한 소수 자릿수 및 날짜 표현을 제공합니다.
사용자는 `show_formats()`을 사용하여이를 볼 수 있으며 `skim_format()`을 사용하여 수정할
수 있습니다.

### 다른 객체 skimming

다른 객체에 대한 스키밍 기능 개발 절차는 추가 객체 지원 비네트에 설명되어 있습니다.

## 현재 버전의 제한 사항

인라인 히스토그램 및 꺾은 선형 차트를 다양한 상황에서 렌더링하는 데 문제가 있음을 알고 있습니다. 그 중 일부는 아래에 설명되어
있습니다.

### 스파크 히스토그램 지원

데이터 프레임을 인쇄할때 스파크 - 히스토그램 문자를 인쇄할 때 알려진 문제가 있습니다. 예를 들어 “▂▅▇”은 “\<U +
2582\> \<U + 2585\> \<U + 2587\>”로 인쇄됩니다. 이 오랜 문제는 데이터 프레임을 인쇄하기위한 저수준
코드에서 기인합니다. 몇 가지 사례가 다루어 지긴했지만, 예를 들어 Emacs ESS에서 이 문제에 대한 보고서가
있습니다.

즉, skimr은 히스토그램을 콘솔과 kable()로 렌더링 할 수 있지만 다른 상황에서는 사용할 수 없습니다. 여기에는 다음이
포함됩니다.

  - pander() 내에서 skimr 데이터 프레임 렌더링
  - skimr 데이터 프레임을 바닐라 R 데이터 프레임으로 변환하지만 티블은 올바르게 렌더링

Windows에서 이러한 문자를 표시하는 한가지 해결 방법은 `Sys.setlocale ("LC_CTYPE",
"Chinese")`을 사용하여 로케일의 CTYPE 부분을 중국어/일본어/한국어로 설정하는 것입니다. 이러한 값은 skim()으로
작성된 데이터 프레임을 목록 (`as.list()`) 또는 행렬 (`as.matrix()`)로 인쇄 할 때 기본적으로 표시됩니다.

### knitted 된 문서에서 스파크 히스토그램 및 선 그래프 인쇄

spark-bar 및 spark-line은 콘솔에서 작동하지만 특정 문서 형식으로 바꿀 때 작동하지 않을 수 있습니다. 올바르게
렌더링 된 HTML 문서를 생성하는 동일한 세션에서 잘못 렌더링된 PDF가 생성될 수 있습니다. 이 문제는 일반적으로 좋은
구성 요소 (히스토그램) 및 점자 지원 (선 그래프)이있는 글꼴로 변경하여 해결할 수 있습니다. 예를 들어
extrafont 패키지의 열린 글꼴 “DejaVu Sans”는 이러한 기능을 지원합니다. knitr :: kable ()에서
결과를 래핑 해 볼 수도 있습니다.

유형이 다른 문서의 디스플레이는 다양합니다. 예를 들어 한 사용자가 “Yu Gothic UI Semilight”글꼴이
Microsoft Word 및 Libre Office Write에 대해 일관된 결과를 산출한다는 사실을 알게되었습니다.

참고: [skimr 공식 홈페이지](https://ropensci.github.io/skimr/index.html)
