# Python爬虫学习[02]

## 自动获取高清壁纸

网页截图

![](/Users/ay/Desktop/Python 100days/Day 02/QQ20190511-160846@2x.png)

## 主要学习任务

1. 掌握异步加载数据的爬取方法【有待掌握】

2. 加深 BeautifulSoup 和 Requests 的学习和命令记忆使用方法

3. 学会链接网页中get 到的下一个链接

4. 学会保存连接图片到本地

```python

from bs4 import BeautifulSoup

import requests

import time 

#%%

#url = 'https://wallpapers.wallhaven.cc/wallpapers/full/wallhaven-765623.jpg'

def download(url):

    img_name = time.strftime("%Y-%m-%d %H:%M:%S",time.localtime())+'.'+url.split('.')[-1]

    content = requests.get(url).content

    with open(img_name, 'wb') as f:

        f.write(content)

        print('下载成功 {}'.format(img_name))

def get_wallpaper(url):

    j=0

    wb_data = requests.get(url)

    print(wb_data)

    time.sleep(2)

    soup = BeautifulSoup(wb_data.text,'lxml')

    img = soup.select(' ul > li > figure > a')

    for i in img:

        wb1_data = requests.get(i.get('href'))

        time.sleep(2)

        

        soup1 = BeautifulSoup(wb1_data.text,'lxml')

        ima_url = soup1.select("#wallpaper")[0].get('src')

        img_url = 'https:'+ima_url

        download(img_url)

        if True:

            j=j+1

            print('第{}张'.format(j))

\#urls = ['https://alpha.wallhaven.cc/search?q=&categories=111&purity=110&resolutions=1920x1080&topRange=1M&sorting=toplist&order=desc&page={}'.format(str(i) for i in range(1,2))]

url = 'https://alpha.wallhaven.cc/search?q=&categories=111&purity=110&resolutions=1920x1080&topRange=1M&sorting=toplist&order=desc&page='

for page in range(1,10):

    

    get_wallpaper(url+str(page))

```