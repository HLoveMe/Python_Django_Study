# 部署

```
1:前提Django能在本地运行

2:FileZilla上传到腾讯云服务器
	》服务器 开放 端口
	》安装和本地开发一致的Django环境 和 虚拟环境
	
2-3:以下命令都是虚拟环境中

3:测试
	项目目录下 python manage.py runserver 0.0.0.0:8000
	浏览器输入119.29.17.140:8000可以正常访问
	
4:安装uwsgi
	pip install uwsgi
	测试uwsgi->python
		创建Dome.py上传到任意目录下
		def application(env, start_response):
		    start_response('200 OK', [('Content-Type','text/html')])
	    	return [u"Hello World"]
	    在该目录下 uwsgi --http :8001 --wsgi-file Dome.py
		浏览器 http://119.29.17.140:8001 是否可以访问
		
	测试uwsgi ->django
	切换到django目录
		uwsgi --http :8888 --module MuOnlie.wsgi 
			(setting.py 同级目录下的wsgi.py)
	http://119.29.17.140:8888 是否可以访问
	
```