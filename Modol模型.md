#Modal ORM

* model

```
Apps.User.models.py
from django.db import models
	
# _*_ coding:utf-8 _*_
class USerMessage(models.Modal):
	id =  AutoField
	多表关联
		一对一
			OntoOneField()
		一对多（部门Dep  -- n消息Msg）
			class Dep:
				pass
			class Msg:
				dep	 = models.ForeignKey(Dep)  
		多对多(老师T -- 学生 S)
			class Student:
				pass
			class Teacher:
				students = models.ManyToManyField(Student)
			
	def__unicode__(self):
		return self.type+":"+self.name
	def__str__(self):
		return self.type+":"+self.name
	def get_absolute_url():
		return "相对|绝对url" admin | resolve_url | redirect 使用得到该model访问地址	
		
	class Meta: (内部类指明一些属性)
		abstract = true/false是否为抽象类
			定义一些公共属性 不会被数据库化
		app_label= "APPName 指定是哪个App下"
		db_table = "自定义表名"
		verbose_name = "为你的类取个理解的名字"
		verbose_name_plural = verbose_name 复数形式名称 默认加个 s
		ordering = "-id" 排序
		...
		pass
	pass
	
>  Run Tasks	
>  创建或更新 makemigrations appname
>  创建或更新 migrate appname
```
* 属性
	
	```
	属性如果传递的是函数 只需要传递地址
		def up_to(req,imgF):
			return "icons/%Y/%h/%m"
		from datetime import datetime
		//datetime.new()
		ImageField(upload_to=up_to,default = datetime.new)
		
	图片
		ImageField(
			upload_to{
				"硬编码"  "icon/" ==> "mediaP/icon/filename"
				strftime "icon/%Y/%m/%d" "mediaP/icon/年/月/日/fileName"
				func -> path def upload_to(instance, fielname):pass
			}
		)
		
	```
	
