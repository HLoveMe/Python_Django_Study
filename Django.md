#项目搭建

* Pycharm创建Django工程
* 创建目录
	* Apps 存放之后所有App 
		* make Directory as Source root
	* log 日志目录
	* media 用户上传图片等目录
	* static 存放js css 静态图片目录
	* templates html文件
	* Extra_apps 放第三方app
		* make Directory as Source root
* Apps路径注册为搜索路径

	```
		Settngs 
		sys.path.append(os.path.join(BASE_DIR,"Apps"))
	```
* Setting数据库配置

	```
		> 安装mysql驱动 
		> Setting.md 正确配置mysql
		> Run Manage.py Task(如果没有初始化就会报错)
		> 初始化数据库 (数据库操作.md)
	``` 

* 静态文件路径配置 static 文件夹路径
	* Settings.md 静态文件路径配置
	* js/css/images

	```
	前提
	STATIC_URL = '/static/'
	STATICFILES_DIRS = [
    	os.path.join(BASE_DIR,'static'),
	]
	静态文件加载 1 （静态文件必须在static目录下）
		<script type="text/javascript" src="/static/js/index.js"></script>
		
	静态文件加载 2 （比较灵活）
		{%load staticfiles %}
		<script type="text/javascript" src={% static "js/index.js" %}></script>
	
	静态文件加载 3 （比较灵活）
	setting.TEMPLATES.OPTIONS.context_processors
		加入'django.template.context_processors.static'
		<link rel="stylesheet" type="text/css" media="screen" href="{{ STATIC_URL }}bbs/style.css">
	```
	
* media文件设置 用户上传文件 [Setting.md]
	
	```
	前段引用1
		/media/xx/00/.png
	前段引用2 推荐
		'django.template.context_processors.media'
		{% MEDIA_URL "a/x/p.jpg" %}
	
	
	开发时
	
	from django.conf import settings
	from django.views.static import serve
	if settings.DEBUG:
	    urlpatterns.append(url(r'^media/(?P<path>.*)$', serve, {'document_root': settings.MEDIA_ROOT}))
	    
	发布时 由nginx管理静态文件
	```
* 模板文件配置
	* settings.md
	
* 创建App
	* pyCharm Tools -> Run manang.py Task
	* startapp AAA
	* settings.md 指明App
	
* 时区设置
	* Setting.md
* 模型操作
	* 模型创建和数据库操作  Modol模型.md
	* 模型和网页数据填充  模板文件.md
	* 模型和页面跳转
	
* urls配置

	```
	匹配请求路径 
		from django.conf.urls import url
		url(r"",action,name="",kwargs={参数})
	路径的合并
		AppUrls
			urlpatterns = [
				ulr1,
				url2
			]	
		Project
			urlpatterns = [
				url(r'^admin/', admin.site.urls),
				url(r'user/',APPurlpatterns),
				url(r"email/",include([
					url(r"^delete/",action),
					url(r"^read/",action),
				]))
				....
			]
	详细说明 (urls.md)
	```
* Request (Request.md)

* 响应 HttpResponse [HttpResponse.md]

	```
	from django.shortcuts import render_to_response
	from django.shortcuts import render
	render是render_to_response的快捷方式
	
	Return Response响应
例子(模板响应):

	return render_to_response('blog_add.html',{'blog': blog, 'form': form, 'id': id, 'tag': tag},
                          context_instance=RequestContext(request))

	return render(request, 'blog_add.html', {'blog': blog, 'form': form, 'id': id, 'tag': tag})
	
	```
	
	
* 用户 [User.md]
	
	```
		继承django User
		
		USer 操作
		认证 登入 退出 刷新 获取当前登入USer
	```
	* 用户权限 [User_Permission.md] 
	
* 后台管理Xadmin [Xadmin.md]