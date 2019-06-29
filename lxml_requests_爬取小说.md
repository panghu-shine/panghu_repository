<b>use python3</b>

使用python解释器运行（python3）

```python
#-*- coding:utf-8 -*-
import requests
from lxml import etree
domain_url = 'https://www.17k.com/'
url = 'https://www.17k.com/list/493239.html'

r = requests.get('https://www.17k.com/list/493239.html')
r.encoding = 'utf8'
s = etree.HTML(r.text)
caption = s.xpath("//div[@class='Main List'][1]/h1/text()")[0]
sections = s.xpath("//div[@class='Main List'][1]/dl[contains(@class,'Volume')]")

for section in sections:
	section_name = section.xpath("./dt/span[@class='tit']/text()")[0]
	artical_list = section.xpath("./dd[1]/a/@href")
	for artical in artical_list:

		artical_link = domain_url + artical

		response = requests.get(artical_link)
		response.encoding = 'utf8'
		selector = etree.HTML(response.text)

		contents_name = selector.xpath("//div[contains(@class,'readAreaBox')]/h1/text()")
		author_info = selector.xpath("//div[contains(@class,'readAreaBox')]/div[1]/text()")
		contents = selector.xpath("//div[contains(@class,'readAreaBox')]/div/p/text()")

		filename = section_name + contents_name[0]
		content = '\n'.join(contents_name) + '\n' +'\n'.join(author_info) + '\n' +'\n'.join(contents) 

		with open('修罗武神/%s'%filename,'w') as fp:
			fp.write(content)
```

