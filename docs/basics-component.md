



# 구성요소 {#ggplot-component}

깔끔한 데이터(tidy data)가 준비되면 이를 시각화하기 위해서는 데이터를 시각화 객체에 매핑하게 되는 메커니즘이 필요하다.
주요 구성요소로 시각적 매핑(Aesthetics), 척도(Scale), 좌표계(Coordiate System), 축(Axis) 등이 필요하다.

## 시각적 매핑(Aesthetics) {#ggplot-aesthetics}

`ggplot`에서 시각적 매핑(Aesthetics)는 `aes()` 내에 시각적으로 표현되는 모든 것을 담고 있다. 
시각화 대상은 크게 두가지로 나눠지는데 연속형과 범주형이다. 온도나 몸무게 같은 경우는 특정 두 데이터 값 사이 연속된 값이 있지만, 
수학에서 정수와 같이 표현되는 쪼갤 수 없는 경우는 국가를 생각하면 한국과 미국 사이 존재하는 값이 없다.
시각화를 할 경우 위치(position), 모양(shape), 크기(size), 색상(color), 선굵기(line width), 선유형(line type)을 사용하여 좌표계(coordinate system), 척도(scale)와 결합하여 최종 시각화 결과물을 제작된다. Claus Wilke 시각화 책에 언급된 다양한 시각적 매핑을 통해 효과적으로 데이터의 정보를 표현할 수 있다.[@wilke2019fundamentals] 당연히 위치, 모양, 크기, 색상, 선굵기, 선유형 중 어떤 것을 사용하느냐에 따라 시각화로 전달되는 정보의 양은 달라진다. 예를 들어, 위치는 시각적 변별력이 선 유형보다 크다. 

\begin{figure}

{\centering \includegraphics{basics-component_files/figure-latex/ggplot-six-aesthetics-1} 

}

\caption{위치, 모양, 크기, 색상, 선굵기, 선유형 등 시각적 매핑}(\#fig:ggplot-six-aesthetics)
\end{figure}

## 척도 매핑 {#scale-mapping}

데이터가 주어지면 시각적 객체(모양, 색상, 크기 등)와 매핑을 해야 되는데 그 둘 사이를 연결하는 것이 척도(Scale)이다.
`x`축과, `y`축 척도에 시각적 객체를 매핑해야 비로서 데이터 값을 그래프로 표현된다.
척도에 위치가 지정되면 시각적 매핑 객체를 다양하게 표현할 수 있다. 만약 위치가 척도에 모호하게 표현되게 되면 
시각적 객체가 제대로 그래프에 표현되는데 문제가 된다.


\begin{figure}

{\centering \includegraphics{basics-component_files/figure-latex/ggplot-scale-1} 

}

\caption{시각적 객체 척도 매핑}(\#fig:ggplot-scale)
\end{figure}

## 좌표계 {#coordinate}

데이터 시각화에서 앞서 데이터를 척도와 시각적 객체로 준비를 했다면 이를 최종 그래프로 제작하기 위해서는 좌표계가 필요하다.
네이버 사전에서 좌표계는 공간상의 한 점의 위치를 표시하는 숫자들의 순서쌍인 좌표를 정하기 위한 체계로서, 원점과 기준 길이, 기준 축이나 기준선들의 집합을 통틀어 이르는 말이다. 가장 널리 사용되는 좌표계는 데카르트 직각좌표계와 극좌표계가 널리 사용된다.
다음은 `mtcars` 데이터 차량 기통수를 막대그래프와 원그래프를 사용하여 동일한 내용을 좌표계만 달리하여 시각적으로 표현해따.


\begin{figure}

{\centering \includegraphics{basics-component_files/figure-latex/ggplot-coordinate-1} 

}

\caption{직교좌표계와 극좌표계}(\#fig:ggplot-coordinate)
\end{figure}


## 축(axis) {#coordinate}

데이터 값을 선형 척도에 매핑하여 시각화하는 것이 일반적이지만 `x, y` 축이
대표적으로 날짜와 같은 시계열 데이터인 경우 혹은 시분초를 나타내는 경우도 있어 이에 맞춰 
적절히 축을 맞춰야 한다. 덧셈이 아니라 곱셈에 대해 선형인 경우 로그변환을 취하여 
데이터 본연의 척도를 반영하도록 축을 조정한다. 많이 사용되는 로그 변환의 경우
밑을 10으로 하는 상용로그와 자연로그가 있어 명확히 축라벨에 적시하여 혼동을 피하는 것이 좋다.
로그 변환시 **0**이 있는 경우 문제가 되기 때문에 `sqrt()` 변환도 로그변환이 갖는 표현법의 장점을
갖추면서도 로그변환 시 생기는 번거러움을 해소하기 자주 사용된다.

\begin{figure}

{\centering \includegraphics{basics-component_files/figure-latex/linear-log-scales-1} 

}

\caption{선형, 로그, 제곱근 축}(\#fig:linear-log-scales)
\end{figure}
