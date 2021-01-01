---
title: Python - seaborn
tags: python visualization seaborn
---

## seaborn


```python
import pandas as pd
```


```python
data = pd.read_csv('https://bit.ly/dsa-04-health')
print(data.shape)
data.head()
```

    (1000, 34)





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
      <th>기준년도</th>
      <th>가입자일련번호</th>
      <th>성별코드</th>
      <th>연령대코드(5세단위)</th>
      <th>시도코드</th>
      <th>신장(5Cm단위)</th>
      <th>체중(5Kg단위)</th>
      <th>허리둘레</th>
      <th>시력(좌)</th>
      <th>시력(우)</th>
      <th>...</th>
      <th>감마지티피</th>
      <th>흡연상태</th>
      <th>음주여부</th>
      <th>구강검진 수검여부</th>
      <th>치아우식증유무</th>
      <th>결손치유무</th>
      <th>치아마모증유무</th>
      <th>제3대구치(사랑니)이상</th>
      <th>치석</th>
      <th>데이터공개일자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016</td>
      <td>465969</td>
      <td>1</td>
      <td>8</td>
      <td>41</td>
      <td>170.0</td>
      <td>70.0</td>
      <td>74.0</td>
      <td>0.7</td>
      <td>0.7</td>
      <td>...</td>
      <td>96.0</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>20171219</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016</td>
      <td>565871</td>
      <td>1</td>
      <td>10</td>
      <td>41</td>
      <td>160.0</td>
      <td>60.0</td>
      <td>81.0</td>
      <td>1.2</td>
      <td>1.0</td>
      <td>...</td>
      <td>14.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>20171219</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016</td>
      <td>115718</td>
      <td>2</td>
      <td>11</td>
      <td>11</td>
      <td>160.0</td>
      <td>55.0</td>
      <td>71.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>...</td>
      <td>20.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>20171219</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2016</td>
      <td>767524</td>
      <td>1</td>
      <td>6</td>
      <td>28</td>
      <td>180.0</td>
      <td>70.0</td>
      <td>79.0</td>
      <td>1.0</td>
      <td>0.9</td>
      <td>...</td>
      <td>16.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>20171219</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2016</td>
      <td>482178</td>
      <td>2</td>
      <td>9</td>
      <td>11</td>
      <td>160.0</td>
      <td>60.0</td>
      <td>85.0</td>
      <td>0.8</td>
      <td>1.2</td>
      <td>...</td>
      <td>13.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>20171219</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 34 columns</p>
</div>




```python
import matplotlib
import seaborn as sns
from IPython.display import set_matplotlib_formats

# window
matplotlib.rc('font', family='NanumGothic')
#Mac
#matplotlib.rc('font', family='AppleGothic')

# retina; 선명도
set_matplotlib_formats('retina')

# minus; (-)기호 깨지는걸 방지하기 위함
matplotlib.rc('axes', unicode_minus=False)
```


```python
sns.countplot(data=data, x='성별코드')
```




    <AxesSubplot:xlabel='성별코드', ylabel='count'>




    
![png](/images/visualization_files/visualization_4_1.png)
    



```python
# 사이즈 조절

import matplotlib.pyplot as plt

plt.figure(figsize = (18,3))
sns.countplot(data=data, x='시도코드')
```




    <AxesSubplot:xlabel='시도코드', ylabel='count'>




    
![png](/images/visualization_files/visualization_5_1.png)
    



```python
# 여거래의 시각화를 한번에 띄우기

figure, (ax1, ax2) = plt.subplots(nrows=1, ncols=2)
figure.set_size_inches(18,3)
sns.countplot(data=data, x='성별코드', ax=ax1)
sns.countplot(data=data, x='시도코드', ax=ax2)
```




    <AxesSubplot:xlabel='시도코드', ylabel='count'>




    
![png](/images/visualization_files/visualization_6_1.png)
    



```python
#figure, ax1 = plt.subplots(nrows=1, ncols=1)
#figure, (ax1, ax2, ax3) = plt.subplots(nrows=1, ncols=3)
#figure, ((ax1, ax2, ax3), (ax4, ax5, ax6)) = plt.subplots(nrows=2, ncols=3)
figure, axes = plt.subplots(nrows=3, ncols=10)

```


    
![png](/images/visualization_files/visualization_7_0.png)
    



```python
# 컬러선택
## https://color.adobe.com/ko/create/color-wheel

plt.figure(figsize = (18, 3))
sns.countplot(data=data, x='시도코드', color='b')
```




    <AxesSubplot:xlabel='시도코드', ylabel='count'>




    
![png](/images/visualization_files/visualization_8_1.png)
    



```python
## palette
palette = sns.color_palette('deep', 10) #colorblind, coolwarm
sns.palplot(palette)
```


    
![png](/images/visualization_files/visualization_9_0.png)
    



