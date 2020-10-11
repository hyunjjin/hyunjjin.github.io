# noom 데이터 분석 수행


```python
import pandas as pd
import numpy as np
import os

from IPython.core.display import display, HTML
display(HTML("<style>.container { width:100% !important; }</style>"))
```


<style>.container { width:100% !important; }</style>


#  0. 데이터 로딩하기


```python
raw_data = pd.read_csv(os.getcwd() + '/noom/noom_user.csv', parse_dates=["Purchased At"]) # 사용자의 프로필(성별, 나이 등)과 구매 정보 등
print(raw_data.shape)
raw_data.head()
```

    (10000, 14)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Access Code</th>
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Initial Weight</th>
      <th>Lowest Weight</th>
      <th>Target Weight</th>
      <th>Product Name</th>
      <th>Status</th>
      <th>Price</th>
      <th>Purchased At</th>
      <th>Payment Type</th>
      <th>Channel</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Y9RY2VSI</td>
      <td>김승혜</td>
      <td>FEMALE</td>
      <td>25.0</td>
      <td>172.0</td>
      <td>66.9</td>
      <td>65.8</td>
      <td>55.000000</td>
      <td>눔 체중감량 프로그램</td>
      <td>completed</td>
      <td>112500</td>
      <td>2017-04-14 19:03:29.976</td>
      <td>Recurring</td>
      <td>others</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3GTN3S3B</td>
      <td>허승준</td>
      <td>MALE</td>
      <td>26.0</td>
      <td>176.0</td>
      <td>70.0</td>
      <td>NaN</td>
      <td>65.000000</td>
      <td>눔 체중감량 프로그램</td>
      <td>completed</td>
      <td>44780</td>
      <td>2017-05-23 20:53:54.368</td>
      <td>Recurring</td>
      <td>others</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6B0IG276</td>
      <td>이지민</td>
      <td>FEMALE</td>
      <td>23.0</td>
      <td>171.0</td>
      <td>98.0</td>
      <td>NaN</td>
      <td>91.140000</td>
      <td>눔 체중감량 프로그램 (천원 체험)</td>
      <td>completed</td>
      <td>132000</td>
      <td>2017-08-23 23:39:21.840</td>
      <td>Recurring</td>
      <td>facebook</td>
    </tr>
    <tr>
      <th>3</th>
      <td>EMGRU2MO</td>
      <td>장설윤</td>
      <td>FEMALE</td>
      <td>20.0</td>
      <td>160.0</td>
      <td>70.7</td>
      <td>NaN</td>
      <td>53.000000</td>
      <td>눔 체중감량 프로그램 (천원 체험)</td>
      <td>completed</td>
      <td>112500</td>
      <td>2017-08-28 20:18:22.824</td>
      <td>Recurring</td>
      <td>naver</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1ELG96TX</td>
      <td>서성빈</td>
      <td>FEMALE</td>
      <td>28.0</td>
      <td>165.0</td>
      <td>55.5</td>
      <td>NaN</td>
      <td>51.615002</td>
      <td>눔 체중감량 프로그램</td>
      <td>completed</td>
      <td>44780</td>
      <td>2017-05-07 17:50:30.944</td>
      <td>Recurring</td>
      <td>facebook</td>
    </tr>
  </tbody>
</table>
</div>




```python
coach = pd.read_csv(os.getcwd() + '/noom/noom_coach.csv') # 값은 코치가 사용자에게 코칭을 한 횟수
print(coach.shape)
coach.head()
```

    (10000, 101)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Access Code</th>
      <th>정은오 코치(VEV4PGJB)</th>
      <th>오승혁 코치(VENPKBP9)</th>
      <th>조소은 코치(D0WASBXN)</th>
      <th>고영재 코치(C91AVNGB)</th>
      <th>조수민 코치(OBCAO3W0)</th>
      <th>강채아 코치(WH2NIKCO)</th>
      <th>황다훈 코치(1I6IWURH)</th>
      <th>백슬은 코치(228BFB50)</th>
      <th>유채우 코치(IW53Y9AW)</th>
      <th>...</th>
      <th>오초빈 코치(A3WOLAQM)</th>
      <th>서수정 코치(F36LORFC)</th>
      <th>정서율 코치(LX1G7EMD)</th>
      <th>고우재 코치(SKNL9Z4P)</th>
      <th>문한규 코치(OU1WVDGA)</th>
      <th>황세안 코치(3QUBQAVE)</th>
      <th>홍성은 코치(2I3QJQ5O)</th>
      <th>고성은 코치(34T7XPYR)</th>
      <th>백한율 코치(HPWAN8R0)</th>
      <th>안슬은 코치(QAVWJSZ1)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Y9RY2VSI</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3GTN3S3B</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6B0IG276</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>EMGRU2MO</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1ELG96TX</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 101 columns</p>
</div>



----
# (1) 데이터 정리

## 1. 전체 컬럼에서 필요한 컬럼만 가져오세요.
>- Access Code(index지정), Name, Gender, Age, Height, Initial Weight, Lowest Weight, Target Weight
>  , Status, Price, Purchased At, Channel


```python
data = raw_data[['Access Code', 'Name', 'Gender', 'Age', 'Height', 'Initial Weight', 
             'Lowest Weight', 'Target Weight', 'Status', 'Price', 'Purchased At', 'Channel']].set_index('Access Code')
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Initial Weight</th>
      <th>Lowest Weight</th>
      <th>Target Weight</th>
      <th>Status</th>
      <th>Price</th>
      <th>Purchased At</th>
      <th>Channel</th>
    </tr>
    <tr>
      <th>Access Code</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Y9RY2VSI</th>
      <td>김승혜</td>
      <td>FEMALE</td>
      <td>25.0</td>
      <td>172.0</td>
      <td>66.9</td>
      <td>65.8</td>
      <td>55.000000</td>
      <td>completed</td>
      <td>112500</td>
      <td>2017-04-14 19:03:29.976</td>
      <td>others</td>
    </tr>
    <tr>
      <th>3GTN3S3B</th>
      <td>허승준</td>
      <td>MALE</td>
      <td>26.0</td>
      <td>176.0</td>
      <td>70.0</td>
      <td>NaN</td>
      <td>65.000000</td>
      <td>completed</td>
      <td>44780</td>
      <td>2017-05-23 20:53:54.368</td>
      <td>others</td>
    </tr>
    <tr>
      <th>6B0IG276</th>
      <td>이지민</td>
      <td>FEMALE</td>
      <td>23.0</td>
      <td>171.0</td>
      <td>98.0</td>
      <td>NaN</td>
      <td>91.140000</td>
      <td>completed</td>
      <td>132000</td>
      <td>2017-08-23 23:39:21.840</td>
      <td>facebook</td>
    </tr>
    <tr>
      <th>EMGRU2MO</th>
      <td>장설윤</td>
      <td>FEMALE</td>
      <td>20.0</td>
      <td>160.0</td>
      <td>70.7</td>
      <td>NaN</td>
      <td>53.000000</td>
      <td>completed</td>
      <td>112500</td>
      <td>2017-08-28 20:18:22.824</td>
      <td>naver</td>
    </tr>
    <tr>
      <th>1ELG96TX</th>
      <td>서성빈</td>
      <td>FEMALE</td>
      <td>28.0</td>
      <td>165.0</td>
      <td>55.5</td>
      <td>NaN</td>
      <td>51.615002</td>
      <td>completed</td>
      <td>44780</td>
      <td>2017-05-07 17:50:30.944</td>
      <td>facebook</td>
    </tr>
  </tbody>
</table>
</div>




## 2. 성별 컬럼을 정리해주세요.
>- 남성과 여성을 굳이 대문자로 사용할 필요는 없을 것으로 보입니다. 가독성을 높이기 위해, 대문자로 되어있는 텍스트를 소문자로 변경해주세요.
>- 또한 Gender 컬럼을 정리한 뒤, 전체 가입자중 남성의 비율과 여성의 비율을 계산해주세요.


```python
data['Gender'].value_counts(dropna=False)
```




    FEMALE    8846
    MALE      1023
    NaN        131
    Name: Gender, dtype: int64




```python
data.loc[data['Gender'] == 'FEMALE', 'Gender(clean)'] = 'female'
data.loc[data['Gender'] == 'MALE', 'Gender(clean)'] = 'male'
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Initial Weight</th>
      <th>Lowest Weight</th>
      <th>Target Weight</th>
      <th>Status</th>
      <th>Price</th>
      <th>Purchased At</th>
      <th>Channel</th>
      <th>Gender(clean)</th>
    </tr>
    <tr>
      <th>Access Code</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Y9RY2VSI</th>
      <td>김승혜</td>
      <td>FEMALE</td>
      <td>25.0</td>
      <td>172.0</td>
      <td>66.9</td>
      <td>65.8</td>
      <td>55.000000</td>
      <td>completed</td>
      <td>112500</td>
      <td>2017-04-14 19:03:29.976</td>
      <td>others</td>
      <td>female</td>
    </tr>
    <tr>
      <th>3GTN3S3B</th>
      <td>허승준</td>
      <td>MALE</td>
      <td>26.0</td>
      <td>176.0</td>
      <td>70.0</td>
      <td>NaN</td>
      <td>65.000000</td>
      <td>completed</td>
      <td>44780</td>
      <td>2017-05-23 20:53:54.368</td>
      <td>others</td>
      <td>male</td>
    </tr>
    <tr>
      <th>6B0IG276</th>
      <td>이지민</td>
      <td>FEMALE</td>
      <td>23.0</td>
      <td>171.0</td>
      <td>98.0</td>
      <td>NaN</td>
      <td>91.140000</td>
      <td>completed</td>
      <td>132000</td>
      <td>2017-08-23 23:39:21.840</td>
      <td>facebook</td>
      <td>female</td>
    </tr>
    <tr>
      <th>EMGRU2MO</th>
      <td>장설윤</td>
      <td>FEMALE</td>
      <td>20.0</td>
      <td>160.0</td>
      <td>70.7</td>
      <td>NaN</td>
      <td>53.000000</td>
      <td>completed</td>
      <td>112500</td>
      <td>2017-08-28 20:18:22.824</td>
      <td>naver</td>
      <td>female</td>
    </tr>
    <tr>
      <th>1ELG96TX</th>
      <td>서성빈</td>
      <td>FEMALE</td>
      <td>28.0</td>
      <td>165.0</td>
      <td>55.5</td>
      <td>NaN</td>
      <td>51.615002</td>
      <td>completed</td>
      <td>44780</td>
      <td>2017-05-07 17:50:30.944</td>
      <td>facebook</td>
      <td>female</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 남여 비율
