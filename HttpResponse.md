#Response

响应    [Response](https://www.cnblogs.com/scolia/p/5635546.html)

> from django.http import HttpResponse,HttpResponseRedirect
> from django.template.response import TemplateResponse

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
	* HttpResponseBase 
	
		```
		status_code = 200
	    def __init__(self, content_type=None, status=None, reason=None, charset=None):
	    	content_type = 内容格式  [Content-Type:content_type]
	    	staus 状态码
	    	reason
	    	charset 字符集
	    	
	    def setdefault(self, key, value):
	    def __delitem__(self, header):
	    def __setitem__(self, header, value):
	    def __getitem__(self, header):
	    def get(self, header, alternate=None):
	    def items(self):
	    	请求头信息 操作
	    
	    def set_cookie(self, key, value='', max_age=None, expires=None, path='/',
                   domain=None, secure=False, httponly=False):
       def delete_cookie(self, key, path='/', domain=None):
	    	cookies 操作
	    	
	    def write(self, content):
	    def flush(self):
	    	Response内容操作
		```
	* HttpResponse(HttpResponseBase)
	
		```
		字符串拼接响应内容
		streaming = False
		init
			content：表示返回的内容，字符串类型 u""
			*args HttpResponseBase __init__
			**kwargs HttpResponseBase __init__
		Dome:
			t1 = loader.get_template('polls/index.html')
    		context = RequestContext(request, {'h1': 'hello'})
    		return HttpResponse(t1.render(context))
		
		```
	* SimpleTemplateResponse
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
			[JSonResponse.md]
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