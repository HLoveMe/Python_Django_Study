# URLS说明

* 配置
	
	```
		ROOT_URLCONF = 'MuStudy.urls'
	```
* 说明

	```
	URL
		from django.conf.urls import url,patterns, include
		from  django.core.urlresolvers import reverse
		
		url(r,view,kwargs,name,prefix)
		{
			r"^user"  正则表达式 支持参数获取
			View      调用的Action
			karags    自定义参数传递给Action
			name      指定别名 用于反向获取url
		}
		
		Regex:
			A:r"^user/$"
			B:r"^user/[0-9]{4}/[0-9]{3}/$"  
			C:r"^user/(?P<name>[a-zA-Z]+)/$"
				A(reques):pass
				B(request,aa,bb):pass
				C:(request,name=""):pass
		karags
			url(r"^login/$",View,{name:xx,...},name="")
				Ac(request,name,...):pass
			如果是包含关系include 那么kwargs 会被所有的都继承
			
		name:name反向得到url
			from django.urls import reverse
			python: reverse(name,args=(444,555,...),kwargs={..})
			html:{% url name 参数1 参数2 ... %}
			ex:
				url(r"^login/[0-9]{4}/$",view,name="login")
				{% url "login" 8286 %} => "/login/8286/"
		
			存在命名空间
			python: reverse("空间名称:URL名称",args=(444,555,...))
			html:{% url "空间名称:URL名称" "V1" "V2" pk=12 pp=99 %}	
		
		
	：
	：
	：
	
	urlpatterns 合并
		urlpatterns = [
				url(r'^admin/', admin.site.urls),
				url(r'user/',APPurlpatterns),
				url(r"email/",include([
					url(r"^delete/",action),
					url(r"^read/",action),
				]))
				url(r"^blog/",include(
					"app.urls.aurls"//必须包含urlpatterns=[]
				),{kwargs全部继承})
				命名空间一个Appurls被多个应用区分
				url(r"^Ablog",include("blog.urls",namespace="Ablog"))
				url(r"^Bblog",include("blog.urls",namespace="Bblog")))
			]
		//编译  1.10.0移除
		urlpatterns + = patterns("appname.views指定AppView",
			(r"^bog/a","detail"),// def detail():pass
			(r"^blog/b","goto"),// def goto():pass
		)
	```	