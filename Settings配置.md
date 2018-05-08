# Setting配置
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
	* 工程目录下的templates文件夹存放html文件
	
	```
	TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [
        		os.path.join(BASE_DIR, 'templates'),
        		"其他路径"
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
		STATICFILES_DIRS = [
		    os.path.join(BASE_DIR,"static"),
		    ...
		]
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