




# 깔끔한 데이터 {#tidy-data}

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 10px;}
</style>
<div class = "blue">

**톨스토이** 장편소설 "안나 카레니나"에 나오는 유명한 문구로 **행복한 가정** 원칙이 있다.

> “행복한 가정은 모두 비슷한 이유로 행복하지만 불행한 가정은 저마다의 이유로 불행하다.”
> ("Все счастливые семьи похожи друг на друга, каждая несчастливая семья несчастлива по-своему",
> Happy families are all alike, but every unhappy family is unhappy in its own way.)

**유발 하라리**도 이를 차용하여 "세상에 실패한 국가의 유형도 각기 저마다의 이유로 다르지만, 성공한 국가는 비슷한 이유를 갖는다"라고 주장했고, **해들리 위컴(Hadley Wickham)**도 "Like families, tidy datasets are all alike but every messy dataset is messy in its own way." [Tidy Data](https://cran.r-project.org/web/packages/tidyr/vignettes/tidy-data.html)에 대한 정의를 내리면서 동일한 주장을 했다.

</div>


## 데이터 깔끔화 {#tidy-data}

시각화를 하기 전에 데이터를 깔끔하게 해야하는 이유는 근본적으로 사람이 보는 데이터와 기계가 처리하는 
데이터의 형태가 다르기 때문이다. 즉, 사람이 보기 좋은 데이터는 기계가 처리하기 어렵고,
기계가 처리하기 좋은 형태는 사람이 보고 이해하는데 어려움이 있다.
따라서, 시각화를 위한 깔끔한 데이터(Tidy datasets)가 준비되면, 데이터를 조작하고, 모형화하고, 시각화는 물론이고
커뮤니케이션도 용이하다.

깔끔한 데이터 원칙은 코드(Codd) 박사의 **관계대수(relational algebra)** 와 깊은 관련이 있지만, 
통계학 전공자에게 친숙한 언어로 표현하여 데이터를 다루는 실무에 익숙한 통계학자와 데이터 분석가에 적합하다.

깔끔한 데이터는 특정한 구조를 갖추고 있는데 **변수** 는 열(column)이고, **관측점** 은 행(row)이며, 
행과 열이 교차하는 셀(Cell)은 **값(Value)**이 위치하게 되고 
**관측단위에 대한 형태** 는 테이블(table)로 구성된다. [@JSSv059i10]

해당 데이터셋에 관측점과 변수를 식별하는 것이 쉽게 생각되지만, 
일반적으로 변수와 관측점을 정확하게 정의하는 것이 놀랍게도 어렵다.
행과 행보다는 변수간 **기능적 관계(functional relationship)**를 기술하는 것이 더 쉽기 때문에
칼럼 그룹집단 비교보다 관측점 그룹집단 사이 비교를 하는 것이 더 쉽다. 
최근에 `rowwise()` 연산자가 도입되어 필요한 경우 행으로 데이터 작업하는 것도 수월해졌다.

## 깔끔한 데이터 {#tidy-data-setup}

깔끔한 데이터는 데이터셋의 의미를 구조에 매칭하는 표준적인 방식이다. 
행, 열, 테이블이 관측점, 변수, 형식에 매칭되는 방식에 따라 
데이터셋이 깔끔하거나 깔끔하지 않은 것으로 나뉜다.

**깔끔한 데이터(tidy data)** 는 결국 데이터 분석을 쉽게 할 수 있는 데이터로 볼 수 있다.

1. 각 변수가 칼럼이 된다.
1. 각 관측점은 행이 된다.
1. 각 셀은 값이 된다.
1. 관측 단위에 대한 형태가 테이블을 구성한다.

|       저장 구분          |     의미    |
|--------------------------|-------------|
| 테이블/파일(table/file)  | 데이터셋 (dataset) |
| 행(row)                  | 관측점 (observation)  |
| 열(column)               | 변수 (variable)  |
| 셀(Cell)                 | 값 (value)  |

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 10px;}
</style>
<div class = "blue">

**데이터셋(Dataset)**
 
데이터셋은 정량적이면 숫자형, 정성적이면 문자열로 저장되는 **값(value)** 의 집합이다.
모든 값은 *변수(variable)* 와 *관측점(observation)* 에 속하게 된다.
변수에 모든 값은 동일한 속성을 측정하게 되고 (예를 들어, 키, 온도, 기간 등), 
관측점은 속성마다 동일한 단위로 측정되는 값이 담겨진다 (예를 들어, 사람, 종족, 날짜).

</div>

