# Spider-by-Python
# Web spiders for some Electronic Commerce-websites in China such as meilele.com etc.
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#spider_meilele_topic_v1.0专题爬虫:暂时只能爬非团购自动读取板块的所有页面产品：

from bs4 import BeautifulSoup
import requests
import urllib
import csv

html = "http://www.meilele.com/topic/2017_chuangdianzt.html"
headers = {
    'User-Agent':'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/47.0.2526.80 Safari/537.36 Core/1.47.933.400 QQBrowser/9.4.8699.400',
}
res = requests.get(html,headers=headers)
res.encoding = 'UTF-8'
soup = BeautifulSoup(res.text,'lxml')
#以上处理完第一个阶段：GET到页面并解析

csvFile = open('spider_meilele_topic_v1.0.csv','w',newline='',encoding='utf-8')
try:
	writer = csv.writer(csvFile,delimiter=' ')
	#增加delimiter,将单独占一格的元素合并到一列中去，但会产生空格
	writer.writerow(('goods-URL'))
	for goods_id in soup.select('a'):                    
	#此处用goods_id命名非团购自动读取版块商品ID，在v2.0中团购版块ID用info_id命名
		writer.writerow((goods_id['href'].split(' ')))   
		#用split删除每个单元格中的空格
finally:
	csvFile.close()

