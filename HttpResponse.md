#Response

响应    [Response](https://www.cnblogs.com/scolia/p/5635546.html)

> from django.http import HttpResponse,HttpResponseRedirect

```
	每一个request 都要进行对于的响应
	
HttpResponse
	最基础的响应
	
HttpResponseRedirect 302	
HttpResponsePermanentRedirect 301
	重定向
	
SimpleTemplateResponse
TemplateResponse	
	模板响应
	模板渲染  [模板Response.md]
	
JsonResponse
	Json API接口
	(data, encoder=DjangoJSONEncoder, safe=True, json_dumps_params=None, **kwargs)
	
StreamingHttpResponse
	流响应
	
FileResponse
	文件响应
	
from django.http import *
	HttpResponseNotFound 404
	HttpResponseForbidden 403没有权限
	HttpResponseNotAllowed 405请求方式不对 get post...
	HttpResponseBadRequest 400
	HttpResponseGone 401
	HttpResponseServerError 500
	HttpResponseNotModified 304
	
快捷函数 生成Response
from django.shortcuts import render
	render()   下面
	render_to_response() 下面
	redirect() 重定向 下面
	resolve() 逆向 [urls.md]
	resolve_url() Model("get_absolute_url")/ViewName/url得到 url
	get_object_or_404()
	get_list_or_404()
	
```

* 函数 类 说明
	* HttpResponse
	
		```
		字符串拼接响应内容
		init
			content：表示返回的内容，字符串类型
			charset：表示response采用的编码字符集，字符串类型
			status_code：响应的HTTP响应状态码
			content-type：指定输出的MIME类型
			
		write(content)：以文件的方式写
		flush()：以文件的方式输出缓存区
		set_cookie(key, value='', max_age=None, expires=None)：设置Cookie
Dome:
		t1 = loader.get_template('polls/index.html')
    context = RequestContext(request, {'h1': 'hello'})
    return HttpResponse(t1.render(context))
		
		```
	* TemplateResponse
		* [模板Response.md]
		
	* HttpResponseRedirect
	* HttpResponsePermanentRedirect
	
		```
		重新请求一个url
		HttpResponseRedirect("相对|绝对路径")
		反向得到url [urls.md]
			reverse("name",args=(1,))
		```
	* JsonResponse
		
		```
			json格式的数据
			JsonResponse(
				data, python字典
				encoder=DjangoJSONEncoder,Json解析器
				safe=True,
					默认为True 只允许dict 实例
					False 可以传递任何对象进行序列化
				json_dumps_params=None,
					con = json.dumps(data,这里的参数)
				**kwargs)
		```
	* redirect
		
		```
			redirect(to,*args,**kwargs)
				**kwargs permanent = True 使用 HttpResponsePermanentRedirect
							permanent = false HttpResponseRedirect
						
				to {
					Model get_absolute_url
					相对|绝对url ---- url
				    view name --> url
 				}
				
		```