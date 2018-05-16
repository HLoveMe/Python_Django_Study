#JsonResponse

用作API

* HttpResponse

	```
	import json  
	from django.http import HttpResponse  
	response_data = {}  
	response_data['result'] = 'failed'  
	response_data['message'] = 'You messed up'  
	return HttpResponse(json.dumps(response_data),content_type="application/json")  
	```
	
* JsonResponse

	```
		from django.http import JsonResponse
		JsonResponse(
				data, python字典
				encoder=DjangoJSONEncoder,Json解析器
				safe=True,
					默认为True 只允许dict 实例
					False 可以传递任何对象进行序列化
				json_dumps_params=None,
					con = json.dumps(data,这里的参数)
				**kwargs
					Response 其他__init__参数
				)
		return JSonRequest(json.dumps({}))
	```
	
* 数据格式化
	* dict
	* list
	* QuerySet
	* Model
	* object
	
	```	
	pip install django-simple-serializer

	from dss.Serializer import serializer
	
		serializer(
			data(Required|(QuerySet, Page, list, django model object))-待处理数据
			datetime_format(Optional|string)-如果包含 datetime 将 datetime 转换成相应格式.默认为 "timestamp"（时间戳）
				时间格式转 
				="string"  "2015-05-10 10:19:22"
				="timestamp"  "1432124420.0"
			output_type(Optional|string)-serialize type. 默认“raw”原始数据，即返回list或dict
				raw 将list或dict中的特殊对象序列化后输出为list或dict
				dict 同raw
				json  作为str
			include_attr(Optional|(list, tuple))-只序列化 include_attr 列表里的字段。默认为 None
			exclude_attr(Optional|(list, tuple))-不序列化 exclude_attr 列表里的字段。默认为 None
				include_attr |  exclude_attr = (xx,oo,..., foreign=True,..)
					foreign(Optional|bool)-是否序列化 ForeignKeyField 。include_attr 与 exclude_attr 对 ForeignKeyField 依旧有效。 默认为 False
					many(Optional|bool)-是否序列化 ManyToManyField 。include_attr 与 exclude_attr 对 ManyToManyField 依旧有效 默认为 False
			through(Optional|bool)-是否序列化 ManyToManyField 中 through 属性数据 默认为 True
		)
		
	1:HttpResponse
		return HttpResponse(serializer(Country.objects.all(),output_type="json"),content_type="application/json")
		return HttpResponse(json.dumps(serializer(Country.objects.all())),content_type="application/json")
		
		
	2:JsonResponse
		return JsonResponse({"data":serializer(Country.objects.all(),output_type="raw")})
		
	3:View(dss.Mixin)
		JsonResponseMixin
			主要是重写 render_to_response
			该方法会被DetailView的get调用,达到替换原先TemplateResponse效果
			
			from dss.Mixin import JsonResponseMixin
			from django.views.generic import DetailView
			class TestView(JsonResponseMixin, DetailView):
		    	model = Article
		    	datetime_type = 'string'
	    		pk_url_kwarg = 'id'
	    		用作单个信息查询
	    		
	    		
	    	url(r"^detail/?<P>/d+/$",as_view())
	    	
	    MultipleJsonResponseMixin(JsonResponseMixin)
	    	主要是重写 render_to_response
			该方法会被DetailView的get调用,达到替换原先TemplateResponse效果	    	class TestView(MultipleJsonResponseMixin, ListView):
			    model = Article
			    query_set = Article.objects.all()
			    paginate_by = 1
			    datetime_type = 'string'
				多条数据插叙
			
				# urls.py
				urlpatterns = patterns('',
			    	url(r'^test/$', TestView.as_view()),
				)
				
		FormJsonResponseMixin(JsonResponseMixin):
			CreateView、UpdateView、FormView
			class TestView(FormJsonResponseMixin, UpdateView):
			    model = Article
			    datetime_type = 'string'
			    pk_url_kwarg = 'id'
	```
	
* 实例
	
	```
	JsonResponse(serializer(Province.objects.filter(country=country).all(),exclude_attr=("country_id",)),safe=False)
	```