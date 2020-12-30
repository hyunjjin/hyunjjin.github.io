# zigzag 데이터 분석


```python
import os
import pandas as pd
import sqlite3

import matplotlib.pyplot as plt
import seaborn as sns
import matplotlib as mpl
```

## 1. data 폴더의 zigzag_DB.db에 연결한 뒤 데이터베이스 스키마를 출력해주세요. 그 다음,  order 테이블을 불러와주세요.
>- timestamp; 주문시각


```python
connect = sqlite3.connect(os.getcwd() + '/zigzag_DB.db')
query = 'select * from `order`'
order = pd.read_sql(query, connect)
print(order.shape)
order.head()
```

    (867, 5)





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
      <th>timestamp</th>
      <th>user_id</th>
      <th>goods_id</th>
      <th>shop_id</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-06-11 00:00:43.032</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>1414</td>
      <td>38</td>
      <td>45000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-06-11 00:02:33.763</td>
      <td>smDmRnykg61KajpxXKzQ0oNkrh2nuSBj</td>
      <td>1351</td>
      <td>12</td>
      <td>9500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-06-11 00:04:06.364</td>
      <td>EyGjKYtSqZgqJ1ddKCtH5XwGirTyOH2P</td>
      <td>646</td>
      <td>14</td>
      <td>22000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-06-11 00:04:17.258</td>
      <td>KQBGi33Zxh5Dgu0WEkOkjN0YqTT_wxC3</td>
      <td>5901</td>
      <td>46</td>
      <td>29800</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-06-11 00:05:26.010</td>
      <td>lq1Je3voA3a0MouSFba3629lKCvweI24</td>
      <td>5572</td>
      <td>89</td>
      <td>29000</td>
    </tr>
  </tbody>
</table>
</div>



## 2. order 테이블을 이용해 지그재그의 당일 매출 상위 10개 쇼핑몰을 구해주세요.


```python
pd.pivot_table(order, index='shop_id', values='price', aggfunc='sum').sort_values(['price'], ascending=False)[:10]
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
      <th>price</th>
    </tr>
    <tr>
      <th>shop_id</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>22</th>
      <td>1365200</td>
    </tr>
    <tr>
      <th>14</th>
      <td>872000</td>
    </tr>
    <tr>
      <th>63</th>
      <td>710700</td>
    </tr>
    <tr>
      <th>32</th>
      <td>707900</td>
    </tr>
    <tr>
      <th>126</th>
      <td>669400</td>
    </tr>
    <tr>
      <th>6</th>
      <td>655900</td>
    </tr>
    <tr>
      <th>11</th>
      <td>653000</td>
    </tr>
    <tr>
      <th>60</th>
      <td>558300</td>
    </tr>
    <tr>
      <th>19</th>
      <td>518400</td>
    </tr>
    <tr>
      <th>12</th>
      <td>446900</td>
    </tr>
  </tbody>
</table>
</div>



## 3. 판매 건수를 포함하여 피벗테이블을 만들어주세요. 또한, 상위 10개 쇼핑몰의 매출을 막대그래프로 보여주세요.


```python
top = pd.pivot_table(order, index='shop_id', values='price', aggfunc=['sum', 'count'])
top.columns = ['sum','count']
top = top.sort_values(['sum', 'count'], ascending=False)[:10]
top
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
      <th>sum</th>
      <th>count</th>
    </tr>
    <tr>
      <th>shop_id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>22</th>
      <td>1365200</td>
      <td>99</td>
    </tr>
    <tr>
      <th>14</th>
      <td>872000</td>
      <td>30</td>
    </tr>
    <tr>
      <th>63</th>
      <td>710700</td>
      <td>27</td>
    </tr>
    <tr>
      <th>32</th>
      <td>707900</td>
      <td>37</td>
    </tr>
    <tr>
      <th>126</th>
      <td>669400</td>
      <td>39</td>
    </tr>
    <tr>
      <th>6</th>
      <td>655900</td>
      <td>24</td>
    </tr>
    <tr>
      <th>11</th>
      <td>653000</td>
      <td>19</td>
    </tr>
    <tr>
      <th>60</th>
      <td>558300</td>
      <td>23</td>
    </tr>
    <tr>
      <th>19</th>
      <td>518400</td>
      <td>19</td>
    </tr>
    <tr>
      <th>12</th>
      <td>446900</td>
      <td>42</td>
    </tr>
  </tbody>
</table>
</div>




```python
sns.barplot(data=top, x=top.index, y='sum', order=top.index)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f55da57af90>




    
![png](Level4_zigzag_files/Level4_zigzag_8_1.png)
    


## 4. 시간대별 지그재그 매출을 구하려고 합니다. lineplot을 이용하여 6월 11일의 시간대별 매출을 시각화 해주세요.


```python
order['timestamp'] = pd.to_datetime(order['timestamp'])

# style set
mpl.rc('font', family='NanumGothic')
mpl.rc('axes', unicode_minus=False)

plt.figure(figsize=(18,4))
sns.lineplot(data=order, x='timestamp', y='price')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f55d9f6b710>




    
![png](Level4_zigzag_files/Level4_zigzag_10_1.png)
    


## 5. 위의 시각화를 구간화(binning) 작업을 거쳐 보기 좋은 형태로 만들어주세요.


```python
# 시간대별 매출액

order['hour'] = order['timestamp'].dt.hour
time_price = pd.pivot_table(order, index='hour', values='price', aggfunc='sum')
time_price
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
      <th>price</th>
    </tr>
    <tr>
      <th>hour</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1479210</td>
    </tr>
    <tr>
      <th>1</th>
      <td>990300</td>
    </tr>
    <tr>
      <th>2</th>
      <td>787830</td>
    </tr>
    <tr>
      <th>3</th>
      <td>467650</td>
    </tr>
    <tr>
      <th>4</th>
      <td>304800</td>
    </tr>
    <tr>
      <th>5</th>
      <td>47100</td>
    </tr>
    <tr>
      <th>6</th>
      <td>192400</td>
    </tr>
    <tr>
      <th>7</th>
      <td>430300</td>
    </tr>
    <tr>
      <th>8</th>
      <td>437060</td>
    </tr>
    <tr>
      <th>9</th>
      <td>581800</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1009710</td>
    </tr>
    <tr>
      <th>11</th>
      <td>906000</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1178050</td>
    </tr>
    <tr>
      <th>13</th>
      <td>1041400</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1045800</td>
    </tr>
    <tr>
      <th>15</th>
      <td>662760</td>
    </tr>
    <tr>
      <th>16</th>
      <td>925100</td>
    </tr>
    <tr>
      <th>17</th>
      <td>681400</td>
    </tr>
    <tr>
      <th>18</th>
      <td>818320</td>
    </tr>
    <tr>
      <th>19</th>
      <td>918700</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1081800</td>
    </tr>
    <tr>
      <th>21</th>
      <td>1399710</td>
    </tr>
    <tr>
      <th>22</th>
      <td>1444170</td>
    </tr>
    <tr>
      <th>23</th>
      <td>927950</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize=(18,4))
sns.lineplot(data=time_price, x=time_price.index, y='price')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f55d961c9d0>




    
![png](Level4_zigzag_files/Level4_zigzag_13_1.png)
    



```python
plt.figure(figsize=(18,4))
sns.pointplot(data=time_price, x=time_price.index, y='price')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f55d85a4410>




    
![png](Level4_zigzag_files/Level4_zigzag_14_1.png)
    


## 6 . user 테이블을 불러와 order 테이블과 병합해주세요.


```python
# user data

query = 'select * from user'
user = pd.read_sql(query, connect)
print(user.shape)
user.head()
```

    (10000, 3)





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
      <th>user_id</th>
      <th>os</th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>--PYPMX8QWg0ioT5zfORmU-S5Lln0lot</td>
      <td>And</td>
      <td>41</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-16-xXbeDcvkZJtTpRwMi57Yo2ZQpORv</td>
      <td>iOS</td>
      <td>31</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-1de9sT-MLwVVvnC0ncCLnqEqpSi3XSN</td>
      <td>iOS</td>
      <td>16</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-3A3L2jnM55B_Q1bRXMjZ6sPnINIj-Y1</td>
      <td>And</td>
      <td>41</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-3bhcSgPOIdQAPkPNcchxvECGqGQQ78k</td>
      <td>And</td>
      <td>42</td>
    </tr>
  </tbody>
</table>
</div>




```python
# order, user data merge

order2 = pd.merge(order, user, on='user_id', how='inner')
order2.head()
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
      <th>timestamp</th>
      <th>user_id</th>
      <th>goods_id</th>
      <th>shop_id</th>
      <th>price</th>
      <th>hour</th>
      <th>os</th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-06-11 00:00:43.032</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>1414</td>
      <td>38</td>
      <td>45000</td>
      <td>0</td>
      <td>iOS</td>
      <td>39</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-06-11 00:02:33.763</td>
      <td>smDmRnykg61KajpxXKzQ0oNkrh2nuSBj</td>
      <td>1351</td>
      <td>12</td>
      <td>9500</td>
      <td>0</td>
      <td>And</td>
      <td>17</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-06-11 00:04:06.364</td>
      <td>EyGjKYtSqZgqJ1ddKCtH5XwGirTyOH2P</td>
      <td>646</td>
      <td>14</td>
      <td>22000</td>
      <td>0</td>
      <td>And</td>
      <td>-1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-06-11 00:04:17.258</td>
      <td>KQBGi33Zxh5Dgu0WEkOkjN0YqTT_wxC3</td>
      <td>5901</td>
      <td>46</td>
      <td>29800</td>
      <td>0</td>
      <td>And</td>
      <td>34</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-06-11 00:05:26.010</td>
      <td>lq1Je3voA3a0MouSFba3629lKCvweI24</td>
      <td>5572</td>
      <td>89</td>
      <td>29000</td>
      <td>0</td>
      <td>And</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>



## 7. 매출 Top 10 쇼핑몰 구매자들의 연령대를 쇼핑몰별로 시각화하여 보여주세요.
>- 쇼핑몰이 설정한 타겟 연령대와 실제 구매층 일치 비교


```python
# 매출 top10 쇼핑몰 추출

order2 = order2[order2['shop_id'].isin(top.index)]
```


```python
# 시각화 

figure, axes = plt.subplots(nrows=1, ncols=2)
figure.set_size_inches(18,3)

sns.boxplot(data=order2[order2['age'] != -1], x='shop_id', y='age', ax=axes[0])
sns.violinplot(data=order2[order2['age'] != -1], x='shop_id', y='age', ax=axes[1])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f55d837e810>




    
![png](Level4_zigzag_files/Level4_zigzag_20_1.png)
    


## 8. user 테이블에 연령대를 나타내는 칼럼을 만들어주세요. 그리고 쇼핑몰이 설정한 타겟 연령대와 실제로 구매를 한 고객의 연령과 일치하는지를 검증해주세요.
>- 지그재그의 쇼핑몰들은 아래의 필터에서 보여지는 것과 같이 타겟 연령대를 가지고 있습니다. 하지만, 실제 구매가 설정되어 있는 타겟 연령대에 맞게 이루어지는지 꾸준히 검증이 이루어져야 합니다. 유저에게 더 적합한 제품이나 쇼핑몰을 추천해주어 유저 경험 (UX)를 증진시키는 것은 추천 플랫폼에게 매우 중요한 요소이기 때문입니다.


** 수행해야 할 작업은 총 3단계입니다.**
1. 실제 나이를 바탕으로 user 테이블에 연령대 칼럼을 만들기
2. shop 테이블을 불러와 user, order 테이블과 병합하기
3. 쇼핑몰의 타겟 연령대와 해당 쇼핑몰에서의 결제를 한 고객의 연령대를 비교하기


```python
# 연령대 컬럼 만들기

def make_generation(age):
    if age == -1:
        return '미입력'
    elif age // 10 >= 4:
        return "30대 후반"
    elif age // 10 == 1:
        return "10대"
    elif age % 10 < 3:
        return str(age // 10 * 10) + f"대 초반"
    elif age % 10 <= 6:
        return str(age // 10 * 10) + f"대 중반"
    else:
        return str(age // 10 * 10) + f"대 후반"

user['연령대'] = user['age'].apply(lambda x: make_generation(x))
user.head()
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
      <th>user_id</th>
      <th>os</th>
      <th>age</th>
      <th>연령대</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>--PYPMX8QWg0ioT5zfORmU-S5Lln0lot</td>
      <td>And</td>
      <td>41</td>
      <td>30대 후반</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-16-xXbeDcvkZJtTpRwMi57Yo2ZQpORv</td>
      <td>iOS</td>
      <td>31</td>
      <td>30대 초반</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-1de9sT-MLwVVvnC0ncCLnqEqpSi3XSN</td>
      <td>iOS</td>
      <td>16</td>
      <td>10대</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-3A3L2jnM55B_Q1bRXMjZ6sPnINIj-Y1</td>
      <td>And</td>
      <td>41</td>
      <td>30대 후반</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-3bhcSgPOIdQAPkPNcchxvECGqGQQ78k</td>
      <td>And</td>
      <td>42</td>
      <td>30대 후반</td>
    </tr>
  </tbody>
</table>
</div>




```python
# load shop table

query = 'select * from shop'
shop = pd.read_sql(query, connect)
print(shop.shape)
shop.head()
```

    (200, 5)





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
      <th>shop_id</th>
      <th>name</th>
      <th>category</th>
      <th>age</th>
      <th>style</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Edna</td>
      <td>의류</td>
      <td>20대 중반/20대 후반/30대 초반</td>
      <td>모던시크/러블리</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Pam</td>
      <td>의류</td>
      <td>20대 중반/20대 후반/30대 초반</td>
      <td>러블리/심플베이직</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Carolyn</td>
      <td>의류</td>
      <td>20대 중반/20대 후반/30대 초반</td>
      <td>모던시크/심플베이직</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Joan</td>
      <td>의류</td>
      <td>30대 초반/30대 중반</td>
      <td>미시스타일/유니크</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Florene</td>
      <td>의류</td>
      <td>20대 중반/20대 후반/30대 초반</td>
      <td>심플베이직/헐리웃스타일</td>
    </tr>
  </tbody>
</table>
</div>




```python
# user, order, shop table merge

shop_user = pd.merge(order, user, on=['user_id'], how='inner')
shop_user = pd.merge(shop_user, shop, on=['shop_id'], how='inner')
shop_user.head()
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
      <th>timestamp</th>
      <th>user_id</th>
      <th>goods_id</th>
      <th>shop_id</th>
      <th>price</th>
      <th>hour</th>
      <th>os</th>
      <th>age_x</th>
      <th>연령대</th>
      <th>name</th>
      <th>category</th>
      <th>age_y</th>
      <th>style</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-06-11 00:00:43.032</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>1414</td>
      <td>38</td>
      <td>45000</td>
      <td>0</td>
      <td>iOS</td>
      <td>39</td>
      <td>30대 후반</td>
      <td>Mabel</td>
      <td>의류</td>
      <td>20대 후반/30대 초반/30대 중반</td>
      <td>모던시크/페미닌</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-06-11 07:33:39.823</td>
      <td>ni3NQK35j-YaSxli-C_Sz7ZmQqOwMljL</td>
      <td>2278</td>
      <td>38</td>
      <td>37000</td>
      <td>7</td>
      <td>And</td>
      <td>32</td>
      <td>30대 초반</td>
      <td>Mabel</td>
      <td>의류</td>
      <td>20대 후반/30대 초반/30대 중반</td>
      <td>모던시크/페미닌</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-06-11 12:56:27.867</td>
      <td>MnvhmV0tA89bN9TLXgRTbLza689bTkT9</td>
      <td>5513</td>
      <td>38</td>
      <td>31000</td>
      <td>12</td>
      <td>And</td>
      <td>37</td>
      <td>30대 후반</td>
      <td>Mabel</td>
      <td>의류</td>
      <td>20대 후반/30대 초반/30대 중반</td>
      <td>모던시크/페미닌</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-06-11 22:57:11.582</td>
      <td>3Vo9NP0qU_176pgbqk6Cu-CY7kpJ2-WB</td>
      <td>7026</td>
      <td>38</td>
      <td>17100</td>
      <td>22</td>
      <td>iOS</td>
      <td>34</td>
      <td>30대 중반</td>
      <td>Mabel</td>
      <td>의류</td>
      <td>20대 후반/30대 초반/30대 중반</td>
      <td>모던시크/페미닌</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-06-11 00:02:33.763</td>
      <td>smDmRnykg61KajpxXKzQ0oNkrh2nuSBj</td>
      <td>1351</td>
      <td>12</td>
      <td>9500</td>
      <td>0</td>
      <td>And</td>
      <td>17</td>
      <td>10대</td>
      <td>Rachel</td>
      <td>의류</td>
      <td>10대/20대 초반</td>
      <td>러블리/심플베이직</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 실제, 쇼핑몰 연령대 일치여부

def check_generation(row):
    if row['category'] == '의류' and row['연령대'] == '미입력':
        return True
    else:
        return row['연령대'] in str(row['age_y'])
    
shop_user['거래연령 일치여부'] = shop_user.apply(lambda x: check_generation(x), axis=1)