```python
sns.countplot(data=data, x='시도코드', palette='coolwarm')
```




    <AxesSubplot:xlabel='시도코드', ylabel='count'>




    
![png](/images/visualization_files/visualization_10_1.png)
    



```python
# countplot

plt.figure(figsize = (18, 3))
sns.countplot(data=data, x='시도코드', hue='성별코드')
```




    <AxesSubplot:xlabel='시도코드', ylabel='count'>




    
![png](/images/visualization_files/visualization_11_1.png)
    



```python
# displot

sns.displot(data['허리둘레'], kde=True)
```




    <seaborn.axisgrid.FacetGrid at 0x7f13039e2890>




    
![png](/images/visualization_files/visualization_12_1.png)
    



```python
sns.displot(data['허리둘레'], kind='kde')
```




    <seaborn.axisgrid.FacetGrid at 0x7f1303964510>




    
![png](/images/visualization_files/visualization_13_1.png)
    



```python
sns.displot(data['허리둘레'])
```




    <seaborn.axisgrid.FacetGrid at 0x7f13039e4450>




    
![png](/images/visualization_files/visualization_14_1.png)
    



```python
sns.displot(data, x='허리둘레', hue='흡연상태', kind='kde')

## pre version
#one = data[data['흡연상태'] == 1]
#two = data[data['흡연상태'] == 2]
#three = data[data['흡연상태'] == 3]

#sns.distplot(one['허리둘레'], hist=False, label='1')
#sns.distplot(two['허리둘레'], hist=False, label='2')
#sns.distplot(three['허리둘레'], hist=False, label='3')
```




    <seaborn.axisgrid.FacetGrid at 0x7f130389f050>




    
![png](/images/visualization_files/visualization_15_1.png)
    



```python
# barplot

plt.figure(figsize = (18, 3))
sns.barplot(data=data, x='연령대코드(5세단위)', y='수축기혈압')
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='수축기혈압'>




    
![png](/images/visualization_files/visualization_16_1.png)
    



```python
import numpy as np

plt.figure(figsize = (18, 3))
sns.barplot(data=data, x='연령대코드(5세단위)', y='수축기혈압', estimator=np.sum)
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='수축기혈압'>




    
![png](/images/visualization_files/visualization_17_1.png)
    



```python
plt.figure(figsize = (18, 3))
sns.barplot(data=data, x='연령대코드(5세단위)', y='수축기혈압', hue='성별코드') # orient='h' ; transpose
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='수축기혈압'>




    
![png](/images/visualization_files/visualization_18_1.png)
    



```python
#boxplot

plt.figure(figsize = (18, 3))
sns.boxplot(data=data, x='연령대코드(5세단위)', y='수축기혈압')
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='수축기혈압'>




    
![png](/images/visualization_files/visualization_19_1.png)
    



```python
# violinplot

plt.figure(figsize = (18, 3))
sns.violinplot(data=data, x='연령대코드(5세단위)', y='수축기혈압')
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='수축기혈압'>




    
![png](/images/visualization_files/visualization_20_1.png)
    



```python
plt.figure(figsize = (18, 3))
sns.violinplot(data=data, x='연령대코드(5세단위)', y='수축기혈압', hue='성별코드', split=True)
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='수축기혈압'>




    
![png](/images/visualization_files/visualization_21_1.png)
    



```python
# pointplot; x축이 순서상 연관성이 있을 때 사용 (없을 때는 boxplot이용)

plt.figure(figsize = (18, 3))
sns.pointplot(data=data, x='연령대코드(5세단위)', y='수축기혈압')
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='수축기혈압'>




    
![png](/images/visualization_files/visualization_22_1.png)
    



```python
plt.figure(figsize = (18, 3))
sns.pointplot(data=data, x='연령대코드(5세단위)', y='수축기혈압', hue='성별코드', linestyles=[':','--'])
## seaborn line style
```




    <AxesSubplot:xlabel='연령대코드(5세단위)', ylabel='수축기혈압'>




    
![png](/images/visualization_files/visualization_23_1.png)
    



```python
# scatterplot; 정보량을 파악할 때

sns.scatterplot(data=data, x='식전혈당(공복혈당)', y='수축기혈압')
```




    <AxesSubplot:xlabel='식전혈당(공복혈당)', ylabel='수축기혈압'>




    
![png](/images/visualization_files/visualization_24_1.png)
    



```python
sns.scatterplot(data=data[data['식전혈당(공복혈당)'] < 150], 
                x='식전혈당(공복혈당)', y='수축기혈압',
               hue='허리둘레', size='총콜레스테롤', sizes=(30, 150))
```




    <AxesSubplot:xlabel='식전혈당(공복혈당)', ylabel='수축기혈압'>




    
![png](/images/visualization_files/visualization_25_1.png)
    

