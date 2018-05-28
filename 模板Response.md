#模板文件

* 模板文件配置
	* Setting.md 模板路径配置
* 模板数据来源
	* {% url 'urlname' args1 args2 ... %}
	* {{ name }}
	* ...
	* render(context参数)
	* 系统内置 processors
		* {% csrf_token %}
		* {% STATIC_URL %}
		* {% MEDIA_URL %}
		* {% request %}
		* {% request.get_host %} ==== get_host()
		* {% 自定义啊 %}
* 内置函数
	
	```
	from django.template import *
		Template 模板对象 html文件
		loader 用于加载Template
		
		Context 渲染使用的上下文
		RequestContext 子类 提供公共参数 (模板使用全局变量     静态变量)
	```
* 加载过程
	* 读取模板文件（字符串内容）
	* 内容渲染渲染

		```
		1:手动0
		from django.template import Context, Template 
			t = Template('My name is {{ name }}.') 
			c = Context({'name': 'nowamagic'}) 
			u"S" = t.render(c)
			HttpResponse(t.render(c))
		
		2:手动A
		from django.template import RequestContext, loader
			tem = loader.get_template("htmlpath.html")
			context = RequestContext(request,content={},...)
			u"S" = tem.render(context)
			response = HttpResponse(tem.render(context))
			
		2:手动B
			from django.template.response import TemplateResponse
				集成手动A多个步骤
				推荐 render render_to_resoponse
				
		3：快速（render 是render_to_response 全新的方式）
			return render_to_response('blog_add.html',{'blog': blog, 'form': form, 'id': id, 'tag': tag},
                          context_instance=RequestContext(request))
			return render(request, 'blog_add.html', {'blog': blog, 'form': form, 'id': id, 'tag': tag})
		```	
* 模板视图 TemplateView  [View.md]

	```
		一种 模板View 视图
		method 过滤
		context 参数提供
		模板配置 和 渲染
		url 配置
		View request处理
		
	```
* API说明

	* Template 一个html文件的抽象
	* loader 加载器
	
	* Context | RequestContext 渲染上下文
	
		```
		{
			Context({"name":"ZZH"})
			RequestContext 支持变量再次利用
				def AA(request):
					return {...}
				
				render_to_response(
					context_instance = RequestContext(request,{},processors= [AAA,...] )
				)
			
		为了方便在所有RequestContext的processors使用共同链
		Settings
			TEMPLATES{
				OPTIONS:{
					context_processors:{
						....公用的链
						'django.template.context_processors.request',			
						该目录下还有很多
							csrf {% csrf_token %}
							static  STATIC_URL
							debug
							tz   时区 TIME_ZONE
							i18n  {LANGUAGES|LANGUAGE_CODE|LANGUAGE_BIDI}
							media   MEDIA_URL
							request {%request%}
						'django.contrib.auth.context_processors.auth'
						...
						自定义"App.xx.oo.aa.funcName 类似AA函数"
					}
				}
			}
		}
		```
	
	* render
	
		```
		{
			请求参数 request, 
			模板路径 template_name, 
			渲染参数 context=None,
			上下文对象 context_instance = RequestContext(默认的) | Context,
			文档类型 content_type=None, 
			响应状态 status=None, 
			current_app=_current_app_undefined,
			dirs=_dirs_undefined,
			dictionary=_dictionary_undefined,
			引擎名称 using=None
	}
		```	
	
	* render_to_response
	
		```
	(
			模板路径 template_name, 
			渲染参数 context=None,
			上下文对象 context_instance= Context(默认的) | RequestContext 
			文档类型 content_type=None, 
			响应状态 status=None, 
			dirs=_dirs_undefined,
			dictionary=_dictionary_undefined, 
			引擎名称 using=None
	)
	```
	
	* SimpleTemplateResponse | TemplateResponse

		```
		SimpleTemplateResponse
			__init__(template, context=None, content_type=None, status=None, charset=None, using=None)
				 template 模板
				 context上下文
				 content_type = 内容格式  [Content-Type:content_type]
			    staus 状态码
			    charset 字符集
			    using 渲染引擎
			resolve_template(template)
				通过 template名字 得到Template实例
			resolve_context(context)
				预处理即将用于渲染模板的上下文数据
			add_post_render_callback()
				添加一个渲染之后调用的回调函数。
				这个钩子可以用来延迟某些特定的处理操作（例如缓存）到渲染之后。
				def my_view(request):
				   response = TemplateResponse(request, 'mytemplate.html', {})
				   //创建 但是还没渲染
				   //my_render_callback 传递一个reponse实例
				   //如果已经渲染 回调会被立即执行
			      response.add_post_render_callback(my_render_callback)
				   return response
			render()
				渲染
				
		TemplateResponse	
			__init__(self, request, template, context=None, content_type=None,
            status=None, current_app=_current_app_undefined, charset=None,
            using=None):
            
		```

