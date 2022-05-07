



# 깔끔화 방법 {#tidy-data-how}

## 깔끔화 동사 {#tidyr-verbs-how}

`dplyr` 패키지 데이터 문법에 `select`, `filter`, `mutate`, `group_by + summarize`, `arrange` 5개 주요 동사(Verb)를 
사용해서 데이터와 커뮤니케이션하며 데이터 조작을 하듯이 `tidyr` 패키지 깔끔한 데이터 조작 동사를 익혀두면 엉망진창인 데이터에 특효약이다.

깔끔한 데이터(Tidy Data)를 만드는데 동원되는 동사에 해당되는 함수는 다음과 같다. 
다음 데이터 깔끔이 함수들을 이용하여 엉망진창인 데이터(Messy Data)를 깔끔한 데이터로 만들어서 
후속 작업에 속도를 높일 수 있다. 
`tidyr` 팩키지에 포함된 Tidy Data 관련 핵심을 이루는 함수로 다음을 꼽을 수 있다.

- `pivot_longer()` / `pivot_wider()` : 데이터프레임 형태 변환 동사
- `separate()`, `separate_rows()` / `unite()` : 변수 쪼개고 합하는 동사
- `expand_grid()` : 결측값 처리 동사

**깔끔한 데이터 관련 R 패키지**는 해드릴 위컴이 주축이 되어 10년 이상 발전시켜 
완성한 개념으로 그 단계 단계마다 개발된 팩키지가 있고 개념을 
구체화하며 실제로 구현한 함수들도 점점 진화해 나갔다. 
과거 진화과정을 살펴보면 어느 순간 깔끔한 데이터(Tidy Data) 개념과 동시에 도구가
나온 것이 아니라 경험과 새로운 지식이 축적되며 진화를 거듭한 결과로 볼 수 있다.

