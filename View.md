#View(基于类的视图)

基类

```
class ContextMixin:pass 
	提供模板 渲染参数 的功能
	def get_context_data(self, **kwargs):pass
	
class View(object):
	提供请求方式限制
	def http_method_not_allowed(self, request, *args, **kwargs):
	变为函数视图
	@classonlymethod
    def as_view(cls, **initkwargs):
    
class TemplateResponseMixin:pass
	template_name = None 指定模板
    template_engine = None 指定渲染引擎
    response_class = TemplateResponse
    content_type = None
	模板配置
	def get_template_names(self):return [template_name]
	模板渲染
	def render_to_response(self, context, **response_kwargs):
///	
class SingleObjectMixin(ContextMixin):
	单个model
    model = None 类型
    queryset = None
    slug_field = 'slug' 
    context_object_name = None 指定上下面中参数{key:object}
    slug_url_kwarg = 'slug'
    pk_url_kwarg = 'pk' 默认参数名称 url匹配参数
    query_pk_and_slug = False
    
    def get_object(self, queryset=None):
    	get_queryset中获取
    	自定义查询
    def get_queryset(self):pass
    	查询all() 可进行过滤等操作
    
class BaseDetailView(SingleObjectMixin, View):
	def get(self, request, *args, **kwargs):
		return response
		
class SingleObjectTemplateResponseMixin(TemplateResponseMixin):
	template_name_field = None
    template_name_suffix = '_detail'
    def get_template_names(self):
    	pass
///
class MultipleObjectMixin(ContextMixin):
    allow_empty = True 容许为空
    queryset = None
    model = None 模型
    paginate_by = 10  分页个数
    paginate_orphans = 0
    context_object_name = None 结果在上下文中的key{key:res}
    paginator_class = Paginator
    page_kwarg = 'page'  页码参数名
    ordering = None   是否倒叙
   
 class MultipleObjectTemplateResponseMixin(TemplateResponseMixin):
 	template_name_suffix = '_list'
 	def get_template_names(self):
 		pass
 
class BaseListView(MultipleObjectMixin, View):
 	def get():
	 	return response
///
```

视图

* TemplateView(TemplateResponseMixin, ContextMixin, View):
	
	```
自定义
	1：创建子类
	2：指定模板 参数指定
	3：重写get_context_data等需要重写方法
		exam:url(r"(?P<pk>\d+_.+)/$",BlogDetaile.as_view(),{"name":"ZZH"}),
		
		template_name=""
		....
		def get_context_data(self,**kwargs[url配置 或者 url匹配到的值]):
			context=super(BlogDetaile,self).get_context_data(**kwargs)
			con=kwargs.get("id",1)
			blog=Blog.objects.get(id=con.split("_")[0])
			context["object"]=blog
			return context
		
	```
	
* RedirectView(View)

	```
	class RedirectView{
		permanent = False
		url = None
   		pattern_name = None
   		query_string = False
    }
    url(r'^go-to-django/$', RedirectView.as_view(url=., permanent=.), name='go-to-django'),
	```

* DetailView(SingleObjectTemplateResponseMixin, BaseDetailView): 仅仅展示一条数据

	```
	1：创建子类
	2：指定模板 参数指定
	3：重写get_context_data等需要重写方法
	
	A:url(r'^blog/(?P<pk>\d+)/(?P<slug>[-_\w]+)/$', BlogDetailView.as_view(
		设置类属性model = BlogModal
	), name='detail'),  
	B:url(r'^ blog/(\d+)/(?P<slug>[-_\w]+)/$', BlogDetailView.as_view(
		设置类属性model = BlogModal
	), name='detail')  
	* 如果使用默认 这里的参数名 pk | sulg 这里系统默认pk指的查询主键
	* 自定义
		1:
			pk_url_kwarg = "IDD"
			url(r'^blog/(?P<IDD>\d+)/(?P<slug>[-_\w]+)/$', BlogDetailView.as_view(), name='detail'),
			其他代码无需修改
		2:
			IDD_URL_KWarg = "MYID"
			url(r'^blog/(?P<MYID>\d+)/(?P<slug>[-_\w]+)/$', BlogDetailView.as_view(), name='detail'),
			获取参数
			MYID =self.kwargs.get(self.IDD_URL_KWarg,None)
	
	class BlogDetailView(DetailView):
		model = BlogModal
		template_name = "blog_detail.html"
		//context_object_name = 'persons'
		def get_object(self,queryset=None):
			参数获取 self.kwargs.xxx
			 //pnum=int(self.kwargs.get(self.IDD_URL_KWarg,None))  
	        query=self.get_queryset()  
	        try:  
	            obj=query[pnum-1]
	        except IndexError:  
	            raise Http404  
	        return obj  
	        {
	        object = super(AuthorDetailView, self).get_object()  这里是使用 PK 字段        	object.last_accessed = timezone.now()    另外操作        	object.save()        	return object
        	}
	   	def get_queryset(self):
	   		return super(AuthorDetailView,self).get_queryset()
	        
    	def get_context_data(self,**kwargs):  
        	context=super(AuthorDetailView,self).get_context_data(**kwargs) 
        	参数获取 self.kwargs.xxx
        	return context  
	
		
	```

	![](./SingleV.png)
	
* class ListView(MultipleObjectTemplateResponseMixin, BaseListView):

	```
	1：创建子类
	2：指定模板 参数指定
	3：重写get_context_data等需要重写方法
	
	A:url(r'^blog/(?P<pk>\d+)/(?P<page>\d+)/$', BlogDetailView.as_view(
		设置类属性  model = BlogModal
	), name='detail'),  
	B:url(r'^ blog/(\d+)/(?P<slug>[-_\w]+)/?page=100', BlogDetailView.as_view(
		设置类属性 model = BlogModal
	), name='detail')  
	* 如果使用默认 这里的参数名 pk | sulg 这里系统默认pk指的查询主键
	* 自定义
		1:
			pk_url_kwarg = "IDD"
			url(r'^blog/(?P<IDD>\d+)/(?P<slug>[-_\w]+)/$', BlogListView.as_view(), name='list'),
			其他代码无需修改
		2:
			IDD_URL_KWarg = "MYID"
			url(r'^blog/(?P<MYID>\d+)/(?P<slug>[-_\w]+)/$', BlogListView.as_view(), name='list'),
			获取参数
			MYID =self.kwargs.get(self.IDD_URL_KWarg,None)
	
	class BlogListView(ListView):
		model = BlogModal
		template_name = "blog_detail.html"
		//context_object_name = 'persons'
		paginate_by = 10  分页个数
       page_kwarg = 'page'  页码参数名 url参数 或者 截取
		
	   	def get_queryset(self):
	   		return super(UserListView,self).get_queryset()
	        
    	def get_context_data(self,**kwargs):  
        	context= super(UserListView,self).get_context_data(**kwargs)  
        	参数获取 self.kwargs.xxx
        	return context 
	```
	![](./ListV.png)