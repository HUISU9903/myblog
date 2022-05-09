---
title: "Plotly"
excerpt: "Plotly으로 만드는 그래프"
categories: 

- Python
tags: 
- [Python]
- [data]
- [visual]

date: 2022-03-25 12:10:00

---

# Plotly

* [Plotly](https://plotly.com/python/)  
  Plotly는 선이나 그림, 산점도 등의 그래프를 만들 수 있게 해주는 라이브러리이다.

```python 
import pandas as pd 
import numpy as np
df = pd.read_csv("/kaggle-survey-2021/kaggle_survey_2021_responses.csv")
df.head() # 대략적인 형태 파악하기
```

```python
from plotly.offline import init_notebook_mode  # offline으로 사용
init_notebook_mode(connected=True)  # 주피터노트북 환경에서 사용하게 만듬
import plotly.express as px  # plotly 기본 라이브러리 불러오기
import plotly.graph_objects as go # plotly 그래픽 오브젝트 블러오기
```

# 그래프 그려보기

## 기본적인 그래프
* 기본 그래프(Q1)
```python
q1_df = df['Q1'].value_counts()

fig = go.Figure()
fig.add_trace(go.Bar(x = q1_df.index, y = q1_df.values))

fig.show()
```
![Bar graph](https://user-images.githubusercontent.com/65166786/160081849-52e84264-0ea0-4f79-9a05-a3ceef75be48.PNG)

* layout 수정
```python

q1_df = df['Q1'].value_counts()

CATEGORY_ORDER = ["18-21", "22-24", "25-29", "30-34", "35-39", "40-44", "45-49", "50-54", "55-59", "60-69", "70+"]

# basic graph
fig = go.Figure()
fig.add_trace(go.Bar(x = q1_df.index, y = q1_df.values))

# styling changes
fig.update_layout(plot_bgcolor = "white", 
                 font  = dict(color = "#909999"), 
                 title = dict(text = "your TITLE text"), 
                 xaxis = dict(title = "your X-AXIS TITLE", linecolor = "#21DBAA", categoryorder = "array", categoryarray = CATEGORY_ORDER), 
                 yaxis = dict(title = "your Y-AXIS TITLE", linecolor = "#DB9021"))

fig.show()
```
![5](https://user-images.githubusercontent.com/65166786/160081854-b0591aa1-d8cd-4c79-b0da-73425dc0f6f7.PNG)

* 다른 데이터(Q3)
```python
q3_df = df['Q3'].value_counts()
fig = go.Figure()
fig.add_trace(go.Bar(x = q3_df.index, y = q3_df.values))
fig.show()
```
![6](https://user-images.githubusercontent.com/65166786/160081856-cd717fa3-820c-46bc-99f2-37ef3e5e901f.PNG)

## 그래프 그룹간 비교
* 데이터 전처리
```python
q3_q25 = df[['Q3', 'Q25']]
q3_q25['Q25'].replace(['$0-999', '1,000-1,999'], '$0-1,999', inplace = True)
q3_q25['Q25'].replace(['2,000-2,999', '3,000-3,999'], '$2,000-3,999', inplace = True)
q3_q25['Q25'].replace(['2,000-2,999', '3,000-3,999'], '$2,000-3,999', inplace = True)
q3_q25['Q25'].replace(['4,000-4,999', '5,000-7,499'], '$4,000-7,499', inplace = True)
q3_q25['Q25'].replace(['25,000-29,999', '60,000-69,999',  
                       '30,000-39,999','15,000-19,999', '70,000-79,999', 
                       '10,000-14,999', '20,000-24,999', '7,500-9,999', 
                       '100,000-124,999', '40,000-49,999', '50,000-59,999', 
                       '300,000-499,999', '200,000-249,999', '125,000-149,999', 
                       '250,000-299,999', '80,000-89,999', '90,000-99,999', 
                       '150,000-199,999', '>$1,000,000', '$500,000-999,999'], '$7,500+', inplace = True)
```
```python
q3_q25 = q3_q25.groupby(['Q3','Q25']).size().reset_index().rename(columns = {0:"Count"})

# India
india_df = q3_q25[q3_q25['Q3'] == "India"].reset_index(drop = True)
india_df['percentage'] = india_df["Count"] / india_df["Count"].sum()
india_df.head()
```
```python
# USA
usa_df = q3_q25[q3_q25['Q3'] == "United States of America"].reset_index(drop = True)
usa_df['percentage'] = usa_df["Count"] / usa_df["Count"].sum()
usa_df.head()
```
```python
india_df['%'] = np.round(india_df['percentage'] * 100, 1)
usa_df['%'] = np.round(usa_df['percentage'] * 100, 1)

india_usa_df = pd.concat([india_df, usa_df]).reset_index()
india_usa_df
```
* 그래프 출력
```python
fig = go.Figure()
for country, group in india_usa_df.groupby("Q3"):
   fig.add_trace(go.Bar(x = group['Q25'], 
                        y = group['%'], 
                        name = country, 
                        text = group['%'].astype(str) + "%", 
                        textposition='auto'))
fig.update_layout(barmode="group", 
                  plot_bgcolor = "white")
fig.show()
```
![7](https://user-images.githubusercontent.com/65166786/160081858-e2534dba-9674-45c2-ada2-15bd81a6148a.PNG)

## 여러 데이터로 그래프 만들기
* 데이터 불러오기
```python
df_2021 = pd.read_csv("../input/kaggle-survey-2021/kaggle_survey_2021_responses.csv")
df_2020 = pd.read_csv("../input/kaggle-survey-2020/kaggle_survey_2020_responses.csv")
df_2019 = pd.read_csv("../input/kaggle-survey-2019/multiple_choice_responses.csv")
```
* 데이터 전처리
```python
programming_list = ["Python", "R", "SQL", "Java", "C", "Bash", "Javascript", "C++"]
programming_df = pd.Series(programming_list)

df_2019 = df_2019[df_2019['Q19'].isin(programming_df)] # isin : 각각의 요소가 dataframe에 존재하는지 파악
df_2020 = df_2020[df_2020['Q8'].isin(programming_df)]
df_2021 = df_2021[df_2021['Q8'].isin(programming_df)]

print("2019:", df_2019['Q19'].unique().tolist()) # unique : 특정한 값을 찾는다 # tolist : Series를 list로 반환한다. 
print("2020:", df_2020['Q8'].unique().tolist())
print("2021:", df_2021['Q8'].unique().tolist())
```

```python
q3_q5_q19_2019 = df_2019.loc[:, ['Q3', 'Q5', 'Q19']]
q3_q5_q19_2019 = q3_q5_q19_2019.rename(columns = {'Q19': 'Q8'}, inplace = False) # 
q3_q5_q8_2020 = df_2020.loc[:, ['Q3', 'Q5', 'Q8']]
q3_q5_q8_2021 = df_2021.loc[:, ['Q3', 'Q5', 'Q8']]

q3_q5_q19_2019.shape, q3_q5_q8_2020.shape, q3_q5_q8_2021.shape # .shape # 행과 열의 개수를 튜플로 반환한다.
```
```python
q3_q5_q19_2019['year'] = '2019'
q3_q5_q8_2020['year'] = '2020'
q3_q5_q8_2021['year'] = '2021'

q3_q5_q19_2019.shape, q3_q5_q8_2020.shape, q3_q5_q8_2021.shape
```
```python
final_df = pd.concat([q3_q5_q19_2019, q3_q5_q8_2020, q3_q5_q8_2021])  #.concat 데이터프레임을 합칠때 사용한다.
final_df.head()
```
```python
year_q5_q8 = final_df.groupby(['year', 'Q8']).size().reset_index().rename(columns = {0:"Count"})

# 2019
q8_2019 = year_q5_q8[year_q5_q8['year'] == "2019"].reset_index(drop = True)
q8_2019['percentage'] = q8_2019["Count"] / q8_2019["Count"].sum()
q8_2019['%'] = np.round(q8_2019['percentage'] * 100, 1)

# 2020
q8_2020 = year_q5_q8[year_q5_q8['year'] == "2020"].reset_index(drop = True)
q8_2020['percentage'] = q8_2020["Count"] / q8_2020["Count"].sum()
q8_2020['%'] = np.round(q8_2020['percentage'] * 100, 1)

# 2021
q8_2021 = year_q5_q8[year_q5_q8['year'] == "2021"].reset_index(drop = True)
q8_2021['percentage'] = q8_2021["Count"] / q8_2021["Count"].sum()
q8_2021['%'] = np.round(q8_2021['percentage'] * 100, 1)
```
* 그래프 출력하기
```python
fig = go.Figure()

fig.add_trace(go.Bar(x = q8_2019['Q8'], 
                     y = q8_2019['%'], 
                     name = "2019", 
                     text = q8_2019['%'].astype(str) + "%", 
                     textposition='auto'))

fig.add_trace(go.Bar(x = q8_2020['Q8'], 
                     y = q8_2020['%'], 
                     name = "2020", 
                     text = q8_2020['%'].astype(str) + "%", 
                     textposition='auto'))

fig.add_trace(go.Bar(x = q8_2021['Q8'], 
                     y = q8_2021['%'], 
                     name = "2021", 
                     text = q8_2021['%'].astype(str) + "%", 
                     textposition='auto'))

fig.show()
```
![10](https://user-images.githubusercontent.com/65166786/160081862-6eb6fb09-132a-4d7a-8fd2-e41a049b221f.PNG)
## 색으로 표현하는 그래프
```python
import plotly.figure_factory as ff # ploty 기본함수에 포함되어있지 않은 고급화된 그래프 툴을 불러온다?

z=[[1, 90, 30, 50, 1], [20, 1, 60, 80, 30], [30, 60, 1, 50, 20]]
x=['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
y=['Morning', 'Afternoon', 'Evening']

fig = ff.create_annotated_heatmap(z, x = x, y = y, colorscale = "Viridis") # 데이터를 색상으로 표현하는 히트맵을 그린다.
fig.show()
```
![13](https://user-images.githubusercontent.com/65166786/160081868-30f07e5f-032d-4a14-bfc2-016ea1d345fb.PNG)
## 색으로 표현하는 그래프 2
```python
import plotly.graph_objects as go # 고급 그래프 툴 불러오기
from functools import reduce # 여러 데이터를 대상으로 누적 집계를 내기 위해서 사용
from itertools import product # 두개 이상의 리스트 또는 집합끼리의 곱집합을 반환한다?

z=[[1, 90, 30, 50, 1], [20, 1, 60, 80, 30], [30, 60, 1, 50, 20]]
x=['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
y=['Morning', 'Afternoon', 'Evening']

def get_text(z_value): # 매개변수가 z인 함수 생성
    text_option =[] # 빈 리스트 생성
    a, b = len(z_value), len(z_value[0]) # a의 값은 3, b의 값은 5
    flat_z = reduce(lambda x,y: x+y, z_value) # reduce라는 이름의 x+y 함수를 만든다.
    coords = product(range(a), range(b))  # 범위는 0~3 , 0~5 # (0,0) ~ (3,5) 까지 곱집합 생성
    for elem,pos  in zip(flat_z,coords):  # elem : , pos=coords : 글자의 좌표 지정
        text_option.append(
                    {'font': {'color': '#FFFFFF'}, # 글씨 색상
                    'showarrow': False, # 화살표 제거
                    'text': str(elem), # 데이터를 텍스트로 가져옴
                    'x': pos[1], # x 좌표 지정 
                    'y': pos[0]} # y 좌표 지정
                                 )
    return text_option 

fig = go.Figure(data=go.Heatmap(
                   z=z,
                   x=x,
                   y=y)) 

fig.update_layout(text_option = get_text(z))
fig.show()
```
![14](https://user-images.githubusercontent.com/65166786/160081870-8a01f9a3-5442-4ef2-95c7-bb86a2588e1e.PNG)