* 数据库操作 [事务](https://blog.csdn.net/ysjian_pingcx/article/details/51015988)----[OCRD](https://www.cnblogs.com/shizhengwen/p/6588834.html)

	```
操作器
		Students.objects | stu.XX_set
多表关联
		逆向查询  obj.[表名_set].all()
	一对多
		逆向 dep.msg_set.all() 部门 查找 (部门消息)
	多对多
		正向 t.students.all()  老师查找学生
		逆向 s.teacher_set.all() 学生查找老师
事务 (尽量不要捕获异常)
		* Setting 事务配置
	
		1:ATOMIC_REQUESTS  = True
			> 所有web都开启事务  开销大
			> 放水水 @transaction.non_atomic_requests
		2:@transaction.atomic
			> 指定某个方法开启事务
		3:上下文
			from django.db import transaction
			def updateuser(request):
				...
				with transaction.atomic():
					...
				
	QuerySet查询出来的都是该对象
		支持便利 
		支持pickle到本地
		支持链式操作
		
		.exists()是否有记录
		[:10]切片 可以节省内存 或 reverse
		list(qs) 作为list
		.query.__str__() 查看sql语句
		
		
	增删改查 基于QuerySet
	增:
		创建
			1:obj = Author(name="WeizhongTu", email="tuweizhong@163.com")
			2:obj = Author() twz.name="WeizhongTu"
			3:obj = Author.create(name=,age=)
			4:obj = Author.objects.get_or_create(name="WeizhongTu", email="tuweizhong@163.com")
		保存
			obj.save()
		关联
			正向 person.book.add(book)
			逆向 book.person_set.add(person)
	删:
		Publisher.objects.get(name="O'Reilly").delete()
		关联
			正向 Person.objects.get(id=1).book.remove(obj)
			逆向 book.person_set.remobe(obj)
			
	改:
		1:Book.objects.filter().update(name=xxx)
		2:先查询 改属性 在save
	查找:
		单个 （必须是一个结果）
			Student.objects.get(id = 8286) try/catch
		所有
			Student.objects.all() 		得到所有
		过滤
			Student.objects.filter(.条件.)  得到指定属性的对象
			Student.objects.exclude()  排除满足条件的
		去重
			distinct()
		排序
			Student.objects.order_by()使用Model指定的排序规则
			Student.objects.order_by("id | -id")
			Student.objects.filter(.条件.).order_by
		限制 切片
			limit(0,100) 、offset(10)、[0:10]  reverse
		数量
			count()
			
		extra  实现 别名，条件，排序
			.extra(select={"别名":"名称"})
			
		排除
			.defer("name") 查询结果中排除某些字段
		反转|倒叙
			切片不知道 负数
			reverse()
			Person.objects.all().reverse()[:2] 去最后2条
			
		关心属性	
			values("name","qq") 
				[{"name":1,"qq":2},...]
			values_list("","") //一个属性 或者多个 
				values_list("name","email") //多个
					[(name,email),(name,email),...]
				values_list("name",flat=True) //单个
					[name,name,,...]
		关心属性
			.only("name")  =>[Modol,Model,..]
			
		聚合函数 
			from django.db.models import Avg
			Avg Max Min Count
			Book.objects.aggregate(Avg("age"),Count("name"),max_Money = Max("count"))
				=>{
					age_avg:100,
					max_Money:8286
				}
			annotate 聚合 (为结果每一项 都加上一个属性（聚合结果)）
				Article.objects.values("author").annotate(Count('authors') | au_co=Count('authors'))
				每个作者的文章数
					1:分组
					2:在组中进行其他的数据统计（annotate）
				[{...,authors_count:11|au_co:11},{}]
			
			表关联 __ 双下划线
				Author.objects.filter().annotate(min_price=Min("book__price"),max_price = Max("book__price"))
				
			自定义聚合功能
				https://code.ziqiangxuetang.com/django/django-queryset-advance.html
				
		分组Gorup By
			班级人  按age划分
			Student.objects.values("age").annotate(count=Count("age"))
			分为两步
		
		多表关联查询优化 (减少对数据库的查询次数)
			一对一 和 多对一  在(多,一 "前")的一方查询 并把关联的 一 查询出来 
				Author  1:n  Article(author外键)
				一般
					art = Article.objects.get(id = 8286) 会查询一次得到Article
					art.author.id 再次查询得到author
				优化 一次把关联的对象查询出来				
					Alticle.object.get(id=8286).select_related("author")
					
			一对多 多对多 
			 	Article(tags 多对多) n:n Tag
			 	一般
			 		arts = Article.objects.all()[:3]
			 		for ar in arts:
			 			print(ar.tags.all())
			 			
			 	优化
			 		articles = Article.objects.all().prefetch_related('tags')[:3]
					for a in articles:
						print a.title, a.tags.all()
			 	
			
		条件
			id = 1;  范围
			id__gt = 1; id>=1 范围
			id__lt = 1; id<=1 范围
			(id__lt=10,id__gt=1) 1<=id<=10 范围
			id__in = [1,2,3] 范围
			id__range = [1,10] 范围
			name__contains = "ZZh" 包含
			name__icontains = "ZZH"包含 大小写不敏感
			startswith，istartswith, endswith, iendswith
			name__regex = r""  正则表达式
			publish_date__date = datetime.date(2018,5,9)日期
			生日__year = 1993  时间
				mouth day week_day hour minute second
			
	```
	
* 参数 属性 说明	
[参数说明](https://www.cnblogs.com/shizhengwen/p/6588834.html)

```				
关联 models.ForeignKey  ManyToManyField
 ManyToManyField：
 		to, 
  		related_name=None,   防止一个类关联多个 取代 [表名_set]
	    related_query_name=None,
       limit_choices_to=None, 
       symmetrical=None, through=None,
       through_fields=None, 
       db_constraint=True, 
       db_table=None,
       swappable=True
ForeignKey         
       to, 
       on_delete=None, 
       related_name=None, 
       related_query_name=None,
       limit_choices_to=None, 
       parent_link=False, 
       to_field=None,
       db_constraint=True, 
		
所有字段和特殊属性

	AutoField 一个能够根据可用ID自增的 IntegerField 

	BooleanField 一个真／假（true/false）字段

	CharField (max_length)一个字符串字段，适用于中小长度的字符串。对于长段的文字，请使用 TextField

	CommaSeparatedIntegerField (max_length)一个用逗号分隔开的整数字段

	DateField ([auto_now], [auto_now_add])日期字段

	DateTimeField 时间日期字段,接受跟 DateField 一样的额外选项 年月日 时分秒

	EmailField 一个能检查值是否是有效的电子邮件地址的CharField 

	FileField (upload_to)一个文件上传字段

	FilePathField (path,[match],[recursive])一个拥有若干可选项的字段，选项被限定为文件系统中某个目录下的文件名

	FloatField (max_digits,decimal_places)一个浮点数，对应Python中的 float 实例

	ImageField (upload_to, [height_field] ,[width_field])像 FileField 一样，只不过要验证上传的对象是一个有效的图片。

	IntegerField 一个整数。

	IPAddressField 一个IP地址，以字符串格式表示（例如： "24.124.1.30" ）。

	NullBooleanField 就像一个 BooleanField ，但它支持 None /Null 。

	PhoneNumberField 它是一个 CharField ，并且会检查值是否是一个合法的美式电话格式

	PositiveIntegerField  和 IntegerField 类似，但必须是正值。

	PositiveSmallIntegerField  与 PositiveIntegerField 类似，但只允许小于一定值的值,最大值取决于数据库.

	SlugField 嵌条 就是一段内容的简短标签，这段内容只能包含字母、数字、下划线或连字符。通常用于URL中

	SmallIntegerField 和 IntegerField 类似，但是只允许在一个数据库相关的范围内的数值（通常是-32,768到+32,767）

	TextField 一个不限长度的文字字段

	TimeField 时分秒的时间显示。它接受的可指定参数与 DateField 和 DateTimeField 相同。

	URLField 用来存储URL的字段。

	USStateField 美国州名称缩写，两个字母。

	XMLField (schema_path) 它就是一个 TextField ，只不过要检查值是匹配指定schema的合法XML。

	字段通用属性

	null 如果设置为 True 的话，Django将在数据库中存储空值为 NULL 。默认为 False 。 

	blank 如果是 True ，该字段允许留空，默认为 False 。

	choices 一个包含双元素元组的可迭代的对象，用于给字段提供选项。

	db_column 当前字段在数据库中对应的列的名字。

	db_index 如果为 True ，Django会在创建表格（比如运行 manage.py syncdb ）时对这一列创建数据库索引。

	default 字段的默认值

	editable  如果为 False ，这个字段在管理界面或表单里将不能编辑。默认为 True 。

	help_text  在管理界面表单对象里显示在字段下面的额外帮助文本。

	primary_key 如果为 True ，这个字段就会成为模型的主键。

	radio_admin  默认地，对于 ForeignKey 或者拥有 choices 设置的字段，Django管理界面会使用列表选择框（<select>）。如果 	radio_admin 设置为 True 的话，Django就会使用单选按钮界面。 

	unique 如果是 True ，这个字段的值在整个表中必须是唯一的。

	unique_for_date 把它的值设成一个 DataField 或者 DateTimeField 的字段的名称，可以确保字段在这个日期内不会出现重复值。

	unique_for_month 和 unique_for_date 类似，只是要求字段在指定字段的月份内唯一。

	unique_for_year 和 unique_for_date 及 unique_for_month 类似，只是时间范围变成了一年。

	verbose_name 说明文字 除 ForeignKey 、 ManyToManyField 和 OneToOneField 之外的字段都接受一个详细名称作为第一个位置参数。
	
	Meta:
		https://www.cnblogs.com/flash55/p/6265405.html