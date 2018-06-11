# 部署

```
1:前提Django能在本地运行

2:FileZilla上传到腾讯云服务器
	》服务器 开放 端口
	》安装和本地开发一致的Django环境 和 虚拟环境
	
2-3:以下命令都是虚拟环境中

3:测试
	项目目录下 python manage.py runserver 0.0.0.0:8000
	浏览器输入119.29.17.140:8000可以正常访问
	
4:安装uwsgi
	pip install uwsgi
	测试uwsgi->python
		创建Dome.py上传到任意目录下
		def application(env, start_response):
		    start_response('200 OK', [('Content-Type','text/html')])
	    	return [u"Hello World"]
	    在该目录下 uwsgi --http :8001 --wsgi-file Dome.py
		浏览器 http://119.29.17.140:8001 是否可以访问
		
	测试uwsgi ->django
	切换到django目录
		uwsgi --http :8888 --module MuOnlie.wsgi 
			(setting.py 同级目录下的wsgi.py)
	http://119.29.17.140:8888 是否可以访问
	
5.配置uwsgi
	工程目录下创建 online_uwsgi.ini
		[uwsgi]
		# uwsgi监听的socket，一会儿配置Nginx会用到
		socket = 127.0.0.1:8000
		# 在app加载前切换到该目录，设置为Django项目根目录
		chdir           = /home/ubuntu/Django/MuOnlie
		# 加载指定的python WSGI模块，设置为Django项目的wsgi文件
		module          = MuOnlie.wsgi
		home            = /home/ubuntu/.virtualenvs/Py3Django
		# 启动一个master进程来管理其他进程
		master          = true
		# 工作的进程数
		processes       = 8
		# 每个进程下的线程数量
		threads = 3
		# 当服务器退出的时候自动删除unix socket文件和pid文件
		vacuum          = true
		# 使进程在后台运行，并将日志打到指定的日志文件或者udp服务器
		daemonize = /home/ubuntu/Django/MuOnlie/logs/uwsgi.log //日志
		
	运行 uwsgi --ini online_uwsgi.ini
	
6:安装ngnix
	sudo apt-get install nginx
	
	sudo /etc/init.d/nginx start
		浏览器 http://119.29.17.140:80正常访问
	
	工程目录下创建uwsgi_params  内容为
		https://github.com/nginx/nginx/blob/master/conf/uwsgi_params
		
	项目静态文件
		
	创建配置文件
		online_nginx.conf
			upstream django {
			    server 127.0.0.1:8000; //uwsgi对应的地址
			}
			
			# configuration of the server
			server {
			    listen      8888;//这里是外网访问的端口
			    server_name 119.29.16.140; //ip 或者 域名
			    charset     utf-8;
			    client_max_body_size 75M;
			    access_log     /home/ubuntu/Django/MuOnlie/logs/online_access.log;  //访问日志
			    error_log      /home/ubuntu/Django/MuOnlie/logs/online_error.log; //错误日志
			    location /media  {
			        alias /home/ubuntu/Django/MuOnlie/media;
			    }
			    location /static {
			        alias /home/ubuntu/Django/MuOnlie/static;
			    }
			    location / {
			        uwsgi_pass  django;
			        include     /home/ubuntu/Django/MuOnlie/uwsgi_params;
			    }
			}
			
	增加配置文件到ngnix
		sudo ln -s ~/Django/MuOnlie/conf/online_nginx.conf /etc/nginx/sites-enabled/

	重启nginx
	
	/var/log/nginx
		系统默认的日志路径
		
7.文件更新
	uwsgi 配置更新
		uwsgi --ini online_uwsgi.ini
		
	nginx 配置或者django文件更新
		配置更新
			路径和之前保持不变
				> 上传配置
				> 不需要 cd /etc/nginx/sites-enabled/ ;删除配置
				> 不需要 sudo ln -s ~/Django/MuOnlie/conf/online_nginx.conf /etc/nginx/sites-enabled/
				> 重启
			路径改变
				执行上面四步rr
		文件更新
			重启 uwsgi

	
```

* 相关命令

	```
	uwsgi
		sudo killall -9 uwsgi
		uwsgi --ini online_uwsgi.ini
	
	nginx
		/etc/init.d/nginx start;启动
		sudo /etc/init.d/nginx restart;重启
		sudo nginx -t检查配置是否正确
		
		关闭
		ps -ef | grep nginx
		kill -QUIT pid
	```