
# 시각화 전용 데이터 {-}




# 깔끔한 데이터 개념 {#tidy-data}

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


## 엉망진창 데이터 {#messy-data-definition} 

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



## 깔끔한 데이터 사례 {#tidy-data-case-study}

깔끔하지 못한 데이터로 지목(?)된 퓨리서치 원데이터는 [Pew Research Center Religion & Public Life](http://pewforum.org/Datasets/Dataset-Download.aspx) 웹사이트에서 
2015년 다운로드 받았고, [GitHub](https://github.com/hadley/tidy-data/blob/master/data/pew.sav)에 
데이터가 공유되어 있다.

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