print(data['Gender(clean)'].value_counts())
print(data['Gender(clean)'].value_counts(normalize=True))
```

    female    8846
    male      1023
    Name: Gender(clean), dtype: int64
    female    0.896342
    male      0.103658
    Name: Gender(clean), dtype: float64



## 3. 키(Height) 컬럼을 정리해주세요.
>- 키가 -1 cm인 사람은 NaN으로 데이터를 넣어주세요.
>- 1) 최소/최대/평균 키를 구하고, 2) 남성/여성별 평균 키를 구해주세요.


```python
data['Height'].min()
```




    -1.0




```python
data[data['Height'] < 100]['Height'].value_counts()
```




    -1.0    21
    Name: Height, dtype: int64




```python
data['Height(clean)'] = [np.nan if x == -1 else x for x in data['Height']]
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Initial Weight</th>
      <th>Lowest Weight</th>
      <th>Target Weight</th>
      <th>Status</th>
      <th>Price</th>
      <th>Purchased At</th>
      <th>Channel</th>
      <th>Gender(clean)</th>
      <th>Height(clean)</th>
    </tr>
    <tr>
      <th>Access Code</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Y9RY2VSI</th>
      <td>김승혜</td>
      <td>FEMALE</td>
      <td>25.0</td>
      <td>172.0</td>
      <td>66.9</td>
      <td>65.8</td>
      <td>55.000000</td>
      <td>completed</td>
      <td>112500</td>
      <td>2017-04-14 19:03:29.976</td>
      <td>others</td>
      <td>female</td>
      <td>172.0</td>
    </tr>
    <tr>
      <th>3GTN3S3B</th>
      <td>허승준</td>
      <td>MALE</td>
      <td>26.0</td>
      <td>176.0</td>
      <td>70.0</td>
      <td>NaN</td>
      <td>65.000000</td>
      <td>completed</td>
      <td>44780</td>
      <td>2017-05-23 20:53:54.368</td>
      <td>others</td>
      <td>male</td>
      <td>176.0</td>
    </tr>
    <tr>
      <th>6B0IG276</th>
      <td>이지민</td>
      <td>FEMALE</td>
      <td>23.0</td>
      <td>171.0</td>
      <td>98.0</td>
      <td>NaN</td>
      <td>91.140000</td>
      <td>completed</td>
      <td>132000</td>
      <td>2017-08-23 23:39:21.840</td>
      <td>facebook</td>
      <td>female</td>
      <td>171.0</td>
    </tr>
    <tr>
      <th>EMGRU2MO</th>
      <td>장설윤</td>
      <td>FEMALE</td>
      <td>20.0</td>
      <td>160.0</td>
      <td>70.7</td>
      <td>NaN</td>
      <td>53.000000</td>
      <td>completed</td>
      <td>112500</td>
      <td>2017-08-28 20:18:22.824</td>
      <td>naver</td>
      <td>female</td>
      <td>160.0</td>
    </tr>
    <tr>
      <th>1ELG96TX</th>
      <td>서성빈</td>
      <td>FEMALE</td>
      <td>28.0</td>
      <td>165.0</td>
      <td>55.5</td>
      <td>NaN</td>
      <td>51.615002</td>
      <td>completed</td>
      <td>44780</td>
      <td>2017-05-07 17:50:30.944</td>
      <td>facebook</td>
      <td>female</td>
      <td>165.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
data[data['Height'] < 100].head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Initial Weight</th>
      <th>Lowest Weight</th>
      <th>Target Weight</th>
      <th>Status</th>
      <th>Price</th>
      <th>Purchased At</th>
      <th>Channel</th>
      <th>Gender(clean)</th>
      <th>Height(clean)</th>
    </tr>
    <tr>
      <th>Access Code</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>O4OWMJG7</th>
      <td>오세윤</td>
      <td>FEMALE</td>
      <td>23.0</td>
      <td>-1.0</td>
      <td>58.0</td>
      <td>57.0</td>
      <td>47.0</td>
      <td>cancelled</td>
      <td>112500</td>
      <td>2017-07-20 20:20:03.880</td>
      <td>facebook</td>
      <td>female</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>H6EV5AXL</th>
      <td>박슬지</td>
      <td>FEMALE</td>
      <td>22.0</td>
      <td>-1.0</td>
      <td>62.9</td>
      <td>49.0</td>
      <td>53.0</td>
      <td>completed</td>
      <td>112500</td>
      <td>2017-08-26 15:58:12.208</td>
      <td>facebook</td>
      <td>female</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>O1IAZS7A</th>
      <td>고솔윤</td>
      <td>FEMALE</td>
      <td>22.0</td>
      <td>-1.0</td>
      <td>53.0</td>
      <td>52.4</td>
      <td>46.0</td>
      <td>completed</td>
      <td>112500</td>
      <td>2017-07-09 17:18:54.632</td>
      <td>facebook</td>
      <td>female</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5NEQOWHW</th>
      <td>손초영</td>
      <td>FEMALE</td>
      <td>30.0</td>
      <td>-1.0</td>
      <td>58.2</td>
      <td>58.2</td>
      <td>53.0</td>
      <td>cancelled</td>
      <td>112500</td>
      <td>2017-08-11 16:02:53.216</td>
      <td>facebook</td>
      <td>female</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>OFAXUNXD</th>
      <td>백채우</td>
      <td>FEMALE</td>
      <td>22.0</td>
      <td>-1.0</td>
      <td>71.0</td>
      <td>71.0</td>
      <td>57.0</td>
      <td>cancelled</td>
      <td>44780</td>
      <td>2017-05-15 00:52:33.920</td>
      <td>facebook</td>
      <td>female</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 1) 최소/최대/평균 키
