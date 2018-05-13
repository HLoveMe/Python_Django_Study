#替换Django admin使用xadmin

* 源码下载

	```
		https://github.com/sshwsfc/xadmin
	```
* 依赖安装
	
	```
	1	
		pip install xadmin
		pip uninstall xadmin  会保留依赖
	
	2：pip install django-import-export
	3: pip install  six
	4: pip install future
	```
* 配置
	
	```
	1:创建extra_apps文件夹 加入路径搜索
		sys.path.append(os.path.join(BASE_DIR,"extra_apps"))
	2：设置app
		"extra_Apps.xadmin",
	    "crispy_forms"
	3：urls配置
		from Extra_Apps import xadmin
		urlpatterns = [
		    # url(r'^admin/', admin.site.urls),
		    url(r'^xadmin/', xadmin.site.urls),
		]
	4:数据库同步
		makemigrations
	```

* 模型注册
	
	```
	User不需要注册
	1:在App下创建admix.py文件  
		from Extra_Apps import xadmin
		from .models import EmailVerify
		class EmailVerifyAdmin(object):
   		 	pass
		xadmin.site.register(EmailVerify,EmailVerifyAdmin);
	```