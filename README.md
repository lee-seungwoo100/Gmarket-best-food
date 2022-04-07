import requests
from bs4 import BeautifulSoup
import time

url = 'http://mobile.gmarket.co.kr/Best/Category?code=G07&subCode='
res = requests.get(url)
soup = BeautifulSoup(res.content,'html.parser')

datas = soup.select('div.kind_artwrap li')
for realdata in datas:
    proudctname = realdata.select_one('strong.tit_pd').get_text()
    proudctprice = realdata.select_one('strong.pd_price').get_text()
    
    productlink = realdata.select_one('li > a')['href']
    linkres = requests.get(productlink)
    linksoup = BeautifulSoup(linkres.content,'html.parser')

    productsupplier = linksoup.select_one('span.text__seller a').get_text()
    try:
        productcoupon = linksoup.select_one('div.box__coupon-state div.text').get_text()
    except:
        productcoupon = '할인쿠폰없음'
    try:
        productreview = linksoup.select_one('span.box__rating-number').get_text()[1:-1]
    except:
        productreview = '리뷰없음'
    print()
    