table = pd.pivot_table(shop_user, index='shop_id', values='거래연령 일치여부', aggfunc=['mean', 'count'])
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

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>mean</th>
      <th>count</th>
    </tr>
    <tr>
      <th></th>
      <th>거래연령 일치여부</th>
      <th>거래연령 일치여부</th>
    </tr>
    <tr>
      <th>shop_id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0.666667</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.937500</td>
      <td>16</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.400000</td>
      <td>5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.000000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.000000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>191</th>
      <td>1.000000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>192</th>
      <td>1.000000</td>
      <td>2</td>
    </tr>
    <tr>
      <th>193</th>
      <td>0.000000</td>
      <td>5</td>
    </tr>
    <tr>
      <th>194</th>
      <td>1.000000</td>
      <td>2</td>
    </tr>
    <tr>
      <th>197</th>
      <td>0.666667</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
<p>135 rows × 2 columns</p>
</div>




```python
# 매출액 top10만 추출

table = table[table.index.isin(top.index)]
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

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>mean</th>
      <th>count</th>
    </tr>
    <tr>
      <th></th>
      <th>거래연령 일치여부</th>
      <th>거래연령 일치여부</th>
    </tr>
    <tr>
      <th>shop_id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>0.750000</td>
      <td>24</td>
    </tr>
    <tr>
      <th>11</th>
      <td>0.684211</td>
      <td>19</td>
    </tr>
    <tr>
      <th>12</th>
      <td>0.857143</td>
      <td>42</td>
    </tr>
    <tr>
      <th>14</th>
      <td>0.566667</td>
      <td>30</td>
    </tr>
    <tr>
      <th>19</th>
      <td>0.789474</td>
      <td>19</td>
    </tr>
    <tr>
      <th>22</th>
      <td>0.929293</td>
      <td>99</td>
    </tr>
    <tr>
      <th>32</th>
      <td>0.540541</td>
      <td>37</td>
    </tr>
    <tr>
      <th>60</th>
      <td>0.695652</td>
      <td>23</td>
    </tr>
    <tr>
      <th>63</th>
      <td>0.000000</td>
      <td>27</td>
    </tr>
    <tr>
      <th>126</th>
      <td>0.000000</td>
      <td>39</td>
    </tr>
  </tbody>
</table>
</div>



의류이외의 제품을 파는 쇼핑몰은 타겟 연령층이 없기 떄문에 일치여부가 0이 나옵니다. 일치여부가 낮은 쇼핑몰의 경우는 더 긴 기간의 로그를 모니터링 한 다음, 태그 수정을 제안하여 타겟 적합도를 높일 수 있습니다.

## 9. 쇼핑몰의 스타일 태그를 정리해주세요.


```python
style_list = ['페미닌', '모던시크', '심플베이직', '러블리', '유니크', '미시스타일', '캠퍼스룩', '빈티지', '섹시글램', '스쿨룩', '로맨틱', '오피스룩',
              '럭셔리', '헐리웃스타일', '심플시크', '키치', '펑키', '큐티', '볼드&에스닉' ]

for i in style_list:
    shop[i] = shop[shop['style'].notnull()]['style'].apply(lambda x: i in x)
shop.head(3)
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
      <th>shop_id</th>
      <th>name</th>
      <th>category</th>
      <th>age</th>
      <th>style</th>
      <th>페미닌</th>
      <th>모던시크</th>
      <th>심플베이직</th>
      <th>러블리</th>
      <th>유니크</th>
      <th>...</th>
      <th>스쿨룩</th>
      <th>로맨틱</th>
      <th>오피스룩</th>
      <th>럭셔리</th>
      <th>헐리웃스타일</th>
      <th>심플시크</th>
      <th>키치</th>
      <th>펑키</th>
      <th>큐티</th>
      <th>볼드&amp;에스닉</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Edna</td>
      <td>의류</td>
      <td>20대 중반/20대 후반/30대 초반</td>
      <td>모던시크/러블리</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Pam</td>
      <td>의류</td>
      <td>20대 중반/20대 후반/30대 초반</td>
      <td>러블리/심플베이직</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>True</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Carolyn</td>
      <td>의류</td>
      <td>20대 중반/20대 후반/30대 초반</td>
      <td>모던시크/심플베이직</td>
      <td>False</td>
      <td>True</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>3 rows × 24 columns</p>
</div>



## 10. 스타일별 실제 구매 기록을 바탕으로 가장 구매가 많이 일어난 스타일 키워드를 찾아주세요. 또한, 매출이 가장 많은 3가지 스타일의 구매 연령대 분포를 그려주세요.


```python
merged = pd.merge(order, user, on=['user_id'], how='inner')
merged = pd.merge(merged, shop, on=['shop_id'], how='inner')
merged.head()
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
      <th>timestamp</th>
      <th>user_id</th>
      <th>goods_id</th>
      <th>shop_id</th>
      <th>price</th>
      <th>hour</th>
      <th>os</th>
      <th>age_x</th>
      <th>연령대</th>
      <th>name</th>
      <th>...</th>
      <th>스쿨룩</th>
      <th>로맨틱</th>
      <th>오피스룩</th>
      <th>럭셔리</th>
      <th>헐리웃스타일</th>
      <th>심플시크</th>
      <th>키치</th>
      <th>펑키</th>
      <th>큐티</th>
      <th>볼드&amp;에스닉</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-06-11 00:00:43.032</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>1414</td>
      <td>38</td>
      <td>45000</td>
      <td>0</td>
      <td>iOS</td>
      <td>39</td>
      <td>30대 후반</td>
      <td>Mabel</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-06-11 07:33:39.823</td>
      <td>ni3NQK35j-YaSxli-C_Sz7ZmQqOwMljL</td>
      <td>2278</td>
      <td>38</td>
      <td>37000</td>
      <td>7</td>
      <td>And</td>
      <td>32</td>
      <td>30대 초반</td>
      <td>Mabel</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-06-11 12:56:27.867</td>
      <td>MnvhmV0tA89bN9TLXgRTbLza689bTkT9</td>
      <td>5513</td>
      <td>38</td>
      <td>31000</td>
      <td>12</td>
      <td>And</td>
      <td>37</td>
      <td>30대 후반</td>
      <td>Mabel</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-06-11 22:57:11.582</td>
      <td>3Vo9NP0qU_176pgbqk6Cu-CY7kpJ2-WB</td>
      <td>7026</td>
      <td>38</td>
      <td>17100</td>
      <td>22</td>
      <td>iOS</td>
      <td>34</td>
      <td>30대 중반</td>
      <td>Mabel</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-06-11 00:02:33.763</td>
      <td>smDmRnykg61KajpxXKzQ0oNkrh2nuSBj</td>
      <td>1351</td>
      <td>12</td>
      <td>9500</td>
      <td>0</td>
      <td>And</td>
      <td>17</td>
      <td>10대</td>
      <td>Rachel</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 32 columns</p>
</div>




```python
# 매출이 가장 많은 3가지 스타일

top_style = pd.DataFrame({'style': [], 'price_sum': []})
for i in range(len(style_list)):
    top_style = pd.concat([top_style, pd.DataFrame({'style': style_list[i], 
                                                    'price_sum': [merged[merged[style_list[i]] == True]['price'].sum()]})], ignore_index=True)
top_style = top_style.sort_values(['price_sum'], ascending=False)[:3]
print(top_style)

top_style = list(top_style['style'])
```

       style  price_sum
    2  심플베이직  9780040.0
    3    러블리  6844600.0
    1   모던시크  4105100.0



```python
# 시각화

plt.figure(figsize=(18,4))

sns.distplot(merged[merged[top_style[0]] == True]['age_x'], hist=False, label=top_style[0])
sns.distplot(merged[merged[top_style[1]] == True]['age_x'], hist=False, label=top_style[1])
sns.distplot(merged[merged[top_style[2]] == True]['age_x'], hist=False, label=top_style[2])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f55d8032fd0>




    
![png](Level4_zigzag_files/Level4_zigzag_33_1.png)
    


## 10. DB에서 로그 데이터를 불러온 다음 timestamp 컬럼을 datetime 형식으로 바꿔주세요. 그리고 user id를 보기 쉽게 간단한 자연수 형태로 변환해주세요.

```
지그재그 로그 데이터의 명세는 다음과 같습니다.

- 컬럼 별 명세
    1. timestamp : 이벤트 발생 시간 (한국 시간 기준)
    2. user_id : 이용자 고유 식별자
    3. event_origin : 이벤트가 발생한 앱 위치
        - event_origin 값 별 의미
            a. goods_search_result : 특정 검색어의 상품 검색 결과
                (Ex: goods_search_result/반팔티)
            b. shops_ranking : '쇼핑몰 랭킹' 영역
            c. shops_bookmark : '즐겨찾기' 영역
            d. category_search_result : 카테고리 검색 결과 
                (Ex:category_search_result/상의)
            e. my_goods : '내 상품' 영역

    4. event_name : 발생한 이벤트 명
        - event_name 값 별 의미
            a. app_page_view : 앱 내 화면 이동
            b. enter_browser : 앱 내 클릭을 통해, 특정 웹페이지로 진입
            c. add_bookmark : 특정 쇼핑몰을 즐겨찾기 추가
            d. remove_bookmark : 특정 쇼핑몰을 즐겨찾기 제거
            e. add_my_goods : 특정 상품을 내 상품 추가
            f. remove_my_goods : 특정 상품을 내 상품 제거

    5. event_goods_id : 이벤트가 발생한 상품 고유 식별자
         - 상품 관련 이벤트가 아닌 경우, 공백

    6. event_shop_id : 이벤트가 발생한 쇼핑몰 고유 식별자
         - 쇼핑몰 관련 이벤트가 아닌 경우, 공백
```


```python
# load log table

query = 'select * from log'
data_logs = pd.read_sql(query, connect)
print(data_logs.shape)
data_logs.head()
```

    (105815, 6)





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
      <th>timestamp</th>
      <th>user_id</th>
      <th>event_origin</th>
      <th>event_name</th>
      <th>event_goods_id</th>
      <th>event_shop_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-06-11 00:00:00.213</td>
      <td>K1d8_t3-QIskaSkrx32oAFu856D8JmLo</td>
      <td>shops_ranking</td>
      <td>app_page_view</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-06-11 00:00:00.810</td>
      <td>lwFZ77v_ygk0uU40t1ud3l30EZ6sE2R3</td>
      <td>shops_bookmark</td>
      <td>app_page_view</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-06-11 00:00:00.956</td>
      <td>mR-bO6hC9g-m8ERXMRQZaRwJFvzNNdd8</td>
      <td>goods_search_result/로브</td>
      <td>app_page_view</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-06-11 00:00:01.084</td>
      <td>K1d8_t3-QIskaSkrx32oAFu856D8JmLo</td>
      <td>shops_bookmark</td>
      <td>app_page_view</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-06-11 00:00:01.561</td>
      <td>Yjny5AchUWLiuv4kdeq50COF-S8OFXPd</td>
      <td>shops_bookmark</td>
      <td>app_page_view</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_logs['timestamp'] = pd.to_datetime(data_logs['timestamp'])
```


```python
# user_id 정수화

user_id = pd.DataFrame({'user_id': user['user_id'].unique()}).sort_values(['user_id'])
user_id['n_user_id'] = list(range(len(user_id)))
user_id.sort_index()
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
      <th>user_id</th>
      <th>n_user_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>--PYPMX8QWg0ioT5zfORmU-S5Lln0lot</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-16-xXbeDcvkZJtTpRwMi57Yo2ZQpORv</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-1de9sT-MLwVVvnC0ncCLnqEqpSi3XSN</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-3A3L2jnM55B_Q1bRXMjZ6sPnINIj-Y1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-3bhcSgPOIdQAPkPNcchxvECGqGQQ78k</td>
      <td>4</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>9995</th>
      <td>zymqXr4nryoIrj8e9ToeLoNbnOfCvcsM</td>
      <td>9995</td>
    </tr>
    <tr>
      <th>9996</th>
      <td>zyugPCF7YB6Fksn51adQa3CpAIn1SwIf</td>
      <td>9996</td>
    </tr>
    <tr>
      <th>9997</th>
      <td>zz-aNy7UWfvyrZxO4Fs4K5ewmqZVaMOs</td>
      <td>9997</td>
    </tr>
    <tr>
      <th>9998</th>
      <td>zznj-LHhddVvuzZmbZpw6MSylLO64982</td>
      <td>9998</td>
    </tr>
    <tr>
      <th>9999</th>
      <td>zzxBQ7i7mttX0cv1GqFuuMstg7keEkdV</td>
      <td>9999</td>
    </tr>
  </tbody>
</table>
<p>10000 rows × 2 columns</p>
</div>




