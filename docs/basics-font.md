


# 글꼴 {#viz-font}

R을 단순히 통계 언어로 생각하지 말고 적용범위를 확대해서 활용하면 
데이터 과학 산출물을 다양한 전자문서로 제작하여 커뮤니케이션 할 수 있다.
PDF, HTML, 워드 등 문서 뿐만 아니라, 파워포인트 같은 발표자료를
슬라이드로 제작하여 배포할 수 있다. 그래프 문법(Grammar of Graphics)에 따라 
`ggplot` 시각화를 산출물에도 다양한 글꼴(font)을 반영하여 좀더 관심을 끌 수 있는 
그래프 제작도 가능하다. 데이터 과학자나 개발자 관점에서도 통합개발환경(IDE)이
필요한데 개발과 저작에 집중할 수 있는 글꼴을 지정하여 활용할 경우
생산성도 높일 수 있고 좀더 쾌적한 환경에서 개발을 진행할 수 있다.

R 스크립트 작성을 위한 글꼴과 그래프에 한글 글꼴(font)을 적용한다.
`ggplot`을 비롯한 시각화를 위해 `extrafont`와 `showtext` 패키지를
활용하여 적절한 한글 글꼴을 사용할 뿐만 아니라 코딩 개발할 때
R 스크립트(`.R`) 및 R마크다운(`.Rmd`)에서도 적절한 한글글꼴 사용을 위해서 
코딩관련 글꼴도 설치한다.

