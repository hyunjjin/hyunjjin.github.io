---
title: Python - Folium으로 지도 시각화
tags: python map
---


### 2. 버거킹, 스타벅스 위치
#### - 빨간점: 버거킹 / 초록점: 스타벅스


```python
#!pip install folium
```


```python
import pandas as pd
import os
import folium
from folium import plugins

bkr = pd.read_csv(os.getcwd() + '/200911_data_bkr.txt', sep='|', dtype=str)
starbucks = pd.read_csv(os.getcwd() + '/200911_data_star.txt', sep='|', dtype=str)

# 위경도 좌표
bkr = bkr[bkr['lat'].notnull()].reset_index(drop=True) # 좌표 없는 row 제거
bkr['lat'], bkr['lng'] = (pd.to_numeric(bkr['lat']), pd.to_numeric(bkr['lng']))
starbucks['lat'], starbucks['lng'] = (pd.to_numeric(starbucks['lat']), pd.to_numeric(starbucks['lng']))

# base 지도
base_map = folium.Map(location=[37.565692, 126.977988], # 서울시청 위경도
                      tiles='cartodbpositron',
                      zoom_start=14)

minimap = folium.plugins.MiniMap().add_to(base_map)

# 버거킹 위치
cat1 = folium.FeatureGroup(name='버거킹').add_to(base_map)
marker = folium.plugins.MarkerCluster(disableClusteringAtZoom=13).add_to(cat1)
for row in bkr.itertuples():
    folium.Circle(location=[row.lat, row.lng],
                  tooltip=row.name,
                  radius=2,
                  color='red').add_to(marker)

# 스타벅스 위치
cat2 = folium.FeatureGroup(name='스타벅스').add_to(base_map)
marker = folium.plugins.MarkerCluster(disableClusteringAtZoom=13).add_to(cat2)
for row in starbucks.itertuples():
    folium.Circle(location=[row.lat, row.lng],
                  tooltip=row.name,
                  radius=2,
                  color='green').add_to(marker)

base_map.add_child(folium.map.LayerControl(collapsed=False))

# 지도 저장
base_map.save(os.getcwd() + '/200911_total_map.html')
```