```python
# user_id dictionary

id_zip = zip(user_id['user_id'], user_id['n_user_id'])
id_dict = dict(id_zip)
id_dict
```




    {'--PYPMX8QWg0ioT5zfORmU-S5Lln0lot': 0,
     '-16-xXbeDcvkZJtTpRwMi57Yo2ZQpORv': 1,
     '-1de9sT-MLwVVvnC0ncCLnqEqpSi3XSN': 2,
     '-3A3L2jnM55B_Q1bRXMjZ6sPnINIj-Y1': 3,
     '-3bhcSgPOIdQAPkPNcchxvECGqGQQ78k': 4,
     '-3fmY1WsLkYJwN_8lZQMmxZd6zJTAcT1': 5,
     '-3q-oynqxFEgSHUwX802hpmi1louyQNv': 6,
     '-428TMckUlhn6ptxN7gR2FGaSyXjSnaD': 7,
     '-4O8WnD8dT6nWho-4KbIm6TvnK4BmjX_': 8,
     '-4ltLPS55n6J2wSUCLxEZwxYdeW37cK5': 9,
     '-5BA0EwkyhGLCC8FxzvvDgyrZWYJM33I': 10,
     '-5Cwn2Fcx9j16QSM2-SLiaLMm0sS4E2I': 11,
     '-5o3lkvJctT3uURb5JWPVxe1VjqhyzAi': 12,
     '-622WUNWBtjX5VGKx8UnOtn2NVHD_NaB': 13,
     '-62U2A3KHjNZ2XXmOgQTSWEfPg1RRWWy': 14,
     '-63J8veARgGL3ulnRKblm4xhhwkvjKzG': 15,
     '-6UZWGgl3AAI7Df2sVWLX6oT6zP43zo0': 16,
     '-6jxyh56lSivkbLm3WNGRCmdyrdsBmNW': 17,
     '-71z4lG_D-eKnOmDCJlUaNvVcwd808yw': 18,
     '-75tFsDSoUwapUvwCUHTZiGTGkaSDleQ': 19,
     '-7SKUZkBmbG2ZMvJ0E0jmMDcd8PgmARb': 20,
     '-7uBbvfy4gff6mHV9XotjVO2YlCY2r8v': 21,
     '-8htVW7UIA8qRupSdCx-6PzIXLI_vk2p': 22,
     '-9qbSavSdufdw9JwmiWX1_URT2E2QxFZ': 23,
     '-Ae6T8G5uAZldwUEOTMR-KzG8C8G6SYn': 24,
     '-BvO7h0cREi0zszH5EhugLlvfnXmK7le': 25,
     '-CHArPAojNa8sfK1Ni0WCyQvtF9rlujb': 26,
     '-CSzbGJ06nTB7UJ8CSsU-IVtn4apPUwX': 27,
     '-DD4QE4Kp4i7tYalqeGSmJyAtfWLcYY7': 28,
     '-DM7pBgq8_K3F5ttDYENsUzf8EMYx1kR': 29,
     '-DhBA7c5OP3gFvBkng5b2jHdNYGDF3at': 30,
     '-DmelscKWaGOgODpbk5udtReEUvRmY25': 31,
     '-Dt4wPzvVjzno0dgjUAhs_BtbniR8fyU': 32,
     '-Dtg2_c38T2WqcxvzxNoQOKjwV_WXW3U': 33,
     '-EBvKufhJMmB-uL4Gb8up_dZubfazQrN': 34,
     '-EpdIEklTRF8V0wFNcJ1hTriLDKGgY7h': 35,
     '-EzeqHwqCE5w2DcivhEZT6OcjzIe-BnO': 36,
     '-G8e4-zH6pTty3OzEgDWpMDxzaQw0Inq': 37,
     '-GCtw39Q5SUAkgnbzaVsWxxYUyvXYESf': 38,
     '-GN_i-M2FVR_OC7ERqsbuXsHT5WHT_19': 39,
     '-HrAJSQTzq7A2a7if0CARcpR-MPQNtjM': 40,
     '-IP3FwhEXrqR1OUDjicLNuZD0pXNhAwh': 41,
     '-IeEef_WmYqN9g87TP-XY_cBDExtMnYM': 42,
     '-IfjJ89djHFZi9rTYHcE_AJqfwdJbqYP': 43,
     '-Ii7NyvhizIwHlbCvi4ovYoB3WOxVR6e': 44,
     '-J1-YI15bmfHqz706XL-t2DELNae3BK4': 45,
     '-JrptE-k2qGZTgqoWgKQ2CkDI3e2i--i': 46,
     '-K76uxrcXqUDULcH-OKfoWxfWrH7-bYc': 47,
     '-KAPzobWDxGY7bCfCSfgeWBH8uzDXS8n': 48,
     '-L2Awbp23c9b1o1R_do--BZEtPivAUua': 49,
     '-LF9kd_LkqLCKsyRECBwDnToEIoZpT4F': 50,
     '-LHdmkfa3uKwlpRTaOpARhyNHqcWAIFI': 51,
     '-LJW8D3rOJs-cTcSsspvuSUh4pKDUdh_': 52,
     '-MhXFZWekBnvZ-Bj4lbt_Q5rDbwOAJIc': 53,
     '-N19jslUgVdZgFhTsbt-Qa8-2avlWjJQ': 54,
     '-NHyDXqV106lyjbh4e3e5JXoIqEYzxpZ': 55,
     '-O78CyLDmPILHvfRINWC7GtxZ6C8XEFd': 56,
     '-O8132_YYdBFHlNUbLWQpeIKGMdX_mjI': 57,
     '-OAaalRdrKcy2JnQuUktMTivUkhzhGqL': 58,
     '-OjNBoKeSseQqxcl7hC7LofbZAxkM7h7': 59,
     '-Q2zMYOSfxJzAT3bCmpb-mm2pBCL9_Hd': 60,
     '-Qhp1QtAa66pHomNmhUl1qkksPW-Jmin': 61,
     '-QmN2AFoHc97kjxWy28HF9flCsfce9hg': 62,
     '-QmYIj-H47FsR_I8SD0qIxC7RACgcpFU': 63,
     '-RX5mi3RbwWxrZX05aE9Oqujjc8v0sDI': 64,
     '-RkcyYiat4mnNUK48f_0xL1rWJC1e5Sy': 65,
     '-SXxoXdABrYMQnFPoS7RUV4grtN_-20j': 66,
     '-SnTp0qfMPhA5YoODFQZHTCI2m1BJATt': 67,
     '-Sp5JstbFxVFSRTFEZ00pVyADBzt6h8d': 68,
     '-TorTFJx8gRSr7xTxVwumR0Rk0ohtsoK': 69,
     '-U-JT5YB3QE68MYwRy0BYeJMT6A2AUTs': 70,
     '-UGZrYZ4hoGNyUEawyoI0Q4UZXW9TNe2': 71,
     '-UiAEis3zGS38VgdUczHp3tGrypLp83k': 72,
     '-UnJPVKthwrXlYyXwvZe8B_eISA4ONLK': 73,
     '-VHehO1XVrDe_wHR1QEuOEVeeG9PgDH7': 74,
     '-VI_fTFPd4n1Iu6LAcF1uKDdhYKgqK44': 75,
     '-WxYaD63mK1pn_T0A_EjiTs4f5VYsLoM': 76,
     '-XRBHrHjtKKB8pubuJ309ZTe8AjLQcaU': 77,
     '-ZsgqTZl3KSd_oXnl8N5XNKNk60JWPJa': 78,
     '-ZxtGvoUVaNpSPcSf-2FfDGJTcmnceK_': 79,
     '-_8SvCI-GlKGtaoS2E0PNSQT3ampaHdr': 80,
     '-__V4fapuZu9JrEXk3S5B0V5Uftme2so': 81,
     '-ayuG0oBDd5HxFZ6g_59IN6s8jc5JoLR': 82,
     '-b0w_nXB4TLZQzp0u7vR7eoxvp0qreVt': 83,
     '-b3xSGJo8dxPFgkGoP1fF4OKQe47WTQ3': 84,
     '-bAmsDSUs_GwVs2sBRRiFtDpclgqnTWp': 85,
     '-bW8vR_yKhs40MmASelnYoyY3daCIMkW': 86,
     '-bpKDs9gKdOVd33-yIwyia68QJXlQA0J': 87,
     '-by1c_0QC-R8GN0ZhZeV8697f3Awzagi': 88,
     '-c59nvb2SpJP-fVBqMkV5nOCI3ySQDWs': 89,
     '-cNzffxv2Egd3c3COdtI2mDrp8ypWv0z': 90,
     '-cckGqSmzK_SQvzueqkrnQzJ6OgD6IAZ': 91,
     '-cm3QsSxiloyb5kpVV1gka38-dTWzzSp': 92,
     '-cmO1MWB0jSPkf836eBLRGmRWQAO-QtQ': 93,
     '-czlIBWDQG0fOX-m6gPh9Rl82FzqOu_d': 94,
     '-d0OtLmJDVgGxab_DNkvvQZgVG0X1Xux': 95,
     '-dmc0sa0gCvJ5KmqkVKZT8Jb5tfRpkjw': 96,
     '-eGH0rie-miqE6oRMLTjyCkZhUWJh_QM': 97,
     '-eRC4daHqu5n662d-Y3a8qjtVx1pOttI': 98,
     '-ed4eHabeosfJaBWnsZxgXRrBAOETXYK': 99,
     '-egWwe3isl7rtKJDxP8ozKC_dzZUhumA': 100,
     '-ekX42BJpQaWE8_19gyjU0of98E5_N9B': 101,
     '-etFrkS-ghB79NyWTUCL92dCvEGXCD-6': 102,
     '-fK48I4C6gjWI3F1xBffm4303nL_-VOU': 103,
     '-fNm3fnrElUu7nF-OwtVxA7fY8ijU0aJ': 104,
     '-fSOxMY6izO8LrY3s5oyaHVmhk7hpprL': 105,
     '-fp540bwSD_DJp-xbwjn395KoX66v6YT': 106,
     '-gACDk5ovMXy6DclIVCPAdjQOqjGq_1l': 107,
     '-gycbv-w_Yfm3QmnQJUOB-KRQJdXhan1': 108,
     '-h8JdR6KGdu_BQeAvIxDtqNCRqG3Zkk4': 109,
     '-hXRmy_ILzSgXkpzCYKfB5GFDPOtV7ys': 110,
     '-icMk-sfTgtBE1aExGorSxYo2mntelyj': 111,
     '-iezn-WitSN0k6W55Lss8SiLcqxZ-PRD': 112,
     '-if4yvtFMDPrtC578OMJ5gta-JNM-VaL': 113,
     '-iyXTao7hdCDWujpJ_lqoBW0K6-JXCRL': 114,
     '-k4EuKu1x29GF9gqHMrAazkd9rrTfZxu': 115,
     '-lEUMUFkTARejo2wQaqRWVnm24Jg98rA': 116,
     '-mZEEW22aqNU6B794YSve6VB0UOFGCwA': 117,
     '-m_o_fjLt2dumaTA3hGFfWDWc5XVFYbh': 118,
     '-mfvXk0bpG9FF9BxhtsPO2oIPbLzpPtM': 119,
     '-mzE8kJNOw_XrQ73bQDSa0wWQRGZEDIg': 120,
     '-nE3YSLqNuTm_mtrZ5L9BwrbypQdXVcf': 121,
     '-nUpZjqKLOzZyBDfw5JEa3YV1X9I9CGH': 122,
     '-ntZw9gYagAhaZJ6jDNcFLorwZ4bzTUr': 123,
     '-o7kzi5qmIZfaIIEd5NGcAzZHV_9QRgH': 124,
     '-oQDz92wu03AZ2-D1irCumssCXQpN-5X': 125,
     '-or848YnAnSJGWOVYW1G68RlyL5F4MnZ': 126,
     '-q1k5EBWQpvt3doDD_yPffzNHxAc4ZOE': 127,
     '-q4JWL7qLn0v4G7sF2zu2qk6oKZX3q8-': 128,
     '-qJDt4SYPRFW4VJ4Vkicp6Uq8Maex4s1': 129,
     '-qSFJnPE73ecwpMi6IEMY-h8JvC3l6-g': 130,
     '-qi0FQUqpRT_HTyqUJkh6xOX8s_nJ2Ig': 131,
     '-rX2fqH11nFsbjhz85EE97M1TDBPt6L0': 132,
     '-rXiq_hr4k7Ahc08xOeY0pBBZiowt0A6': 133,
     '-rca_qeVKg_LY2-qzo9cSd_zJGWlpDSj': 134,
     '-s5PfPAiXs-afbkx18hh5W5PgO0qBXGj': 135,
     '-sMkdXHIJZNnQcDVLmXgtvViedasPi1x': 136,
     '-sgzu6doO3Y_3K6NRTbBxKBrea12eboi': 137,
     '-t2VveCySpPTSKUSl9-pMzo3u62Ggit-': 138,
     '-tW5GAv8KvgKrFo2-Z19DWlPcLk8TTTF': 139,
     '-tzA6cOZvW5Efu0xpECy40CowqfkPwO8': 140,
     '-tzXKYtM-zPSNz8FuSe3S00plYhVss2s': 141,
     '-uP3hBXAvdArCk1kddfJEMJpLDB-7wkg': 142,
     '-uWVJ7qhriUBwmcXPp1J6hiaPnJgzo-k': 143,
     '-vi0zV05ls8IREioBZFot7kEfW07mzzZ': 144,
     '-w6Hn7to-3UNAYeCgfZz65QY4BG1Qpm3': 145,
     '-wcrIwgwRiN9I748jpQUPFcSLq183rx_': 146,
     '-x35t_xG2_8rLJydgJ7uog3IlE1__M6U': 147,
     '-x5yC2fNbmZ1y1TZUQG8-BiqZao_Q_UN': 148,
     '-x6YjF3A7Tm_cOG2S689FT3vgjWVbJaO': 149,
     '-xUJVucG0yeNyx0jos2Vpt7vjR4tVG_D': 150,
     '-xY1NMPEmDc7HPv7Ar7-HhCV6rkFNbn5': 151,
     '-yHKGC8ESwJbi6ysS4cMoBCAjhmvymvV': 152,
     '-yYxdTxSPvod6MHBdrpRf-4eUFVLy80Q': 153,
     '-yh-CtwaVROnUvKvhEV4-pwmxkU1dYUb': 154,
     '-ytNEbQLZTfvkGpycZ0_j1t4Y8Glgv3H': 155,
     '-zU17fYsoGBmwr3198kSvz0Qc4K3Ht61': 156,
     '-zYXYlWkMTgpQOQYuk4TpXpKqUKilB9n': 157,
     '0-gltdZTK2hQMBKsXc1uzlWe3SsWFgma': 158,
     '00Eh8XFntJ9gWn4xFj8U_tDbk6kO0eI0': 159,
     '00X9QSX5rgLtmdgNSPhg6OSO25l53OnS': 160,
     '00gZAUnFn0Ask_plQ9OG6eN0Ze2EEoOL': 161,
     '014pMyWVzj4zAYL4OlGGroRL5tdKNPhv': 162,
     '01KUOJqNtZUrItHzjf1pgo5UFMswmFg-': 163,
     '01Nj5u5R5nIhoUZVSG2WA8hy5dM2pG3p': 164,
     '01XiiHWG6ex33qwoaUO6m3OLdcxI52OA': 165,
     '01grFSp_ulX9ezy6CfeWesfYrj3sm7v0': 166,
     '01m8FY2yeGk9raQDP_FKPXnqWCrZY0Y5': 167,
     '01oEO-_tOKpAiMkUAJwEreqD3qTgSU3U': 168,
     '024d801eT2x_sk-1hS8tqUIZJQgoffxL': 169,
     '02D_k1dBXOaRksPQzwx4WOviMRsTnaVu': 170,
     '02H7ZMqxlxiWdrj3CMNvgxIwkD8nqmXJ': 171,
     '02iSxgOe4uNFpOBo4xBYQ3ZnkYk9c9iL': 172,
     '031IrKuZ-JWtE242Z5V4kk9xC0gs089g': 173,
     '034N8bUP-LDPaIrgDyzHLLhrtyn4QrYD': 174,
     '03NMvIIU-VSut44TktPL1eQ2U2GEQ2vT': 175,
     '03ZPQuOqobYFqRdE5LZgcDrAOV9b7BCa': 176,
     '04-4OZ0ymZ_-_FAVcOTAZCfs2rdvnVoS': 177,
     '04iVDKAyu75QU3KlclcU2sFBtIkjK4Y_': 178,
     '05Nnjm26REx1v0ZiA9Ope9BMk9FbVwFL': 179,
     '05hXopf36nfEF7E76ga3BTZ5rv0YsoCQ': 180,
     '069VNqpO_kh7D_Cd9qww-B5CalJzLpGL': 181,
     '06KK25MxvBiv3SkYlX_UqVA7TWS-OwSM': 182,
     '06OK0BrZPe9kam_jNNs91aU5D5GVOTxu': 183,
     '06PPERX_ghAqv2sBqzuu3f0EGQvxH485': 184,
     '07i5CvcIsrFGamyYUQn_AvP0tX_AENHT': 185,
     '07spgxiiUETUmIocoG-8ufYW-hEx7uFz': 186,
     '07ulvZ4sDElozBKCRxrY2hSE9NmK4XQf': 187,
     '08KSaST5YdgsbKomkja6LKzO4_upevtO': 188,
     '08Kdc6Yynsn4cYY8CfZmeHUimCt4BpVW': 189,
     '08Vj8RCzTySLKA9xchuy14Dwt6-c74mh': 190,
     '0A8OQkB3vtVZfrR_va9GuohGwnOC3bz8': 191,
     '0APXGmH7Ok52R80Dk6Pmvt8Wj_YGQ0AB': 192,
     '0AXScEXtOR6BZ2LoCJPflPR1EFBXCwwx': 193,
     '0AczcUWa3arslb53eXOTJPUYzE7-xi2N': 194,
     '0BDgEfQJwF0cudqtYkzIKniWA3D9-iwU': 195,
     '0BDwr2zgQfJiNYehRGhPH7Whpu1tVCYE': 196,
     '0C7ChjFwTnR9xHyIAR1bhdCDzd6CbLHZ': 197,
     '0DMjVnSXpASdqQQaQwopEkGaIReiLCUa': 198,
     '0DQvdfu6eAQUja9cBpLE7zKdjdtmAm62': 199,
     '0DT8_c1omY8k-rkbEto46XXpCyh1JYT2': 200,
     '0DXsYrJ-ffcI8ApKXHb03ibFQmqVKYhv': 201,
     '0DmK3YpGHiIqjGLGeLcuPxuFVUFNBXNm': 202,
     '0FYscHJu7mSgwCQVTsFTLUHnlIK4TQWh': 203,
     '0FlD3jD1b9mDW5c0Rbes3LPWVgPZyvH_': 204,
     '0FnBTJU0QJFDAOj9TGsKtea3Zkplzia3': 205,
     '0Fx5LbfOeqs9wtsb8HQJhJgrGcnDlAf8': 206,
     '0Gb0YtIWqOts5NJrVduPw-QDukGcF0zg': 207,
     '0GmiHxmWRI9yWtGgZNDsZfYGWVXlvkJL': 208,
     '0GuqiwNd8_1TBrw8ig0LzqY6jO61lP9C': 209,
     '0HxrF9vnFhT-1HREcMRGTfgKb0gWFzxo': 210,
     '0IVkO10P9oorsf_yUHFrXt6fPcrqiwYe': 211,
     '0IWhGUJc8g7XK3MtzeqcRHTIot4EwO1Y': 212,
     '0IczEJlRXPQ7rWVs6SU9wS10k41Etn2t': 213,
     '0Is1QmxVRf6SzJi6Qm6H5-21QIlC6Ek5': 214,
     '0JKhLtIx-NkFM440Adbg1wVi4JXzCylu': 215,
     '0JXPzy9rbTMO2974aUNNxjfHyVYRSQTz': 216,
     '0KU_nrrjmsoYBkDKTxMwb35DmdVKqUQd': 217,
     '0L8x9JBvv76aaw6vfpu2pF4zpv_ysRp-': 218,
     '0MLy1VJJ9QwY3mqZaSQKOBugwAnbJZVH': 219,
     '0Mo4DHeFEi9ox7cENQBE8v90K4gZ-pjS': 220,
     '0NaqAt2fjN-JhPRX79Btu0V-P4UhtwEq': 221,
     '0NunOZYHrDHIQBSTzia2t2ujljWeFIdU': 222,
     '0O5UkP2PjckTrtmiI5T_vq9xpjgbCKXG': 223,
     '0OlMbb3hrJw8eCgJYkVDYrEYIezR9JbF': 224,
     '0Q4ALyHqMT9h5UyAghB-GGLKoPSWlIdX': 225,
     '0QDaEav42T34tlNXPni3kzqr3Jy8R-ac': 226,
     '0RMN5gHlIFemGGvgXesN1DWfxXj4ce6B': 227,
     '0Rh5zykltVmn_tRl6QmxBgbRcDgsPJxW': 228,
     '0Rk8GxhkCGSiOy8erVRvEITaXkkRgXlq': 229,
     '0RsDTwKEYNt8tlWoNk1kJi8H7yFpK8ll': 230,
     '0SGJPS4QooPt1MV-CIp9aKYlUIwNIqnW': 231,
     '0Shc3mYNv6HapWlAL8C5nb_O8XEZy-lI': 232,
     '0SrUbI4m_nOPyTzC19KHNFn_jRTeGHXh': 233,
     '0TFp43Z5GtmPIvFAjJlJ4VBYRyU-XV_k': 234,
     '0Ui4C1Al9g4w_RWzUaNMqxljuQnWqrQ-': 235,
     '0V97gBVq8-sVO1sfVQuVwhYduz56fABA': 236,
     '0VQoPKljij0vNzMRxiyiRsiH_H0YBmAs': 237,
     '0VqIpVCjNZTUKf4diGRj0daT8YVdOBQk': 238,
     '0W43jLHSMhsaH2ggCfrp5KmWEP4ZlqiD': 239,
     '0WE-8x36fShcC3VpQ3FmeI7W-Tu11F4U': 240,
     '0XDOdDmxiNj2f-EDRtinS_vXNXC8aYPp': 241,
     '0XFToWYIDpGw813B_7xv6OhrSALXxxdy': 242,
     '0XGmnerLJmMrXpCiBE_tjLU8cfc4dctz': 243,
     '0XPWqMdoFqWKHcQhnB7NaO9k5zvmCNvx': 244,
     '0YNZe4EjjOPQkcRl0_aXgUsalLFPkrCh': 245,
     '0Z0KgIwqfdku8UsKE_ou2gjEyhBR7fQ1': 246,
     '0Z6h_ap4-9HiXGx9pqzQu8BL75BRDuzH': 247,
     '0ZECVOXqREDC_GkM26ZI4UlTVqNrvW3q': 248,
     '0Zf0GUoHH_jUkTzi3lYUzmIL1DsjuMsm': 249,
     '0ZpjflYqN2U5OXM_wLF_1tUYz2uJDcya': 250,
     '0_Cn3oC0Kh-iNUpa1tPOFPnQE4_sKNir': 251,
     '0_bjfcGmMkwQAa5OOJlZclavXm96UL4v': 252,
     '0a05D4dY0tbwmtnuuOE5jeuIpoMpDyvQ': 253,
     '0aotjYOyR7ko18AswfRHroY-zdf8pv_n': 254,
     '0bHdH2xtnsZG_OWAazx6qpEY7GCVLGHw': 255,
     '0cCZgeF8UnJZuG2-B4RfLS98Nrq2lmyp': 256,
     '0cr6hffVAdoCRVHw055FGOAgcgwZdus3': 257,
     '0dS3hsANFXxP6_gu1UxK77Nw8oa9XJsg': 258,
     '0dr-gYBcJ3l27jiq8hexxkZEHGIZx3yM': 259,
     '0eHNCO93JYE9haSP9xga_BqFDs1hFZHU': 260,
     '0eXufTpwvtTJw_LCsI-d1eCQwOYenX-j': 261,
     '0fOZKkH3DyccewemxeiUX2PVlodRrw0E': 262,
     '0fT221loKrCKHBjnuqHy3C5E16P6Sati': 263,
     '0fb3gjWcIFEZhmXxppsgceIfQPCyBRll': 264,
     '0h770WXoLOIoR35Q4fICQHfn77GJK0vq': 265,
     '0hLk22zcupwQQNQeOCNtaZ3Mp6L1DcHY': 266,
     '0i5UOrJ_Czfntj933oFJZvXjcwiohrSK': 267,
     '0i7yulFZRdYYDkaXjacFPBxmHWzQA2ci': 268,
     '0iB3Q2iNsefqk0WbFCgLn6SsuDRA7fKk': 269,
     '0iGJ6o5tisw1QIvxL8EvpPqkCfRzqJpS': 270,
     '0iksS9880WhuXnPhaxIxTzdGhOk9bSke': 271,
     '0irHusHHH_OFwwZ-o0a8CV8-0dgtfa7I': 272,
     '0iwz9g--onT-efA6XkkHUZMZuS_CJzfK': 273,
     '0jDJEHkM5qzpJp1dH8f_vl1pt3OagluS': 274,
     '0kGYO7tlwhIt-L14QDcHKbTIg4UfqJCD': 275,
     '0kQZr5aGOvPtUXhy8CdCw6K_-ZN0JQR6': 276,
     '0kT_MaV5iDk0x0HhSx4O3sMC1BWgvP9c': 277,
     '0kWYTXVESpS19o-n0orNiH80G2YeAm4f': 278,
     '0kyZPjj-YgjoAWzoWA66Zbas1KwjWX1V': 279,
     '0lBszkbz9kovzQT9kFqxmdF3dZgBySl6': 280,
     '0lqhixdLVBd-OwnAmDXYYFUTuYXbHrNX': 281,
     '0lskkqdtaMhWq6eRzo6C_sXCNUL8yYIg': 282,
     '0m93Pf39llcsraknvNOydN7i1ZDaussh': 283,
     '0nijm7Rm2CT19_2w03b64vv4sDgURQgC': 284,
     '0oW2TKuLD52MVnHOp59TkfI0UrJ6fyXa': 285,
     '0pqtZNif9M7bG3JlTrZFeZqxZ68-2265': 286,
     '0qI6xdz-eW3YdbWKU40O_W9iwrlDDiHZ': 287,
     '0qkSOZNXgBIsTk-OicgAoPAHAFoS9ewi': 288,
     '0re14M5dlXLX96c0nppbi8lgBysDXAX9': 289,
     '0rsUT9-6YmSYMHwet2JDXZTDm2NGTIuC': 290,
     '0smySpfFcVkfvqNjp2wX1M3tWjKjiZWp': 291,
     '0ssxm5cQEJgNRBRIH4qAsqRc0NnsP_oN': 292,
     '0t99FIhUsy_ZUc0jFVIrSvB2sFCTDeM0': 293,
     '0unthUZRKC3pnIOPKi1iNUAB40cjJxzR': 294,
     '0vA6pPSSVurO_ngTNEWag3Y46kbNB2uB': 295,
     '0vLItdbtHdwAzdkq0Cx-iFhwu9-HoWgI': 296,
     '0vUUJAAk4ajw0yDPqp-CCyKYfDQIhZOq': 297,
     '0vYBY_pYEf0M6-ZLliAidPi7RNUzoaqk': 298,
     '0vbOpEGgMq35ZRiic3EmiJg9nhvcHf85': 299,
     '0wYpjA95SAaJBo1qxpUrCjmuGtXNV9JT': 300,
     '0wh_rlOMGI9bIUwE3opajvEY1rx2CdRA': 301,
     '0wiOOlG4jSCZA6f1Xfp9MBB5RsAqbGku': 302,
     '0wtRkLBCYD5mxdj4WhN1-DBDIwApp8JZ': 303,
     '0xVTAc_xhemQ6Nm5_22pQzCm5RMal4VP': 304,
     '0yLFmLjRqLr5sHysNpshgTJLI6dPkgSB': 305,
     '0ybXl-z_oeHprYjYQN8RhEWgEBHB5qRs': 306,
     '0zG5WqRlSV8cTpBluRcykdjLvljV4RRH': 307,
     '0zH9odveWsdGzqcLAQq8jXILtbQAeYM3': 308,
     '0zRbO-8SB59HNnbnnWukr2peecVXjB11': 309,
     '104M4u84kiL5aF64NLY0vL9qXmfSTaub': 310,
     '10DY5fJQXOx7KalUWWQCfGwkbUF4_tSj': 311,
     '10V-ulgTHS2DLolh4KW5tp85XO9bCx4m': 312,
     '10fNHxo7I0UeslZ_jWO0u6Ks-gheaRM_': 313,
     '10iAIDcSdelLbl727vZlrD0by-uCTME0': 314,
     '112p8Eubmt0BGipihoMYmBYKcss29t5s': 315,
     '11NVzbZdGxYQE5_-gctsEfldK6wdbtXK': 316,
     '11VVVSXb7c1ckNPv7_Vk6rKqO-s4RieM': 317,
     '11XECLB3yN9wqqFLg6hW4e2N-eamqWuE': 318,
     '12FCBLx8K7kBgFDwiOvJ_IeyC9bMNhip': 319,
     '12bzBfTJ6oCjrn0D4rH3_KZpTimQ2gHg': 320,
     '13Z3ivsDVWRZsbm-_FGIgEv9DbEZzROQ': 321,
     '13a0jS7wPrQHu1RXaU37k_mFy1Zcuxru': 322,
     '14m1Xuuji5NKN-W_r6gcv6aalPQKapiT': 323,
     '14rNnVtNC8putPL8uG00qwvfvlbqrhgI': 324,
     '17i5A1oJK4pgHuLplyCYQtaavxVS-sxs': 325,
     '17vFMhpucWeA7YPxhVCcbSk0sXXYvasG': 326,
     '18N29EQm4FrLiq_O-e6jaK0o06yQxnM8': 327,
     '18ydVrQEd5cHpg8yfQa2aFdHdOcqfDOn': 328,
     '19CzwF8XzU1ot754JLbJx3Fpf8VevfkM': 329,
     '19aRDkVkz0c0jRjkuPT7el0ExIB6CTIO': 330,
     '19qyPUeyHtky7DtEF-z5jWIpx5kcqvRO': 331,
     '19vOH1W4-cFanhVEGQd_eJeo6YofJZiP': 332,
     '1A4QseB1hB802AE8hX3yOmN_NUeL_dz9': 333,
     '1AYyk6F9YhbM0VfBanxosdrV7UZxlIqf': 334,
     '1B03YqTegi5rWzLl3fGyv-C2CD6_We6Q': 335,
     '1B8S7rUTZyaH6bTUft9NXFvcGdpWMvrK': 336,
     '1BMz17rMd7z9tT1ro-D7sQpt0R34qQX3': 337,
     '1Bd4UXH5we3OcmHlc7R4jYEux9Pc6So-': 338,
     '1DIV8bDQkNGrU7jN3o4r5UVceBROpLrO': 339,
     '1DTMQE_BODMnM91lzy2ueuWtSi3zRpWI': 340,
     '1DUYh7L-d_IW6f6AKvqcE4mOGb78X_Xv': 341,
     '1DZPhpJ2aaxpVGcSBJWjsa7kIG-FMpYG': 342,
     '1De_f5uGduK-EuRMJ9bCQdEJQ9okt5Eq': 343,
     '1E7_sCfoczuOsgwEOuHHTqzZoExkYhn2': 344,
     '1EQDT6V6rDh5ggghF1AzaxQYqp5dYRkt': 345,
     '1Ew3cVtiFPG7suOuRwoNYyd2gWzu9LX1': 346,
     '1FBkQtWUeOW-aNZTiVr9o0_nu23C7DLC': 347,
     '1GHNmC7S_bX62J8MI5MZ7Py0C_7UcSPn': 348,
     '1GU6Ooc3qpY4OQyNc24ufGk7Y7jP-n4A': 349,
     '1HI4dPEZ-y21jYKhjfQE6gBGPEm877px': 350,
     '1HSzg5OwHx_szDwoLRTrk-AcTdQEB7hT': 351,
     '1HYpmK191903zM4bePZXe4asdhwuZViN': 352,
     '1IbeQqYXFyQQECqQQLtDIfTcMyioCGhI': 353,
     '1ImVVVl3_ksamJz7GbAo4lyCZNqo7MZt': 354,
     '1IygVyx527vpKAqhUjaVE2nQNnuluOmR': 355,
     '1JcrkjyqVsQPp6WFNAa29HYQt5sBhjC4': 356,
     '1Jf9FuYyyx6j6pabPWQB7CvCmShz3fbe': 357,
     '1Jg4yMAvdCvI3WXcGeqvvtrdrdqd3b-l': 358,
     '1K7_rSGjsLKDKWd9VOpHQQq06qUM54Wt': 359,
     '1KnoOIPFzuMsD_PItAtDnZdLiAYihrKz': 360,
     '1L5xgu5SKZCpxuhCKjVS4SSfPvG_v_zI': 361,
     '1L_JXMkZFtQ3r9_hQJxfBw92aHqc2zOu': 362,
     '1M4ut3jN5QclQfsst-YzP_m9BMogaFwx': 363,
     '1MGnWMFJeWFB3QX8uxM6TLBmq_Fhw3tO': 364,
     '1NSgBL12iqrfwjkovKFN-Ur3EMbqwo7t': 365,
     '1NrUWXwi1AlWjpLZruXGmk53sbDkfj-9': 366,
     '1OINw1ZOt_ORW1oBMsc-KdLl02cskDl9': 367,
     '1Obc9B7eSwQewlujrRkWq7kUJfcw5J2b': 368,
     '1Oip4wEMJv2ng1Aqe83PAjx56HGiMTtX': 369,
     '1Oy4KGGOlh1c8wUUUdO4PZKBLrpBpdIr': 370,
     '1PBPsoBPqFWHRfT9fvDEm0zPNVGDGlOW': 371,
     '1PRNbvTl51TTGQ5zV0nRkCkmJMMiG4ne': 372,
     '1PwSmg5OXG0sKu9InlOINBRfh7QtHFgz': 373,
     '1QFtOeiyU_agnNAPHRILYsan4VkA-U9J': 374,
     '1QR_F1PTejVat5774nIa9UTX9d7S471v': 375,
     '1QuPn7hnGAwz7X6H1CWzwB20DGziBJ_S': 376,
     '1R50flueeATWMQ9q4qE1TsNKBO39b4d8': 377,
     '1REpaSmBGCDilAggy3M0B9LA6dq3C8pC': 378,
     '1SOB8K3767HRXQibM8yM6CURzq4qpBO3': 379,
     '1SZp7sMFWAfcLfGOZOWKLyDgfaIw2jT7': 380,
     '1Sc5Z0wfPctbwkk4YCUcc1L6nChEDZq_': 381,
     '1T57BY7FCh7Jw_baywaGT4EG-GJWzjmj': 382,
     '1U9nVLKX2MvRPmaWCJXYU4Yn0kdxYYfr': 383,
     '1UIY5zj1VUAlk2qFS_5MWBJxXrdIaS4w': 384,
     '1Ud-h_HARSSP4Tjjhc0-HUDVd6soarT2': 385,
     '1UvF3NrkZf24wENUp8It0ZGF8QWKnKVE': 386,
     '1V2qyn-5WTsZm47-jPLkIR3bNPZ6IudQ': 387,
     '1VjLsA1Ra4tcPqZNUemDZzil5lsIJkcF': 388,
     '1VuGX5MFEMjI1pfLdyaJvp5BpKWv_UEY': 389,
     '1W9k1jIcgnZl8tajA-W3EtcROTSkfk8g': 390,
     '1WF9sIGXdgCPXFGqfB6ad_YwDNYKcmUr': 391,
     '1WMzJ7fXNLeJY9mp3SmVBYEkNO-qBkSU': 392,
     '1WNWYzlw1kc_ZYVHayEIYSzScUXMWYB2': 393,
     '1WXqBxcqRoHZR-kny3GcwkyFlSX6jyXQ': 394,
     '1Waq690jmuPnR0T9njwXqk-KsHP-QRQo': 395,
     '1WgvWDWtK3O0NnLkjHcZxjTod-au8wSe': 396,
     '1WvmJ2K50lVupBdV8NDihwOabfqF9oTz': 397,
     '1XBQ-o8v2YHnJ4RzjzbkwlK6MYTdkawz': 398,
     '1XQdBeA3dgcGo79Q7BcsNX4nb7WtrV30': 399,
     '1Y10GrORCJp09R855KIEDMNNi9mM93Of': 400,
     '1YQoX0msz86oNuQm7GktS3r0bwTngnmE': 401,
     '1Z2jLqgMt2oPkCC-nDIoPsk6LItvLFVY': 402,
     '1ZgnLSzuuP5JZnM_Ccv_j3fVnuDaVi2U': 403,
     '1ZjA72_d_tFGEggXa9Q3KIk7jC6IzWCh': 404,
     '1Zw9IYLc_1dTPlThgdhjz_wpvsZvvrQM': 405,
     '1aRvFZ_x433FpXLyzCY-pxDAN3DmTyir': 406,
     '1aXpa4_iXlGZ0SC3ARONEEf3zPGwA8HY': 407,
     '1aijUXlIyIeC-qDJkIiz4LttS5_39fqs': 408,
     '1aj7uuV4ZRxGdoaVMdY0dpJ58ZeCSK9B': 409,
     '1anVuyXldQUTrsFmgHCD7L_IGIiTD9Nm': 410,
     '1bJ1R8cPBMomopERf8vK_WrS3mT-YYKz': 411,
     '1bRed8uTlCZAHvHm3vqcZc9GEdaKn2ht': 412,
     '1bSW6d7k9gKOK4K1rOxeoAUJx3o_Xyf8': 413,
     '1bZNv2012OVPN8FtxFMnqypwyVZD7DBC': 414,
     '1bvgRyI1X05R4t2WSrk5WlplTGoxdLbQ': 415,
     '1cDxUS2U8yKjmtXAA95BMehgCKW4lKBz': 416,
     '1cIJJkvsvJpsroO6v-A9JUwABvC7pgR6': 417,
     '1cLQRI-6caCbkqVHqp2HHAdvBt2TnobX': 418,
     '1cNiEz6Kd_JZWOvmN55JD7tGOpscbljl': 419,
     '1cSJdNSbLpGJegymzm8yT7jmJjz4Z2rX': 420,
     '1ckAhGAmbHPpzWUZQIqPtTLQAXDohnfJ': 421,
     '1cv67BXJdByOsAnhgK8KnzhhX17iDjzd': 422,
     '1dBeTGoClhinOrIwXA2ATL_3CLZfTcKy': 423,
     '1dEG0k_XvkuMTO-hTEc8YfGoAJoUsGw5': 424,
     '1e-j-oyqCiTZ0l261_paxr31kUDi2hc2': 425,
     '1eHqv0_f3wlGaezxnznWxMmktV3CX3gD': 426,
     '1f2DSllXtn-izegn_AGH9i6hFhQcmbZ8': 427,
     '1f8GcvWC5nogX58P0yRQBUZH0mQEMZ8Y': 428,
     '1f8cay9_mVE6kf2yq_9ImDy7UPpYwHOU': 429,
     '1feFupTY-OBxN89dhIJsra1MzKf78Ibj': 430,
     '1ftSQEKFEre0nH8-rr-gsrdC6EoWKWN4': 431,
     '1g44MQYprRumF9F7nVvGuZrPh2MLAKBL': 432,
     '1gRedqoFomYyPP551id0qbQH9Qesybgu': 433,
     '1hlnlle6X8caPWChTTqTv4qcPhV34wKl': 434,
     '1htAKqqU-G6Cqwhl1AQZdrcqm9i-xhn7': 435,
     '1iA0pr_QrCOKhjsc5fU3ejyyKEIpsMcE': 436,
     '1ii_9Nn_ux--ITv9abIW9cRPrhZmLNSk': 437,
     '1iruKpK-kBlKGyDXkbj9Chxbqp_LL5_6': 438,
     '1itdNpamXOtd8mi0q2ZjCEIvqx1N7xr6': 439,
     '1jdrNSJeVkzEltc_K7Mk2-IsrEyWfbur': 440,
     '1jf3uD0ZFVqE9iQYiYOO-VE6odm7OaDM': 441,
     '1kAS3sdh1P0gJ3U_DQmyJ0DqmaPSzUoG': 442,
     '1kE9OOfeZ7_F6OQeFUMWt_5-uq05Vizl': 443,
     '1kYCSEcDW_-nwTrBQPsCoUfXGHM8MvN6': 444,
     '1kqumLwCPfPRBTZJm0tEKVAzbeHtUPXN': 445,
     '1kt1VtiQM5FdJ-y3uQeQIjTT9q5_SZnG': 446,
     '1lZcLyrNae8pdIqX58G667YoVJ5Zbqpw': 447,
     '1la4E6OyV36DsnisYxeJMQJ1qc9g4p3z': 448,
     '1ltpqGcpXDCsBIfXqZVYYs1RoEJ2pNuM': 449,
     '1m1KoQlANgUhQ1cLTUHU6QbvfoCfYqDI': 450,
     '1mlBvhGsVy1MZCR_Cdy_-fFEO97lIWe9': 451,
     '1n47E4hpnfNyr6QJTEK8CmkFW0w5P5in': 452,
     '1nDNb65XLzS6z3iHDsN03UeOnzMNyiQx': 453,
     '1oI1E_inhroGuubv10Zxmiavustn-xma': 454,
     '1oxvx6QU3Fu-8FsPvaWYm_uTVeuEQcQj': 455,
     '1pA5JWMDttZTv37wV7iaFj9y79MSn6nE': 456,
     '1pKa7AahA287dRDVJoZ3M05rbVcpunM0': 457,
     '1qFzIIp9eUzXnZZL0ZH-pMAU0jwWashr': 458,
     '1rSpRFWvYdCxqttrl-2I4zjXLspTxCZ_': 459,
     '1s2IljhWFsisB7B3XT06G6pYlx2y0ijU': 460,
     '1sFJ-TanktdMCx4TrnXhZJFtKJPGLnpV': 461,
     '1sQ7u_Mnu59Z9jdS0zAZX_GgFLk3kXTJ': 462,
     '1sYHGf76K4vYBlUzN7rymJCNmrNf9fPk': 463,
     '1skzYa7AZfFS7smFkEjBhiw6X_c3iRjS': 464,
     '1svSF84pb8dJ0GHWhMjo10SpIbEvoIgG': 465,
     '1t61qKKjelgtm36MSBk_C_2rNs8nfZYI': 466,
     '1tMp-b0zm9iEuTHKcbfT1kLMe8hRMEUg': 467,
     '1tffPgyd8F6VAo1L6dcuKvmg76JWg4JY': 468,
     '1tpd9HwRSnfBRfVH4CvpCyJvnqGcBecI': 469,
     '1tyxzqtsqIjOw7x89shkTEIhJYfKUt9y': 470,
     '1uJDKqRfdhmuiCTX__v37Bjix4Cfw1gJ': 471,
     '1ujUd1Y_M_AgHP_ZMdg7mxNu3TmmjLeS': 472,
     '1upuGPVh_TF-bCVblux0jaf9YqGw0ihI': 473,
     '1vCU4q2AgWuScMV5RyH8SS8cswOFPUxz': 474,
     '1vieClDxMdBPSeLhZPj1rnr5dP8OATEg': 475,
     '1vqtN6J2VexAfykY9ayzbtSn-0CZjES2': 476,
     '1vtVnw2WCi0LSAbAuL5-8Tax24ogWCM7': 477,
     '1wGcB92Cw_GI9rIDVazzOPVoPJ_JeG-K': 478,
     '1yXRH_ONSgDxTptIVvrHt7QEJSauwh3X': 479,
     '1yaOztkceY3_RaCa_Usj4ju6jlYCWkie': 480,
     '1ycd5yWwd2XW98qjws_ci7bOMgXcSlcA': 481,
     '1zLIXtKrAx0c_CVhitmzpRBguBJbDKSP': 482,
     '1zXOhoedJcbb5BDCRgDfv4lNFXn4PeRv': 483,
     '1zXcV0MchLwux-27envVuBM0vSQZEANu': 484,
     '2-Mj5qJtC_hAbyhe_HPi9XScqJ8WcFIT': 485,
     '2-WZgciWGVInPQ-d11FHfsIApiZ6zsW0': 486,
     '21d3xGeFyRYR94vDx0mnaRdStmi-HTF_': 487,
     '21gQZTSokkiF5Q3JLIcbywYGbLPNsxdm': 488,
     '21odkmLfwvSoM6f1WzVF5Jy5Jx57pgK-': 489,
     '22NVuUJYPus1Qg8NR96muz13ZZUtz0-W': 490,
     '23uStLFgUsRXqLv9o_IxgH_zELB6bWdX': 491,
     '24uCYBvWTg5IGFvWiHb1LNlfXVHm5GxR': 492,
     '25OY31Z51hpksR1rsQSpAoArscDmk_eO': 493,
     '25fGrJaNXpkC8KFNG6RmnaRZXjkCG7K6': 494,
     '25fpUJtAqIv_lliHZNFvCoz0-pAGrhqM': 495,
     '264bPKxM4mbHcfDsRUia8KPKMflxcpoF': 496,
     '266M4mTwfBgTsxyMHw6qF_aFDJ20gcQh': 497,
     '27xcM228lHAI3XIOmoMbcgfujDdj6JFK': 498,
     '28E1m4kX2_weQHMm-D3sjcz4X-LDbg-H': 499,
     '28i8bwqceXDwV4RYNvLbtCSODe9MDB4O': 500,
     '28zTBqGuDcmPacyvUNRzHRN3emCa1pRy': 501,
     '292Fi2ccIYPOAp9ldEDb0lhLF0weGgxB': 502,
     '299hJLbX-1VHPQgPzjRoMdaEMhWmMVyc': 503,
     '29NM6JRlHEmA4dOnne7tLDgj4uKViN_k': 504,
     '29b6bz1eLgTq7oEMU_QjK8vUp1a57G1C': 505,
     '29bmJswm39lTCKm5-dqrSLoMOm5nNRnh': 506,
     '29wBqtPBkIUm2f4dY_eiK62Dqa9t4cHi': 507,
     '29xPZAacKpc_dOVhbRhQRjA7vgktsRXP': 508,
     '2A7C43DhTs0_HsA99uM3Udy2JV-iFs3a': 509,
     '2AFyNp0ZyH_cHbZRpW7Pm3l3uDrlVTpP': 510,
     '2AXZm5I7yNLRVjDPyR5VxGzxLL8ip1Uh': 511,
     '2AZlrEAdcyT6WBY5bzf5HTlfba5d-RBe': 512,
     '2BFqNazfbmcvz1ZEsMNmv1s3Uk88koSb': 513,
     '2CC97FzvDQQSFWdrSbsJ3_oWmhwz_Uf8': 514,
     '2EB-AwdM5WOLhHghDSpC7BQz19FPlUnC': 515,
     '2EGIJtn9LrHlyOvkYNxhY4Umo8RnoIXj': 516,
     '2ELWzGOLvrDaLeh8f95PNfpCqURa8xbG': 517,
     '2ExiBngP70__xW9v7X4HPr_6hxuiZcDj': 518,
     '2FctdBzG8mlIN3-TDipag_GW3l4o6cj3': 519,
     '2Fj-wtut6ib3GxItwIFRFVsJPWso7QKt': 520,
     '2G2HSRx6MJ0a5-Hil7BZxOnPJPYmX3lb': 521,
     '2GbSB47wNXtCYeY7jVJ9PYCoBo730Fw2': 522,
     '2GdCrYmp7IVnMbA7R3i5tt5mXCyWFw-r': 523,
     '2H-j8azYlCU-LAAOL7VZePPbU3t65Kln': 524,
     '2H5cAmytxgWUv2zMX_UMMceiNuPovHyd': 525,
     '2H9mwGqGh-0gn5NUdXUoS9taRdYHMJKr': 526,
     '2I3AnSEvEirZML-a1XhKQcKGU8rlDmru': 527,
     '2I9YYAtdqh6avO3ZlNZQ7Z-e_arvibhU': 528,
     '2IJVrPbZbQ0A3ob0UXjmHjZsl7ccuXjd': 529,
     '2Ioz7gi6uYgKZyry9iVbH_xd7KdxO_2u': 530,
     '2JBqQ_xvoIu_sL6oSZRJIaKcC4548UgH': 531,
     '2JIzmfXZwOgejGJtkQ3Y_pUPnTmKsuH2': 532,
     '2JK-C5xpf8lcXZLrUoEnEzrCBNwxh202': 533,
     '2JfJuOhSuVix0556r6eCu_VgXF-vTde9': 534,
     '2JiOmsyREeLf1KlAeyDIPWf-03uWHior': 535,
     '2KRPHX0XgibwxEGTOrMNXGZ4dKGGEn7H': 536,
     '2Kw4DHzKYH_SU2lRBILCFQ9KnmIGGCxH': 537,
     '2LryHAcV_frn9qIKtT7BsOpDLu5zJBaS': 538,
     '2M0nsWQK2CLbzipyQh8wlLb1P0kqAl4Z': 539,
     '2M7MBfPERGTMcefEI8789_Bo84PTj6FT': 540,
     '2MMk5U1d3cULX_xTUNMQfoMEX-aPOBfG': 541,
     '2MqT0iMIA-EKjFVlI0oyLnF6P9VtHsqi': 542,
     '2MtgHxSZ-CUi6BIiahW_1MUhUmy9h8cg': 543,
     '2Nr6AMsPzfv8mPmsadmoRDOW_kO_d0Fe': 544,
     '2NxQ3XRmdy32nAkg7ovE0c0hJ_NV1bdH': 545,
     '2OErx_5KYxVeJ0NNLpGJY42rLf7B5dXq': 546,
     '2OOxe7-9cfnE-2qStynzYHpNg2kVrURY': 547,
     '2OTRL4Vk7cvcnVUSO2ZFSfDvmIY1E834': 548,
     '2OnqVvr8NTnGm-KuR25AU1sJ-mC0O4C6': 549,
     '2PFbeRSsfeXxOwm02rLWlXBmU8mu3yCP': 550,
     '2PUIvK3CqLF_pHULYmRVBQreM58VtV6-': 551,
     '2QIG_Csz7oS_gb9vBM0AZwKgmXvNeAWi': 552,
     '2QXAECqExTyMajnU46SuUXl9OrIsONcb': 553,
     '2QaV2zjB4-of5tj1G-ZpIaJe_mWvkrOW': 554,
     '2R4ghmq5-U07M1cEmRfvzgey8qTJs0kp': 555,
     '2REA4LsVawbVf0dL2i5Qg8pH7UVmvf1F': 556,
     '2RHgUATllvsnw6bFMKts2BHDGFNVXpF7': 557,
     '2SDgCCJsgHBN4K6ni09rgtWvuNff5LM2': 558,
     '2SUHPhoMfJgdCByqaucka-aZIl2X-D09': 559,
     '2TC1iazRqLu_QuNnp12qMO565lpvGNk-': 560,
     '2TGjDwtrDsk3LkXrwLOmbsFQp7urFjzI': 561,
     '2TGtXoTC58l8lDPcHbQYgLeUoF_NKMXL': 562,
     '2UXzXPtwePTtqK8HvdjbAXeqtDwxX5Iz': 563,
     '2Uxv3lsyi7OOZmbUMda9vJUwOJkxPrIR': 564,
     '2UzMB-RWy6u7ZjkJQTDY3WeGeg3enfKu': 565,
     '2VMr0CPWDCYFD1fJvPVNlUxX0r1uj-e0': 566,
     '2VVVrT9ygtul0pI53z58umLWJCJ3475f': 567,
     '2Vs8sjAukjr5uzwbFzB3V565183w7ovT': 568,
     '2VuCVDTz_M-NzQC-FQlBOakekG91-TYJ': 569,
     '2VzMj2uPU-SXbs-0Zn8lZLY32PusfEnv': 570,
     '2W8mtYP2jJXkNUjSXLSX1pPwFFwS3hL3': 571,
     '2WeQWS5NFcvyHZkeIgeCxuRE32l2Na0e': 572,
     '2Wlubn7CNV7iITy8MOo6XLbwhyDkaOlg': 573,
     '2Y0hTCPTowUyGWZ2SLMzFPtbxtV5CDrP': 574,
     '2Y5loh8asrmXtYhHEu6VC7Cs_6R-IVnl': 575,
     '2YXVnV1GxU2fp25TL26ZYmGVvaU3BP0L': 576,
     '2YfiPJVIhdnNQOirxLyKhQBBj76MGELH': 577,
     '2ZSLHCSWR9F2fC9x-KHWry9cXSukSV95': 578,
     '2ZYSZpfKxp9IdlSOGiptNVAC6CmGIPNK': 579,
     '2ZpXG8kvBU0qCqrAIwTRKccDUkJlHuuD': 580,
     '2_JF--K2bp4W-gx7Bnttgi5LLqJWihML': 581,
     '2_KDqwjd93fch60jpkCdLNwwOtDbdpyg': 582,
     '2_qoyEhX8Y4qaxI2CYC_73rbocB2QdXy': 583,
     '2_yUwNH7bRZMBHRL4gy-e3SIJnaJ7bO7': 584,
     '2a2UB8-haR0iaIwOy5F69nbeb5mhJrD7': 585,
     '2aK-rPd-fC40QlV0yusu55GpUphqqB7y': 586,
     '2ahVwk0WHFlGBzUxzvEMGqThn5wdP1Wd': 587,
     '2bhK1rYQONmkKiL376U4dVdyT-dUHTLL': 588,
     '2c1Lmy2yaxWQeDw32OvwbL6zJWdsx6x4': 589,
     '2dfQ1jFUyyyBfzW4S6Yi4ERFMdo-Ngq3': 590,
     '2dkaK7LvUgo2IJsx4ChvEDst2PJaGQTb': 591,
     '2ea2af65-9jDtBIh3JECCxI662fFZ1qa': 592,
     '2eb5qysY4MRSlB4wy9N6ZsfNffmvyoKY': 593,
     '2enQoMRP2kb3o1-SLnWB-phfvA8DmyD5': 594,
     '2fQ0rTt3cyqRm9wl_mPO_KvddIp_XBdr': 595,
     '2fSp8nmBID6t3RnmCockQTLi407GfDGG': 596,
     '2favKmuB3TLTE_1BkLja9dXl-xD-9yeC': 597,
     '2gIV2EwclvGNbVMuuW9RX2DidcCyvJ0U': 598,
     '2gN0DmFaz3BGu0cdEUfMINkM7ZsVqVhG': 599,
     '2gYosQ5_VBzIpLHT7ZmWNloB57_MosB6': 600,
     '2gdgivtU9VWV28dVpD-5Sjn_1i14D36V': 601,
     '2hE74HZImMQXRiF27d9vMPOyEYdrTXqI': 602,
     '2hFlOG0cSG9ykl5MbRa_xvUUyeuHxP8_': 603,
     '2hHSvS7nDc7JIRDZuYbENfTh_qBOIpum': 604,
     '2hi3PvtPcUjlMPCs7RxG0IQ_kFPev_uL': 605,
     '2iUmmQu8Y1736ji_r2n3R0uKXvt2l1T_': 606,
     '2iWVemdgaEpfFFmCTd6RUDO2jPbfXLn3': 607,
     '2iq5de96l9r-ItqEVzsghZPiUWWxcg9a': 608,
     '2jEUGPzofqTCF1NWsKgeB4UJn69ycPH-': 609,
     '2kmmdwoQXrHv0Jh4Mg-wQySTA1E_mRNj': 610,
     '2kuPZouZ8Ov5N-4MSTcF2pcU5HtXaaHh': 611,
     '2lBLLfhnWrRAoLc-7vCaUKQHf99FCPLY': 612,
     '2lQgKJA00NWx_MIugn85gE4opJKo0P_q': 613,
     '2lUgcVYWqZX_iSxyEwdx451Lfz7sN0FC': 614,
     '2ldLYl9Qs1czIeVpwl9ssKRIepCWL0F6': 615,
     '2ljQt40V1O2NWXv0sy1DJyyVOm-qMofl': 616,
     '2luwce2q-sVdmJGAcjuGF5HVCnv0Fl4h': 617,
     '2m1CSRFfIjPCY0p6GwuJTuQ3rojkAxbH': 618,
     '2mZCQNY2YSMPULzXNe6A2cBD36TtnQ9-': 619,
     '2nP3DqdT40-TdoWfXJUoelg_PrXgZTjS': 620,
     '2o5-TTPIOe6ogyf4YuoRyRotY29-Ummn': 621,
     '2orWV9cLujBLSdKLg68-oZkWXu7fE6Q6': 622,
     '2ouPGDAm6CYGD68YTkaZXm53ES-55E2G': 623,
     '2oxNxEGKiEKQCm76hu_9V4BW1IE3rYYD': 624,
     '2pFG7Gl6XzZ0H4jU9EdEh5kwA14xR8E8': 625,
     '2pkwTo5_DQZFqK04TFdvgLOddG95xruG': 626,
     '2pomFjuSpbZU5NW6aKiosPR8gwemOKZY': 627,
     '2pufrxUcaS3NBfNofd1xGREVwRJwWJ01': 628,
     '2qWFM9hUZ1zLT07ZsPmk4rvvAYcQvQuf': 629,
     '2qbHSofNWZwaDs9tzFAH7drK5afqlGqF': 630,
     '2qxVWoFe0VnIqTNANButMUYuMKebNMEh': 631,
     '2r8mYDfL1MZu4zp2ZFEmOC9WHDaEYses': 632,
     '2rmJ2nm8GzL5XB8Rf594DIcu1LpSKOUf': 633,
     '2rxVwFjqHwOqMAbqwwI2BVgQ7fbnk2vZ': 634,
     '2tYPcKfoUsoROKxUwFZrAjDrmjuoWivx': 635,
     '2tdWjdAzZ-WNBnnzMCsHGiaPtOZifBDH': 636,
     '2tjF3yHPG8JSuzlBcH2Xyux9URR7gioo': 637,
     '2tqCbmKZtkXiZcF02I4mQV-t-Ul5Yna7': 638,
     '2vEgWEYekOD30tlh33DBNwb3g_jWsTkb': 639,
     '2viJZBbDu6HY_wtPpiFqZ3P550h47Ej_': 640,
     '2vrTCA_UgqQUlcXKoTwSlvw70PYfswFa': 641,
     '2w3VVowSzWReKKVgghC6SiiACTOlDhrv': 642,
     '2w4GtDaxdU61SGCRRBzVKvc-_CPmj7rY': 643,
     '2w8rMMYEwX15FCvCSq-sCgxP1wBz6ADn': 644,
     '2wtWQ8KRCLrJOJKIeKSuQtFFDajlIq4g': 645,
     '2x7pd7gmbExGtOAKA7R3ZI5ERxqaU3o1': 646,
     '2x8cobwQWg8aMoXQ_w0FydK4NMaM_TNT': 647,
     '2y31YniV8lFkOg8j9kcGEe58W1HL42uT': 648,
     '2yIF5RMXrr2rhTPo77YOUcX1-nW3DCPp': 649,
     '2yoE_ceHn4fZdo4Btojp2C8x4soikMwx': 650,
     '2yu9_VBUc6LBHzhPSQnunXg_lgtDan9g': 651,
     '2zcR-ykdM128rEhBRIqKG_rXscM4sPL4': 652,
     '3-BHm7Qsxv_j5QbmS_6IytvAkzLNLCmo': 653,
     '3-jcMi9X-TugvI5JPCMatCy0ApL9GKFt': 654,
     '3-w2NggoP7AxnkRaKmQ94XWobjbRV8_F': 655,
     '316TRyGB0N4gUii2dxIsUat1TftKWEpE': 656,
     '31DCyqwnJrTq27KA2ZFU833Aabe1Bm2M': 657,
     '31DFBHjxHj9et3f0J9c294OdVDSwOnvP': 658,
     '32-IQL8Q_4qwebVR9HmUbs5gl2hKv3kw': 659,
     '32dth4wVVKpeBOvikQinyFfKc5UwcMA4': 660,
     '32xHxoa4WZTZPA9K9M8KLCSSrvbnkRw8': 661,
     '33CVrEnmILJAdpH29TAOSIlw5lLs5Nn9': 662,
     '33s5lsr7Q9Ix9sV-xxv4jnC7kG4yIlaS': 663,
     '34F5UccouPRMKTn5omdSUGGJWA3A6bwA': 664,
     '34M4AbuZ3QE3YxPb9YN0Tyo4KJlKjDV6': 665,
     '352YlQC72eian835oYihpJPW8VLoWcLk': 666,
     '35F0U1b9b0GzGB9Y9n0lWLlAoKaYbSWb': 667,
     '35R3TUjOBqRkYNIRKYbOdyc2Khp6m3zw': 668,
     '35bmKe7pBhzlJeUZuk4FlvxNC9mwhtX3': 669,
     '361BbLGl2CMLrXQdCtAidSx9TVBaajEM': 670,
     '36RiZ2e7bDsh7qawrng1GlqdqoOwmfRP': 671,
     '36juXs9XAQYhY88n5ZuduRdx-MLtfc1O': 672,
     '371vunWHQ3nMh2byzu--4mvvHVwIGVKj': 673,
     '372e0sBx6sXsQ2xk-bSlaFsg3FyKJdQx': 674,
     '37Xml_-NJW3Tuq_JVuOCP5GrA7u0M_eU': 675,
     '37hQiBsELk7x5g2wAPXRZm-LNYJUE3pF': 676,
     '37wHri0d6UhHI_5LpZTeLDgxIFl2a1yD': 677,
     '38aTKFXVaQ1PYIXa1iUvCsxHuUeqw0m_': 678,
     '38b1Spz-mgWoI8m4UhsYf0eDfUaDPkmO': 679,
     '39M26oMzfZ9Ri7slPZ9pR9GZpqZxUDtU': 680,
     '39p3YEg8a1lcpEiFpxbiEHzPJRqRKJWM': 681,
     '3AB_C0gkyMJ7WnpuSzeeLsPmeTBtAidW': 682,
     '3BzX1B03T8JOF59LFARoaeKYTXbq_0HB': 683,
     '3CPa-xat3t70sYLQSCRl-h5PpK3SDtmS': 684,
     '3DBE2emw_yVx0qiHt3fBsuO7koD9zTdR': 685,
     '3EnU_oBgFEmpPjZdbiNliZu2cmIvzqXl': 686,
     '3GFCAZr8KsUy1vyfLQ2boJaSknP-K_88': 687,
     '3GMDpRkDIIqcpgMRXkayojQS5mZU-5Dd': 688,
     '3H35JLJDv9FwaMaTcfAaikuJ8JM51g6u': 689,
     '3Hvxi52Z_gsUoxGBL7t-DOH1zdlDed5s': 690,
     '3J88s73PvWGtKMze3NGp-aRgxkFSjVdQ': 691,
     '3J9-leIj3z6aoHUqkyGyyv9SXzXZDiJA': 692,
     '3L6oKHFP6GnbDTMMEE4P3tiTSKCDuDNb': 693,
     '3LGSst6B7iam6ua5uoPG_D43HkkIGY0v': 694,
     '3LffzcWX3Za2XP6ukSgRXpoMEc4NjNWF': 695,
     '3M2YxbbPrRjUUR5pfDECU4wqVv8Hbttb': 696,
     '3NgErGjxiT-PrbBOmxVPK5Ymvka0bXFi': 697,
     '3Nw8kTCy5JnEmWqJNZuav493mo7NT-PF': 698,
     '3O0kKszroAIQiiti_rAvhysjMv20MgQ4': 699,
     '3OVSzuRPnQa517I79gpAdaDL-Bnu0ZgD': 700,
     '3PaGN_YNmd-s3Tc-kVQ_gmnz1qJfAUHl': 701,
     '3Q0d12url4VzO7IxjwPK0OoWDKQd5GnH': 702,
     '3Q1kVD94ZqyPZ4u0KM2twUKHTtCln9X6': 703,
     '3Qu6qWI3mFexpzbBxn_J5CDGz35k64nW': 704,
     '3RXZB5WjHD3amDdDK66AjQa9ECiCxY92': 705,
     '3R_4GV5KOCoarNCGAzSXgpMVjekTYMkT': 706,
     '3Rd_uagxkHDztjgtQ8pU-DeOyGcdnDmV': 707,
     '3T4FRN6-LkJFjSVE15AMkSkmiSl5fKWU': 708,
     '3T6SC7kHyQb_ILUr2wuYJPc613L0Gwor': 709,
     '3T8VgqPH8lrsTgg-u5rFVQ84VuJpO8zf': 710,
     '3TOEvIQ7xFc6YBf9dFLUq7tq5xCbERMz': 711,
     '3UE6lkTPPZV4wxA7vTJT14B7bzauNQ-c': 712,
     '3UyfxlJNKpr--Hq5nZaYG1Y-qNJE0Y76': 713,
     '3VJUKZtHaarZVvBY7gTOeX8Amd6wz4--': 714,
     '3VPycn4-dAI1dWHGHpYpMAwlePldxdNi': 715,
     '3Vo9NP0qU_176pgbqk6Cu-CY7kpJ2-WB': 716,
     '3W-rYXJxKPrTPczKPbKgXiJCbC7Xgt_N': 717,
     '3W5YLccdAzcdrAGU1_unkngnq8qZiQlD': 718,
     '3W7qP4zf89YXHwkiY7AHujibneg0lRjt': 719,
     '3W9R6BtTpDwz4O3sHCGHw3QXiEHfQity': 720,
     '3WSe9LFJSINJHQQKUEltApbt5F20rtDb': 721,
     '3W__YolXb87BhzqosWyJczgoeRyPv8BA': 722,
     '3Xm8Y8UrBAmeMj_ZI96LcyykZaoj5Vri': 723,
     '3YkQkXI0wuv2Bxem3ICNWU7HCxpogvsk': 724,
     '3ZgmyImLc4IDgqlk-gyRz2pcTCtrZs_r': 725,
     '3ZpuX9NlIP9JqEmVz_IR_7xm0qSz9zQz': 726,
     '3_A8lFgxrTsBnj8P8YR7tPY7_yTXkJrt': 727,
     '3_IMM_g250HQWsDOXnlgjP_cY9mzJUtX': 728,
     '3_gu5K88nuLfw1lzTkkuGugPHEd37wYA': 729,
     '3_tEnHCfJV2PII55sh5B3nmkzuZtahRF': 730,
     '3a_0HrRwwRjt3jcXXl2H3UZTLWM4DOy_': 731,
     '3b8Zo_g7sLWXj3LyJ4fS6DgWS16ERx44': 732,
     '3c2CeiDThxLymOoy0xeu7LKlLf5l28LX': 733,
     '3cOwMHUWT32LKeYzehnmpG48CEny5uRz': 734,
     '3dodNXUor8NvetwHL6vySIjDhfGE7vXn': 735,
     '3f1F8S_XEv0TO1FSh8I2aFK4LKYCE3fe': 736,
     '3fUfX6lL73972EwX79drMHvvFrnWNMcc': 737,
     '3fZ8geGvtIE8mAGBFcOV3_IbC0M7EAzl': 738,
     '3fq3zjmANg_F9vxKMgLBdqphejeVwdgC': 739,
     '3gEMrWgy-4lu8oaW_HLOx_KP18B4PzVZ': 740,
     '3gUxZJ_KOSrv3yCax6Xitc43vSG-AqtN': 741,
     '3g_ZHBajeE5KwRMRH-puiptkr0NAKoi3': 742,
     '3hG1b8nUkp74cZ3wUpsIH9qtFeB7u67I': 743,
     '3hGicNFMVSSWDDezaDWXqxSUwP6J9FnW': 744,
     '3hjQ-jt6bpoSCXan_s2KwdOmjLmTQLhr': 745,
     '3iW3Di79SqENzSg9sT-79tjNeGgEfvBE': 746,
     '3j6Ak3rtgM-nZnF4rYHnjoPhKV-EpleH': 747,
     '3jTjkgfkaZJx17_FTq3KO-lFidV2EcTk': 748,
     '3kfCGwUvHInXf36I5EFZAjcp_JGZd8dD': 749,
     '3ktPVxdkkfvPxXT7KrpJ7ut2APwWmn4u': 750,
     '3lFYyNo__o-utiLDmby3mZ00SfcVCJwV': 751,
     '3loXwsb-wc-mR7ili3a3B8EZWLWvvFKF': 752,
     '3nP4RUw4sZiF2d1WlaqHnv7QsnvK73-l': 753,
     '3nc8Djl0rBHGAEPlFGtkyHLgXTwL_8SX': 754,
     '3ntrLPHn8Xu0IsVQ1z00T2g1FnaJG6jf': 755,
     '3oGkieXgdcyJFEBmNqvlaAqPn5nrzcbt': 756,
     '3oodir1dwph3mJipgwJzs8PiV0ziJBcg': 757,
     '3pqMiLsGQ7juDnCzhxw-jMO7UDcP4ZhC': 758,
     '3qciWxd4O1k219qIsSpvIBcVQe0O4w7g': 759,
     '3rG0EKehjCLcyFdoXHa9ln4X4TTfsDOo': 760,
     '3rzk_Zb93hHxE86bG-sxlfovl6SQtTz-': 761,
     '3s3x0UtTcSGNmBqwy97QNSBFw48GM2KB': 762,
     '3s4DZXiZwmXlV-52Y6-AwZOX88-kMHPI': 763,
     '3s6b2IuNRNScyEiNb2Bck1smA3URxC1j': 764,
     '3sxn1odNdJVQIQYt_O2Ygm4KRxVuME7h': 765,
     '3v81OtXvwH1B8oMaKMGjKlBh1k301M42': 766,
     '3vgd0qkhIGG-ToibESLfVCPfNBLjhVnF': 767,
     '3vspF5hfYJFs7Cs_C9p32fWsg_tgPJBm': 768,
     '3vvFty7e4E3kYopxslUqoyA4yDkKQXkg': 769,
     '3wMRUYhhEiX_96V4QZ4hAZaEq0jm835x': 770,
     '3wtiTl-L_dBoerMnuISR7L5Q9Xf8Q0pa': 771,
     '3wwWTYOrvkZ2FGHWdlX5RVuFJDOxwOGo': 772,
     '3x-tIETNPoGD2XvkzSx7wYgQkptZXAmW': 773,
     '3yBJYtWhXCO4ROSsGp5ACT7ZdN5lS-GL': 774,
     '3yby3wMAAKFnwYi-C2I3k0ufxRuUHdht': 775,
     '3ypKKB64CBEAuiwUm4oeCtR9EY9Eyypw': 776,
     '3ysSTp_xwu0yT25Ku1ka0zaT4zLv8xbS': 777,
     '3z-shimOO1skpclrkA9gyx1AV3-MAn15': 778,
     '3zBH7k9Y-3spUaJi1xY9tK6HvL8Ye_at': 779,
     '4-9aTr7FNWVfE4--8b-x4xBNLdke8bQl': 780,
     '4-V2QJQy21qkQ2hS8RM390lX5jq0Yb2j': 781,
     '4-eAo4BJ5euk_kbJEvZTgNZq1tMoVOru': 782,
     '406QLnJ_jtj8Y3e5FLRtavScRWivR5Aa': 783,
     '40UCgZgOsQnTIoj-1HtvVZBvd-UiNK33': 784,
     '40jdGtb3w3sPr0JkgX_zMqNKii4kvo8m': 785,
     '411WQJm9kG89-PwVLid1cKMoyfeBR4CL': 786,
     '41G373B8slNieX38YyjIvHXYhrwz06Nq': 787,
     '41RhApWr22-mwo6QUIoWenKHQ8qkDFuN': 788,
     '41gAzCmN4A_EvseecpqRtEBznlxGeqMN': 789,
     '41jLpf_LoZ-ALU19HtAKQXytM2pF_l1t': 790,
     '41xSKhntNnqgYlIfXrd5t0sj642h9hdB': 791,
     '42nJzdAmAd_PovRI2OZcunWLfs6VWyUI': 792,
     '43ExCgywmeoDQiGQD_sqZkYhSmqDKRSn': 793,
     '43ZC89EOSHJ5ofjS5FbzYCCidhvkclrU': 794,
     '4410ZbvChIi9KX5MsoEbsmqtJxrxETYD': 795,
     '44291OEfL34muF6cL-t4qJospOkTbeht': 796,
     '44zvseG9fhDvBdogQQDVAwfesSBg8cTT': 797,
     '45EbYhshOB6X7SuZW2PbKX553gDTt0wJ': 798,
     '45TmDQuhC0bjgTIsSStEj6ttHdgF_jOr': 799,
     '45sggATS-5O3dT4TmIgHw232Gsj9BXsP': 800,
     '46auWJOjQBU7ip_5jYUK0GGjXXDrGYfn': 801,
     '4706gg7MI0NDzeY6J9T9wr4l6FA0URKX': 802,
     '477026M-WwBGX6H7R7uAy9jYiNPSyKyU': 803,
     '47BUyMUoiWRREVbUYTUwcrqYK65hr3sh': 804,
     '47Zsi7hdYWc0mkC8c-8fNJbjRm4TFuVA': 805,
     '47g3zrBWMIU0YfTEE5tck6ssCgN-Rca4': 806,
     '47gaY9jckiZr7XvGh_vYbNI2R867PZwo': 807,
     '485Je17jo7HjucnukyxdOkXxVCVBY6BQ': 808,
     '486yS1MK7xd3KLAQkgTJeusUlFbumoyb': 809,
     '489HH9DWdnmZqy_TLs8kvigvyxax0rld': 810,
     '48ZzjnZCUG3tum9h6Yz57iubRvcT8taQ': 811,
     '49JWS9E7KuSnvv1B7uqMnsiT82ZmtRlO': 812,
     '49NWlNgyXEbVxExxD_v92qTImx45RKjt': 813,
     '49ZNRunJz2CinBpJLEgX0AQzldgXez4c': 814,
     '49lX2Tkx_OzK1LY7cepz0kqXUDV1y2Yy': 815,
     '49sNrj7V3OO2hh90a_Z6R5voQiG-H2Ha': 816,
     '4Ac-6oopEzUPxCOl-BOUe8nHjrUI3wt0': 817,
     '4BIazM5UOqam4tIDbAJHXqjAsZEb7Q_5': 818,
     '4DK_28SteVujsg--lDvYSOyVl3WlrUHn': 819,
     '4DTQ9-W7ccSJjV7pvkjB8dexMg5cA4zQ': 820,
     '4DX7s9Jnv7uLmTU0AlWLsQhoa0JBb4pA': 821,
     '4D_m1AKzT03busl3SDou2PVVAo82fYgp': 822,
     '4D_rdRYXzIWNGIguAbgnYoXGMRTiWWch': 823,
     '4EIkvOcQAR--nhBRSpeCK2BOgLwHUcIJ': 824,
     '4EKN_ISnJHTTasizExhENltDvKuhOQ51': 825,
     '4EQyfCc5-mow_U212x-IPwq341cc5afx': 826,
     '4ETqZkMHxAXmD2ZzaZXjjmVtPsv6Sbq2': 827,
     '4FVYWrii59MME-1803XxvgI2n1eduyrp': 828,
     '4FXjrlMpVAz6bumFBfsufywpJgCcpwvE': 829,
     '4G0oWBgzbOfgzVE9E7qrKHeLZBPGj4_q': 830,
     '4GW7RbedBpz9OracRhs2AxlRD0jpnmu4': 831,
     '4GqwIngq5--ERXc3NdXDK44fe_UUmlQD': 832,
     '4GrQDVKFuBwFaqNr4VXrSWqaQiWCNjWR': 833,
     '4H0T8PMefCG17EOGR12gRWrQ74Iqe2XG': 834,
     '4H432QIngXL5nhtDcDwqYdNKynbXv-dZ': 835,
     '4HbrpffP0EdKro4EoR0vQINl73taIOXR': 836,
     '4J-s_U9as7_B1qPwthRJKNTWAztt8rSE': 837,
     '4JZfHSGAa50K2jfJ-Z0f07HzjcZxo9TG': 838,
     '4K2IZxhqGtKjw5QuS3KUHaztFOS--8kW': 839,
     '4KALSbzAOjKiGEwvcMJwLeH5k1eVfwMa': 840,
     '4L4Kb5iLFYt2-ZwRbjpwIIDq2KrTawQ0': 841,
     '4MXyfHYDV5OO-j40vluEUrrWL1pb7rus': 842,
     '4Mfke4gSYVsrCuzRtPQnUd4QU6jU8sNs': 843,
     '4MhZS_r8TatbtD2ehVobUwcJHPHsBWDi': 844,
     '4N3rulgOoee_JAsy44eKeizeiF1L23q5': 845,
     '4NUdMNYzHaeVX4nixYGv5j9vHPgzaVqc': 846,
     '4NhRKZOH6DVCBEeXZJfy30evRbhUsof8': 847,
     '4O5iLrDE9hpz45LO-MQ2STT1BKwsbfWn': 848,
     '4P6mpYdZR3RwHSz2Z5PCK4J_bcwOF_HU': 849,
     '4Q8B7XBnR1DJYny3x1iXOK63AvewYWOh': 850,
     '4R-pR78UU17lcW6mMoUfjBkKVpoRAfBa': 851,
     '4RQTO7CQcCfdfDuGUca0gRo0jIFTsN_6': 852,
     '4RQx92741iUZfCPCWkeetO208lRb_Gyb': 853,
     '4RR1vbngnybZjd5hjz6V09cd-fibje5F': 854,
     '4Rls-AL03umGHf8-D26FS3ONsJtD3PYi': 855,
     '4RvyPujjyDQmzMps3IrBC4m-Zr1L2uqW': 856,
     '4T7XHqft6zF9pcETZnOBc5XehLQ6mDZ8': 857,
     '4T91pid_BvqpJJVk6hrGw8C7BvhaJLAJ': 858,
     '4T95OfGup6ZLZn5rViQSTUeS7JI8HNQw': 859,
     '4T9buseRYH9VBwK6tDS_Z3xZN7sYk1ik': 860,
     '4UXinMnG51liZqI9CK01s3bLFx_r95Di': 861,
     '4Umtzmfvc2J34sKhezWRGoQ1rQxVpIUi': 862,
     '4UpIZ3aJcQonB-UYIcdYjsvbVWFBezw2': 863,
     '4Uz9teUhN4tZrmzd9AD25S3YpdljIN7L': 864,
     '4V93J414hLyJOqTUK1fxLOaXa8vAo5lo': 865,
     '4Von9wJS4JJ9HrIHgs8C3dnazulK2jZp': 866,
     '4WjShZUBaJm6lWQF9Cnzp6PVfZz2c2EH': 867,
     '4WwnZaRbD5EyAr8U8ILTI-d7QZ-Mfrka': 868,
     '4X9XjHD6DAuF2uTvw4yssa-aIQzSbKh7': 869,
     '4XCC-OR3FNun2agRRQ2q19Xs1h7IGLHs': 870,
     '4Xy7E07nLANaW6HRlqdPdsboV4GKcmAl': 871,
     '4YIJBxdUpYXqIs_4YSKFp2gRCSGkm9Om': 872,
     '4YSIXrnQjqYjTgDBnQWT_S7cB-NkMT0-': 873,
     '4YiYgw9bY1OQzQb3MX5aBTgrZCq0H7Z-': 874,
     '4YlmRBKM7ZZfaLqT_KfLiCQcXXFUHb4b': 875,
     '4ZfCT4b_tVpo5KLi7j2bS4lT9oG9ePvj': 876,
     '4ZzqZO8UeDbtSETYM7WFwuwjEZONI_Xt': 877,
     '4_1LWJ4QkXSzsfTuznPKfBg8X2G8_VO6': 878,
     '4_VLhHoSA7ntO1WhnJAfyIkCjMUTEvSc': 879,
     '4aSys6KOMGiSfqpbQ3s7xMYP890owDcL': 880,
     '4brfUkxRTHLzbKVWip6Q_w4tXVIuBYng': 881,
     '4c3lDZh1c5LI09HhZQCSXDlbWNWqUrl0': 882,
     '4cVMkHoqFwu0taFXSzbHG9ud05zQYbmX': 883,
     '4dIAPjIFU5FJUP15v4RJY-95gtDKCo7v': 884,
     '4dJdE0l6aVjfxpfiUjdfMPUNiA_EU2h2': 885,
     '4dSMHqbsJn9CElpXms9r7FZwuyW6ssyd': 886,
     '4eS7iyQhUqa17v0cFsUvCWTA-dPyN7-n': 887,
     '4eV1w1ONY7N6PfEETef9a5Ic5SLZoeGX': 888,
     '4enQvE2kUbKYV70_O_Ul6Dd5DaG9TmAN': 889,
     '4fPdsDyCWs27EfBGMhN5roiKFPrRK6VH': 890,
     '4fcT5XmJdG6Xp8N112jeBX970MCruFHn': 891,
     '4g2p-C8OF1nKwIJuNLPmVg-Uwmfu5tDT': 892,
     '4hQnv-dNj-NtnnhyPHQw1z2fi9pstyhX': 893,
     '4iGSV3mGmlUF9v-LxYQDMpCcN4NYPCDQ': 894,
     '4jtvevgAk-iHvq5zZhgJYcjAJKrQ5lVg': 895,
     '4jvauHnKo6_j7OpGaj0DOwpe3dNd-EtA': 896,
     '4keRojyj0zJcchjR6u-ZmQvdpRkWKnod': 897,
     '4lBVgtvAv9YQvFL7-WaQae8A4oriVPLQ': 898,
     '4mAlLixhUN2om4-wa3XDTKM9OYofNDaK': 899,
     '4mFTk0wk3AkEdUlvM7ZI_aHxeibRqE8-': 900,
     '4mn16q5dJemBuA4JdHVWIoFDbR6qLoGE': 901,
     '4msp4tay4ftI4L5byUoJS9YGnRZw-u6m': 902,
     '4n7hN6ydVRSukiwXBmcYyno4bL9OiLE_': 903,
     '4nd8eMyLU0zxrbPo1oe7WYdt_v6VPY6-': 904,
     '4nqg988GFxQp4LLbW5onMflj54WlJvra': 905,
     '4oVj6y0dwROdfyFzoA5M2t9S4YjDQ5F8': 906,
     '4ob9y7xOPZrNFobZJurSWzDZkAbJKp-3': 907,
     '4orSn6XfWeHsnaMfqds6x5DsaSXU1BBq': 908,
     '4ouNdQMW47T5j8fCKnHd7OZSJHrmWnrb': 909,
     '4p6NoLm-aQfp0UeFiEzR9ALrrqkdoehd': 910,
     '4pQFFwITd_fgus8vMt0nshxCeWOyRlyl': 911,
     '4qRNG7_zqvIQ08dQVNRuNdRmeQUEhDf8': 912,
     '4qscDhyNbUEUcNJJR6-UqBzLUfvX98wr': 913,
     '4rPsfkmZ718_-SUdXMZvtsjDeU8F5Cwh': 914,
     '4rarA0c5f0yv4H9viqzcbakoBqzJxL3G': 915,
     '4s3zbVPU1Ccy3VuTKMemyGbUBB6QpfmK': 916,
     '4sUXzRaMk_86WF_uPiLinQnILSjztlbk': 917,
     '4szxQc-VF4RyfmamkhQlANd0vFCiW2TA': 918,
     '4tJTRLZCH8jdin0Y2ZrTidpelFdNk3ZA': 919,
     '4tPLsLE7Mtkco7v6EK2ofc4Zcsqg3joF': 920,
     '4tVqWHkxOLpFuBvmjm29nbxGkgzeNpJn': 921,
     '4u3hslcMKVTuRa29LqQExPGPJL1SmFD0': 922,
     '4u3okc8uvwi6wdUJzbQtEmMFRFjCJwxf': 923,
     '4uzbfZ1IcKLg5P2weKyjM6HKDe6VXRjy': 924,
     '4vTm4XXm6uTq5iRKbjDT5fSBEn7ujPM0': 925,
     '4vjmCaFj17JYxKxLOCcGF2ms1EB8iFtm': 926,
     '4wXI6Hj7OeiarwANmTrDS3339DEJWwd6': 927,
     '4x4MeTEpmYuyUPRTFeVkvsGtW71DaAWX': 928,
     '4xlN7cPvaYk8YmFTZJyE5UlnB6MR6exb': 929,
     '4yNyJhQnTOFubOj69U5DyQqjxV7PcZpF': 930,
     '4zB0armWSKlrCz7_2KpiQFq37Or0D14o': 931,
     '4zEC2VzsSUrpr6QH-aeGcLza2_CEwNdk': 932,
     '5-FpnyPWSH6EXeLmI_wjovtPCJn3bM6e': 933,
     '5-YSXnfqfR6c4BzKp7uHFpsgp7laAAEU': 934,
     '50JllO7U2pwhEQgNCme5TxaINqIOfWWK': 935,
     '50_dgYsSYnTeKdYc4Sy0D5qo6UxvM4Jv': 936,
     '511zFStHNuPlWv5yx-4-0ToPmHOI-DA8': 937,
     '51331WZvvqbvKJ2d8pKrUldH1V1CDDA8': 938,
     '51AJgn1VmCb_KtFs0-JoMNi32X7z3ue2': 939,
     '51VkvvbjdcOwwaCVT5HKuUL1Uyb83_ED': 940,
     '51a9_iBXNLujPshssUN1Lv5dQ6ZR3rwU': 941,
     '51cggs-aKxLP24D655IX2BjzA_8NvjP8': 942,
     '51oPQSdGNKGFa4XTtO_LHimY3CVOoH1N': 943,
     '53IXlwt_bDzeZ5rp7Uycf7yXN9KDqRVc': 944,
     '53ZeorDUP_zEbxtnDiMCLf4Jdn-9A1dM': 945,
     '53lpINI37q982LFHKl6E7HgvRf8i0eBx': 946,
     '53rF94jhIooroRf7h7rz3Oo-9X9Fbvv6': 947,
     '54bVWbFzC3pbj9qYJvyXC4dc63zF_5JJ': 948,
     '54qtceLF9yYnRP83_W58h9X0ankIBUk6': 949,
     '55l8n4HWKEd4ZYM3TFwkqu1CLqDoct0j': 950,
     '56EU0TVyi8UorpOFhJmTLseCA8TsCmrm': 951,
     '56Gw1PWzOqLRQBNvcsRQcRZ27NS0CRqu': 952,
     '56e47a3fKvBURcnj_zx4H9k2yysZxcbY': 953,
     '57AdF8Pg0f9Gj20DYItyJ_0H0TdHQKvr': 954,
     '57KScQlbv8eUq3ckpgA-rthyOQ63bKl-': 955,
     '57_l338OuPuWPvyagNZ98A2GQ_EGUMNJ': 956,
     '57cohfYdQB5XWa5xwj62CcqQw-izhfg1': 957,
     '58elvaD1Pg0lFC5Zgjng7dPtHfGM_jkP': 958,
     '59Itqhq-72tvCYVsPXC1INALPCCQOvu1': 959,
     '59a7r1bxIMRypvOPIwzetvXsaCgH6nhZ': 960,
     '59a7vrM26nCNWj3SYBTK3h9EgbW_TX_l': 961,
     '59lr29fu8SFzIL3DOOzEeMpsvAYRZ7Zq': 962,
     '59o7LehMGOfL0cGEvDERUaim9zYWR6mN': 963,
     '59p_2YUIYlDGEMghymRBJaue0MRh-_aI': 964,
     '59t7MYv9D0lYO3OwL1ZnTEkffNEu4p5o': 965,
     '5A9mcgtTvHdndhHHQbn4eFiEHLO2Vi1L': 966,
     '5ATcNS9tsSrlEvloWZjGf4sS1gda2bsw': 967,
     '5BDR0qbmRNOXeWtn8YqyN8TbRYaZMptE': 968,
     '5BcrYUl9iTrWRYpI-mEEjV4Ls5NS_x_v': 969,
     '5BdGEHdJcjVujQLaXwVTlf0Gz7QBbtQC': 970,
     '5C05g_vxGqZYZwOZv8zuIsdy1cGUhdB7': 971,
     '5DlFMgnNQD5e_avzgoZzOwc8tQIDgFgM': 972,
     '5EA1bVBvgYoVG5vQAExlx6ArxfyZlHYu': 973,
     '5EL8eUTWOl02kHw93bM1Dh20PEaWjgwj': 974,
     '5G7MgsUHGjFQIEqAk__6XyHEp2-GMcv9': 975,
     '5GV6S7mh_SiBDS54_IDbDAf4G28YGWWD': 976,
     '5H8y5BBRUDvdqT7m2U1B4QplqI1FK-gH': 977,
     '5HHdaECJ_UsYQYlMiVhkqpKeMW0Z5_V9': 978,
     '5HMZLo2KWysOJUHXP4vWhVYtG_0pqTIa': 979,
     '5HY38_iZNN8N4nTX3i0nd_xezzi07Yhe': 980,
     '5HeynkzvXh7fP4mCLEKFYI_LUcMbS7iu': 981,
     '5HxsLNw_8w4CLChr0YUfl5QVDyIIpW4h': 982,
     '5Io3YXB7dYm2k7HYJJ_nWVC57H1EE9kG': 983,
     '5J0wWFkDiJAYR3FJY2O82gNXYaQFBoFJ': 984,
     '5KuMjIfOgUU42zP3zNzU0xFgla5AZRnM': 985,
     '5LKLimaj94Tt85pbhhW-Uf69o8pPpO8-': 986,
     '5M7aMeqZNL-WZKziTN1zNNtWIN1s4VMB': 987,
     '5MnFx33ZnloO0g9NBkXfp6mt209ERN5l': 988,
     '5MrvgHtigUo8W2wxKPgAcMpo9lc9jgDo': 989,
     '5NDuk1HB6dypgi4eS_jTA8ptiyTwSqsg': 990,
     '5NSbOHD6_x5RJtJTxXnnhVw2X2YuXU4F': 991,
     '5NjzUGBt-0pJKjaomqeIR2EjkAPQiQTP': 992,
     '5O7FFwQlpmu7hXAOHExPAtodHAcRp6Xj': 993,
     '5OKziFBotwzKH9zyyvLtCmm3yCZAtJUA': 994,
     '5OQEexCzgG08BEWnDrBOAu4bjA6mrfqG': 995,
     '5Obj4WDYp72wxVmyHOD_h3Ml11JK3asu': 996,
     '5P8TDWQLIIOrlBRw0Y7GZIPN8IelX0Gg': 997,
     '5QkEjzSnMlXfnnF044bYN4MRjQmpjM5V': 998,
     '5QzvuS_olXP4UVqjQSTU8WhdwUKRWamn': 999,
     ...}




