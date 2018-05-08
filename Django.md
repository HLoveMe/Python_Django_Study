#项目搭建

* Pycharm创建Django工程
* 创建目录
	* Apps 存放之后所有App 
	* log 日志目录
	* media 用户上传图片等目录
	* static 存放js css 静态图片目录
	* templates html文件
* Apps路径注册为搜索路径

	```
		Settngs 
		sys.path.append(os.path.join(BASE_DIR,"Apps"))
	```
* Setting数据库配置

	```
		> 安装mysql驱动 pip install MySQL-python
		> Setting.md 正确配置mysql
		> Run Manage.py Task(如果没有初始化就会报错)
		> 初始化数据库 (数据库操作.md)
	``` 

* 静态文件路径配置 static 文件夹路径
	* Settings.md 静态文件路径配置
	* js/css/images
* 模板文件配置
	* settings.md
* 创建App
	* pyCharm Tools -> Run manang.py Task
	* startapp AAA
	* settings.md 指明App
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
	
* 模板渲染  [模板文件.md]

	```
	from django.shortcuts import render_to_response
	from django.shortcuts import render
	
	render是render_to_response的快捷方式
	
	return render_to_response('blog_add.html',{'blog': blog, 'form': form, 'id': id, 'tag': tag},
                          context_instance=RequestContext(request))

	return render(request, 'blog_add.html', {'blog': blog, 'form': form, 'id': id, 'tag': tag})
	{
		强求参数 request, 
		模板路径 template_name, 
		渲染参数 context=None,
		——————— context_instance=_context_instance_undefined,
		文档类型 content_type=None, 
		响应状态 status=None, 
		current_app=_current_app_undefined,
		dirs=_dirs_undefined,
		dictionary=_dictionary_undefined,
		引擎名称 using=None
	}	
		
	
	from django.template import Context, Template
	let tem = Template("My name is {% name %}")
	let con = Context({name:"ZZH"})
  	u =  tem.render(con)
		
	```
* 响应 HttpResponse