깔끔한 데이터가 의미가 있으려면 깔끔한 데이터가 입력으로 들어가서 깔끔한 데이터가 입력으로 나와야만 된다.

- (깔끔한) 데이터 입력 &rarr; 함수  f(x)  &rarr; (깔끔한) 데이터 출력 
- Tidy-input      &rarr; 함수  f(x)  &rarr; Tidy-output


## 엉망진창 데이터 (Messy Data) {#messy-data-definition} 

**깔끔하지 않는 데이터(messy data)** 는 앞서 언급한 깔끔한 데이터와 다른 형태 데이터를 지칭한다. 
컴퓨터 과학에서 말하는 코드 제3 정규형이지만, 해들리 위컴이 통계적 언어로 다시 표현한 것이다.
또한, 깔끔한 데이터는 R같은 벡터화 프로그래밍 언어에 특히 잘 맞는다. 
왜냐하면 동일한 관측점에 대한 서로 다른 변수 값이 항상 짝으로 매칭되는 것을 보장하기 때문이다.

변수와 관측점의 순서가 분석에 영향을 끼치는 것은 아니지만, 
순서를 잘 맞춰 놓으면 값을 스캔해서 검색하는 것이 용이하다.
고정된 변수(fixed variable)가 실험계획법에 기술되고 나서 측정된 값이 뒤에 나오는 것처럼 
변수와 값에 대한 순위를 잘 맞춰 정리해 놓게 되면 장점이 많다.

깔끔한 데이터를 사실 이해하기는 쉽지 않다. 하지만 엉망진창인 데이터(Messy Data)를 
보게 되면 상대적으로 깔끔한 데이터(Tidy Data)에 대한 이해를 높일 수 있다.
깔끔하지 않은 데이터의 대표적인 문제점을 다음과 같이 5가지로 유형화시켜 정리할 수 있다.

1. 칼럼 헤더에 변수명이 아닌 값이 온다.
1. 변수 다수가 한 칼럼에 저장되어 있다.
1. 변수가 행과 열에 모두 저장되어 있다.
1. 관측 단위에 대한 다양한 형태가 동일한 테이블에 저장되어 있다.
1. 한가지 관측 단위가 테이블 다수에 흩어져 저장되어 있다.

<div class = "row">
  <div class = "col-md-6">
  
**깔끔한 데이터가 아닌 보통 데이터**

| id | x    | y     |
|----|------|-------| 
| 1  |22.19 | 24.05 | 
| 2  |19.82 | 22.91 | 
| 3  |19.81 | 21.19 | 
| 4  |17.49 | 18.59 | 
| 5  |19.44 | 19.85 | 

  </div>
  <div class = "col-md-6">
  
**깔끔하게 처리한 데이터 **

| id|변수| 값|
|---|---|-------|
| 1 | x | 22.19 | 
| 2 | x | 19.82 | 
| 3 | x | 19.81 | 
| 4 | x | 17.49 | 
| 5 | x | 19.44 | 
| 1 | y | 24.05 | 
| 2 | y | 22.91 | 
| 3 | y | 21.19 | 
| 4 | y | 18.59 | 
| 5 | y | 19.85 | 

  </div>
</div>


## 깔끔한 데이터 동사 {#tidyr-verbs}

`dplyr` 패키지 데이터 문법에 `select`, `filter`, `mutate`, `group_by + summarize`, `arrange` 5개 주요 동사(Verb)를 
사용해서 데이터와 커뮤니케이션하며 데이터 조작을 하듯이 `tidyr` 팩키지 깔끔한 데이터 조작 동사를 익혀두면 엉망진창인 데이터에 
특효약이다.

깔끔한 데이터(Tidy Data)를 만드는데 동원되는 동사에 해당되는 함수는 다음과 같다. 
다음 데이터 깔끔이 함수들을 이용하여 엉망진창인 데이터(Messy Data)를 깔끔한 데이터로 만들어서 
후속 작업에 속도를 높일 수 있다. 
`tidyr` 팩키지에 포함된 Tidy Data 관련 핵심을 이루는 함수로 다음을 꼽을 수 있다.

- `pivot_longer()`
- `pivot_wider()`
- `separate()`, `separate_rows()`
- `unite()`
- `expand_grid()`

**깔끔한 데이터 관련 R 패키지**는 해드릴 위컴이 주축이 되어 10년 이상 발전시켜 
완성한 개념으로 그 단계 단계마다 개발된 팩키지가 있고 개념을 
구체화하며 실제로 구현한 함수들도 점점 진화해 나갔다.