```python
data_logs['n_user_id'] = data_logs['user_id'].map(id_dict)
order['n_user_id'] = order['user_id'].map(id_dict)
user['n_user_id'] = user['user_id'].map(id_dict)
```

## 12. 주문 데이터, 로그 데이터를 concat해주세요.


```python
print(order.shape)
order.head()
```

    (867, 7)





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
      <th>timestamp</th>
      <th>user_id</th>
      <th>goods_id</th>
      <th>shop_id</th>
      <th>price</th>
      <th>hour</th>
      <th>n_user_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-06-11 00:00:43.032</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>1414</td>
      <td>38</td>
      <td>45000</td>
      <td>0</td>
      <td>6241</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-06-11 00:02:33.763</td>
      <td>smDmRnykg61KajpxXKzQ0oNkrh2nuSBj</td>
      <td>1351</td>
      <td>12</td>
      <td>9500</td>
      <td>0</td>
      <td>8899</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-06-11 00:04:06.364</td>
      <td>EyGjKYtSqZgqJ1ddKCtH5XwGirTyOH2P</td>
      <td>646</td>
      <td>14</td>
      <td>22000</td>
      <td>0</td>
      <td>2527</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-06-11 00:04:17.258</td>
      <td>KQBGi33Zxh5Dgu0WEkOkjN0YqTT_wxC3</td>
      <td>5901</td>
      <td>46</td>
      <td>29800</td>
      <td>0</td>
      <td>3387</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-06-11 00:05:26.010</td>
      <td>lq1Je3voA3a0MouSFba3629lKCvweI24</td>
      <td>5572</td>
      <td>89</td>
      <td>29000</td>
      <td>0</td>
      <td>7832</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_logs[data_logs['user_id'] == 'bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx']
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
      <th>timestamp</th>
      <th>user_id</th>
      <th>event_origin</th>
      <th>event_name</th>
      <th>event_goods_id</th>
      <th>event_shop_id</th>
      <th>n_user_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>878</th>
      <td>2018-06-11 00:06:45.357</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>goods_search_result/린넨</td>
      <td>app_page_view</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6241</td>
    </tr>
    <tr>
      <th>901</th>
      <td>2018-06-11 00:06:54.034</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>goods_search_result/린넨바지</td>
      <td>app_page_view</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6241</td>
    </tr>
    <tr>
      <th>1062</th>
      <td>2018-06-11 00:08:00.579</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>goods_search_result/린넨바지</td>
      <td>enter_browser</td>
      <td>2048.0</td>
      <td>46.0</td>
      <td>6241</td>
    </tr>
    <tr>
      <th>1259</th>
      <td>2018-06-11 00:09:38.881</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>goods_search_result/린넨바지</td>
      <td>app_page_view</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6241</td>
    </tr>
    <tr>
      <th>1439</th>
      <td>2018-06-11 00:11:04.446</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>goods_search_result/린넨바지</td>
      <td>enter_browser</td>
      <td>3486.0</td>
      <td>38.0</td>
      <td>6241</td>
    </tr>
    <tr>
      <th>1473</th>
      <td>2018-06-11 00:11:20.354</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>goods_search_result/린넨바지</td>
      <td>app_page_view</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6241</td>
    </tr>
    <tr>
      <th>1526</th>
      <td>2018-06-11 00:11:48.284</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>goods_search_result/린넨바지</td>
      <td>enter_browser</td>
      <td>4006.0</td>
      <td>24.0</td>
      <td>6241</td>
    </tr>
    <tr>
      <th>2423</th>
      <td>2018-06-11 00:18:21.906</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>goods_search_result/린넨바지</td>
      <td>app_page_view</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6241</td>
    </tr>
    <tr>
      <th>2529</th>
      <td>2018-06-11 00:19:01.928</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>goods_search_result/린넨</td>
      <td>app_page_view</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6241</td>
    </tr>
    <tr>
      <th>2758</th>
      <td>2018-06-11 00:20:30.432</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>shops_bookmark</td>
      <td>app_page_view</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6241</td>
    </tr>
    <tr>
      <th>4502</th>
      <td>2018-06-11 00:32:29.738</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>shops_bookmark</td>
      <td>app_page_view</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6241</td>
    </tr>
    <tr>
      <th>5156</th>
      <td>2018-06-11 00:37:22.757</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>shops_bookmark</td>
      <td>app_page_view</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6241</td>
    </tr>
  </tbody>
