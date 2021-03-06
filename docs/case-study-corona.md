

---
output: html_document
editor_options: 
  chunk_output_type: console
---



# 코로나19 {#case-corona}

장기간 코로나19로 인해 가장 약한 사슬인 자영업자를 중심으로 경제적인 어려움을 호소하고 있지만 방역당국의 가장 중요한 국정목표는 코로나19로부터 국민은 안전하게 지키는 것도 사실이다. 이런 점에서 2019년 12월 중국 후베이성 우한시에서 촉발된 코로나19 범유행이 2020년 전세계를 강타했고 대한민국을 비롯한 세계 각국을 혼돈의 도가니로 몰아넣었다.
 
정말 2020년 대한민국은 다른 국가와 비교하여 바이러스로부터 국민은 안전하게 지켰을까?

이런 질문에 답을 하기 위해서 2015년부터 2020년까지 대한민국과 비교되는 주요 국가를 추려 월별 사망자수를 뽑아 비교해보자. 이를 위해서  먼저 데이터는 [World Mortality Dataset](https://github.com/akarlinsky/world_mortality) 을 다운로드 받아 데이터를 전처리한 후에 시각화를 통해 해답을 구해보자.

전세계 코로나19 월별 사망자 데이터를 가져와서 우리나라와 비교가 되는 국가를 추려 `ggplot` 시각화를 위해 데이터 전처리 작업을 수행하고 작은 창(facet)에 2015년부터 2020년 1~12월까지 시각화한다. 코로나19로 인한 국가별 사망자 비교를 위해 색상을 달리하여 강조한다.


```r
library(tidyverse)
library(lubridate)
library(reactable)

mortality_raw <- read_csv("https://raw.githubusercontent.com/akarlinsky/world_mortality/main/world_mortality.csv") 

mortality_tbl <- mortality_raw %>% 
  filter(country_name %in% c("South Korea", "Japan", "Brazil", "United Kingdom", "France", "Germany", "United States", "Italy", "Sweden")) %>% 
  filter(! year %in% c(2021, 2022) )

## 국가별로 달리되어 있는 월별, 주별 사망자 데이터를 월별로 통합
mortality_monthly_tbl <- mortality_tbl %>% 
  filter(time_unit == "monthly") %>% 
  mutate(year_mon = lubridate::ymd(glue::glue("{year}-{time}-01"))) %>% 
  select(국가 = country_name, 연도 = year_mon, 사망자 = deaths)

mortality_weekly_tbl <- mortality_tbl %>% 
  # Week --> Date
  mutate(base_year = lubridate::ymd(glue::glue("{year}-01-01"))) %>% 
  filter(time_unit == "weekly") %>% 
  mutate(date = base_year + lubridate::weeks(time -1)) %>% 
  # Date --> Year + Month
  mutate(month = month(date)) %>% 
  group_by(country_name, year, month) %>% 
  summarise(사망자 = sum(deaths)) %>% 
  ungroup() %>% 
  # Year + Month --> 연도 
  mutate(year_mon = lubridate::ymd(glue::glue("{year}-{month}-01"))) %>% 
  select(국가 = country_name, 연도 = year_mon, 사망자 = 사망자)

deaths_tbl <- bind_rows(mortality_monthly_tbl, mortality_weekly_tbl)  %>% 
  mutate(year = year(연도),
         month = month(연도)) %>% 
  mutate(is_2020 = if_else(year == 2020, "2020년", "2015~2019년")) %>% 
  mutate(국가명 = case_when(국가 == "Brazil" ~ "브라질",
                            국가 == "France" ~ "프랑스",
                            국가 == "Germany" ~ "독일",
                           국가 == "Italy" ~ "이탈리아",
                           국가 == "Japan" ~ "일본",
                           국가 == "South Korea" ~ "대한민국",
                           국가 == "Sweden" ~ "스웨덴",
                           국가 == "United Kingdom" ~ "영국",
                           국가 == "United States" ~ "미국"))

deaths_tbl %>% 
  ggplot(aes(x = as.integer(month), y=사망자, group = year, color = is_2020)) +
    geom_line() +
    facet_wrap(~국가명, scales = "fixed") +
    scale_x_continuous(limits = c(1,12), breaks = c(2,4,6,8,10)) +
    scale_y_continuous(labels = scales::comma) +
    scale_color_manual(values = c("gray50", "red")) +
    theme_minimal(base_family = "NanumGothic") +
    labs(x="", y="사망자", title = "코로나19로 인한 주요국가 월별 사망자수 비교",
         subtitle = "2015년부터 2020년 월별 사망자수",
         color = "") +
    theme(legend.position = "top")
```

<img src="case-study-corona_files/figure-html/case-study-corona-1.png" width="576" style="display: block; margin: auto;" />


대한민국을 비롯한 주요 선진국 총 9 개국은 인구가 다르기 때문에 절대적인 사망자수만 놓고 보게되면 아무래도 인구가 많은 나라가 사망자수가 많다. 월별 절대 사망자수를 놓고 봐도 그 이전 5년과 확연히 다른 사망자 패턴 즉 유난히 2020년 사망자가 많이 늘어난 것은 코로나19로 인한 국가적인 방역실패로 단정지을 수 있다. 특히 2020년 상반기 코로나19로 인한 사망자 급증과 도시 봉쇄 등 언론을 통해 널리 알려진 국제뉴스 진원지가 영국, 이탈리아, 프랑스, 미국, 브라질이라는 것은 우연이 아니다.

국가별로 절대 인구규모가 다르기 때문에 `y`-축을 상대척도로 바꿔 국가내 사망자수가 코로나19의 영향을 받았는지 확대해서 면밀히 비교하도록 시각화한다.


```r
deaths_tbl %>% 
  ggplot(aes(x = as.integer(month), y=사망자, group = year, color = is_2020)) +
    geom_line() +
    facet_wrap(~국가명, scales = "free_y") +
    scale_x_continuous(limits = c(1,12), breaks = c(2,4,6,8,10)) +
    scale_y_continuous(labels = scales::comma) +
    scale_color_manual(values = c("gray50", "red")) +
    theme_minimal(base_family = "NanumGothic") +
    labs(x="", y="사망자", title = "코로나19로 인한 주요국가 월별 사망자수 비교",
         subtitle = "2015년부터 2020년 월별 사망자수",
         color = "") +
    theme(legend.position = "top")
```

<img src="case-study-corona_files/figure-html/case-study-corona-relative-1.png" width="576" style="display: block; margin: auto;" />


국가별 인구상황을 고려하여 코로나19 방역능력을 비교할 수 있도록 각 국가별 사망자수를 상대적으로 시각화한 그래프를 살펴봐도 유사한 결론에 도달할 수 있다. 선진국으로 불리는 영국, 스웨덴, 프랑스, 이탈리아는 2020년 봄 코로나19로 인한 사망자 증가를 자체 방역역량으로 극복을 하였으나 브라질은 여전히 예전 수준을 회복하지 못한 것이 확인된다. 
대한민국, 독일, 일본은 코로나19로 인해 특별히 사망자가 더 늘어났다는 점이 눈에 띄지는 않고 있다. 이를 통해서 사회 전반적으로 다른 여타 선진국과 비교하여 사망자 증가로 이어지지 않은 것은 K방역의 저력을 보여준 사례로 보여진다. 


