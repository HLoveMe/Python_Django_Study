# USer

[文档](https://docs.djangoproject.com/en/1.11/topics/auth/customizing/)

* 模型 （AbstractBaseUser）
	
	* 文档跳转 MyUser
	
* 模型 (AbstractUser )

	```
	from django.contrib.auth.models import AbstractUser
	
	AbstractUser(AbstractBaseUser用户, PermissionsMixin权限):
	class User(AbstractUser):
		//默认的 基础属性
		id
		password
		last_login
		username
		first_name
		last_name
		email
		is_superuser 是否为超级用户
		is_staff 是否为Admin站点管理员
		is_active  是否被激活
		date_joined
		//自己添加、、
		nick_name = models.CharField(
			max_length=50,
			verbose_name=u"昵称",
			default="",
			null=False,
			blank=False
		)
		//
		
		//方法
		is_authenticated() 是否已经被认证
		set_password(raw_password)  设置新密码
		check_password(raw_password) 检查密码是否正确
		set_unusable_password() 设置为没有设置密码 不是 空密码
		
		
		USERNAME_FIELD = "id"指明唯一标示
		REQUIRED_FIELDS = ['date_of_birth', 'height']仅仅在存在superuser时提示用户
		REQUIRED_FIELDS = [不要 包含唯一标识 和  password]//创建时必填字段
		
	    
	配置 Setting 
		AUTH_USER_MODEL = 'customauth.MyUser'  "Users.UserProfile"
	更新数据库
		makemigrations Users
		migrate
		//yes 删除
	    
	```
* User操作
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
			//数据库认证用户 见（自定义User认证）
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
* USer  表单
	
	```
	AbstractUser 默认表单
	提供给Admin管理器
	或者渲染在页面中
	
		AuthenticationForm
		SetPasswordForm
		PasswordChangeForm
		AdminPasswordChangeForm
		PasswordResetForm:
		UserCreationForm
		UserChangeForm
		
	创建你的表单
		class CustomUserCreationForm(UserCreationForm):
		    class Meta(UserCreationForm.Meta):
		        model = CustomUser
		        fields = UserCreationForm.Meta.fields + ('custom_field',)
	完全自定义
		文档跳转  UserChangeForm
	```
* 自定义User认证
	
	```
	用于用户登入 | 权限认证 
	from django.contrib.auth.backends import ModelBackend
	from django.contrib.auth.hashers import check_password
	from django.conf import settings
	from django.contrib.auth.hashers import check_password
	from django.contrib.auth.models import User
	class HUserBackend(ModelBackend):
		//最基本的 authenticate方法
	    def authenticate(self, request, username=None, password=None, **kwargs):
	        try:
	            user = UserProfile.objects.get(Q(username=username) | Q(email=username))
	            if user and user.check_password(password):
	                return user
	
	        except Exception as e:
	            return None
	            
	     def has_perm(self, user_obj, perm, obj=None):
	     def get_user(self, user_id):
	     .....
	            
	class  HLoveMeBackend(object):
		def authenticate(self, request, username=None, password=None):
		def get_user(self, user_id):
		....
		
			
	class SettingsBackend(object):
	    """
	    自动注册Admin管理者
	    ADMIN_LOGIN = 'admin'
	    ADMIN_PASSWORD = 'pbkdf2_sha256$30000$Vo0VlMnkR4Bk$qEvtdyZRWTcOsCnI/oQ7fVOu1XAURIZYoOZ3iq8Dr4M='
	    """
	
	    def authenticate(self, request, username=None, password=None):
	        login_valid = (settings.ADMIN_LOGIN == username)
	        pwd_valid = check_password(password, settings.ADMIN_PASSWORD)
	        if login_valid and pwd_valid:
	            try:
	                user = User.objects.get(username=username)
	            except User.DoesNotExist:
	                # Create a new user. There's no need to set a password
	                # because only the password from settings.py is checked.
	                user = User(username=username)
	                user.is_staff = True
	                user.is_superuser = True
	                user.save()
	            return user
	        return None
	
	    def get_user(self, user_id):
	        try:
	            return User.objects.get(pk=user_id)
	        except User.DoesNotExist:
	            return None
	
	settings.py
		AUTHENTICATION_BACKENDS	 = (
			"HUserBackend path",
			"HLoveMeBackend path",
			"SettingsBackend path"
		)
		
	认证顺序
		一直验证 直到返回User 或者BACKENDS链完成
	```
	
* User模型管理器
	* 系统为默认User有默认的管理器
	* 如果你的完全自定义的User 。 如果有上述全部基础数据 就会使用系统管理器 否则需要自定义管理器
	* 自定义管理器

		```
		from django.contrib.auth.models import BaseUserManager
			class AUserManager(BaseUserManager):
				def create_user(*username_field*, password=None, **other_fields):
				pass
			def create_superuser(*username_field*, password, **other_fields):
				pass
			
		class USer(object):
			objects = AUserManager()
		
		dome:
			上面文档跳转  MyUserManager
		```
	
* Admin管理者
	
	```
		用户 和Admin管理者 有区别
			用户 不是  Admin管理者
			Admin管理者 是 用户
		继承 AbstractBaseUser | AbstractBaseUser 的User模型
			django 会提供默认的Admin管理器	
		自定义的Admin管理器
			文档跳转 UserAdmin
			
		
		1:creatsuperuser >=  Admin管理者
		2:用户(AbstractUser) 成为 Admin管理者
			class YouUser(AbstractUser):
				is_admin 自己增加的 
				is_staff 必须返回true
					@property
					def is_staff(self):
						return self. is_admin
				is_active true
				
				PermissionsMixin 权限父类
					has_perm(self,perm, obj=None):
						具有命名权限，则返回True。
						如果提供obj参数，则需要针对特定​​对象实例检查权限。
						默认 this - > PermissionsMixin - > 认证类(或者自定义User认证)
					has_module_perms(self,app_label):
						True 是否有该app的权限
						
					。。。
					
		3:自己的User (PermissionsMixin)
			文档跳转 PermissionsMixin 
	```
	