</table>
</div>




```python
print('해당 날짜에 구매한 총 고객수: ', len(set(order['user_id'])))
print('해당 날짜 log데이터에 기록된 총 고객수: ', len(set(data_logs['user_id'])))
print('중복되는 고객수: ', len(set(order['user_id']) & set(data_logs['user_id'])))
```

    해당 날짜에 구매한 총 고객수:  832
    해당 날짜 log데이터에 기록된 총 고객수:  9909
    중복되는 고객수:  742



```python
# order, data_logs table concat (data_logs column에 맞추기)
# ** 로그가 없는 사용자는 제외 -> 잔존시간 계산에 오류를 일으키므로

order_copy = order[order['user_id'].isin(data_logs['user_id'])].copy()
order_copy = order_copy.rename(columns = {'shop_id': 'event_origin',
                                          'goods_id': 'event_goods_id'})
order_copy['event_name'] = 'purchase'
order_copy = order_copy[['timestamp','n_user_id','user_id','event_origin','event_name','event_goods_id','price']]


log_order = pd.concat([order_copy, data_logs], ignore_index=True)
log_order.head()
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
      <th>timestamp</th>
      <th>n_user_id</th>
      <th>user_id</th>
      <th>event_origin</th>
      <th>event_name</th>
      <th>event_goods_id</th>
      <th>price</th>
      <th>event_shop_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-06-11 00:00:43.032</td>
      <td>6241</td>
      <td>bvu0aLTqiFDoU-963xnr5nzQWTNLUMjx</td>
      <td>38</td>
      <td>purchase</td>
      <td>1414.0</td>
      <td>45000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-06-11 00:02:33.763</td>
      <td>8899</td>
      <td>smDmRnykg61KajpxXKzQ0oNkrh2nuSBj</td>
      <td>12</td>
      <td>purchase</td>
      <td>1351.0</td>
      <td>9500.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-06-11 00:05:26.010</td>
      <td>7832</td>
      <td>lq1Je3voA3a0MouSFba3629lKCvweI24</td>
      <td>89</td>
      <td>purchase</td>
      <td>5572.0</td>
      <td>29000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-06-11 00:05:35.182</td>
      <td>2745</td>
      <td>GM0-EsJPHjkpteIpAQIwaCdUjU81lhW1</td>
      <td>22</td>
      <td>purchase</td>
      <td>55.0</td>
      <td>11200.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-06-11 00:06:14.314</td>
      <td>7800</td>
      <td>lgvWxrv7r5RGklXSJqM2x6NUBZ5H-RQZ</td>
      <td>22</td>
      <td>purchase</td>
      <td>2451.0</td>
      <td>19800.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
log_order.loc[pd.notnull(log_order['price']), 'purchase'] = True
log_order['purchase'] = log_order['purchase'].fillna(False)

log_order = log_order.sort_values(['user_id','timestamp']).reset_index(drop=True)
```