data['Height(clean)'].min(), data['Height(clean)'].max(), data['Height(clean)'].mean()
```




    (106.0, 203.2, 163.54161860276196)




```python
# 2) 남성/여성별 평균 키
pd.pivot_table(data, index='Gender(clean)', values='Height(clean)', aggfunc='mean')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Height(clean)</th>
    </tr>
    <tr>
      <th>Gender(clean)</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>female</th>
      <td>162.116913</td>
    </tr>
    <tr>
      <th>male</th>
      <td>175.831965</td>
    </tr>
  </tbody>
</table>
</div>




## 4. 나이 컬럼을 정리해주세요.
>- 나이가 0인 데이터는 NaN으로 변경
>- 나이가 60세 이상인 데이터는 NaN으로 변경
>- 1) 최소/최대/평균 나이를 구하고, 2) 남성/여성별 평균 나이를 구해주세요.


```python
data['Age'].min(), data['Age'].max()
```




    (0.0, 173.0)




```python
data['Age(clean)'] = [np.nan if (x == 0) | (x >= 60) else x for x in data['Age']]
```


```python
# 1) 최소/최대/평균 나이
data['Age(clean)'].min(), data['Age(clean)'].max(), data['Age(clean)'].mean()
```




    (13.0, 59.0, 27.39381024860477)




```python
# 2) 남성/여성별 평균 나이
pd.pivot_table(data, index='Gender(clean)', values='Age(clean)', aggfunc='mean')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age(clean)</th>
    </tr>
    <tr>
      <th>Gender(clean)</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>female</th>
      <td>27.172929</td>
    </tr>
    <tr>
      <th>male</th>
      <td>29.309127</td>
    </tr>
  </tbody>
</table>
</div>



----
# (2) VIP 구하기 - 운영팀 요청

>- 1) 유료 사용자 중, 잘못된 정보를 기입한 사용자(invalid user)와, 2) 유료 사용자 중, 눔 코치를 사용하여 큰 성과(ex: 몸무게 감량)를 본 사용자(VIP user)를 찾아보도록 하겠습니다.

>- 1번 사용자의 경우, 유료 결제를 했으나 사용자 정보(나이, 키, 몸무게 등)가 잘못 기입되어있다면 담당 코치가 정확한 코칭을 제공해 줄 수 없는 문제가 있습니다. 그러므로 운영팀은 가능한 빠르게 정보를 잘못 기입한 고객을 찾아서 다시 기입해달라고 요청할 필요가 있습니다.
>- 2번 사용자의 경우, 눔의 VIP 사용자로서 추가 혜택(ex: 서비스 무료 이용)을 제공해주는 것을 조건으로, 눔 코치를 대표하는 홍보 모델로서 활동해줄 것을 요청할 수 있습니다.

>- 특히나 다이어트 관련 서비스는 VIP 사용자의 Before / After를 보여주는 것 만큼 좋은 홍보 수단은 없습니다. 그러므로 데이터 분석 팀에서 특정 조건(ex: 10kg 이상 감량 성공)에 만족하는 코어 사용자를 찾아내는 것을 중요합니다.


## 5. 전체 컬럼에서 필요한 컬럼만 가져오세요.
>- Name
,Age(clean)
,Height(clean)
,Initial Weight
,Lowest Weight
,Target Weight
,Status


```python
data2 = data[['Name' ,'Age(clean)' ,'Height(clean)' ,'Initial Weight' ,'Lowest Weight' ,'Target Weight' ,'Status']]
data2.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Age(clean)</th>
      <th>Height(clean)</th>
      <th>Initial Weight</th>
      <th>Lowest Weight</th>
      <th>Target Weight</th>
      <th>Status</th>
    </tr>
    <tr>
      <th>Access Code</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Y9RY2VSI</th>
      <td>김승혜</td>
      <td>25.0</td>
      <td>172.0</td>
      <td>66.9</td>
      <td>65.8</td>
      <td>55.000000</td>
      <td>completed</td>
    </tr>
    <tr>
      <th>3GTN3S3B</th>
      <td>허승준</td>
      <td>26.0</td>
      <td>176.0</td>
      <td>70.0</td>
      <td>NaN</td>
      <td>65.000000</td>
      <td>completed</td>
    </tr>
    <tr>
      <th>6B0IG276</th>
      <td>이지민</td>
      <td>23.0</td>
      <td>171.0</td>
      <td>98.0</td>
      <td>NaN</td>
      <td>91.140000</td>
      <td>completed</td>
    </tr>
    <tr>
      <th>EMGRU2MO</th>
      <td>장설윤</td>
      <td>20.0</td>
      <td>160.0</td>
      <td>70.7</td>
      <td>NaN</td>
      <td>53.000000</td>
      <td>completed</td>
    </tr>
    <tr>
      <th>1ELG96TX</th>
      <td>서성빈</td>
      <td>28.0</td>
      <td>165.0</td>
      <td>55.5</td>
      <td>NaN</td>
      <td>51.615002</td>
      <td>completed</td>
    </tr>
  </tbody>
</table>
</div>




## 6. 주어진 컬럼으로 다음의 추가 정보를 계산해주세요.
>- Weight Loss(goal) - 목표 감량치. Initial Weight 컬럼과 Target Weight의 차이를 나타냅니다. (마이너스가 나올 수 있습니다)
>- Weight Loss(current) - 최대 감량치. Initial Weight 컬럼과 Lowest Weight의 차이를 나타냅니다. (마이너스가 나올 수 있습니다)
>- 체질량지수(BMI) - 키(Height(clean))와 체중(Initial Weight)으로 체지방의 양을 추정하는 공식입니다. (BMI=체중(kg) / (키(m)×키(m)) )
>- PS) 주의: BMI공식에서 사용하는 키는 센치미터(cm)가 아닌 미터(m)라는것에 주의해주세요.


