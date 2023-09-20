# kumoh-py
## Python 기반 빅데이터 사이언티스트 ( 2023.07.12 ~ 2023. 10.10 ) 392시간
### 교과목명 : 정보기술 빅데이터 분석
### 교과목명 : Python기반의 빅데이터 분석

1. naver Blog Module
```
import requests
import json
import urllib.request
from urllib.parse import quote
from bs4 import BeautifulSoup
import pandas as pd
import matplotlib.pyplot as plt
from wordcloud import WordCloud
from matplotlib import font_manager
from konlpy.tag import Okt
from collections import Counter

client_id = "dZld5466VWxrKooDKlSX"
client_secret = "VOaxDMgBwa"

# CALL() 함수
def call(keyword, start):
    encText = quote(keyword)
    url = "https://openapi.naver.com/v1/search/blog?query=" + encText +"&sort=sim&display=100&start="+str(start)
    result = requests.get(url=url, headers ={"X-Naver-Client-Id":client_id, "X-Naver-Client-Secret":client_secret} )
    return result.json() 

# SEARCH() 함수
def search(keyword):
    lst = []
    for page in range(0,10): 
        lst = lst + call(keyword, page *100 +1)['items']
    return lst

# REMOVE_HTML_TAGS() 함수
def remove_html_tags(text):
    soup = BeautifulSoup(text, "html.parser")
    return soup.get_text()  

# WorldCloud 시각화
def word_cloud(df, font_path):

    fontprop = font_manager.FontProperties(fname=font_path)
    
    # df['description'] 컬럼의 모든 텍스트를 하나의 문자열로 
    text = ' '.join(df['description'].values)
    
    # 명사 추출과 단어 빈도수 
    okt = Okt()
    nouns = okt.nouns(text) # 명사추출
    words = [n for n in nouns if len(n) > 1]# 단어길이가 1인것은 제외
    text = Counter(words)
    

    # WorldCloud 객체 생성
    wordcloud = WordCloud(width=800, height=400, background_color='white', font_path=font_path).generate_from_frequencies(text)
    
    # 워드클라우드를 matplotlib으로 시각화
    plt.figure(figsize=(10, 5))    
    plt.imshow(wordcloud, interpolation='bilinear')
    plt.axis('off')
    plt.show()
```
[네이버 블로그](http://blog.naver.com/5html)