기본적인 작업흐름은 운영체제에 먼저 외부에서 가져온 폰트를 설치한다.
그리고 나서 `extrafont` 팩키지 `font_import()` 함수를 사용해서 폰트를
R에서 불러 사용할 수 있도록 설치한다. 그리고 나서 `loadfonts()` 함수를
사용해서 글꼴을 `ggplot`등에서 불러 사용한다.
[구글 글꼴](https://fonts.google.com/)을 사용하고자 할 경우 `showtext` 패키지를 
사용해서 로컬 컴퓨터에 설치하여 적용한다.

![R 폰트/글꼴 설치](assets/images/font_overview.png){width="100%"}

## R 코딩 글꼴 {#font-coding}

문서를 위해 작성하는데 사용되는 글꼴과 R 코딩을 위해 사용되는 글꼴은
차이가 난다. 왜냐하면 R 코딩에 사용되는 글꼴은 가독성이 좋아야하고
디버깅에 용이해야 된다. 영어는 `consolas` 글꼴을 많이 사용하는데 무료가
아니다. 그래서 `consolas`에서 영감을 받은 SIL 오픈 폰트 라이선스를
따르는 [Inconsolata](https://en.wikipedia.org/wiki/Inconsolata)가 R
코딩에 많이 사용되고 있다. 하지만, R코드를 작성할 때 주석을 한글로
달거나 R마크다운 작업을 할 경우 유사한 기능을 하는 한글 글꼴이 필요하다.

-   [네이버 나눔고딕 코딩글꼴](https://github.com/naver/nanumfont/blob/master/README.md)
-   [D2 Coding 글꼴](https://github.com/naver/d2codingfont)

"네이버 나눔고딕 코딩글꼴"과 "D2 Coding 글꼴"을 설치하고 나서 RStudio
IDE에서 "Tools" &rarr; "Global Options..."를 클릭하면 "Options"창에서
`Appearance`에서 **Editor font:**에서 설치한 코딩전용 글꼴을 선택하고
**Editor theme:**도 지정한다.

![D2 코딩폰트 설치](assets/images/font_d2coding.png){width="100%"}

## `ggplot` 시각화 글꼴 {#r-viz-font}

`extrafont` 팩키지에서 `font_import()` 함수로 운영체제(윈도우/리눅스)에
설치된 글꼴을 R로 가져온다. 그리고 나서 `loadfonts()` 함수를 사용해서
설치된 글꼴을 사용하는 작업흐름을 따르게 된다.


```r
library(extrafont)
font_import(pattern = "D2")

Importing fonts may take a few minutes, depending on the number of fonts and the speed of the system.
Continue? [y/n] y
Scanning ttf files in C:\Windows\Fonts ...
Extracting .afm files from .ttf files...
C:\Windows\Fonts\D2Coding-Ver1.3.2-20180524.ttf => C:/Users/tidyverse_user/Documents/R/win-library/3.5/extrafontdb/metrics/D2Coding-Ver1.3.2-20180524
C:\Windows\Fonts\D2CodingBold-Ver1.3.2-20180524.ttf => C:/Users/tidyverse_user/Documents/R/win-library/3.5/extrafontdb/metrics/D2CodingBold-Ver1.3.2-20180524
C:\Windows\Fonts\MOD20.TTF => C:/Users/tidyverse_user/Documents/R/win-library/3.5/extrafontdb/metrics/MOD20
Found FontName for 3 fonts.
Scanning afm files in C:/Users/tidyverse_user/Documents/R/win-library/3.5/extrafontdb/metrics
Writing font table in C:/Users/tidyverse_user/Documents/R/win-library/3.5/extrafontdb/fontmap/fonttable.csv
Writing Fontmap to C:/Users/tidyverse_user/Documents/R/win-library/3.5/extrafontdb/fontmap/Fontmap...

font_import(pattern = "Nanum")
```


### `ggplot` 한글 글꼴 사례 {#font-viz-font-example}

`extrafont` 패키지 `loadfonts()` 함수를 사용해서 `ggplot`에서 적용시킬 수 있는
글꼴을 불러냈다. R 내장 데이터셋 `iris`를 사용하여 나눔글꼴 "Nanum Pen Script"을 기본 글꼴로 적용시켰다.


```r
library(tidyverse)
library(extrafont)
loadfonts() # 로컬 PC 에서 설치된 글꼴을 불러냄!!!

iris %>% 
  ggplot(aes(x=Sepal.Length, y=Petal.Length, color=Species)) +
    geom_point()+
    labs(title="붓꽃 데이터 한글 글꼴 적용", color="붓꽃 종류") +
    theme_minimal(base_family = "Nanum Pen Script") +
    theme(legend.position = "top")
```

<img src="basics-font_files/figure-html/ggplot-extrafont-in-r-1.png" width="576" style="display: block; margin: auto;" />

## `showtext` 패키지 [^showtext] {#font-showtext}

[^showtext]: [showtext: Using Fonts More Easily in R
    Graphs](https://cran.rstudio.com/web/packages/showtext/index.html)

[extrafont](https://github.com/wch/extrafont) 패키지를 통해 한자를
포함한 한글을 처리할 수 있었으나, [extrafont](https://github.com/wch/extrafont)는 트루타입폰트(`.ttf`)를
PDF 그래픽 장치에 초점을 맞춰 개발이 되었다. 따라서, 데이터과학
최종산출물이 PDF 형태 책이 아닌 경우 여러가지 면에서 다양한 한글 글꼴을
표현하는데 있어 한계가 있다. 

새로 개발된 [showtext](https://cran.rstudio.com/web/packages/showtext/index.html)
팩키지는 `Ghostscript`같은 외부 소프트웨어를 활용하지 않고도 다양한
(그래픽) 글꼴을 지원한다. [showtext](https://cran.rstudio.com/web/packages/showtext/index.html)로
R 그래프를 생성할 때, 다양한 글꼴(TrueType, OpenType, Type 1, web fonts
등)을 지원한다.

과거 PDF와 같은 책형태로 정보를 공유하고 전달하는 방식이 주류를 이뤘다면 
인터넷 등장 이후 웹으로 정보 생성과 소비가 주류로 떠오르게 되면서 글꼴에도 
변화가 생겼다. 가까운 미래에는 웹을 우선시하는 글꼴이 대세를 이룰 것으로 보인다.

![`showtext` 글꼴](assets/images/font-showtext.png){width="100%"}


사용자가 그래프에 텍스트를 넣기 위해 R 함수에서 `text()`를 호출할 때
`showtext`가 활성화 되어 있으면 `showtext` 팩키지 `text()` 함수를
호출해서 그래픽 혹은 이미지 파일에 텍스트를 표현하고 그렇지 않는 경우는
디폴트 장치함수 `text()` 함수를 호출하게 되어 있다.

내부적으로 상세 작동 로직은 글꼴 위치를 파악해서 글리프(glyph) 정보를
추출하고 비트맵 형식, 벡터그래픽 형식에 따라서 비트맵일 경우 `raster()`
장치함수를 호출하고, 벡터그래픽인 경우 `path()` 장치함수를 호출해서
기능을 수행한다.


### R 설치 글꼴 확인 {#showtext-korean-example}

`extrafont` 팩키지 `loadfonts()` 함수를 통해 `.ttf` 파일 정보를
확인한다. 현재 [구글 글끌](http://www.google.com/fonts) 페이지에서 많은 한글
글꼴을 지원하지 않고 있다. 구글에서 전세계 글꼴을 지원하다보 동아시아 3국 대상으로 
지원되는 글꼴은 적은 것으로 보인다.


```r
# 0. 환경설정 --------------------------------------------------------------------------
library(tidyverse)
library(showtext) # 글꼴, install.packages("showtext")
library(extrafont)
loadfonts()
```

### `ggplot` 글꼴 적용 {#font-showtext-korean-example-ggplot}

한글 글꼴을 바로 적용하기에 앞서 `showtext` 패키지 포함된 영문글꼴 적용 사례를 먼저 돌려보자.
`ggplot` 그래픽에 적용되는 `showtext` 활용 기본 작업흐름은 다음과 같다.

1.  글꼴을 적재한다.
2.  그래픽 장치를 연다
3.  `showtext`를 통해 텍스트를 표시한다고 지정한다.
4.  그래프를 그린다.
5.  장치를 닫는다.


```r
library(tidyverse)
library(showtext)

# ggplot 그래픽 ----------------------------

dat <- data.frame(cond = factor(rep(c("A","B"), each=200)), 
                  rating = c(rnorm(200),rnorm(200, mean=.8)))

font_add_google("Schoolbell", "bell") # 글꼴 적재

showtext.begin() # 그래픽 장치 열기

ggplot(dat, aes(x=rating)) + 
  geom_histogram(binwidth=.5)+ 
　annotate("text", 1, 2.1, family = "bell", size = 15, color="red", label = "histogram")

showtext.end() # 그래픽 장치 닫기
```

<img src="basics-font_files/figure-html/font-showtext-showtext-ggplot-1.png" width="576" style="display: block; margin: auto;" />

## 로컬 글꼴 적용 {#font-showtext-korean-example-ttf}

로컬 컴퓨터에 저장된 `.ttf` 파일을 사용자 지정해서 가져온 후 이를
`ggplot`에 반영하여 한글을 R 그래프에 적용하는 것도 가능하다.
`showtext`는 `extrafont` 보다 나중에 개발되어 `extrafont`가
로컬 컴퓨터에 설치된 글꼴을 `ggplot`에 구현되는데 전력을 다했다면
`showtext`는 이를 발판으로 나중에 개발되어 구글 폰트와 같은
인터넷 글꼴과 최근 웹출판에 대한 개념도 넣어 개발된 것이 차이점이다.


```r
# ３. 한글 그래픽 --------------------------------------------------------------------------
## 나눔펜　스크립트
font_add("NanumBarunGothic", "NanumBarunGothic.ttf")

showtext.auto()

p <- ggplot(NULL, aes(x = 1, y = 1)) + ylim(0.8, 1.2) +
  theme(axis.title = element_blank(), axis.ticks = element_blank(),
        axis.text = element_blank()) +
  annotate("text", 1, 1.1, family = "NanumBarunGothic", size = 15, color="red",
           label = "한글 사랑") +
  annotate("text", 1, 0.9, label = 'korean for "Hello, world!"',
           family = "NanumBarunGothic", size = 12)

print(p)
```

<img src="basics-font_files/figure-html/showtext-showtext-korean-1.png" width="576" style="display: block; margin: auto;" />