```python
data2['Weight Loss(goal)'] = data2['Initial Weight'] - data2['Target Weight']
data2['Weight Loss(current)'] = data2['Initial Weight'] - data2['Lowest Weight']
data2['BMI'] = data2['Initial Weight'] / ( (data2['Height(clean)']*0.01)*(data2['Height(clean)']*0.01) )
data2.head()
```

    /home/hyunji/anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      """Entry point for launching an IPython kernel.
    /home/hyunji/anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      
    /home/hyunji/anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      This is separate from the ipykernel package so we can avoid doing imports until





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Age(clean)</th>
      <th>Height(clean)</th>
      <th>Initial Weight</th>
      <th>Lowest Weight</th>
      <th>Target Weight</th>
      <th>Status</th>
      <th>Weight Loss(goal)</th>
      <th>Weight Loss(current)</th>
      <th>BMI</th>
    </tr>
    <tr>
      <th>Access Code</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Y9RY2VSI</th>
      <td>김승혜</td>
      <td>25.0</td>
      <td>172.0</td>
      <td>66.9</td>
      <td>65.8</td>
      <td>55.000000</td>
      <td>completed</td>
      <td>11.900000</td>
      <td>1.1</td>
      <td>22.613575</td>
    </tr>
    <tr>
      <th>3GTN3S3B</th>
      <td>허승준</td>
      <td>26.0</td>
      <td>176.0</td>
      <td>70.0</td>
      <td>NaN</td>
      <td>65.000000</td>
      <td>completed</td>
      <td>5.000000</td>
      <td>NaN</td>
      <td>22.598140</td>
    </tr>
    <tr>
      <th>6B0IG276</th>
      <td>이지민</td>
      <td>23.0</td>
      <td>171.0</td>
      <td>98.0</td>
      <td>NaN</td>
      <td>91.140000</td>
      <td>completed</td>
      <td>6.860000</td>
      <td>NaN</td>
      <td>33.514586</td>
    </tr>
    <tr>
      <th>EMGRU2MO</th>
      <td>장설윤</td>
      <td>20.0</td>
      <td>160.0</td>
      <td>70.7</td>
      <td>NaN</td>
      <td>53.000000</td>
      <td>completed</td>
      <td>17.700000</td>
      <td>NaN</td>
      <td>27.617187</td>
    </tr>
    <tr>
      <th>1ELG96TX</th>
      <td>서성빈</td>
      <td>28.0</td>
      <td>165.0</td>
      <td>55.5</td>
      <td>NaN</td>
      <td>51.615002</td>
      <td>completed</td>
      <td>3.884998</td>
      <td>NaN</td>
      <td>20.385675</td>
    </tr>
  </tbody>
</table>
</div>




## 7. 잘못된 정보를 기입한 사용자(invalid user)를 찾기
>- 필수 (다음의 조건을 만족하지 않으면 Invalid값에는 언제나 False가 들어갑니다)
>    * 눔의 프로그램을 결제한 구매자. (Status == "completed")
>- 옵션 (다음의 조건 중 하나라도 만족할 경우 Invalid값에 True가 들어가야 합니다)
>    * 나이(Age(clean)), 키(Height(clean))와 몸무게(Initial Weight, Lowest Weight, Target Weight) 중 어느 하나라도 NaN이 들어가 있는 경우.
>    * 키를 너무 작게 기입했거나(140cm 미만)나, 정 반대로 너무 크게(200cm 초과) 기입한 사용자.
>    * BMI수치가 너무 낮거나(18.5 미만) 너무 높은 사용자. (30.0 초과)
>    * 목표 감량치(Weight Loss(goal))가 마이너스인 경우. (보통 현재 체중보다 목표 체중을 낮게 설정합니다)


```python
def invalid_user(data):
    
    if (data['Status'] == 'completed') & (
        pd.isnull(data['Age(clean)']) | 
        pd.isnull(data['Height(clean)']) | 
        pd.isnull(data['Weight Loss(goal)']) | 
        pd.isnull(data['Weight Loss(current)']) |
        ( (data['Height(clean)'] < 140) | (data['Height(clean)'] > 200) ) |  # 키
        ( (data['BMI'] < 18.5) | (data['BMI'] > 30) ) |                      # BMI
        ( data['Weight Loss(goal)'] < 0 )):                                  # 목표감량
        
        return True
    
    else:
        return False
```


```python
data2['invalid'] = data2.apply(lambda x: invalid_user(x), axis=1)
data2.head()
```

    /home/hyunji/anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      """Entry point for launching an IPython kernel.





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Age(clean)</th>
      <th>Height(clean)</th>
      <th>Initial Weight</th>
      <th>Lowest Weight</th>
      <th>Target Weight</th>
      <th>Status</th>
      <th>Weight Loss(goal)</th>
      <th>Weight Loss(current)</th>
      <th>BMI</th>
      <th>invalid</th>
    </tr>
    <tr>
      <th>Access Code</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Y9RY2VSI</th>
      <td>김승혜</td>
      <td>25.0</td>
      <td>172.0</td>
      <td>66.9</td>
      <td>65.8</td>
      <td>55.000000</td>
      <td>completed</td>
      <td>11.900000</td>
      <td>1.1</td>
      <td>22.613575</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3GTN3S3B</th>
      <td>허승준</td>
      <td>26.0</td>
      <td>176.0</td>
      <td>70.0</td>
      <td>NaN</td>
      <td>65.000000</td>
      <td>completed</td>
      <td>5.000000</td>
      <td>NaN</td>
      <td>22.598140</td>
      <td>True</td>
    </tr>
    <tr>
      <th>6B0IG276</th>
      <td>이지민</td>
      <td>23.0</td>
      <td>171.0</td>
      <td>98.0</td>
      <td>NaN</td>
      <td>91.140000</td>
      <td>completed</td>
      <td>6.860000</td>
      <td>NaN</td>
      <td>33.514586</td>
      <td>True</td>
    </tr>
    <tr>
      <th>EMGRU2MO</th>
      <td>장설윤</td>
      <td>20.0</td>
      <td>160.0</td>
      <td>70.7</td>
      <td>NaN</td>
      <td>53.000000</td>
      <td>completed</td>
      <td>17.700000</td>
      <td>NaN</td>
      <td>27.617187</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1ELG96TX</th>
      <td>서성빈</td>
      <td>28.0</td>
      <td>165.0</td>
      <td>55.5</td>
      <td>NaN</td>
      <td>51.615002</td>
      <td>completed</td>
      <td>3.884998</td>
      <td>NaN</td>
      <td>20.385675</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>




## 8. VIP 사용자 체크하기
>- 눔의 프로그램을 결제한 구매자. (Status == "completed")
>- 목표 감량치(Weight Loss(goal)), 최종 감량치(Weight Loss(current)), BMI 수치 모두 NaN이 아닌 값이 들어가 있는 사용자.
>- 최종 감량치(Weight Loss(current))가 10kg 이상.
>- BMI 수치가 높은 사용자. (30.0 이상)
>- 최종 감량치(Weight Loss(current))가 목표 감량치(Weight Loss(goal))보다 큰 경우. (다이어트에 성공한 사람)


```python
def vip_user(data):
    
    if (data['Status'] == 'completed') & (
        ( pd.notnull(data['Weight Loss(goal)']) & pd.notnull(data['Weight Loss(current)']) & pd.notnull(data['BMI']) ) &
        ( data['Weight Loss(current)'] >= 10 ) &
        ( data['BMI'] >= 30 ) &
        ( data['Weight Loss(current)'] > data['Weight Loss(goal)'] )):
        
        return True
        
    else: 
        return False
```


