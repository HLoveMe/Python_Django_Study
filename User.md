# USer

```
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
	 id
	 password
	 last_login
	 is_superuser
	 username
	 first_name
	 last_name
	 email
	 is_staff
	 is_active
	 date_joined
    nick_name = models.CharField(
        max_length=50,
        verbose_name=u"昵称",
        default="",
        null=False,
        blank=False
    )
    
Setting 配置

makemigrations Users
migrate
//yes 删除
    
```
* User对象

	```
		from django.contrib.auth import authenticate,login,get_user_model,get_user,update_session_auth_hash
		在已经登入login情况下
		get_user_model
		get_user
		得到当前对象
	```
* 注册

	```
		from django.contrib.auth import authenticate
	```

* 登录和验证

	```
		from django.contrib.auth import authenticate,login
		
		网页提交账号 密码
		name = request.POST.get("name","")
		pws = request.POST.get("pws","")
		//数据库认证用户
 		user = authenticate(xx=name,oo=pws)
 		if user:
 			//登入
 			/**
 				request中注册user变量
 				设置session,cookies
 				之后的请求会自动认证user
 			*/
 			login(request,user)
 			.
 			return response
	```
	
* 退出  注销

	```
	from django.contrib.auth import logout
	
	//清除request user
	logout(req)
	```