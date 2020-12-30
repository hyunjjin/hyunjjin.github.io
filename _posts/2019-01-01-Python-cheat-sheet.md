---
title: Python cheat sheet
tags: python cheat_sheet
---



-----
# environment  

## - jupyter notebook cell width
```
from IPython.core.display import display, HTML
display(HTML("<style>.container { width:100% !important; }</style>"))
```

## - memory max size / availity
```
import sys
print(sys.maxsize)

!cat /proc/meminfo | grep Mem
```

## - create folder
```
import os
def createfolder(dir):
	try:
		if not os.path.exists(dir):
				os.makedirs(dir)
				print("Directory ", dir, " created")
		else:
				print("Directory ", dir, " already exists")
```

## - get file/folder list
```
import os, glob
os.chdir('folder_path')
file = glob.glob('*.txt')
```

## - timezone
```
import datetime
datetime.datetime.now(timezone('Asia/Seoul')).strftime('%y%m%d')
```



-----
# string

## - fuzzywuzzy; 문자간 비슷한 정도, 확률 구하기
```
pip install fuzzywuzzy
pip install python-Levenshtein

from fuzzywuzzy import fuzz
from fuzzywuzzy import process

fuzz.ration("this is a test", "this is a test!”)
fuzz.partial_ratio("this is a test", "this is a test!")
fuzz.token_sort_ratio("fuzzy wuzzy was a bear", "wuzzy fuzzy was a bear”)
fuzz.token_set_ratio("fuzzy was a bear", "fuzzy fuzzy was a bear")
process.extract(query, [word1, word2])
process.extractOne(query, [word1, word2])
fuzz.WRatio(one, two)
```

## - explode
```
data.assign(var = data['want_to_split_column'].str.split(',')).explode('var')
```



-----
# group by

## - ave(x,y,FUN=length)
```
apt_info['num'] = apt_info.groupby('pnu')['pnu'].transform('count')
apt_info['num'] = apt_info[['kaptCode','pnu']].groupby(['pnu']).agg(['count'])
df.groupby(['col1', 'col2']).size().reset_index(name='counts', drop=True)
```
## - ave(x,y,FUN=seq_along)
```
apt['num'] = apt.groupby('PK').cumcount().add(1)
```



-----
# calculate

## - truncate(버림)
```
import math
def truncate(number, decimals=0):
    """
    Returns a value truncated to a specific number of decimal places.
    """
    if not isinstance(decimals, int):
        raise TypeError("decimal places must be an integer.")
    elif decimals < 0:
        raise ValueError("decimal places has to be 0 or more.")
    elif decimals == 0:
        return math.trunc(number)

    factor = 10.0 ** decimals
    return math.trunc(number * factor) / factor
```

## - round up
```
def round_up(number, decimals=0):
    multiplier = 10 ** decimals
    return math.floor(number * multiplier + 0.5) / multiplier
```



-----
# graph

## - pivot table
```
pd.pivot_table(data, index='', values='', aggfunc=['',''])
```