```python
data2['vip'] = data2.apply(lambda x: vip_user(x), axis=1)
data2[data2['vip'] == True]
```

    /home/hyunji/anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      """Entry point for launching an IPython kernel.





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Age(clean)</th>
      <th>Height(clean)</th>
      <th>Initial Weight</th>
      <th>Lowest Weight</th>
      <th>Target Weight</th>
      <th>Status</th>
      <th>Weight Loss(goal)</th>
      <th>Weight Loss(current)</th>
      <th>BMI</th>
      <th>invalid</th>
      <th>vip</th>
    </tr>
    <tr>
      <th>Access Code</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3T1I8I8E</th>
      <td>임솔지</td>
      <td>23.0</td>
      <td>158.0</td>
      <td>80.0137</td>
      <td>54.5</td>
      <td>77.745742</td>
      <td>completed</td>
      <td>2.267958</td>
      <td>25.5137</td>
      <td>32.051634</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>PJYKU9OW</th>
      <td>홍윤오</td>
      <td>31.0</td>
      <td>174.0</td>
      <td>99.9000</td>
      <td>76.6</td>
      <td>84.000000</td>
      <td>completed</td>
      <td>15.900000</td>
      <td>23.3000</td>
      <td>32.996433</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>0EMTSGLJ</th>
      <td>류선정</td>
      <td>34.0</td>
      <td>167.0</td>
      <td>86.0000</td>
      <td>73.2</td>
      <td>80.050003</td>
      <td>completed</td>
      <td>5.949997</td>
      <td>12.8000</td>
      <td>30.836531</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>FBEAIFW0</th>
      <td>서서원</td>
      <td>23.0</td>
      <td>170.0</td>
      <td>95.0000</td>
      <td>75.7</td>
      <td>85.000000</td>
      <td>completed</td>
      <td>10.000000</td>
      <td>19.3000</td>
      <td>32.871972</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>8QQV2YDW</th>
      <td>홍서율</td>
      <td>23.0</td>
      <td>170.0</td>
      <td>95.0000</td>
      <td>75.7</td>
      <td>85.000000</td>
      <td>completed</td>
      <td>10.000000</td>
      <td>19.3000</td>
      <td>32.871972</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>99KOLRU8</th>
      <td>고서연</td>
      <td>26.0</td>
      <td>162.0</td>
      <td>106.0000</td>
      <td>95.2</td>
      <td>98.580000</td>
      <td>completed</td>
      <td>7.420000</td>
      <td>10.8000</td>
      <td>40.390184</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>IBOWZ9WZ</th>
      <td>손서애</td>
      <td>22.0</td>
      <td>166.0</td>
      <td>83.0000</td>
      <td>66.1</td>
      <td>73.000000</td>
      <td>completed</td>
      <td>10.000000</td>
      <td>16.9000</td>
      <td>30.120482</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>6EH2LGR5</th>
      <td>문세영</td>
      <td>34.0</td>
      <td>164.0</td>
      <td>88.8000</td>
      <td>56.3</td>
      <td>82.584003</td>
      <td>completed</td>
      <td>6.215997</td>
      <td>32.5000</td>
      <td>33.016062</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>QQLYGTWD</th>
      <td>황수윤</td>
      <td>26.0</td>
      <td>165.0</td>
      <td>105.0000</td>
      <td>87.8</td>
      <td>97.650000</td>
      <td>completed</td>
      <td>7.350000</td>
      <td>17.2000</td>
      <td>38.567493</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4Z1WB3UZ</th>
      <td>허지예</td>
      <td>26.0</td>
      <td>162.0</td>
      <td>106.0000</td>
      <td>95.2</td>
      <td>98.580000</td>
      <td>completed</td>
      <td>7.420000</td>
      <td>10.8000</td>
      <td>40.390184</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2YAKET8R</th>
      <td>서승희</td>
      <td>30.0</td>
      <td>164.0</td>
      <td>99.8000</td>
      <td>86.4</td>
      <td>92.814000</td>
      <td>completed</td>
      <td>6.986000</td>
      <td>13.4000</td>
      <td>37.105889</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>LDPPDM0M</th>
      <td>윤지안</td>
      <td>19.0</td>
      <td>168.0</td>
      <td>93.1000</td>
      <td>65.5</td>
      <td>69.000000</td>
      <td>completed</td>
      <td>24.100000</td>
      <td>27.6000</td>
      <td>32.986111</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>PKHJWII8</th>
      <td>정선영</td>
      <td>23.0</td>
      <td>170.0</td>
      <td>95.0000</td>
      <td>75.7</td>
      <td>85.000000</td>
      <td>completed</td>
      <td>10.000000</td>
      <td>19.3000</td>
      <td>32.871972</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3B3WQA4A</th>
      <td>홍슬비</td>
      <td>28.0</td>
      <td>158.0</td>
      <td>84.0000</td>
      <td>69.3</td>
      <td>75.000000</td>
      <td>completed</td>
      <td>9.000000</td>
      <td>14.7000</td>
      <td>33.648454</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>SDY4VS0P</th>
      <td>오채현</td>
      <td>NaN</td>
      <td>170.0</td>
      <td>103.1000</td>
      <td>57.1</td>
      <td>100.099998</td>
      <td>completed</td>
      <td>3.000001</td>
      <td>46.0000</td>
      <td>35.674740</td>
      <td>True</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>



----
# (2) 결제 체크 - 마케팅 요청
>- 가장 중요시 여기는 지표
>   * 한 명의 고객을 데려오는데 필요한 비용, 줄여서 고객 획득 비용(Customer Acquision Cost, 이하 CAC)
>   * 한 명의 고객을 데려왔을 때, 고객이 회사에게 제공해주는 수익(Customer Lifetime Value, 이하 LTV)
>   * 눔 코치에 헌신하는 모든 팀은 LTV를 최대한 높이고, 동시에 CAC를 최대한 낮추는 쪽으로 서비스를 개선합니다.

>- 마케팅팀이 데이터분석가에게 요청하는 내용은 다음과 같습니다.
>   * LTV가 높은 고객군의 인구통계학적 정보. 가령 눔 코치와 같은 다이어트 서비스에서는 남성보다 여성이 서비스의 만족도가 높고 많은 지출을 할 가능성이 있습니다. 이 경우, 페이스북 마케팅을 할 때 여성 고객들에게 집중적으로 광고를 보여주도록 타게팅 할 수 있습니다.
>   * 요일/시간별 결제 비율. 가령 주중보다 주말에 결제할 확률이 높다면, 서비스를 유료로 결제할 의사가 있는 고객들에게 주말에 결제를 유도하는 메일을 보낼 수 있습니다.


## 9. 결제 / 캔슬 / 환불의 총 인원 수와 비율을 구해주세요.
>-  1) 서비스를 유료로 이용중인 사람(completed), 2) 서비스를 더 이상 이용하지 않고 캔슬한 사람(cancelled) / 3) 서비스를 결제했으나 환불한 사람(refunded)의 비율을 알고 싶습니다. 


```python
print(data['Status'].value_counts())
print(data['Status'].value_counts(normalize=True))
```

    completed    5400
    cancelled    4010
    refunded      590
    Name: Status, dtype: int64
    completed    0.540
    cancelled    0.401
    refunded     0.059
    Name: Status, dtype: float64



## 10. 성별과 나이별 결제 / 캔슬 / 환불의 총 인원 수와 비율을 구해주세요.
>- age group: 17세 이하
/ 18세 이상, 24세 이하
/ 25세 이상, 35세 이하
/ 36세 이상, 44세 이하
/ 45세 이상, 54세 이하
/ 55세 이상


```python
data['Age(group)'] = pd.cut(data['Age(clean)'], 
                            bins=[0,17,24,35,44,54,99],
                            labels=['00 ~ 17', '18 ~ 24', '25 ~ 35', '36 ~ 44', '45 ~ 54', '55 ~ 99'])
```


```python
table = pd.pivot_table(data, 
                       index=['Gender(clean)', 'Age(group)'], 
                       values='Age',
                       columns=['Status'],
                       fill_value=0,
                       aggfunc='count',
                       margins=True, margins_name='total')
table['conversion'] = table['completed'] / table['total']
table
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Status</th>
      <th>cancelled</th>
      <th>completed</th>
      <th>refunded</th>
      <th>total</th>
      <th>conversion</th>
    </tr>
    <tr>
      <th>Gender(clean)</th>
      <th>Age(group)</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="6" valign="top">female</th>
      <th>00 ~ 17</th>
      <td>25</td>
      <td>35</td>
      <td>3</td>
      <td>63</td>
      <td>0.555556</td>
    </tr>
    <tr>
      <th>18 ~ 24</th>
      <td>1637</td>
      <td>1827</td>
      <td>149</td>
      <td>3613</td>
      <td>0.505674</td>
    </tr>
    <tr>
      <th>25 ~ 35</th>
      <td>1664</td>
      <td>2288</td>
      <td>271</td>
      <td>4223</td>
      <td>0.541795</td>
    </tr>
    <tr>
      <th>36 ~ 44</th>
      <td>206</td>
      <td>421</td>
      <td>46</td>
      <td>673</td>
      <td>0.625557</td>
    </tr>
    <tr>
      <th>45 ~ 54</th>
      <td>74</td>
      <td>160</td>
      <td>25</td>
      <td>259</td>
      <td>0.617761</td>
    </tr>
    <tr>
      <th>55 ~ 99</th>
      <td>0</td>
      <td>5</td>
      <td>0</td>
      <td>5</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th rowspan="6" valign="top">male</th>
      <th>00 ~ 17</th>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>4</td>
      <td>0.250000</td>
    </tr>
    <tr>
      <th>18 ~ 24</th>
      <td>80</td>
      <td>100</td>
      <td>11</td>
      <td>191</td>
      <td>0.523560</td>
    </tr>
    <tr>
      <th>25 ~ 35</th>
      <td>235</td>
      <td>404</td>
      <td>57</td>
      <td>696</td>
      <td>0.580460</td>
    </tr>
    <tr>
      <th>36 ~ 44</th>
      <td>21</td>
      <td>72</td>
      <td>9</td>
      <td>102</td>
      <td>0.705882</td>
    </tr>
    <tr>
      <th>45 ~ 54</th>
      <td>9</td>
      <td>13</td>
      <td>3</td>
      <td>25</td>
      <td>0.520000</td>
    </tr>
    <tr>
      <th>55 ~ 99</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>total</th>
      <th></th>
      <td>3954</td>
      <td>5327</td>
      <td>574</td>
      <td>9855</td>
      <td>0.540538</td>
    </tr>
  </tbody>
</table>
</div>



>- 분석 결과는 다음과 같습니다.

> * 가장 많은 양의 결제가 일어난 구간은 여성 25 ~ 35세입니다. 총 2288개로, 결제 완료의 40% 이상이 이 구간에서 발생했습니다. 심지어 전환율(conversion)도 54.1%로 평균 이상입니다.
또한 어느 정도 모수가 받쳐주는(결제 완료 100회 이상) 채널 중 이보다 전환율이 높은 채널은 1) 여성 36 ~ 54세, 2) 남성 25 ~ 35세, 3) 남성 36 ~ 44세 입니다. 이 채널들은 전환율이 60% 이상으로 매우 높습니다.
다만 이 채널들의 총 결제자(total)가 낮다는 것은 1) 아직 이 마케팅 채널이 최적화가 덜 되었거나, 2) 고객 획득 비용(CAC)이 높은 편이라 마케팅 비용을 늘리지 않았을 가능성이 있습니다. 또한 아주 희소한 경우이지만, 3) 주 마케팅 채널(ex: 페이스북)에 위 채널에 해당하는 고객의 인원수가 부족할 수도 있습니다.
이런 상황에서, 데이터분석가는 퍼포먼스 마케터와 함께 다음의 아이디어를 제시하여 회사의 매출을 증대할 수 있습니다.

> * 마케팅 예산을 여성 36 ~ 54세쪽에 집중한다. 이 채널이 전환율이 높기 때문에, CAC가 여성 25 ~ 35세와 동일하다면 여성 36 ~ 54세에 마케팅 예산을 늘리는 것은 좋은 전략입니다.
여성 36 ~ 54세 채널의 CAC가 상대적으로 높다면, 이 CAC을 낮추는 시도를 합니다. 이 전략이 성공하면 그 후에 마케팅 예산을 집중하는 것도 방법입니다.
현재 이용하고 있는 광고 채널을 다각화하여, 여성 36 ~ 54세가 활동하는 곳에 집중적으로 마케팅 예산을 투입하는 것도 시도해볼만 합니다.


## 11. 날짜와 요일 / 시간별 결제 / 캔슬 / 환불 비율을 구해주세요.
>- 마케팅팀이 이 정보를 파악할 수 있다면, 1) 전환율이 높은 시기에 마케팅 예산 투입 비중을 줄이고/늘려서 CAC를 낮추거나, 2) 특정 시간대에 눔 코치의 유로 서비스를 아직 구매하지 않은 무료 사용자에게 유료 서비스 구매를 유도하는 메일을 보내서 매출을 늘릴 것입니다.
>- 1) 0시 ~ 23시 사이의 결제/캔슬/환불 비율. 2) 월요일-일요일 사이의 결제/캔슬/환불 비율을 구해주세요.


```python
# 1) 0시 ~ 23시 사이의 결제/캔슬/환불 비율
data['Purchased At(hour)'] = data['Purchased At'].dt.hour

table2 = pd.pivot_table(data,
                        index='Purchased At(hour)',
                        values='Name',
                        columns='Status',
                        aggfunc='count', 
                        margins=True, margins_name='total')
table2['conversion'] = table2['completed'] / table2['total']
table2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Status</th>
      <th>cancelled</th>
      <th>completed</th>
      <th>refunded</th>
      <th>total</th>
      <th>conversion</th>
    </tr>
    <tr>
      <th>Purchased At(hour)</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>236</td>
      <td>344</td>
      <td>26</td>
      <td>606</td>
      <td>0.567657</td>
    </tr>
    <tr>
      <th>1</th>
      <td>156</td>
      <td>207</td>
      <td>28</td>
      <td>391</td>
      <td>0.529412</td>
    </tr>
    <tr>
      <th>2</th>
      <td>90</td>
      <td>97</td>
      <td>4</td>
      <td>191</td>
      <td>0.507853</td>
    </tr>
    <tr>
      <th>3</th>
      <td>58</td>
      <td>66</td>
      <td>5</td>
      <td>129</td>
      <td>0.511628</td>
    </tr>
    <tr>
      <th>4</th>
      <td>59</td>
      <td>45</td>
      <td>7</td>
      <td>111</td>
      <td>0.405405</td>
    </tr>
    <tr>
      <th>5</th>
      <td>36</td>
      <td>47</td>
      <td>6</td>
      <td>89</td>
      <td>0.528090</td>
    </tr>
    <tr>
      <th>6</th>
      <td>48</td>
      <td>70</td>
      <td>6</td>
      <td>124</td>
      <td>0.564516</td>
    </tr>
    <tr>
      <th>7</th>
      <td>80</td>
      <td>114</td>
      <td>20</td>
      <td>214</td>
      <td>0.532710</td>
    </tr>
    <tr>
      <th>8</th>
      <td>171</td>
      <td>264</td>
      <td>29</td>
      <td>464</td>
      <td>0.568966</td>
    </tr>
    <tr>
      <th>9</th>
      <td>162</td>
      <td>239</td>
      <td>36</td>
      <td>437</td>
      <td>0.546911</td>
    </tr>
    <tr>
      <th>10</th>
      <td>208</td>
      <td>323</td>
      <td>38</td>
      <td>569</td>
      <td>0.567663</td>
    </tr>
    <tr>
      <th>11</th>
      <td>212</td>
      <td>263</td>
      <td>27</td>
      <td>502</td>
      <td>0.523904</td>
    </tr>
    <tr>
      <th>12</th>
      <td>205</td>
      <td>235</td>
      <td>36</td>
      <td>476</td>
      <td>0.493697</td>
    </tr>
    <tr>
      <th>13</th>
      <td>205</td>
      <td>286</td>
      <td>41</td>
      <td>532</td>
      <td>0.537594</td>
    </tr>
    <tr>
      <th>14</th>
      <td>192</td>
      <td>253</td>
      <td>20</td>
      <td>465</td>
      <td>0.544086</td>
    </tr>
    <tr>
      <th>15</th>
      <td>187</td>
      <td>231</td>
      <td>14</td>
      <td>432</td>
      <td>0.534722</td>
    </tr>
    <tr>
      <th>16</th>
      <td>187</td>
      <td>235</td>
      <td>26</td>
      <td>448</td>
      <td>0.524554</td>
    </tr>
    <tr>
      <th>17</th>
      <td>180</td>
      <td>246</td>
      <td>26</td>
      <td>452</td>
      <td>0.544248</td>
    </tr>
    <tr>
      <th>18</th>
      <td>194</td>
      <td>260</td>
      <td>20</td>
      <td>474</td>
      <td>0.548523</td>
    </tr>
    <tr>
      <th>19</th>
      <td>163</td>
      <td>269</td>
      <td>36</td>
      <td>468</td>
      <td>0.574786</td>
    </tr>
    <tr>
      <th>20</th>
      <td>184</td>
      <td>236</td>
      <td>22</td>
      <td>442</td>
      <td>0.533937</td>
    </tr>
    <tr>
      <th>21</th>
      <td>231</td>
      <td>329</td>
      <td>32</td>
      <td>592</td>
      <td>0.555743</td>
    </tr>
    <tr>
      <th>22</th>
      <td>248</td>
      <td>332</td>
      <td>41</td>
      <td>621</td>
      <td>0.534622</td>
    </tr>
    <tr>
      <th>23</th>
      <td>318</td>
      <td>409</td>
      <td>44</td>
      <td>771</td>
      <td>0.530480</td>
    </tr>
    <tr>
      <th>total</th>
      <td>4010</td>
      <td>5400</td>
      <td>590</td>
      <td>10000</td>
      <td>0.540000</td>
    </tr>
  </tbody>
</table>
</div>



>- 분석 결과는 다음과 같습니다.

> - 아쉽게도, 구매 시간별 전환율(conversion)은 큰 차이가 없어 보입니다, 그 의미는 **특정 시간대에 구매한 사용자들이 서비스를 이탈할 확률이 높아지거나 낮아지는 현상은 없다고 볼 수 있습니다.**
다만 전환율과는 별개로, **주로 점심시간(10시 ~ 12시)나 새벽(23시 ~ 24시)에 구매량이 대폭 늘어난다는 것을 알 수 있습니다.** 만일 광고 예산을 집행한다면 이 시기에 집중적으로 집행하거나, 무료 사용자에게 유료 사용자로 전환을 유도하는 이메일을 보냄으로써 전환율을 높이는 것은 시도해볼만 합니다.


```python
# 2) 월요일-일요일 사이의 결제/캔슬/환불 비율
data['Purchased At(weekday)'] = data['Purchased At'].dt.day_name()

table3 = pd.pivot_table(data,
                        index='Purchased At(weekday)',
                        values='Name',
                        columns='Status',
                        aggfunc='count', 
                        margins=True, margins_name='total')
table3['conversion'] = table3['completed'] / table3['total']
table3 = table3.reindex(['Monday','Tuesday','Wednesday','Thursday','Friday','Saturday','Sunday'])
table3
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Status</th>
      <th>cancelled</th>
      <th>completed</th>
      <th>refunded</th>
      <th>total</th>
      <th>conversion</th>
    </tr>
    <tr>
      <th>Purchased At(weekday)</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Monday</th>
      <td>691</td>
      <td>863</td>
      <td>93</td>
      <td>1647</td>
      <td>0.523983</td>
    </tr>
    <tr>
      <th>Tuesday</th>
      <td>694</td>
      <td>935</td>
      <td>102</td>
      <td>1731</td>
      <td>0.540150</td>
    </tr>
    <tr>
      <th>Wednesday</th>
      <td>679</td>
      <td>953</td>
      <td>90</td>
      <td>1722</td>
      <td>0.553426</td>
    </tr>
    <tr>
      <th>Thursday</th>
      <td>616</td>
      <td>813</td>
      <td>88</td>
      <td>1517</td>
      <td>0.535926</td>
    </tr>
    <tr>
      <th>Friday</th>
      <td>490</td>
      <td>674</td>
      <td>56</td>
      <td>1220</td>
      <td>0.552459</td>
    </tr>
    <tr>
      <th>Saturday</th>
      <td>412</td>
      <td>537</td>
      <td>73</td>
      <td>1022</td>
      <td>0.525440</td>
    </tr>
    <tr>
      <th>Sunday</th>
      <td>428</td>
      <td>625</td>
      <td>88</td>
      <td>1141</td>
      <td>0.547765</td>
    </tr>
  </tbody>
</table>
</div>



>- 분석 결과는 다음과 같습니다.

> - 구매 시간과 마찬가지로, 구매 요일별 전환율(conversion)은 큰 차이가 없어 보입니다. 어느 요일이나 마찬가지로, 구매한 사람이 서비스를 이탈하거나 남을 확률은 거의 동일합니다.
하지만 사용자들은 전반적으로 **주말(금-일)이 다가올수록 구매를 덜 하게되고, 주중(월-수)이 다가올수록 구매를 많이 하게 되는 현상을 발견할 수 있습니다.** 이 시기에 광고 예산을 크게 집행하거나, 구매를 유도하는 메일이나 모바일 노티피케이션을 보내는 것은 좋은 아이디어입니다.


## 12. 채널별 결제 / 캔슬 / 환불 비율을 알고 싶다.
>- 이 채널별 마케팅 효율 정보를 알 수 있다면, 마케팅 팀에서 마케팅 예산을 재조정하여 1) 마케팅 효율이 좋은 채널에 예산을 집중하고, 2) 반대로 마케팅 효율이 좋지 않은 채널에 예산을 빼는 재조정(rebalancing)을 할 수 있습니다.


```python
data['Channel'].value_counts()
```




    facebook     6880
    others       1390
    naver        1009
    direct        297
    email         271
    google        120
    instagram      33
    Name: Channel, dtype: int64




```python
table4 = pd.pivot_table(data,
                        index='Channel',
                        values='Name',
                        columns='Status',
                        aggfunc='count',
                        margins=True, margins_name='total')
table4['conversion'] = table4['completed'] / table4['total']
table4
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Status</th>
      <th>cancelled</th>
      <th>completed</th>
      <th>refunded</th>
      <th>total</th>
      <th>conversion</th>
    </tr>
    <tr>
      <th>Channel</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>direct</th>
      <td>119</td>
      <td>169</td>
      <td>9</td>
      <td>297</td>
      <td>0.569024</td>
    </tr>
    <tr>
      <th>email</th>
      <td>93</td>
      <td>155</td>
      <td>23</td>
      <td>271</td>
      <td>0.571956</td>
    </tr>
    <tr>
      <th>facebook</th>
      <td>2812</td>
      <td>3654</td>
      <td>414</td>
      <td>6880</td>
      <td>0.531105</td>
    </tr>
    <tr>
      <th>google</th>
      <td>42</td>
      <td>66</td>
      <td>12</td>
      <td>120</td>
      <td>0.550000</td>
    </tr>
    <tr>
      <th>instagram</th>
      <td>13</td>
      <td>17</td>
      <td>3</td>
      <td>33</td>
      <td>0.515152</td>
    </tr>
    <tr>
      <th>naver</th>
      <td>386</td>
      <td>568</td>
      <td>55</td>
      <td>1009</td>
      <td>0.562934</td>
    </tr>
    <tr>
      <th>others</th>
      <td>545</td>
      <td>771</td>
      <td>74</td>
      <td>1390</td>
      <td>0.554676</td>
    </tr>
    <tr>
      <th>total</th>
      <td>4010</td>
      <td>5400</td>
      <td>590</td>
      <td>10000</td>
      <td>0.540000</td>
    </tr>
  </tbody>
</table>
</div>



>- 이 결과를 통해 알 수 있는 정보는 다음과 같습니다.

> - 현재 가장 많은 구매가 일어나는 채널은 **페이스북(facebook)** 입니다. 거의 대부분의 구매가 이 채널에서 일어났습니다.
구매량이 100회 이상인 채널 중 가장 전환율이 높은 채널은 이메일(email) 입니다. 이 채널은 사용자가 눔의 웹사이트에 방문한 뒤, 바로 구매하지 않고 이메일 주소만만 남겨놨을 경우에 해당됩니다.
아직 구매량이 페이스북만큼 많지는 않지만, 전환률이 페이스북보다 높은 채널 중 하나는 네이버(naver)입니다. 전환율이 56%로 페이스북보다 다소 높은 편입니다.
네이버만큼이나 전환율이 높은 채널은 기타(others)입니다. 이 채널은 결제율이 페이스북만큼 높음에도 불구하고, 아쉽게도 기록이 잘 되어있지 않기 때문에 분석이 어렵습니다.
이 분석 결과를 통해 얻을 수 있는 아이디어는 다음과 같습니다.

> - 먼저 내부에서 트래킹 코드나 데이터 클리닝 코드를 수정하여, **기타(others) 채널을 더 세분화시킬 필요가 있습니다.** 기타 채널은 1) 페이스북 만큼이나 구매량이 많으며, 2) 전환율이 페이스북보다 높습니다. 이 채널을 더 세분화시켜 분석한다면 마케팅 효율을 높일 수 있는 새로운 아이디어가 나올 수 있습니다.
페이스북 다음으로 네이버 검색채널을 집중적으로 튜닝하거나 예산을 배정하여 마케팅 채널을 다각화할 수 있습니다.
이메일(email)로 들어온 사용자가 전환율이 높은 이유를 더 분석할 수 있다면 좋겠습니다. 추측컨데, 눔 코치에 대한 신뢰도를 높일 다양한 정보를 이메일로 수신하였기 때문에 다른 채널에 비해 전환율이 높다는 가설을 세울 수 있습니다. 이 가설이 맞다면, 눔 코치를 이용하는 다른 사용자에게도 동일한 정보를 제공한다면 전체 전환율을 높일 수 있을 것입니다.

----
# (3) 코치 데이터와 매칭 - 코칭팀
>- 코치 데이터 분석에서 가장 중요한 것은, 좋은 코칭을 하는 사람과 그렇지 못한 사람을 구분하는 것입니다.
코칭팀에서는 좋은 코칭을 하는 코치의 노하우를 정리하여 다른 코치들에게 전파할 필요가 있고, 정 반대로 좋지 않은 코칭을 하는 코치와는 개별 면담을 통해 코칭 퀄리티를 높여야 합니다. 좋은 코칭과 좋지 않은 코칭은 결제 비율과 캔슬 비율, 그리고 환불 비율로 판단할 수 있습니다.

## 13. 기존의 데이터와 코치 데이터를 합쳐주세요.


```python
#coach = coach.set_index('Access Code')
data3 = pd.merge(data[['Name','Status']], coach, on=['Access Code'], how='left')
data3.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Access Code</th>
      <th>Name</th>
      <th>Status</th>
      <th>정은오 코치(VEV4PGJB)</th>
      <th>오승혁 코치(VENPKBP9)</th>
      <th>조소은 코치(D0WASBXN)</th>
      <th>고영재 코치(C91AVNGB)</th>
      <th>조수민 코치(OBCAO3W0)</th>
      <th>강채아 코치(WH2NIKCO)</th>
      <th>황다훈 코치(1I6IWURH)</th>
      <th>...</th>
      <th>오초빈 코치(A3WOLAQM)</th>
      <th>서수정 코치(F36LORFC)</th>
      <th>정서율 코치(LX1G7EMD)</th>
      <th>고우재 코치(SKNL9Z4P)</th>
      <th>문한규 코치(OU1WVDGA)</th>
      <th>황세안 코치(3QUBQAVE)</th>
      <th>홍성은 코치(2I3QJQ5O)</th>
      <th>고성은 코치(34T7XPYR)</th>
      <th>백한율 코치(HPWAN8R0)</th>
      <th>안슬은 코치(QAVWJSZ1)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Y9RY2VSI</td>
      <td>김승혜</td>
      <td>completed</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3GTN3S3B</td>
      <td>허승준</td>
      <td>completed</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6B0IG276</td>
      <td>이지민</td>
      <td>completed</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>EMGRU2MO</td>
      <td>장설윤</td>
      <td>completed</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1ELG96TX</td>
      <td>서성빈</td>
      <td>completed</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 103 columns</p>
</div>




## 14. 코치별 담당 사용자(total) / 구매 완료 횟수(completed) / 캔슬 횟수(canceled) / 환불 횟수(refunded)를 구해주세요.


```python
data3 = data3.reset_index().melt(id_vars=['Access Code','Name','Status'])
data3 = data3[data3['value'] != 0]

table5 = pd.pivot_table(data3,
                        index='variable',
                        values='value',
                        columns='Status',
                        aggfunc='sum',
                        fill_value=0,
                        margins=True, margins_name='total')
table5
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Status</th>
      <th>cancelled</th>
      <th>completed</th>
      <th>refunded</th>
      <th>total</th>
    </tr>
    <tr>
      <th>variable</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>index</th>
      <td>20227915</td>
      <td>26772885</td>
      <td>2994200</td>
      <td>49995000</td>
    </tr>
    <tr>
      <th>강은우 코치(EJIHL7OE)</th>
      <td>122</td>
      <td>171</td>
      <td>16</td>
      <td>309</td>
    </tr>
    <tr>
      <th>강지희 코치(NOEP7X8B)</th>
      <td>15</td>
      <td>19</td>
      <td>1</td>
      <td>35</td>
    </tr>
    <tr>
      <th>강채아 코치(WH2NIKCO)</th>
      <td>9</td>
      <td>10</td>
      <td>2</td>
      <td>21</td>
    </tr>
    <tr>
      <th>고성은 코치(34T7XPYR)</th>
      <td>24</td>
      <td>36</td>
      <td>1</td>
      <td>61</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>황세안 코치(3QUBQAVE)</th>
      <td>48</td>
      <td>61</td>
      <td>8</td>
      <td>117</td>
    </tr>
    <tr>
      <th>황소영 코치(91YZ8NY0)</th>
      <td>5</td>
      <td>13</td>
      <td>2</td>
      <td>20</td>
    </tr>
    <tr>
      <th>황시준 코치(M7EJJXFT)</th>
      <td>16</td>
      <td>16</td>
      <td>2</td>
      <td>34</td>
    </tr>
    <tr>
      <th>황재우 코치(YIFMV1GQ)</th>
      <td>7</td>
      <td>6</td>
      <td>1</td>
      <td>14</td>
    </tr>
    <tr>
      <th>total</th>
      <td>20233198</td>
      <td>26779850</td>
      <td>2995009</td>
      <td>50008057</td>
    </tr>
  </tbody>
</table>
<p>101 rows × 4 columns</p>
</div>




## 15. 코치별 전환율(conversion rate) / 취소율(cancellation rate)를 계산해주세요.
>- 전환율 = 구매 완료(completed)를 한 사람 / 전체 구매자
>- 취소율 = 취소(cancelled)나 환불(refunded)을 한 사람 / 전체 구매자

>- 1) 전환율이 높은 코치, 2) 취소율이 높은 코치 순으로 정렬해주세요. 단 모수가 부족한 경우를 배제하기 위해, 코칭을 100회 이상 하지 않은 사용자는 배제하도록 하겠습니다.


```python
# 전환율
table5['conversion_rate'] = table5['completed'] / table5['total']
# 취소율
table5['cancellation_rate'] = (table5['cancelled'] + table5['refunded']) / table5['total']
table5 = table5[table5['total'] >= 100]
```


```python
# 1) 전환율이 높은 코치
table5.sort_values(['conversion_rate'], ascending=False).head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Status</th>
      <th>cancelled</th>
      <th>completed</th>
      <th>refunded</th>
      <th>total</th>
      <th>conversion_rate</th>
      <th>cancellation_rate</th>
    </tr>
    <tr>
      <th>variable</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>조우찬 코치(WWN531JQ)</th>
      <td>36</td>
      <td>65</td>
      <td>6</td>
      <td>107</td>
      <td>0.607477</td>
      <td>0.392523</td>
    </tr>
    <tr>
      <th>허슬지 코치(DWVG5IFL)</th>
      <td>43</td>
      <td>71</td>
      <td>3</td>
      <td>117</td>
      <td>0.606838</td>
      <td>0.393162</td>
    </tr>
    <tr>
      <th>허성원 코치(9124O1IH)</th>
      <td>43</td>
      <td>76</td>
      <td>7</td>
      <td>126</td>
      <td>0.603175</td>
      <td>0.396825</td>
    </tr>
    <tr>
      <th>조설영 코치(U7L98DAO)</th>
      <td>48</td>
      <td>78</td>
      <td>6</td>
      <td>132</td>
      <td>0.590909</td>
      <td>0.409091</td>
    </tr>
    <tr>
      <th>권슬영 코치(E3GD4106)</th>
      <td>42</td>
      <td>70</td>
      <td>9</td>
      <td>121</td>
      <td>0.578512</td>
      <td>0.421488</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 2) 취소율이 높은 코치
table5.sort_values(['cancellation_rate'], ascending=False).head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Status</th>
      <th>cancelled</th>
      <th>completed</th>
      <th>refunded</th>
      <th>total</th>
      <th>conversion_rate</th>
      <th>cancellation_rate</th>
    </tr>
    <tr>
      <th>variable</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>조수민 코치(OBCAO3W0)</th>
      <td>49</td>
      <td>46</td>
      <td>8</td>
      <td>103</td>
      <td>0.446602</td>
      <td>0.553398</td>
    </tr>
    <tr>
      <th>박도영 코치(I4KVQ5G0)</th>
      <td>77</td>
      <td>70</td>
      <td>6</td>
      <td>153</td>
      <td>0.457516</td>
      <td>0.542484</td>
    </tr>
    <tr>
      <th>오동완 코치(0O48DQCH)</th>
      <td>55</td>
      <td>56</td>
      <td>7</td>
      <td>118</td>
      <td>0.474576</td>
      <td>0.525424</td>
    </tr>
    <tr>
      <th>조초연 코치(3JBE9GKO)</th>
      <td>55</td>
      <td>56</td>
      <td>5</td>
      <td>116</td>
      <td>0.482759</td>
      <td>0.517241</td>
    </tr>
    <tr>
      <th>오초빈 코치(A3WOLAQM)</th>
      <td>124</td>
      <td>150</td>
      <td>27</td>
      <td>301</td>
      <td>0.498339</td>
      <td>0.501661</td>
    </tr>
  </tbody>
</table>
</div>