- `reshape`, `reshape2` ([Wickham 2007](http://www.jstatsoft.org/v21/i12/))
- `plyr` ([Wickham 2011](http://www.jstatsoft.org/v40/i01/))
- `tidyr` ([Wickham 2013](https://www.jstatsoft.org/article/view/v059i10))

엉망진창인 데이터는 빅데이터와 공공데이터가 널려있는 현시점 어디서나 마주할 수 있지만,
다행히도 앞선 데이터 과학자들이 각자 맞닥드린 문제를 해결하며 경험을 공유하고 있다.
다음에 나온 사례가 엉망진창 데이터 전부는 아니지만 각 사례별로 깔끔한 데이터로 변환시키는 
과정을 살펴보면 향후 맞닥드릴 수 있는 데이터 문제에 좋은 영감을 줄 것으로 생각된다.

## `pivot_*()` 동사 {#pivot-revolution}

해들리 위컴이 언급했듯이, `gather`/`spread`는 사라지지 않고 훨씬 더 나은 모습으로 재탄생했습니다.
`pivot_longer()`, `pivot_wider()` 함수는 `gather()`, `spread()` 함수를 개선하면서 다른 팩키지의 최신 기능을 추가시켰다.

- `pivot_longer()`은 `data.table` 패키지 `melt()`, `dcast()`와 연관됨.
- `cdata` 패키지에 영감을 받아 `pivot_longer()`, `pivot_wider()` 명칭으로 통일됨.

기억하기 좋게 인텍스(index)를 갖는 긴 자료형(long)과 데카르트 평면(Cartesian)과 같은 
넓은(wide) 자료형으로 구분한다. 
`tidyr` 패키지가 버전 1.0으로 정식 버젼업하면서 생겨난 가장 큰 변화 중 하나가 아닐까 싶다.

### 데카르트 평면 &rarr; 인덱스: `pivot_longer()` {#pivot-longer-example}

`pivot_longer()` 함수를 사용해서 데카르트 평면과 같이 넓은 데이터(wide)를 인덱스가 붙은 긴 형태(long) 데이터로 변환시킬 수 있다. `relig_income` 데이터는 `gather/spread` 시절부터 자주 예제 데이터로 사용되던 예제 데이터셋이다.

`religion` 변수를 제외하고 나머지 변수가 `names_to`를 통해 인덱스 명을 바꿀 수 있고 이들이 담고 있던 값은 `values_to`로 떨어지게 된다.

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 10px;}
</style>
<div class = "blue">


```r
dataframe %>% 
  pivot_longer(
    cols,
    ...
  )
```


`cols`: `dplyr::select()` 구문과 동일하게 작성함.

</div>


<div class = "row">
  <div class = "col-md-6">
**데카르트 평면 넓은 데이터**


```r
library(tidyr)

relig_income %>% head
```

```
## # A tibble: 6 × 11
##   religion  `<$10k` `$10-20k` `$20-30k` `$30-40k` `$40-50k` `$50-75k` `$75-100k`
##   <chr>       <dbl>     <dbl>     <dbl>     <dbl>     <dbl>     <dbl>      <dbl>
## 1 Agnostic       27        34        60        81        76       137        122
## 2 Atheist        12        27        37        52        35        70         73
## 3 Buddhist       27        21        30        34        33        58         62
## 4 Catholic      418       617       732       670       638      1116        949
## 5 Don’t k…       15        14        15        11        10        35         21
## 6 Evangeli…     575       869      1064       982       881      1486        949
## # … with 3 more variables: `$100-150k` <dbl>, `>150k` <dbl>,
## #   `Don't know/refused` <dbl>
```

  </div>
  <div class = "col-md-6">
**인덱스 긴 형식 데이터**


```r
relig_income %>% 
  pivot_longer(-religion, names_to="소득", values_to = "사람수")
```

```
## # A tibble: 180 × 3
##   religion 소득    사람수
##   <chr>    <chr>    <dbl>
## 1 Agnostic <$10k       27
## 2 Agnostic $10-20k     34
## 3 Agnostic $20-30k     60
## 4 Agnostic $30-40k     81
## 5 Agnostic $40-50k     76
## 6 Agnostic $50-75k    137
## # … with 174 more rows
```
  </div>
</div>

### 접두사(prefix) 제거: `pivot_longer()` {#pivot-longer-example-prefix}

`names_prefix`를 사용해서 `$`, `<$`, `>` 붙은 값을 제거시킬 수 있다.


```r
relig_income %>% 
  pivot_longer(-religion, names_to="소득", 
               values_to = "명수",
               names_prefix = "\\$|<\\$|>")
```

```
## # A tibble: 180 × 3
##   religion 소득    명수
##   <chr>    <chr>  <dbl>
## 1 Agnostic 10k       27
## 2 Agnostic 10-20k    34
## 3 Agnostic 20-30k    60
## 4 Agnostic 30-40k    81
## 5 Agnostic 40-50k    76
## 6 Agnostic 50-75k   137
## # … with 174 more rows
```

### 자료형 변환: `pivot_longer()` {#pivot-longer-example-type}

<div class = "row">
  <div class = "col-md-6">
**빌보드 원본 데이터: 자료변환 전**

일단 주간 순위는 어찌해서 만들었지만, 주간 칼럼에 `wk`가 들어 있어 이를 `names_prefix`를 통해 날려버리고 나도 숫자로 되어 있는 문자를 다시 숫자형으로 칼럼 자료형을 바꿔야 한다.


```r
billboard %>% 
  pivot_longer(
    cols = starts_with("wk"), 
    names_to = "주간", 
    values_to = "순위",
    values_drop_na = TRUE
  )
```

```
## # A tibble: 5,307 × 5
##   artist track                   date.entered 주간   순위
##   <chr>  <chr>                   <date>       <chr> <dbl>
## 1 2 Pac  Baby Don't Cry (Keep... 2000-02-26   wk1      87
## 2 2 Pac  Baby Don't Cry (Keep... 2000-02-26   wk2      82
## 3 2 Pac  Baby Don't Cry (Keep... 2000-02-26   wk3      72
## 4 2 Pac  Baby Don't Cry (Keep... 2000-02-26   wk4      77
## 5 2 Pac  Baby Don't Cry (Keep... 2000-02-26   wk5      87
## 6 2 Pac  Baby Don't Cry (Keep... 2000-02-26   wk6      94
## # … with 5,301 more rows
```


  </div>
  <div class = "col-md-6">
**빌보드 원본 데이터: 자료변환 후**


```r
billboard %>% 
  pivot_longer(
    cols = starts_with("wk"), 
    names_to = "주간", 
    values_to = "순위",
    names_prefix = "wk",
    names_ptypes = list(`주간` = as.character()),
    values_drop_na = TRUE
  )
```

```
## # A tibble: 5,307 × 5
##   artist track                   date.entered 주간   순위
##   <chr>  <chr>                   <date>       <chr> <dbl>
## 1 2 Pac  Baby Don't Cry (Keep... 2000-02-26   1        87
## 2 2 Pac  Baby Don't Cry (Keep... 2000-02-26   2        82
## 3 2 Pac  Baby Don't Cry (Keep... 2000-02-26   3        72
## 4 2 Pac  Baby Don't Cry (Keep... 2000-02-26   4        77
## 5 2 Pac  Baby Don't Cry (Keep... 2000-02-26   5        87
## 6 2 Pac  Baby Don't Cry (Keep... 2000-02-26   6        94
## # … with 5,301 more rows
```

  </div>
</div>


### 다수 칼럼: `pivot_longer()` {#pivot-longer-example-columns}

`who` 데이터셋과 같이 다수 칼럼이 포함된 경우가 있을 수 있다. 이런 경우 `names_pattern` 정규표현식을 적용시켜 변수를 추출할 수 있다. 먼저 `who` 데이터셋을 살펴보자.

- `new_`/`new` 접두어
- `sp`/`rel`/`sp`/`ep` 진단 구분
- `m`/`f` 성별
- `014`/`1524`/`2535`/`3544`/`4554`/`65` 연령대

먼저, `country`, `iso2`, `iso3`,  `year` 4개 변수는 정리되어 있어 그대로 두고, 나머지 변수를 `dplyr::select` 문법에 맞춰 선택하고 이를 변경시킨다.
`names_to`로 변수명 3개를 지정하고, `new_`/`new` 접두어는 고정되어 `names_pattern`에서 정규표현식으로 패턴을 적의한다. 이와 더불어 `names_ptypes`에서 자료형도 함께 지정한다.


```r
who %>% pivot_longer(
  cols = new_sp_m014:newrel_f65,
  names_to = c("diagnosis", "gender", "age"), 
  names_pattern = "new_?(.*)_(m|f)(.*)",
  names_ptypes = list(
    gender = factor(levels = c("f", "m")),
    age = factor(
      levels = c("014", "1524", "2534", "3544", "4554", "5564", "65"), 
      ordered = TRUE
    )
  ),
  values_to = "count",
) %>% 
  arrange(desc(count))
```

```
## # A tibble: 405,440 × 8
##   country iso2  iso3   year diagnosis gender age    count
##   <chr>   <chr> <chr> <int> <chr>     <fct>  <ord>  <int>
## 1 India   IN    IND    2007 sn        m      3544  250051
## 2 India   IN    IND    2007 sn        f      3544  148811
## 3 China   CN    CHN    2013 rel       m      65    124476
## 4 China   CN    CHN    2013 rel       m      5564  112558
## 5 India   IN    IND    2007 ep        m      3544  105825
## 6 India   IN    IND    2007 ep        f      3544  101015
## # … with 405,434 more rows
```

### 한행에 다수 관측점: `pivot_longer()` {#pivot-longer-example-rows}

한행에 관측점이 다수 있는 재미있는 데이터도 있다.
즉, 첫번째 가정에 아이가 2명 있는데 첫째 아이 생일과 성별, 둘째 아이 생일과 성별이 한 행에 놓여있는 경우가 이에 해당된다.


```r
family <- tribble(
  ~family,  ~dob_child1,  ~dob_child2, ~gender_child1, ~gender_child2,
       1L, "1998-11-26", "2000-01-29",             1L,             2L,
       2L, "1996-06-22",           NA,             2L,             NA,
       3L, "2002-07-11", "2004-04-05",             2L,             2L,
       4L, "2004-10-10", "2009-08-27",             1L,             1L,
       5L, "2000-12-05", "2005-02-28",             2L,             1L,
)
family
```

```
## # A tibble: 5 × 5
##   family dob_child1 dob_child2 gender_child1 gender_child2
##    <int> <chr>      <chr>              <int>         <int>
## 1      1 1998-11-26 2000-01-29             1             2
## 2      2 1996-06-22 <NA>                   2            NA
## 3      3 2002-07-11 2004-04-05             2             2
## 4      4 2004-10-10 2009-08-27             1             1
## 5      5 2000-12-05 2005-02-28             2             1
```


이를 원하는 이름으로 변경시키기 위해서 `names_to=`에 `.value`라는 특수명칭을 사용하여 한 관측점에 다수 관측점 정보가 포함된 문제를 해결한다.


```r
family %>% 
  pivot_longer(
    -family, 
    names_to = c(".value", "child"), 
    names_sep = "_", 
    values_drop_na = TRUE
  ) %>% 
  dplyr::mutate(dob = parse_date(dob))
```

```
## # A tibble: 9 × 4
##   family child  dob        gender
##    <int> <chr>  <date>      <int>
## 1      1 child1 1998-11-26      1
## 2      1 child2 2000-01-29      2
## 3      2 child1 1996-06-22      2
## 4      3 child1 2002-07-11      2
## 5      3 child2 2004-04-05      2
## 6      4 child1 2004-10-10      1
## # … with 3 more rows
```

### 칼럼명이 중복됨: `pivot_longer()` {#pivot-longer-example-duplicated}

종복된 칼럼명이 존재하는 경우 작업하기 까다로운데... `pivot_longer()`에서 자동으로 칼럼을 추가시켜 문제를 풀어준다.

<div class = "row">
  <div class = "col-md-6">
**중복칼럼 데이터셋**


```r
df <- tibble(x = 1:3, y = 4:6, y = 5:7, y = 7:9, .name_repair = "minimal")
df
```

```
## # A tibble: 3 × 4
##       x     y     y     y
##   <int> <int> <int> <int>
## 1     1     4     5     7
## 2     2     5     6     8
## 3     3     6     7     9
```

  </div>
  <div class = "col-md-6">
**중복칼럼 데이터셋 작업결과**


```r
df %>% 
  pivot_longer(-x, names_to = "name", values_to = "value")
```

```
## # A tibble: 9 × 3
##       x name  value
##   <int> <chr> <int>
## 1     1 y         4
## 2     1 y         5
## 3     1 y         7
## 4     2 y         5
## 5     2 y         6
## 6     2 y         8
## # … with 3 more rows
```

  </div>
</div>

## 인덱스 &rarr; 데카르트 평면: `pivot_wider()` {#pivot-longer-wider}

`pivot_wider()`는 깔끔한 데이터를 만드는데 그다지 흔한 경우는 아니지만, 요약표를 만드거나 할 때 종종 사용되고, `pivot_longer()`와 정반대라고 보면 된다.

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 10px;}
</style>
<div class = "blue">


```r
dataframe %>% 
  pivot_wider(
    names_from,
    values_from,
    ...
  )
```

- `names_from`은 칼럼값을 지정하는 칼럼
- `values-from`은 값(value)을 지정하는 칼럼

</div>

`tidyr::fish_encounters` 패키지에 내장된 `fish_encounters` 데이터셋은 ["Visualizing Fish Encounter Histories", February 3 2018](https://fishsciences.github.io/post/visualizing-fish-encounter-histories/)에 나온 물고기 포획 방류, 재포획 즉, capture-recapture 데이터셋이다. `pivot_wider()` 함수에 `names_from`, `value_from`을 지정하여 데카르트 평면에 좌표로 찍듯이 데이터를 펼친다. 결측값이 생기는 것은 `values_fill`을 사용해서 0으로 채워넣게 되면 결측값도 정리되고 사람이 이해하기 쉬운 데이터로 즉시 일별하여 확인이 가능하다.

<div class = "row">
  <div class = "col-md-4">
**깔끔한 데이터: 인덱스 긴 형식**


```r
fish_encounters
```

```
## # A tibble: 114 × 3
##   fish  station  seen
##   <fct> <fct>   <int>
## 1 4842  Release     1
## 2 4842  I80_1       1
## 3 4842  Lisbon      1
## 4 4842  Rstr        1
## 5 4842  Base_TD     1
## 6 4842  BCE         1
## # … with 108 more rows
```

  </div>
  <div class = "col-md-8">
**요약표 형태 데이터**


```r
fish_encounters %>% 
  pivot_wider(names_from = station, 
              values_from = seen,
              values_fill = list(seen = 0)) %>% 
  head(10)
```

```
## # A tibble: 10 × 12
##   fish  Release I80_1 Lisbon  Rstr Base_TD   BCE   BCW  BCE2  BCW2   MAE   MAW
##   <fct>   <int> <int>  <int> <int>   <int> <int> <int> <int> <int> <int> <int>
## 1 4842        1     1      1     1       1     1     1     1     1     1     1
## 2 4843        1     1      1     1       1     1     1     1     1     1     1
## 3 4844        1     1      1     1       1     1     1     1     1     1     1
## 4 4845        1     1      1     1       1     0     0     0     0     0     0
## 5 4847        1     1      1     0       0     0     0     0     0     0     0
## 6 4848        1     1      1     1       0     0     0     0     0     0     0
## # … with 4 more rows
```

  </div>
</div>

### 총계(aggregation): `pivot_wider()` {#pivot-longer-wider-aggregation}

`pivot_wider()`를 통해 총계(aggregate)를 내야하는 상황이 발생하곤 한다. 
`datasets` 패키지에 내장된 실험계획법이 적용된 데이터 `warpbreaks`를 보면 `wool`, `tension` 두가지 요인으로 총 9번 실험한 결과가 `breaks`에 담겨진 것을 확인할 수 있다.


```r
warpbreaks <- datasets::warpbreaks %>% 
  as_tibble() %>% 
  select(wool, tension, breaks)

warpbreaks %>% 
  count(wool, tension)
```

```
## # A tibble: 6 × 3
##   wool  tension     n
##   <fct> <fct>   <int>
## 1 A     L           9
## 2 A     M           9
## 3 A     H           9
## 4 B     L           9
## 5 B     M           9
## 6 B     H           9
```

현재 인덱스로 잘 정제된 긴형태 데이터를 요약표 형태로 데카르트 평면과 같이 정리하고자 하면 다음과 같이 작업하게 되면 `wool`, `tension` 요인별로 총 9개 `breaks`값이 한 곳에 몰려있는 것을 파악할 수 있다. `values_fn`을 사용해서 총계로 평균, 최대, 최소 등을 사용해서 하나의 값으로 요약할 수 있다.

<div class = "row">
  <div class = "col-md-6">
**요인별 원데이터**


```r
warpbreaks %>% 
  pivot_wider(
    names_from = wool,
    values_from = breaks
  )
```

```
## # A tibble: 3 × 3
##   tension A         B        
##   <fct>   <list>    <list>   
## 1 L       <dbl [9]> <dbl [9]>
## 2 M       <dbl [9]> <dbl [9]>
## 3 H       <dbl [9]> <dbl [9]>
```


  </div>
  <div class = "col-md-6">
**요인별 총계: 최대값**


```r
warpbreaks %>% 
  pivot_wider(
    names_from = wool,
    values_from = breaks,
    values_fn = list(breaks = max)
  )
```

```
## # A tibble: 3 × 3
##   tension     A     B
##   <fct>   <dbl> <dbl>
## 1 L          70    44
## 2 M          36    42
## 3 H          43    28
```

  </div>
</div>


## `separate()`와 `unite()` {#separate-unite-verbs}

### 칼럼에 변수 두개 포함 {#separate-columns}

칼럼 하나에 다수 변수가 포함된 경우를 흔히 발견할 수 있다. `dplyr` 팩키지에는 `starwars` 데이터셋에 등장하는 인물에 대한 인적정보가 담겨있다. 예를 들어, `name` 칼럼은 두 변수가 숨어 있다. 하나는 성(`last name`) 다른 하나는 이름(`first name`)이다. 이를 두개로 쪼개어 두는 것이 Tidy Data를 만든다고 볼 수 있다. 꼭 그런 것은 아니고 경우에 따라 차이가 있지만, 개념적으로 그렇다는 것이다. 상황에 맞춰 유연하게 사용한다. 


```r
starwars_name_df <- dplyr::starwars %>% 
  select(name, species, height, mass) %>% 
  filter(str_detect(species, "Human"))

starwars_name_df
```

```
## # A tibble: 35 × 4
##   name               species height  mass
##   <chr>              <chr>    <int> <dbl>
## 1 Luke Skywalker     Human      172    77
## 2 Darth Vader        Human      202   136
## 3 Leia Organa        Human      150    49
## 4 Owen Lars          Human      178   120
## 5 Beru Whitesun lars Human      165    75
## 6 Biggs Darklighter  Human      183    84
## # … with 29 more rows
```

`separate()` 함수를 사용해서 칼럼을 두개로 쪼갠다. 이런 경우 `sep=` 인자를 통해 구분자를 지정한다. 공백, `;`, `,` 등 문제에 따라 구분자를 달리 사용한다.


```r
starwars_name_df %>% 
  separate(name, into = c("first_name", "last_name"), sep=" ")
```

```
## # A tibble: 35 × 5
##   first_name last_name   species height  mass
##   <chr>      <chr>       <chr>    <int> <dbl>
## 1 Luke       Skywalker   Human      172    77
## 2 Darth      Vader       Human      202   136
## 3 Leia       Organa      Human      150    49
## 4 Owen       Lars        Human      178   120
## 5 Beru       Whitesun    Human      165    75
## 6 Biggs      Darklighter Human      183    84
## # … with 29 more rows
```

### 칼럼에 변수 다수 포함 {#separate-columns-many}

TidyTuesday에서 나왔던 칵테일 데이터를 보게 되면 각 칵테일을 제작하는데 필요한 재표가 `ingredient` 칼럼 안에 콤마(`,`)로 묶여있다. 이런 데이터는 절대로 Tidy한 데이터가 아니라서 적절한 조치가 필요하다.


```r
# cocktail_data <- tidytuesdayR::tt_load('2020-05-26')

cocktails <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-05-26/cocktails.csv')

cocktail_df <- cocktails %>% 
  select(drink, ingredient)

cocktail_tbl <- cocktail_df %>% 
  group_by(drink) %>% 
  nest() %>% 
  mutate(ingredient_tmp = map(data, function(df) df %>%  select(ingredient) %>% pull) ) %>% 
  mutate(ingredient = map_chr(ingredient_tmp, paste, collapse = ", ")) %>% 
  ungroup 

cocktail_tbl %>% 
  select(drink, ingredient)
```

```
## # A tibble: 546 × 2
##   drink                                ingredient                               
##   <chr>                                <chr>                                    
## 1 '57 Chevy with a White License Plate Creme de Cacao, Vodka                    
## 2 1-900-FUK-MEUP                       Absolut Kurant, Grand Marnier, Chambord …
## 3 110 in the shade                     Lager, Tequila                           
## 4 151 Florida Bushwacker               Malibu rum, Light rum, 151 proof rum, Da…
## 5 155 Belmont                          Dark rum, Light rum, Vodka, Orange juice 
## 6 24k nightmare                        Goldschlager, Jägermeister, Rumple Minze…
## # … with 540 more rows
```

먼저 `stringr` 팩키지 `str_split()` 함수로 `ingredient` 칼럼을 `, `을 구분자로 삼아 쪼갠 후에 list-column 형태 칼럼(`ingredient_lc`)으로 저장시킨다. 그리고 나서 `unnest()` 함수로 중첩된 것을 풀게 되면 다음과 같은 티블 데이터프레임이 되어 Tidy Data로 변환된다.


```r
cocktails_tbl <- cocktail_tbl %>% 
  select(drink, ingredient) %>% 
  mutate(ingredient_lc = str_split(ingredient, ", ")) %>% 
  unnest(ingredient_lc)

cocktails_tbl
```

```
## # A tibble: 2,104 × 3
##   drink                                ingredient                  ingredient_lc
##   <chr>                                <chr>                       <chr>        
## 1 '57 Chevy with a White License Plate Creme de Cacao, Vodka       Creme de Cac…
## 2 '57 Chevy with a White License Plate Creme de Cacao, Vodka       Vodka        
## 3 1-900-FUK-MEUP                       Absolut Kurant, Grand Marn… Absolut Kura…
## 4 1-900-FUK-MEUP                       Absolut Kurant, Grand Marn… Grand Marnier
## 5 1-900-FUK-MEUP                       Absolut Kurant, Grand Marn… Chambord ras…
## 6 1-900-FUK-MEUP                       Absolut Kurant, Grand Marn… Midori melon…
## # … with 2,098 more rows
```

자, 이제 깔끔한 데이터의 힘을 느껴보자. 칵테일 중에 가장 다양한 재료가 포함된 칵테일은 무엇인가? 라는 질문에 단순한 `dplyr` 동사로 확인이 바로 가능하다.


```r
cocktails_tbl %>% 
  group_by(drink) %>% 
  summarise(num_ingredients = n()) %>% 
  arrange(desc(num_ingredients))
```

```
## # A tibble: 546 × 2
##   drink                  num_ingredients
##   <chr>                            <int>
## 1 Angelica Liqueur                    12
## 2 Amaretto Liqueur                    11
## 3 Egg Nog #4                          11
## 4 Arizona Twister                      9
## 5 1-900-FUK-MEUP                       8
## 6 151 Florida Bushwacker               8
## # … with 540 more rows
```

### `separate_rows()` 함수로 빠르게 {#separete-rows-quickly}

상기 과정이 다소 많은 동사를 조합해서 결과를 도출하고 있다고 생각된다면 `separate_rows()` 함수를 사용해서 깔끔한 데이터를 만든 후에 `count` 함수와 `sort = TRUE`를 활용하여 동일한 작업을 간단히 마무리 할 수 있다.


```r
cocktail_tbl %>% 
  separate_rows(ingredient, sep = ", ") %>% 
  count(drink, sort = TRUE)
```

```
## # A tibble: 546 × 2
##   drink                      n
##   <chr>                  <int>
## 1 Angelica Liqueur          12
## 2 Amaretto Liqueur          11
## 3 Egg Nog #4                11
## 4 Arizona Twister            9
## 5 1-900-FUK-MEUP             8
## 6 151 Florida Bushwacker     8
## # … with 540 more rows
```

## 결측 데이터 {#fix-missing-data}

[data.world Nuclear Weapon Explosions BY THOMAS DRÄBING](https://data.world/tdreabing/nuclear-weapon-explosions) 웹사이트에서 핵폭탄 실험 데이터를 다운로드 받을 수 있다. 
데이터 자체는 완벽하지만 의미상 문제가 있다. 
즉 1945년 이후 데이터를 보자면 숨어있는 결측값이 보인다. 
확인이 안된 것 한건 빼면 7개국이 핵폭탄 실험을 한 것으로 나오는데 1945년에는 아무런 기록이 없다. 
데이터프레임에는 정상인 깔끔한 데이터지만 사실 1945년 0 건에 예를 들어 중국이나 러시아가 포함되어 있는 것이 맞다.
이런 데이터 자체 결측값을 찾아내서 채워넣는 것이 깔끔한 데이터를 만드는 또하나 중요한 과정이다.


```r
library(lubridate)

nuclear_raw <- read_csv("data/nuclear_weapon_explosions_1945-1998.csv")

nuclear_trials_df <- nuclear_raw %>% 
  janitor::clean_names() %>% 
  mutate(datetime = lubridate::parse_date_time(datetime, "%m/%d/%Y %H:%M:%S %p")) %>% 
  mutate(연도 = year(datetime)) %>% 
  count(country, 연도, name = "핵실험횟수") %>% 
  rename(국가 = country) %>% 
  arrange(연도) %>% 
  filter(연도 <= 1954)

nuclear_trials_df 
```

```
## # A tibble: 13 × 3
##   국가    연도 핵실험횟수
##   <chr>  <dbl>      <int>
## 1 USA     1945          3
## 2 USA     1946          2
## 3 USA     1948          3
## 4 Russia  1949          1
## 5 Russia  1951          2
## 6 USA     1951         16
## # … with 7 more rows
```

### 결측값 명시: `expand_grid()` {#expand-grid-comes-in}

상기와 같은 문제를 해결하기 위해 `expand_grid()` 함수를 사용해서 핵실험을 수행한 모든 국가에 대해 해당 년도를 모두 생성할 필요가 있다. 
3개 국가에 대해 10년을 `expand_grid()` 함수로 조합하게 되면 30개 관측점이 생기는데 미국이 핵실험을 하지 않은 1947년의 경우 `NA` 값이 생긴다. 이를 채워주어야 한다. 앞서 `expand_grid()` 함수로 생성된 Tidy Data를 염두에 두고 이를 `left_join()` 함수와 조인을 걸어 나중에 치환시킬 데이터프레임을 사전 제작한다.



```r
country_year_tbl <- expand_grid(국가 = c("USA", "Russia", "England"),
                               연도 = seq(1945, 1954, 1))

nuclear_tbl <- country_year_tbl %>% 
  left_join(nuclear_trials_df)

nuclear_tbl
```

```
## # A tibble: 30 × 3
##   국가   연도 핵실험횟수
##   <chr> <dbl>      <int>
## 1 USA    1945          3
## 2 USA    1946          2
## 3 USA    1947         NA
## 4 USA    1948          3
## 5 USA    1949         NA
## 6 USA    1950         NA
## # … with 24 more rows
```

### 결측값 치환: `replace_na()` {#expand-grid-comes-in-replace_na}

결측값을 특정 값으로 채워 넣고자 하는 경우 `replace_na()` 함수를 사용한다.
먼저 `replace_na()` 함수를 사용해서 `n_bombs` 변수에 `NA` 결측값을 0으로 채워넣는다.
다른 국가명이나 연도에 결측값이 있는 경우 동일하게 변수명을 지정하고 결측값을 지정하여 해결한다.


```r
nuclear_tbl %>% 
  replace_na(list(핵실험횟수 = 0,
                  연도 = 1945))
```

```
## # A tibble: 30 × 3
##   국가   연도 핵실험횟수
##   <chr> <dbl>      <int>
## 1 USA    1945          3
## 2 USA    1946          2
## 3 USA    1947          0
## 4 USA    1948          3
## 5 USA    1949          0
## 6 USA    1950          0
## # … with 24 more rows
```


### 결측값 제거: `drop_na()` {#expand-grid-comes-in-drop}

다른 것 다 모르겠고 데이터프레임에 결측값이 있으면 안되기 때문에 그냥 제거한다.
이럴 때 사용하는 함수가 `drop_na()`다.


```r
nuclear_tbl %>% 
  drop_na(핵실험횟수)
```

```
## # A tibble: 13 × 3
##   국가   연도 핵실험횟수
##   <chr> <dbl>      <int>
## 1 USA    1945          3
## 2 USA    1946          2
## 3 USA    1948          3
## 4 USA    1951         16
## 5 USA    1952         10
## 6 USA    1953         11
## # … with 7 more rows
```