## 13. 동일한 사용자에 대한 연속한 로그들 사이의 시간 간격을 구해주세요.
>- 고객 잔존시간 구하는 것
>- 현 데이터에서 앱 종료 기록 없음 --> 마지막 로그의 log_duration=0


```python
# 잔존시간 구하기

log_order['timestamp'] = pd.to_datetime(log_order['timestamp'])

log_order['timestamp_after'] = log_order.groupby('n_user_id')['timestamp'].shift(-1)
log_order['log_duration'] = log_order['timestamp_after'] - log_order['timestamp']
log_order['log_duration'] = log_order['log_duration'].fillna(pd.Timedelta(seconds=0))
```


```python
# 잔존시간 초(seconds)로 변환

log_order['log_duration'] = log_order['log_duration'].dt.total_seconds()
log_order['log_duration'] = log_order['log_duration'].astype(float)
```

## 14. 고객이 한 번 앱에 들어와서 활동하는 시간인 잔존시간을 구하기 위해 cycle을 정의해주세요.
>- cycle; 고객이 한번 app에 접속하여 나가기까지의 활동
>- is_out; 고객이 cycle을 종료하고 앱을 나갔는지 여부
> *  조건1) log_duration=0; 고객의 당일 마지막 로그
> *  조건2) log_duration >= 40min; 고객이 한 cycle을 종료하고 다음 cycle로 돌아온 것


