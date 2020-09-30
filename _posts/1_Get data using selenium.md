# 1. Selenium을 이용해 스타벅스, 버거킹 정보 크롤링
## * 버거킹은 지점의 x,y 좌표를 제공하지 않으므로 카카오 api를 이용
> - chrome 설치
> - chromedriver linux버전 다운로드 (https://chromedriver.chromium.org/downloads)




```python
#!pip install selenium
#!pip install pyvirtualdisplay 
```


```python
from selenium import webdriver
from pyvirtualdisplay import Display 
import time
import pandas as pd
from bs4 import BeautifulSoup
import os
from tqdm import tqdm
import urllib.request
import json
```


```python
# xpath 존재여부
def hasxpath(xpath):
    try:
        driver.find_element_by_xpath(xpath)
        return True
    except:
        return False
    
# 카카오api
KakaoAK = 'bd036d6591ce21bcdaa37ff416e6d5ea'

def get_request_url(url):
    
    req = urllib.request.Request(url)
    req.add_header("Authorization", 'KakaoAK ' + KakaoAK)
    response = urllib.request.urlopen(req)
    rescode = response.getcode()
    if(rescode==200):
        response_body = response.read()
        return response_body.decode('utf-8')
    else:
        return "Error Code:" + rescode

def get_Addr(addr):

    base = 'https://dapi.kakao.com//v2/local/search/address.json'
    node = ''
    parameters = '?query=%s' % urllib.parse.quote(addr)
    url = base + node + parameters
    
    retData = get_request_url(url)
    
    if (retData == None):
        return None
    else:
        return json.loads(retData)
```

## (1) 스타벅스 크롤링


```python
display = Display(visible=0, size=(1920, 1080)) 
display.start()
driver = webdriver.Chrome(os.getcwd() + '/chromedriver')
driver.get("https://www.starbucks.co.kr/store/store_map.do?disp=locale")
time.sleep(30)

si = pd.DataFrame(range(1, 18))

data_star = pd.DataFrame()
for i in tqdm(si.index):

    driver.find_element_by_xpath('//*[@id="container"]/div/form/fieldset/div/section/article[1]/article/article[2]/div[1]/div[2]/ul/li[' + str(si[0][i]) + ']/a').click()
    time.sleep(10)
    if hasxpath('//*[@id="mCSB_3_container"]/ul/li[1]/strong') == False:
        driver.find_element_by_xpath('//*[@id="mCSB_2_container"]/ul/li[1]/a').click()
        time.sleep(5)
    else:
        pass
    soup = BeautifulSoup(driver.page_source)

    total = soup.find('ul', class_='quickSearchResultBoxSidoGugun')
    li_list = total.find_all('li')
    

    for j in li_list:
        
        code = j.attrs['data-code']
        lat = j.attrs['data-lat']
        long= j.attrs['data-long']
        storecd= j.attrs['data-storecd']
        name = j.attrs['data-name']
        addr = j.find('p').text
        star = j.find('i').attrs['class'][0]

        df = pd.DataFrame({'code': [code],
                           'lat': [lat],
                           'lng': [long],
                           'storecd': [storecd],
                           'name': [name],
                           'addr': [addr],
                           'star': [star]})
        data_star = pd.concat([data_star, df], ignore_index=True)

    driver.find_element_by_xpath('//*[@id="container"]/div/form/fieldset/div/section/article[1]/article/header[2]/h3/a').click()
    time.sleep(5)
        
driver.quit()
display.stop()
```

    100%|██████████| 17/17 [04:45<00:00, 16.81s/it]





    <pyvirtualdisplay.display.Display at 0x7fdcf85bc250>




```python
data_star.head()
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
      <th>code</th>
      <th>lat</th>
      <th>lng</th>
      <th>storecd</th>
      <th>name</th>
      <th>addr</th>
      <th>star</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3762</td>
      <td>37.501087</td>
      <td>127.043069</td>
      <td>1509</td>
      <td>역삼아레나빌딩</td>
      <td>서울특별시 강남구 언주로 425 (역삼동)</td>
      <td>pin_general</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3672</td>
      <td>37.510178</td>
      <td>127.022223</td>
      <td>1434</td>
      <td>논현역사거리</td>
      <td>서울특별시 강남구 강남대로 538 (논현동)</td>
      <td>pin_general</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3858</td>
      <td>37.514132</td>
      <td>127.020563</td>
      <td>1595</td>
      <td>신사역성일빌딩</td>
      <td>서울특별시 강남구 강남대로 584 (논현동)</td>
      <td>pin_general</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3669</td>
      <td>37.499517</td>
      <td>127.031495</td>
      <td>1527</td>
      <td>국기원사거리</td>
      <td>서울특별시 강남구 테헤란로 125 (역삼동)</td>
      <td>pin_general</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3703</td>
      <td>37.494668</td>
      <td>127.062583</td>
      <td>1468</td>
      <td>스탈릿대치R</td>
      <td>서울특별시 강남구 남부순환로 2947 (대치동)</td>
      <td>pin_reserve</td>
    </tr>
  </tbody>
</table>
</div>



### - 주소에 전화번호가 함께 기재되어 있으므로, 클렌징 작업 수행
> - 사실 데이터를 보는데 이 가공을 하지 않아도 크게 문제 되어보이지 않지만, 예쁜 데이터는 보기도 좋으니까...


```python
data_star['addr'] = data_star['addr'].str.replace('1522-3232', '')
data_star.head()
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
      <th>code</th>
      <th>lat</th>
      <th>lng</th>
      <th>storecd</th>
      <th>name</th>
      <th>addr</th>
      <th>star</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3762</td>
      <td>37.501087</td>
      <td>127.043069</td>
      <td>1509</td>
      <td>역삼아레나빌딩</td>
      <td>서울특별시 강남구 언주로 425 (역삼동)</td>
      <td>pin_general</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3672</td>
      <td>37.510178</td>
      <td>127.022223</td>
      <td>1434</td>
      <td>논현역사거리</td>
      <td>서울특별시 강남구 강남대로 538 (논현동)</td>
      <td>pin_general</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3858</td>
      <td>37.514132</td>
      <td>127.020563</td>
      <td>1595</td>
      <td>신사역성일빌딩</td>
      <td>서울특별시 강남구 강남대로 584 (논현동)</td>
      <td>pin_general</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3669</td>
      <td>37.499517</td>
      <td>127.031495</td>
      <td>1527</td>
      <td>국기원사거리</td>
      <td>서울특별시 강남구 테헤란로 125 (역삼동)</td>
      <td>pin_general</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3703</td>
      <td>37.494668</td>
      <td>127.062583</td>
      <td>1468</td>
      <td>스탈릿대치R</td>
      <td>서울특별시 강남구 남부순환로 2947 (대치동)</td>
      <td>pin_reserve</td>
    </tr>
  </tbody>
</table>
</div>



## (2) 버거킹 크롤링
### 1) selenium


```python
display = Display(visible=0, size=(1920, 1080)) 
display.start()
driver = webdriver.Chrome(os.getcwd() + '/chromedriver')
driver.get("https://www.burgerking.co.kr/#/home")

si = pd.DataFrame(range(2, 19))


# popup 닫기
if hasxpath('//*[@id="app"]/div/div[3]/div[8]/div/div[1]') == True:
    driver.find_element_by_xpath('//*[@id="app"]/div/div[3]/div[8]/div/div[2]/button[1]/span').click()
else: pass

# 매장찾기
driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
driver.find_element_by_xpath('//*[@id="app"]/div/div[4]/div[1]/div/ul/li[2]/ul/li/a/span').click()
# 지역검색
driver.find_element_by_xpath('//*[@id="app"]/div/div[3]/div[3]/div/div[1]/div[1]/div[1]/ul/li[3]/button/span').click()
time.sleep(1)


def change(var):
    if 'none' in str(var):
        return 0
    else:
        return 1
    
data_bkr = pd.DataFrame()
for i in tqdm(si.index):
    
    # 특별/광역시 선택
    driver.find_element_by_xpath('//*[@id="app"]/div/div[3]/div[3]/div/div[1]/div[1]/div[2]/div[4]/div/select[1]').click()
    driver.find_element_by_xpath('//*[@id="app"]/div/div[3]/div[3]/div/div[1]/div[1]/div[2]/div[4]/div/select[1]/option[' + str(si[0][i]) + ']').click()

    soup = BeautifulSoup(driver.page_source)
    total= soup.find('ul', class_='list02')
    total.find_all('p', class_='tit')

    bkr = pd.DataFrame({'name': [i.get_text() for i in total.find_all('p', class_='tit')]})
    bkr['addr'] = pd.DataFrame([i.get_text() for i in total.find_all('p', class_='addr')])
    bkr['phone'] = pd.DataFrame([i.get_text() for i in total.find_all('p', class_='')])
    bkr[['delivery','king','night','breakfast','parking','dt']] = pd.DataFrame([x.find_all('li') for x in total.find_all('ul', class_='shoptype_list ico_type')])
    for x in bkr.index:
        bkr['delivery'][x]= change(bkr['delivery'][x])
        bkr['king'][x]= change(bkr['king'][x])
        bkr['night'][x]= change(bkr['night'][x])
        bkr['breakfast'][x]= change(bkr['breakfast'][x])
        bkr['parking'][x]= change(bkr['parking'][x])
        bkr['dt'][x]= change(bkr['dt'][x])

    data_bkr = pd.concat([data_bkr, bkr], ignore_index=True)
    del bkr
    
    time.sleep(5)
    
driver.quit()
display.stop()
```

    100%|██████████| 17/17 [01:29<00:00,  5.26s/it]





    <pyvirtualdisplay.display.Display at 0x7fdcc9bed2d0>



### 2) kakao api
> - 위경도가 null인 지점도 존재함을 고려


```python
for i in tqdm(data_bkr.index):
    
    try:
        data_bkr.loc[i, 'lng'] = get_Addr(data_bkr['addr'][i])['documents'][0]['x']
        data_bkr.loc[i, 'lat'] = get_Addr(data_bkr['addr'][i])['documents'][0]['y']
    
    except IndexError:
        pass
```

    100%|██████████| 412/412 [01:03<00:00,  6.44it/s]



```python
data_bkr.head()
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
      <th>name</th>
      <th>addr</th>
      <th>phone</th>
      <th>delivery</th>
      <th>king</th>
      <th>night</th>
      <th>breakfast</th>
      <th>parking</th>
      <th>dt</th>
      <th>lng</th>
      <th>lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>종로점</td>
      <td>서울특별시 종로구 종로 94</td>
      <td>02-2285-4838</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>126.988087440713</td>
      <td>37.5699451391001</td>
    </tr>
    <tr>
      <th>1</th>
      <td>종로구청점</td>
      <td>서울특별시 종로구 삼봉로 57</td>
      <td>02-722-0238</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>126.980473808393</td>
      <td>37.5725615164337</td>
    </tr>
    <tr>
      <th>2</th>
      <td>시청역점</td>
      <td>서울특별시 중구 서소문로 136</td>
      <td>02-777-0332</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>126.97604290682</td>
      <td>37.5635491216557</td>
    </tr>
    <tr>
      <th>3</th>
      <td>충무로역점</td>
      <td>서울특별시 중구 퇴계로 216</td>
      <td>02-2285-0337</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>126.995813274893</td>
      <td>37.5613345759958</td>
    </tr>
    <tr>
      <th>4</th>
      <td>회현역점</td>
      <td>서울특별시 중구 퇴계로 52 티마크그랜드호텔 명동 1층</td>
      <td>070-7462-6807</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>126.978544421858</td>
      <td>37.5584186963798</td>
    </tr>
  </tbody>
</table>
</div>



### - 지점명에 거리가 함께 기재되어 있어, 클렌징 작업 수행


```python
data_bkr['name'] = data_bkr['name'].str.replace('\d+\.\d+km', '')
data_bkr.head()
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
      <th>name</th>
      <th>addr</th>
      <th>phone</th>
      <th>delivery</th>
      <th>king</th>
      <th>night</th>
      <th>breakfast</th>
      <th>parking</th>
      <th>dt</th>
      <th>lng</th>
      <th>lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>종로점</td>
      <td>서울특별시 종로구 종로 94</td>
      <td>02-2285-4838</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>126.988087440713</td>
      <td>37.5699451391001</td>
    </tr>
    <tr>
      <th>1</th>
      <td>종로구청점</td>
      <td>서울특별시 종로구 삼봉로 57</td>
      <td>02-722-0238</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>126.980473808393</td>
      <td>37.5725615164337</td>
    </tr>
    <tr>
      <th>2</th>
      <td>시청역점</td>
      <td>서울특별시 중구 서소문로 136</td>
      <td>02-777-0332</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>126.97604290682</td>
      <td>37.5635491216557</td>
    </tr>
    <tr>
      <th>3</th>
      <td>충무로역점</td>
      <td>서울특별시 중구 퇴계로 216</td>
      <td>02-2285-0337</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>126.995813274893</td>
      <td>37.5613345759958</td>
    </tr>
    <tr>
      <th>4</th>
      <td>회현역점</td>
      <td>서울특별시 중구 퇴계로 52 티마크그랜드호텔 명동 1층</td>
      <td>070-7462-6807</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>126.978544421858</td>
      <td>37.5584186963798</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 데이터 저장
data_star.to_csv(os.getcwd() + '/200911_data_star.txt', sep='|', index=False)
data_bkr.to_csv(os.getcwd() + '/200911_data_bkr.txt', sep='|', index=False)
```
