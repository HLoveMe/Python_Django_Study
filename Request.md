#Request

* 属性

	```
		scheme [http//https]
		
		body 一个字符串，代表请求报文的主体。在处理非 HTTP 形式的报文时非常有用，例如：二进制图片、XML等。
		
		path 一个字符串，表示请求的路径组件（不含域名）。
		path_info
		
		method "GET"、"POST"
		
		REQUEST 一个类似于字典的对象，它首先搜索POST，然后搜索GET，主要是为了方便（使用GET/POST）
		GET
		POST
		
		META 包含HTTP首部
			CONTENT_LENGTH —— 请求的正文的长度（是一个字符串）。
			CONTENT_TYPE —— 请求的正文的MIME 类型。
			HTTP_ACCEPT —— 响应可接收的Content-Type。
			HTTP_ACCEPT_ENCODING —— 响应可接收的编码。
			HTTP_ACCEPT_LANGUAGE —— 响应可接收的语言。
			HTTP_HOST —— 客服端发送的HTTP Host 头部。
			HTTP_REFERER —— Referring 页面。
			HTTP_USER_AGENT —— 客户端的user-agent 字符串。
			QUERY_STRING —— 单个字符串形式的查询字符串（未解析过的形式）。
			REMOTE_ADDR —— 客户端的IP 地址。
			REMOTE_HOST —— 客户端的主机名。
			REMOTE_USER —— 服务器认证后的用户。
			REQUEST_METHOD —— 一个字符串，例如"GET" 或"POST"。
			SERVER_NAME —— 服务器的主机名。
			SERVER_PORT —— 服务器的端口（是一个字符串）。
		
		user
			表示当前登入用户
			开启 AuthenticationMiddleware才会有
			
		session 开启回话支持
		
		resolver_match  ResolverMatch实例 表示解析的到url
		
	```
	
* 方法
	
	```
	get_host()
		127.0.0.1:8000
		
	get_full_path()
		/music/xx/00?id=1111
		
	build_absolute_uri(Aurl)
		返回Aurl的绝对url 
			Aurl存在	hhtp://localhost:8081/ss/ss/a?xx=1
			不存在 get_full_path()
			
	is_secure()
		是否是安全的https
		
	is_ajax()
		是否通过XMLHttpRequest
		
	read(sise)读文件一样读取内容（行为和文件存在一致）
		HttpRequest.readline()
		HttpRequest.readlines()
		HttpRequest.xreadlines()
	```