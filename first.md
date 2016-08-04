# 关于配服务器的一些乱七八糟 #
## LNMP ##
+ MySQL
	+ 简单，安装直接apt-get设置密码之类的。
	+ 记得配置编码utf8
+ PHP
	+ 需要安装php-fpm	
	+ php.ini需要做一点修改。进入`vim /etc/php5/fpm/php.ini`修改`cgi.fix_pathinfo=0`
	+ 让PHP支持其他模块如 `php5-mysql php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl`
+ nginx
	+ 下载简单
	+ 配置文件 `/etc/nginx/sites-available/default `
	+ 修改支持php的路由 

			 location ~ .php$ {
                try_files $uri =404;
                fastcgi_split_path_info ^(.+.php)(/.+)$;
                # NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini

                # With php5-cgi alone:
                #fastcgi_pass 127.0.0.1:9000;
                # With php5-fpm:
                fastcgi_pass unix:/var/run/php5-fpm.sock;
                fastcgi_index index.php;
                include fastcgi_params;
        	}
	+	让php-fpm使用TCP连接`vim /etc/php5/fpm/pool.d/www.conf`
			
			[...]
			;listen = /var/run/php5-fpm.sock
			listen = 127.0.0.1:9000
			[...]
		注释掉`fastcgi_pass unix:/var/run/php5-fpm.sock;`反注释掉`#fastcgi_pass 127.0.0.1:9000;`

## Ghost ##
+	Nodejs
	+	编译安装可以同时装好nodejs和npm
	+	apt-get需要分别安装nodejs和npm
	+	nvm 和 n用来做nodejs版本控制
+	ghost
	+	安装直接从官网
	+	默认为sqlist3数据库可改MySQL
	+	用forever来管理并后台运行
	+	生产环境和开发环境不同
				 
			production: {
        		url: 'http://honeybee.net.cn:2368',
        		mail: {},
        		database: {
            		client: 'mysql',
            		connection: {
		                host     : '127.0.0.1',
		                user     : 'root',
		                password : '',
		                database : 'ghost',
		                charset  : 'utf8'
		            },
		            debug: false
		        },
		        server: {
		            host: '0.0.0.0',
		            port: '2368'
		        }
		    },
		url加上端口号是因为nginx反代没做成功
+	nginx
	+	主要用来做反代
	+	按理说应该是
		 			
			server {
			    listen 80;
			    server_name honeybee.net.cn;
			
			    location / {
			        proxy_set_header   X-Real-IP $remote_addr;
			        proxy_set_header   Host      $http_host;
			        proxy_pass         https://127.0.0.1:2368;
			    }
			}
		不过并没有成功，要找出原因所在。


**add:** *重新安装了nginx，在其defalu中直接修改默认路由进行了反代，取得了成功。关于反代主要就是一些nginx的配置问题*