- `reshape`, `reshape2` ([Wickham 2007](http://www.jstatsoft.org/v21/i12/))
- `plyr` ([Wickham 2011](http://www.jstatsoft.org/v40/i01/))
- `tidyr` ([Wickham 2013](https://www.jstatsoft.org/article/view/v059i10))


## 깔끔한 데이터 사례 {#tidy-data-case-study}

깔끔하지 못한 데이터로 지목(?)된 퓨리서치 원데이터는 [Pew Research Center Religion & Public Life](http://pewforum.org/Datasets/Dataset-Download.aspx) 웹사이트에서 
2015년 다운로드 받았고, [GitHub](https://github.com/hadley/tidy-data/blob/master/data/pew.sav)에 
데이터가 올라가 있다.

깔끔하지 않은 `messy` 상태 데이터를 깔끔한 `tidy` 상태 데이터로 변환시킨 사례를 
통해 실전 감각을 익혀보자.

<div class = "row">
  <div class = "col-md-6">
  
**깔끔하기 전 데이터**

| 종교  | <$10k | $10-20k | $20-30k | $30-40k | $40-50k | $50-75k |
|------------|-----|-----|-----|-----|-----|-----| 
| Agnostic   |  27 |  34 |  60  | 81  | 76  |137 |
| Atheist    |  12 |  27 |  37  | 52  | 35  | 70 |
| Buddhist   |  27 |  21 |  30  | 34  | 33  | 58 |
| Catholic   | 418 | 617 | 732  |670  |638  |1116| 
| 모름/거절     |  15 |  14 |  15  | 11  | 10  | 35 |
| Evangel    | 575 | 869 | 1064 | 982 | 881 | 1486| 
| Hindu      |   1 |   9 |   7  |  9  | 11  | 34 |
| Black Prot | 228 | 244 | 236  |238  |197  |223 |
| 여호와의 증인  |  20 |  27 |  24  | 24  | 21  | 30 |
| Jewish     |  19 |  19 |  25  | 25  | 30  | 95 |

  </div>
  <div class = "col-md-6">
  
**깔끔하게 만든 후 데이터**

| religion | income    | freq | 
|----------|----------|-------|
| Agnostic | < $10k    |  27  |
| Agnostic | $10-20k   |  34  |
| Agnostic | $20-30k   |  60  |
| Agnostic | $30-40k   |  81  |
| Agnostic | $40-50k   |  76  |
| Agnostic | $50-75k   | 137  |
| Agnostic | $75-100k  | 122  |
| Agnostic | $100-150k | 109  |
| Agnostic | >$150k    |  84  |
| Agnostic | 모름/거절    |  96  |

  </div>
</div>

사실 깔끔하게 정리된 데이터는 사람이 보기에 적합하지는 않다. 오히려 엉망진창인
퓨리서치 원데이터가 사람이 이해하고 보기에는 더 편하다. 


## 깔끔한 데이터 제작 코드 {#make-tidy-data-code}

다운로드 받은 `pew.sav` 파일을 데이터프레임으로 만든 후 범주형 자료분석을 위해서 요인형(factor) 변수로 수준을 정리한 후 `count` 함수를 빈도수를 산출한다.

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> religion </th>
   <th style="text-align:left;"> income </th>
   <th style="text-align:right;"> n </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Atheist </td>
   <td style="text-align:left;"> &lt;$10k </td>
   <td style="text-align:right;"> 39 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Atheist </td>
   <td style="text-align:left;"> $10-20k </td>
   <td style="text-align:right;"> 61 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Atheist </td>
   <td style="text-align:left;"> $20-30k </td>
   <td style="text-align:right;"> 97 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Atheist </td>
   <td style="text-align:left;"> $30-40k </td>
   <td style="text-align:right;"> 133 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Atheist </td>
   <td style="text-align:left;"> $40-50k </td>
   <td style="text-align:right;"> 111 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Atheist </td>
   <td style="text-align:left;"> $50-75k </td>
   <td style="text-align:right;"> 207 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Atheist </td>
   <td style="text-align:left;"> $75-100k </td>
   <td style="text-align:right;"> 195 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Atheist </td>
   <td style="text-align:left;"> $100-150k </td>
   <td style="text-align:right;"> 168 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Atheist </td>
   <td style="text-align:left;"> &gt;150k </td>
   <td style="text-align:right;"> 158 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Atheist </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 172 </td>
  </tr>
</tbody>
</table>

사람이 보기 편한 형태 `wide` 표형태 데이터는 다음과 같이 나타낼 수 있다. 과거 `spread()` 동사를 사용했다면 직관적인 함수인 `pivot_wider()`를 사용하는 것이 좋다. 인자도 `names_from=`, `values_from=` 을 사용해서 머릿속에 담긴 내용이 코드에 직관적으로 담기도록 코딩한다.

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> income </th>
   <th style="text-align:right;"> Atheist </th>
   <th style="text-align:right;"> Buddhist </th>
   <th style="text-align:right;"> Catholic </th>
   <th style="text-align:right;"> Muslim </th>
   <th style="text-align:right;"> Orthodox </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> $10-20k </td>
   <td style="text-align:right;"> 61 </td>
   <td style="text-align:right;"> 21 </td>
   <td style="text-align:right;"> 617 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 17 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> $20-30k </td>
   <td style="text-align:right;"> 97 </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 732 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 23 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> $30-40k </td>
   <td style="text-align:right;"> 133 </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:right;"> 670 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 32 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> $40-50k </td>
   <td style="text-align:right;"> 111 </td>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:right;"> 638 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 32 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> $50-75k </td>
   <td style="text-align:right;"> 207 </td>
   <td style="text-align:right;"> 58 </td>
   <td style="text-align:right;"> 1116 </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:right;"> 47 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> $75-100k </td>
   <td style="text-align:right;"> 195 </td>
   <td style="text-align:right;"> 62 </td>
   <td style="text-align:right;"> 949 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 38 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> $100-150k </td>
   <td style="text-align:right;"> 168 </td>
   <td style="text-align:right;"> 39 </td>
   <td style="text-align:right;"> 792 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 42 </td>
  </tr>
</tbody>
</table>



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




`cols`: `dplyr::select()` 구문과 동일하게 작성함.

</div>


<div class = "row">
  <div class = "col-md-6">
**데카르트 평면 넓은 데이터**


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


```
## # A tibble: 180 × 3
##   religion 소득     명수
##   <chr>    <chr>   <dbl>
## 1 Agnostic <$10k      27
## 2 Agnostic $10-20k    34
## 3 Agnostic $20-30k    60
## 4 Agnostic $30-40k    81
## 5 Agnostic $40-50k    76
## 6 Agnostic $50-75k   137
## # … with 174 more rows
```
  </div>
</div>

### 접두사(prefix) 제거: `pivot_longer()` {#pivot-longer-example-prefix}

`names_prefix`를 사용해서 `$`, `<$`, `>` 붙은 값을 제거시킬 수 있다.


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


```
## # A tibble: 405,440 × 8
##   country     iso2  iso3   year diagnosis gender age   count
##   <chr>       <chr> <chr> <int> <chr>     <fct>  <ord> <int>
## 1 Afghanistan AF    AFG    1980 sp        m      014      NA
## 2 Afghanistan AF    AFG    1980 sp        m      1524     NA
## 3 Afghanistan AF    AFG    1980 sp        m      2534     NA
## 4 Afghanistan AF    AFG    1980 sp        m      3544     NA
## 5 Afghanistan AF    AFG    1980 sp        m      4554     NA
## 6 Afghanistan AF    AFG    1980 sp        m      5564     NA
## # … with 405,434 more rows
```

### 한행에 다수 관측점: `pivot_longer()` {#pivot-longer-example-rows}

한행에 관측점이 다수 있는 재미있는 데이터도 있다.
즉, 첫번째 가정에 아이가 2명 있는데 첫째 아이 생일과 성별, 둘째 아이 생일과 성별이 한 행에 놓여있는 경우가 이에 해당된다.


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



- `names_from`은 칼럼값을 지정하는 칼럼
- `values-from`은 값(value)을 지정하는 칼럼

</div>


["Visualizing Fish Encounter Histories", February 3 2018](https://fishsciences.github.io/post/visualizing-fish-encounter-histories/)에 나온 물고기 포획 방류, 재포획 즉, capture-recapture 데이터셋으로 볼 수 있다.
`pivot_wider()` 함수에 `names_from`, `value_from`을 지정하여 데카르트 평면에 좌표로 찍듯이 데이터를 펼친다. 결측값이 생기는 것은 `values_fill`을 사용해서 0으로 채워넣는다.

<div class = "row">
  <div class = "col-md-4">
**깔끔한 데이터: 인덱스 긴 형식**


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
실험계획법이 적용된 데이터 `warpbreaks`를 보면 `wool`, `tension` 두가지 요인으로 총 9번 실험한 결과가 `breaks`에 담겨진 것을 확인할 수 있다.


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


```
## 
## 	Downloading file 1 of 2: `cocktails.csv`
## 	Downloading file 2 of 2: `boston_cocktails.csv`
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

[data.world Nuclear Weapon Explosions BY THOMAS DRÄBING](https://data.world/tdreabing/nuclear-weapon-explosions) 웹사이트에서 핵폭탄 실험 데이터를 다운로드 받을 수 있다. 데이터는 완벽하지만 논리상 문제가 생긴다. 즉 1945년 이후 데이터를 보자면 결측값이 보인다. 
확인이 안된 것 한건 빼면 7개국이 핵폭탄 실험을 한 것으로 나오는데 1945년에는 아무런 기록이 없다. 데이터프레임에는 정상인 깔끔한 데이터지만 사실 1945년 0 건이 예를 들어 중국이나 러시아가 포함되어 있는 것이 맞다. 


```
## # A tibble: 13 × 3
##   country  year n_bombs
##   <chr>   <dbl>   <int>
## 1 USA      1945       3
## 2 USA      1946       2
## 3 USA      1948       3
## 4 Russia   1949       1
## 5 Russia   1951       2
## 6 USA      1951      16
## # … with 7 more rows
```

### `expand_grid()` {#expand-grid-comes-in}

상기와 같은 문제를 해결하기 위해 `expand_grid()` 함수를 사용해서 핵실험을 수행한 모든 국가에 대해 해당 년도를 모두 생성할 필요가 있다. 
3개 국가에 대해 10년을 `expand_grid()` 함수로 조합하게 되면 30개 관측점이 생기는데 미국이 핵실험을 하지 않은 1947년의 경우 `NA` 값이 생긴다. 이를 채워주어야 한다.


```
## # A tibble: 30 × 3
##   country  year n_bombs
##   <chr>   <dbl>   <int>
## 1 USA      1945       3
## 2 USA      1946       2
## 3 USA      1947      NA
## 4 USA      1948       3
## 5 USA      1949      NA
## 6 USA      1950      NA
## # … with 24 more rows
```

### `replace_na()` {#expand-grid-comes-in-replace_na}

앞서 `expand_grid()` 함수로 생성된 Tidy Data를 염두에 두고 이를 `left_join()` 함수와 조인을 걸어 나중에 치환시킬 데이터프레임을 사전 제작한다.
결측값을 특정 값으로 채워 넣고자 하는 경우 `replace_na()` 함수를 사용한다.
먼저 `replace_na()` 함수를 사용해서 `n_bombs` 변수에 `NA` 결측값을 0으로 채워넣는다.


```
## # A tibble: 30 × 3
##   country  year n_bombs
##   <chr>   <dbl>   <int>
## 1 USA      1945       3
## 2 USA      1946       2
## 3 USA      1947       0
## 4 USA      1948       3
## 5 USA      1949       0
## 6 USA      1950       0
## # … with 24 more rows
```

### `fill()` {#expand-grid-comes-in-fill}

또 다른 사례로 발생되는 결측값의 유형으로 위와 같이 특정 결측값으로 채워넣는 대신 누적합과 같은 경우 이전 값이나 이후 값을 참조하여 채워넣는 것이 더 유의미한 결측값을 채워넣는 사례가 된다. 


```
## # A tibble: 13 × 4
## # Groups:   country [3]
##   country  year n_bombs cumsum_bombs
##   <chr>   <dbl>   <int>        <int>
## 1 England  1952       1            1
## 2 England  1953       2            3
## 3 Russia   1949       1            1
## 4 Russia   1951       2            3
## 5 Russia   1953       5            8
## 6 Russia   1954      10           18
## # … with 7 more rows
```

연도별 누적 핵폭탄 투하 실험 횟수는 `fill()` 함수에 `.direction = "down"`을 넣어 방향도 지정하여 의미있는 결측값 치완방식이 되도록 코드를 작업한다.


```
## # A tibble: 30 × 4
##   country  year n_bombs cumsum_bombs
##   <chr>   <dbl>   <int>        <int>
## 1 USA      1945       3            3
## 2 USA      1946       2            5
## 3 USA      1947       0            5
## 4 USA      1948       3            8
## 5 USA      1949       0            8
## 6 USA      1950       0            8
## # … with 24 more rows
```


### `drop_na()` {#expand-grid-comes-in-drop}

다른 것 다 모르겠고 데이터프레임에 결측값이 있으면 안되기 때문에 그냥 제거한다.
이럴 때 사용하는 함수가 `drop_na()`다.


```
## # A tibble: 13 × 3
##   country  year n_bombs
##   <chr>   <dbl>   <int>
## 1 USA      1945       3
## 2 USA      1946       2
## 3 USA      1948       3
## 4 USA      1951      16
## 5 USA      1952      10
## 6 USA      1953      11
## # … with 7 more rows
```



