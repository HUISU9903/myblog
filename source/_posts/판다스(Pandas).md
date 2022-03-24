---
title: "Pandas"
excerpt: "Pandas에 대하여"
categories: 

- Python
tags: 
- [Python]
- [data]
date: 2022-03-23 18:10:00

---

# Pandas (판다스)

판다스(Pandas)는 파이썬에서의 데이터 분석을 위한 라이브러리이다.

```python
import pandas as pd # 추가 방법
```
판다스 라이브러리를 불러오는 방법이다.

## 판다스 기본 사용법

```python
temp_dic = {"col1" : [1, 2, 3], 
            "col2" : [3, 4, 5]}
df = pd.DataFrame(temp_dic) # 데이터프레임 만들기
print(df)
print(type(df))

```
```python
temp_dic = {'a':1, 'b':2, 'c':3}
ser = pd.Series(temp_dic) # 시리즈 만들기.
print(ser)
print(type(ser))
```

## 데이터의 요약

* 구글 코랩에서 구글 드라이브 불러오기
```python
from google.colab import drive
drive.mount('/content/drive')
```
* 데이터 불러오기
```python
DATA_PATH = '데이터 파일 경로'
juice = pd.read_csv(DATA_PATH)
juice
```
* 데이터 파일 요약
```python
juice.info()
```
변수들의 갯수와 타입을 보여준다.

* 데이터 통계 요약
```python
juice.describe() 
```
갯수, 평균, 최솟값, 1~4사분위를 보여준다.

* 데이터 빈도
```python
print(juice['Location'].value_counts())
print(type(juice['Location'].value_counts()))
```
특정 변수의 갯수를 센다.

## 데이터 다루기
* 더하기로 추가
```python
juice['Sold'] = juice['Lemon'] + juice['Orange']
print(juice.head(3))
```
'Lemon'과 'Orange'를 더해 'Sold'라는 새로운 열을 추가한다.
그리고 내림차순으로 3개 열만 출력한다.
* 곱하기로 추가
```python
juice['Revenue'] = juice['Price'] * juice['Sold']
print(juice.head(3))
```
'Price'과 'Sold'를 곱해 'Revenue'라는 새로운 열을 추가한다.
그리고 내림차순으로 3개 열만 출력한다.
* 행 날리기
```python
juice_row_drop = juice.drop(0, axis = 0)
print(juice_row_drop.head(3))
```
행 인덱스 "0" 을 기준으로 행을 drop한다. 내림차순으로 3개 열만 출력한다.
* 열 날리기
```python
juice_column_drop = juice.drop('Sold', axis = 1) 
print(juice_column_drop.head(3))
``` 
"Sold"를 기준으로 열 전체를 drop한다, 내림차순으로 3개 열만 출력한다. 


## iloc와 loc의 차이

### iloc
* 숫자를 통해서 데이터가 있는 순서에 접근하는 방법 (더 빠름)
```python
juice.iloc[0:3, 0:2]
```
### loc 
* 칼럼명이나 특정 조건식을 통해 사람이 이해하기 쉬운 방식으로 데이터에 접근하는 방법 (상대적으로 느림)   
```python
juice.loc[0:2, ['Date', 'Location']]
juice.loc[juice['Leaflets'] >= 100, ['Date', 'Location']]
```

## 정렬

* sort_values()
```python
juice.sort_values(by=['Revenue'], ascending=False).head(3) # ascending=False : 내림차순
```
```python
juice2 = juice.sort_values(by=['Price', 'Temperature'], ascending=[False, True]).reset_index(drop=True)
# ascending= True : 오름차순 # .reset_index(drop=True) : 기존 인덱스 번호를 버리고 재배열해서 깔끔하게
```
```
```python
condlist = [temp_arr > 6, temp_arr < 3]  # 6보다 큰값 * 4 , 3보다 작은값 + 10
choielist = [temp_arr * 4, temp_arr + 10] 
np.select(condlist, choielist, default = temp_arr)
# array([10, 11, 12,  3,  4,  5,  6, 28, 32, 36])
```

## Groupby() 

```python
juice.groupby(by = 'Location').count()
```
'Location'의 갯수를 센다.
```python
juice.groupby('Location')['Sold'].sum() 
```
'Location' 그룹에 대한 'Sold'의 갯수를 더 해서 출력한다.

```python
juice.groupby(['Location', 'Temperature', 'Date'])['Sold'].sum()
```
'Location', 'Temperature', 'Date' 그룹에 대한 'Sold'의 갯수를 더해서 출력한다.

```python
juice.groupby(['Location', 'Temperature', 'Date'], as_index=False)['Sold'].agg(['sum', 'mean']).reset_index()
```
'Location', 'Temperature', 'Date' 그룹에 대해 'sum','mean' 통계 함수를 적용한다.

## 결측치와 이상치
### 결측치
* 일반 기준
```python
import pandas as pd 
import numpy as np 