```python
log_order.loc[(log_order['log_duration'] == 0) | (log_order['log_duration'] >= 40*60), 'is_out'] = True
log_order['is_out'] = log_order['is_out'].fillna(False)
```


```python
# log_duration이 40min이상인 경우, 앱을 지속해서 켜놨다고 볼 수 없음. 그러므로 log_duration을 0으로 변환

log_order.loc[log_order['is_out'] == True, 'log_duration'] = 0
```


```python
# cycle별 id

log_order['cycle_idx_unique'] = (log_order['is_out'].cumsum().shift(1).fillna(0).astype(int))


# 고객별 cycle id (cycle 횟수)

log_order['cycle_idx_daily'] = (log_order.groupby('n_user_id')['is_out'].cumsum().shift(1).fillna(0).astype(int))
head_index = log_order.groupby('n_user_id')['cycle_idx_daily'].head(1).index
log_order.loc[head_index, 'cycle_idx_daily'] = 0

```

### 14-1. cycle별 log 수(접속별 활동 개수)


```python
cycle_log_count = log_order.groupby(['n_user_id','cycle_idx_daily']).size().reset_index().rename(columns={0: 'log_count'})
cycle_log_count.head()
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
      <th>n_user_id</th>
      <th>cycle_idx_daily</th>
      <th>log_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0</td>
      <td>13</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2</td>
      <td>31</td>
    </tr>
  </tbody>
</table>
</div>



### 14-2. user별 cycle당 평균 log수(고객별 접속당 평균 활동수)


```python
cycle_user_log_count = cycle_log_count.groupby('n_user_id')['log_count'].mean().reset_index().rename(columns={'log_count': 'log_count_mean'})
cycle_user_log_count.head()
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
      <th>n_user_id</th>
      <th>log_count_mean</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>2.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>7.00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>16.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>7.75</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>2.00</td>
    </tr>
  </tbody>
