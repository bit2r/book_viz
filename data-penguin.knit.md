


# 준비된 데이터 {#ggplot-datasets}

시각화 작업을 할 때 가장 문제되는 것 중 하나가 원천데이터다. 데이터가 깔끔하게 
준비되어 있으면 시각화 업무에 집중하여 놀라운 성과물을 낼 수 있다. 반면에 데이터가
엉망진창인 경우 시각화 범위도 축소되고 시각화 각 단계를 건널 때마다 
문제를 해결해 가면서 시각화 작업을 수행하게 되어 노력대비 기대한 산출물을 얻을
가능성은 낮아진다.

데이터 사이언스와 시각화의 예제 데이터로 가장 많이 추천되는 것이 
[갭마인더(`gapminder`)](https://github.com/jennybc/gapminder)와 [팔머펭귄(`palmerpenguins`)](https://allisonhorst.github.io/palmerpenguins/) 데이터셋이다.
두 데이터셋 모두 CRAN에 등록된 공식 데이터 패키지다.

## 팔머 펭귄 {#palmer-penguins}

미국에서 "George Floyd"가 경찰에 의해 살해되면서 촉발된 ["Black Lives Matter"](https://ko.wikipedia.org/wiki/Black_Lives_Matter) 운동은 아프리카계 미국인을 향한 폭력과 제도적 인종주의에 반대하는 사회운동이다. 

데이터 과학에서도 최근 R.A. Fisher의 과거 저술한 "The genetical theory of natural selection" [@fisher1958genetical] 우생학(Eugenics) 대한 관점이 논란이 되면서 R 데이터 과학의 첫 데이터셋으로 붓꽃 `iris` 데이터를 다른 데이터, 즉 펭귄 데이터로 대체하는 움직임이 활발히 전개되고 있다. [`palmerpenguins`](https://github.com/allisonhorst/palmerpenguins) 데이터셋이 대안으로 많은 호응을 얻고 있다.

### 펭귄 공부 {#penguins-study}

팔머(Palmer) 펭귄은 3종이 있으며 자세한 내용은 다음 링크된 나무위키에서 참조 가능하다.

- [젠투 펭귄(Gentoo Penguin)](https://namu.wiki/w/젠투펭귄): 머리에 모자처럼 둘러져 있는 하얀 털 때문에 알아보기가 쉽다. 암컷이 회색이 뒤에, 흰색이 앞에 있다. 펭귄들 중에 가장 빠른 시속 36km의 수영 실력을 자랑하며, 짝짓기 할 준비가 된 펭귄은 75-90cm까지도 자란다.
- [아델리 펭귄(Adelie Penguin)](https://namu.wiki/w/아델리펭귄): 프랑스 탐험가인 뒤몽 뒤르빌(Dumont D’Urville) 부인의 이름을 따서 ‘아델리’라 불리게 되었다. 각진 머리와 작은 부리 때문에 알아보기 쉽고, 다른 펭귄들과 마찬가지로 암수가 비슷하게 생겼지만 암컷이 조금 더 작다.
- [턱끈 펭귄(Chinstrap Penguin)](https://namu.wiki/w/턱끈펭귄): 언뜻 보면 아델리 펭귄과 매우 비슷하지만, 몸집이 조금 더 작고, 목에서 머리 쪽으로 이어지는 검은 털이 눈에 띈다. 어린 고삐 펭귄들은 회갈색 빛을 띄는 털을 가지고 있으며, 목 아래 부분은 더 하얗다. 무리를 지어 살아가며 일부일처제를 지키기 때문에 짝짓기 이후에도 부부로써 오랫동안 함께 살아간다.




![팔머 펭귄 3종 세트](assets/images/penguin-species.png){width="100%"}

다음으로 `iris` 데이터와 마찬가지로 펭귄 3종을 구분하기 위한 변수로 조류의 부리에 있는 중앙 세로선의 융기를 지칭하는 능선(`culmen`) 길이(culmen length)와 깊이(culmen depth)를 이해하면 된다.




![팔머 펭귄 능선 변수](assets/images/penguin-species-variable.png)

### 데이터셋 설치 {#penguin-install-dataset}

CRAN에 등록되어 있어 `install.packages()` 함수로 직접 설치해도 되고,
`remotes` 팩키지 `install_github()` 함수로 GitHub 저장소의 팔머펭귄 데이터를 설치해도 된다.
정식 CRAN에 등록되기 이전에 GitHub에 먼저 등록되어 몇년간 정제과정을 거쳐 초기부터 사용하신 분은
GitHub 저장소 설치가 편할 수 있다. 안정화 되었기 때문에 CRAN 데이터나 GitHub 데이터나 이제 차이는 없다.



도서관에서 책을 꺼내 열람실에서 살펴보듯이 설치된 `palmerpenguins` 데이터셋을
`library(palmerpenguins)` 함수로 불러온다. 책에 어떤 내용이 담겼는지 살펴보듯이
 `tidyverse`를 구성하는 `dplyr` 패키지 `glimpse()` 함수로 펭귄 데이터를 일별한다.


```
## Rows: 344
## Columns: 8
## $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel~
## $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse~
## $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, ~
## $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, ~
## $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186~
## $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, ~
## $ sex               <fct> male, female, female, NA, female, male, female, male~
## $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007~
```

### 자료구조 일별 {#penguin-EDA-skimr}

[`skimr`](https://cran.r-project.org/web/packages/skimr/) 패키지를 사용해서 
`penguins` 데이터프레임 자료구조를 일별한다.
이를 통해서 344개 펭귄 관측값이 있으며, 7개 칼럼으로 구성된 것을 확인할 수 있다.
또한, 범주형 변수가 3개, 숫자형 변수가 4개로 구성되어 있다. 
그외 더 자세한 사항은 범주형, 숫자형 변수에 대한 요약 통계량을 참조한다.















