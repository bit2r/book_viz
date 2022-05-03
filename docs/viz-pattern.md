




# 시각화 패턴 {#ggplot-pattern}

## 라벨 붙은 시계열  {#gghighlight-timeseries}

시계열 데이터를 제작하게 되면 추세를 파악할 수 있지만 결국 그래서 가장 최근 값이 어떻게 되는지 관심이 많다.
이런 사용자 요구를 맞추는데 시계열 데이터 마지막 시점에 라벨값을 붙이게 되면 가독성도 좋아진다.
기본적인 작업흐름은 데이터셋에서 가장 최근 관측점을 뽑아서 별도 데이터프레임으로 저장하고 
이를 `geom_text()` 혹은 `geom_text_repel()` 함수를 사용해서 해결한다.

BLOGR 님이 작성한 [Label line ends in time series with ggplot2](https://drsimonj.svbtle.com/label-line-ends-in-time-series-with-ggplot2) 코드를 참조하여 `ggplot`으로 코드를 작성한다.


\begin{center}\includegraphics{viz-pattern_files/figure-latex/best-viz-label-1} \end{center}

## 막대그래프 그룹별 색상  {#gghighlight-barplot-color}

RStudio를 거쳐 IBM에서 근무하고 있는 Alison Presmanes Hill 의 GitHub 저장소에 공개된 TV 시리즈 데이터를 사용해서 막대그래프를 작성할 때 그룹별 색상을 적용하여 가시성을 높인다. TV 시리즈별 색상을 달리할 경우 `RColorBrewer` 패키지 생상 팔레트를 범주형에 맞춰 각 시리즈별로 가장 잘 구분될 수 있도록 색상을 칠해 시각화를 한다.



\begin{center}\includegraphics{viz-pattern_files/figure-latex/best-viz-barbplot-color-1} \end{center}


## 추세선 강조 + 라벨 {#gghighlight-trend-line-label}

시각화의 백미는 아무래도 대조와 비교를 통해 강한 인상을 주는 것이다.
앞선 `ratings` TV 시리즈 시청자 평가 데이터를 대상으로 추세선에 강조를 넣고 라벨 텍스트도 넣어 
하이라이트 강조 그래프를 작성해보자.
`geom_line()`을 두개 포함시켜 강조하고하는 색상을 별도로 지정하고 선굵기도 달리한다.
라벨도 동일한 방법으로 `geom_text()`를 두개 포함시켜 강조하고자하는 색상과 글꼴크기도 달리 지정한다.


\begin{center}\includegraphics{viz-pattern_files/figure-latex/best-viz-trend-line-1} \end{center}


## 롤리팝(lolli-pop) 그래프 {#gghighlight-barplot-color}

롤리팝(Lollipop) 사탕 그래프는 막대그래프와 클리블랜드 점그래프를 합성한 것으로 한축에는 연속형, 다른 한축에는 범주형을 두고 사용자의 관심을 점그래프로 집중시키는데 효과적이다. 단순히 막대그래프를 제작하는 것과 비교하여 임팩트있는 시각화를 가능하게 한다.

제작순서는 막대그래프 &rarr; 점그래프 &rarr; 롤리팝 그래프로 뼈대 골격을 만들어 나간다.
대략 골격이 제작되고 나면 외양과 필요한 경우 값도 텍스트로 넣어 시각화 제품을 완성한다.
롤리팝 사탕 그래프를 작성할 때 `geom_point()`를 사용해서 롤리팝 사탕을 제작하고,
`geom_sgement()` 함수를 사용해서 사탕 막대를 그린다. 이때 막대 사탕의 시작과 끝을 
시작은 `x`, `y`에 넣어주고 끝은 `xend`와 `yend`에 넣어 마무리한다.

데이터는 `ggplot2`에 내장된 `midwest` 데이터를 사용하자. `midwest` 데이터셋은
2000년 미국 중서부 센서스 데이터로 인구통계 조사가 담겨있다.
`percollege` 변수는 카운티(우리나라 군에 해당) 별 대학졸업비율을 나타낸다.


\begin{center}\includegraphics{viz-pattern_files/figure-latex/best-viz-lolli-pop-1} \end{center}

한발더 나아가, 평균값에서 얼마나 차이가 있느냐를 롤리팝 그래프로 시각화하는 패턴이 많이 사용된다.
이를 위해서, 앞서와 마찬가지로 15개 카운티를 뽑아내고 평균을 구하고 평균이상, 평균이하에 대한 요인(factor)도 함께 만들어낸다.
반영한다.


\begin{center}\includegraphics{viz-pattern_files/figure-latex/best-viz-lolli-pop-polish-1} \end{center}


## 아령(dumbbell) 그래프 {#gghighlight-dumbbel-plot}

두시점을 비교하여 전후를 비교한다던가 두 지역을 비교할 때 아령 그래프는 매우 효과적이다.
TV 시리즈별로 회차를 달리하여 첫번째와 가장 마지막 시청자수를 비교하여 시각화하는데 아령(dumbbell) 그래프가 적절한 예시가 될 것으로 보인다. 이를 위해서 `ggplot()`에 들어가는 자료형을 미리 준비하고 이에 맞춰 `geom_line()`과 `geom_point()`를 결합시켜 시각화한다.


\begin{center}\includegraphics{viz-pattern_files/figure-latex/best-viz-dumbbell-plot-1} \end{center}


## 경사(Slope) 그래프{#gghighlight-slope-plot}

아령 그래프를 제작한 동일한 데이터를 [`터프티(tufte)` 스타일 경사그래프](https://www.edwardtufte.com/bboard/q-and-a-fetch-msg?msg_id=0003nk)로 구현하면 
시즌별 첫회와 최종회 시청자수 비교를 좀더 직관적으로 만들 수 있다.

`ggplot`의 기본기능을 활용하여 경사그래프를 시각화하고 강조하고자 하는 시즌을 
색상을 달리하여 표현한다. 이를 통해 1~7번째 시즌은 1회차 시청율은 낮으나 최종회는 높게 
마무리된 것을 알 수 있고, 더불어 시즌이 진행될 수록 1회차 시청율도 높아지고 있었다.
하지만 8번째 시즌은 다른 시즌과 달리 낮게 시작했고 최종회 시청률도 크게 나아지지 않은 것을 
한눈에 파악할 수 있다.


\begin{center}\includegraphics{viz-pattern_files/figure-latex/best-viz-slope-plot-base-1} \end{center}

경사그래프를 제작하고는 싶으나 전반적으로 시간이 더 필요하신 분을 위해 
[`slopegraph`](https://github.com/leeper/slopegraph) 패키지가 있다.

`slopegraph`는 Base 그래픽을 기본으로 삼고 있어 자료구조도 `rownames`를 갖는 전통적인 데이터프레임이다.
기본 Base 그래픽을 염두에 두고 상기 TV 연속물 경사그래프를 다음과 같이 작성할 수 있다.


\begin{center}\includegraphics{viz-pattern_files/figure-latex/best-viz-slope-plot-slopegraph-series-1} \end{center}

`slopegraph()` 함수 대신 `ggslopegraph()` 함수를 사용하게 되면 `ggplot()`으로도 시각화를 할 수 있다.
`slopegraph()` 함수는 자료구조가 직관적이라 처음 시각화를 하는 분에게 적절한 듯 보인다.
따라서, 앞서 `ggplot` 기반 경사그래프를 제작하고자 하는 경우 `ggslopegraph()`을 통해서도 `ggplot` 나머지 기능을 그대로 적용 가능하다.


\begin{center}\includegraphics{viz-pattern_files/figure-latex/best-viz-slope-plot-ggslopegraph-series-1} \end{center}