</table>
</div>



### 14-3. 하루동안 가장 많은 cycle을 갖는(가장 많이 활동한) 상위 5명의 user를 구해주세요.


```python
cycle_log_count.groupby('n_user_id')['cycle_idx_daily'].nunique().sort_values().tail(5)
```




    n_user_id
    6234     9
    2424     9
    2249    10
    6010    10
    5847    11
    Name: cycle_idx_daily, dtype: int64



## 15. 잔존시간을 구해주세요
>- 현업에서는 잔존 시간을 이용하여 통해 어떤 채널을 이용한 고객 또는 어떤 광고를 통해 유입된 고객이 웹사이트/app에 오래 머물고 제품을 구매하는지에 대한 분석 또는 시간대/요일별 노출전략을 세우는 등 다양한 insight를 얻을 수 있습니다.

### 15-1. user별 평균 잔존시간을 구해주세요


```python
# cyclea별 잔존시간

remaining_time_cycle = log_order.groupby(['n_user_id','cycle_idx_daily'])['log_duration'].sum().reset_index().rename(columns = {'log_duration' : 'remaining_time'})


# user별 평균 잔존시간

remaining_time_cycle_user = pd.DataFrame(remaining_time_cycle.groupby('n_user_id')['remaining_time'].mean()).rename(columns={'remaining_time': 'duration'})
remaining_time_cycle_user.head()
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
      <th>duration</th>
    </tr>
    <tr>
      <th>n_user_id</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>114.890000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1011.541000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1346.260667</td>
    </tr>
    <tr>
      <th>3</th>
      <td>460.531250</td>
    </tr>
    <tr>
      <th>4</th>
      <td>49.130000</td>
    </tr>
  </tbody>
</table>
</div>



### 15-2. 구매/비구매 cycle별 평균 잔존시간 구하기
>- 구매기록이 있다면, 잔존시간이 길 것임.


```python
# 구매기록이 있는 cycle id

cycle_purchase = log_order[log_order['purchase'] == True]['cycle_idx_unique'].unique()
```


```python
# 구매기록 있는 cycle의 log_data

data_purchase = log_order.loc[log_order['cycle_idx_unique'].isin(cycle_purchase)]


# 구매기록 없는 cycle의 log_data

data_npurchase = log_order.loc[~log_order['cycle_idx_unique'].isin(cycle_purchase)]
```


```python
# 구매기록 있는 cycle의 잔존시간

remaining_time_cycle_purchase = data_purchase.groupby(['n_user_id','cycle_idx_unique'])['log_duration'].sum().reset_index().rename(columns={'log_duration': 'cycle_duration'})
remaining_time_cycle_purchase.head(10)
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
      <th>n_user_id</th>
      <th>cycle_idx_unique</th>
      <th>cycle_duration</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>4</td>
      <td>3075.422</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8</td>
      <td>15</td>
      <td>412.025</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8</td>
      <td>16</td>
      <td>1791.231</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9</td>
      <td>19</td>
      <td>1657.393</td>
    </tr>
    <tr>
      <th>4</th>
      <td>47</td>
      <td>88</td>
      <td>3116.367</td>
    </tr>
    <tr>
      <th>5</th>
      <td>49</td>
      <td>92</td>
      <td>2615.611</td>
    </tr>
    <tr>
      <th>6</th>
      <td>65</td>
      <td>123</td>
      <td>3093.858</td>
    </tr>
    <tr>
      <th>7</th>
      <td>86</td>
      <td>168</td>
      <td>1906.721</td>
    </tr>
    <tr>
      <th>8</th>
      <td>97</td>
      <td>192</td>
      <td>3031.770</td>
    </tr>
    <tr>
      <th>9</th>
      <td>117</td>
      <td>232</td>
      <td>1191.110</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 구매기록 없는 cycle의 잔존시간

remaining_time_cycle_npurchase = data_npurchase.groupby(['n_user_id','cycle_idx_unique'])['log_duration'].sum().reset_index().rename(columns={'log_duration': 'cycle_duration'})
remaining_time_cycle_npurchase.head(10)
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
      <th>n_user_id</th>
      <th>cycle_idx_unique</th>
      <th>cycle_duration</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>114.890</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>1011.541</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2</td>
      <td>893.742</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>3</td>
      <td>69.618</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>5</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>3</td>
      <td>6</td>
      <td>45.911</td>
    </tr>
    <tr>
      <th>6</th>
      <td>3</td>
      <td>7</td>
      <td>85.183</td>
    </tr>
    <tr>
      <th>7</th>
      <td>3</td>
      <td>8</td>
      <td>1711.031</td>
    </tr>
    <tr>
      <th>8</th>
      <td>4</td>
      <td>9</td>
      <td>49.130</td>
    </tr>
    <tr>
      <th>9</th>
      <td>5</td>
      <td>10</td>
      <td>0.000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 시각화

figure, axes = plt.subplots(nrows=2, ncols=1)
figure.set_size_inches(15,8)

axes[0].set_title('구매 cycle')
axes[1].set_title('비구매 cycle')
sns.boxplot(data=remaining_time_cycle_purchase, x='cycle_duration', ax=axes[0], orient='h')
sns.boxplot(data=remaining_time_cycle_npurchase, x='cycle_duration', ax=axes[1], orient='h')

figure.tight_layout()
plt.show()
print('구매 cycle 잔존 시간 평균: ', remaining_time_cycle_purchase.cycle_duration.mean())
print('비구매 cycle 잔존 시간 평균: ', remaining_time_cycle_npurchase.cycle_duration.mean())
```


    
![png](Level4_zigzag_files/Level4_zigzag_67_0.png)
    


    구매 cycle 잔존 시간 평균:  2280.0800799999997
    비구매 cycle 잔존 시간 평균:  611.9902027010454

