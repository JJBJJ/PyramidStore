
## Pyramid爬虫写法

目前所有爬虫继承[spider.py](https://github.com/lm317379829/PyramidStore/blob/main/base/spider.py)

spider提供了一些需要被实现的方法和一些公共方法，请自行查阅

#### 快速开发

参考[py_bilibilivd.py](https://github.com/lm317379829/PyramidStore/blob/main/plugin/py_bilibilivd.py)进行快速开发
##### 1. 爬虫方法

```python
    # 这些具体的写法和Java版本的爬虫一致
    # 主页
    def homeContent(self,filter):pass
    # 推荐视频
    def homeVideoContent(self):pass
    # 分类
    def categoryContent(self,tid,pg,filter,extend):pass
    # 详情
    def detailContent(self,ids):pass
    # 搜索
    def searchContent(self,key,quick):pass
    # 翻页搜索
    def searchContentPage(self, key, quick, page):pass
    # 播放
    def playerContent(self,flag,id,vipFlags):pass
    # 视频格式
    def isVideoFormat(self,url):pass
    # 视频检测
    def manualVideoCheck(self):pass
```

##### 2. 本地代理

代理地址写法```http://127.0.0.1:9978/proxy?do=py&type=```,其中{key}表示配置文件中key的名称,其他参数追加到地址最后即可。样例请参考py_bilibilivd.py playerContent方法

```python
    # 以下代码来自py_bilibilivd.py，完整代码请自行查看 
    # 本地代理
	def localProxy(self, params):
		if params['type'] == "mpd":
			return self.proxyMpd(params)
		if params['type'] == "media":
			return self.proxyMedia(params)
		return None

	def proxyMpd(self, params):
		content, durlinfos, mediaType = self.getDash(params)
		if mediaType == 'mpd':
			return [200, "application/dash+xml", content] # 200 返回string
		else:
			# 略
			if '127.0.0.1:7777' in url:
				header["Location"] = url
				return [302, "video/MP2T", None, header] # 302重定向到url
			r = requests.get(url, headers=header, stream=True)
			return [206, "application/octet-stream", r.content]  # 206 返回bytes
```
##### 3. 配置写法

* 所有文件以 py_ 开头
* ext写extend内容
* api写py的网络地址或者本地地址

```json
{
    "key": "py_bilibilivd",
    "name": "B站",
    "type": 3,
    "api": "http://地址/py_bilibilivd",
    "searchable": 1,
    "quickSearch": 1,
    "filterable": 1,
    "ext": {"cookie": "cookies信息"}
}
```

##### 问题请反馈到[tg](https://t.me/+A3SLQRmPVi9kOThl)群里