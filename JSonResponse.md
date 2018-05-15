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
	```
	
* 数据格式化
	* dict
	* list
	* QuerySet
	* Model
	* object