---

title: "R 패키지 설치하는 방법"
excerpt: "R 패키지를 설치하고 적용하는 방법을 알아보자 "
categories:

- R
tags:
- [R Study]
date: 2022-03-11 10:10:10

---

# 패키지의 설치 방법


패키지는 **라이브러리**라고도 하며, Rstudio에 내장되어 있는 내장함수만이 아닌 다른 함수 들도 
사용 할 수 있게 해주는 귀중한 역할을 한다.



```r
install.packages("패키지 이름") # 패키지를 설치
library(패키지 이름) # 패키지를 불러옴
```

패키지는 한번 설치하면 이후에는 설치하는 과정은 필요 없지만  파일을 열때 마다 불러오는 과정은 필요하다.

데이터 관리에서 흔히 쓰는 패키지의 종류에는 [ggplot2](https://ggplot2.tidyverse.org/),dplyr, foreign 등이 있다.