dict_01 = {
    'Score_A' : [80, 90, np.nan, 80], 
    'Score_B' : [30, 45, np.nan, np.nan], 
    'Score_C' : [np.nan, 50, 80, 90]
}

df = pd.DataFrame(dict_01)
df
```
|      | Score_A | Score_B | Score_C |
|-----:|--------:|--------:|--------:|
|    0 |    80.0 |    30.0 |     NaN |
|    1 |    90.0 |    45.0 |    50.0 |
|    2 |     NaN |     NaN |    80.0 |
|    3 |    80.0 |     NaN |    90.0 |
```python
df.isnull() # 결측치 위치 확인
```
|  |  Score_A | Score_B |  Score_C |
| --- |---------:|--------:|---------:|
| 0 |    False |   False |     True |
| 1 |    False |   False |    False |
| 2 |     True |    True |    False |
| 3 |    False |    True |    False |

```python
df.isnull().sum() # 결측치 갯수 확인
```
 "Score_A 　1                     
  Score_B 　  2   
  Score_C  　 1    
  dtype: int64 "

```python
df.fillna("0") # 결측치를 0으로 대체 
```
|  |  Score_A |  Score_B |  Score_C |
| --- |---------:|---------:|---------:|
| 0 |     80.0 |     30.0 |        0 |
| 1 |     90.0 |     45.0 |     50.0 |
| 2 |        0 |        0 |     80.0 |
| 3 |     80.0 |        0 |     90.0 |
```python
df.fillna(method="pad") # 바로 위에 위치한 값으로 대체한다. 없을 경우에는 대체 하지 않는다.
```

|  |  Score_A |  Score_B | Score_C |
| --- |---------:|---------:|--------:|
| 0 |     80.0 |     30.0 |     NaN |
| 1 |     90.0 |     45.0 |    50.0 |
| 2 |     90.0 |     45.0 |    80.0 |
| 3 |     80.0 |     45.0 |    90.0 |

* 행과 열 기준
```python
import pandas as pd 
import numpy as np 

dict_01 = {
    'Score_A' : [80, 90, np.nan, 80], 
    'Score_B' : [30, 45, np.nan, 60], 
    'Score_C' : [np.nan, 50, 80, 90], 
    'Score_D' : [50, 30, 80, 60]
}
df = pd.DataFrame(dict_01)
df
```
|  |  Score_A |  Score_B |  Score_C |  Score_D |
| --- |---------:|---------:|---------:|---------:|
| 0 |     80.0 |     30.0 |      NaN |       50 |
| 1 |     90.0 |     45.0 |     50.0 |       30 |
| 2 |      NaN |      NaN |     80.0 |       80 |
| 3 |     80.0 |     60.0 |     90.0 |       60 |
```python
df.dropna(axis = 1) # 행 기준으로 결측치가 있는 값 제거해서 출력
```
|  |  Score_A |  Score_B |  Score_C |  Score_D |
| --- |---------:|---------:|---------:|---------:|
| 1 |     90.0 |     45.0 |     50.0 |       30 |
| 3 |     80.0 |     60.0 |     90.0 |       60 |
```python
df.dropna(axis = 0) # 열 기준으로 결측치가 있는 값 제거해서 출력
```
|  | Score_D |
| --- |--------:|
| 0 |      50 |
| 1 |      30 |
| 2 |      80 |
| 3 |      60 |

### 이상치
* Q0(0), Q1(25%), Q2(50%), Q3(75%), Q4(100%)
* 이상치의 하한 경계값 : Q1 - (1.5 * (Q3-Q1))
* 이상치의 상한 경계값 : Q3 + (1.5 * (Q3-Q1))
* 
```python
juice[['Leaflets']].describe() # 1,2,3,4분위를 본다?
```
일반적인 통계적인 공식

IQR - 박스플롯 - 사분위수

Q0(0), Q1(25%), Q2(50%), Q3(75%), Q4(100%)

이상치의 하한 경계값 : Q1 - (1.5 * (Q3-Q1))

이상치의 상한 경계값 : Q3 + (1.5 * (Q3-Q1))

도메인(각 비즈니스 영역, 미래 일자리)에서 바라보는 이상치 기준(관습)

```python
Q1 = juice['Leaflets'].quantile(0.25) # 1사분위수
Q3 = juice['Leaflets'].quantile(0.75) # 3사분위수

# Q1보다 낮은 값을 이상치로 간주 
outliers_q1 = (juice['Leaflets'] < Q1) # 하한 경계값

# Q3보다 높은 값을 이상치로 간주
outliers_q3 = (juice['Leaflets'] > Q3) # 상한 경계값
```

```python
print(juice['Leaflets'][~(outliers_q1 | outliers_q3)]) # 이상치를 제거한 값
```