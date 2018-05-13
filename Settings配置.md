# Setting配置
* DEBUG 会把错误信息直接返回给前段

* BASE_DIR  执行项目更目录

* ROOT_URLCONF urls根目录文件 

* 数据库配置

	```
		DATABASES = {
		    'default': {
        		 "ENGINE": 'django.db.backends.mysql',
        		 "NAME": "MuStudy",
        		 "USER":"root",
        		 "PASSWORD":"4634264015",
        		 "HOST":"localhost",
    		}
		}
	```
* 模板文件配置
	
	```
	TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [
        		os.path.join(BASE_DIR, 'templates'),
        		"其他路径模板方法路径"
        	],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
	```
* 静态文件路径配置

	```
		解决js css images
		STATIC_URL = '/static/'
		STATIC_ROOT = os.path.join(BASE_DIR,'static')
		STATICFILES_DIRS = [
		    os.path.join(BASE_DIR,"static"),
		    ...
		    公用的
		    os.path.join(BASE_DIR,"common"),jq...
		]
	```
* 时区设置
	
	```
	LANGUAGE_CODE = 'zh-Hans'

	TIME_ZONE = 'Asia/Shanghai'

	USE_TZ = False
	```
* Media 配置

	```
	MEDIA_URL = '/media/'
	MEDIA_ROOT = os.path.join(BASE_DIR,'media')
	```
* App配置
	
	```
		INSTALLED_APPS = [
	    'django.contrib.admin',
    	'django.contrib.auth',
	    'django.contrib.contenttypes',
    	'django.contrib.sessions',
	    'django.contrib.messages',
   		 'django.contrib.staticfiles',
   		 "USer-APPNAME",
   		 "Apps.User"
		.....   		 
	]
	```
* 事务
	
	```
		全局事务（可选）
		ATOMIC_REQUESTS = True
		
	```
* User继承AbstractUser之后配置
	
	```
		AUTH_USER_MODEL = 'Users.UserProfile' "APPName.userName 不需要models"
	```