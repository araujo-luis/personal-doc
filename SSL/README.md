
## SSL ON NGINX

SSL issuer should send a private key, crt file and a ca_bundle.

You need to concat the crt file with the ca_bundle

    cat yourdomain.crt yourdomain.ca_bundle >> yourdomain_bundle.crt

Also, you need to concat your private key and crt file in a single `.pem` or `.key` file


	cat yourdomain.crt yourdomain.key > yourdomain.pem

Make sure concatenation is done correctly, sometimes breaklines aren't copied properly like this:

````
-----END CERTIFICATE----------START CERTIFICATE-----
````

Update your file like this (5 dashes per header):

````
-----END CERTIFICATE-----
-----START CERTIFICATE-----
````

After that you can add this file in your nginx configuration, this config has a A grade in GTMetrix

**default.conf**

	server {
	    listen       80;
	    server_name  luisaraujo.io www.luisaraujo.io;
	    server_tokens off;
	    return 301 https://$host$request_uri;
	}

	server {
	    listen       443 ssl http2;
	    server_name  luisaraujo.io www.luisaraujo.io;
	    server_tokens off;
	    ssl_certificate /etc/ssl/luisaraujo_io_bundle.crt;
	    ssl_certificate_key /etc/ssl/luisaraujo_io.pem;

	 location / {
	        root   /usr/share/nginx/html;
	        index  index.html index.htm;
	        try_files $uri /index.html;
	        if ($request_uri ~* ".(ico|css|js|gif|jpe?g|png|json)$") {
	           expires 30d;
	           access_log off;
	           add_header Pragma public;
	           add_header Cache-Control "public";
	           break;
	        }
	    }

	    error_page   500 502 503 504  /50x.html;
	    location = /50x.html {
	        root   /usr/share/nginx/html;
	    }

	}
**nginx.conf**

	user  nginx;
	worker_processes  1;

	error_log  /var/log/nginx/error.log warn;
	pid        /var/run/nginx.pid;


	events {
	    worker_connections  1024;
	}


	http {
	    include       /etc/nginx/mime.types;
	    default_type  application/octet-stream;

	    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
	                      '$status $body_bytes_sent "$http_referer" '
	                      '"$http_user_agent" "$http_x_forwarded_for"';

	    access_log  /var/log/nginx/access.log  main;

	    sendfile        on;
	    #tcp_nopush     on;

	    keepalive_timeout  65;

	    gzip on;
	    gzip_disable "msie6";
	    gzip_comp_level 6;
	    gzip_min_length 1100;
	    gzip_buffers 16 8k;
	    gzip_proxied any;
	    gzip_types
	       text/plain
	       text/css
	       text/js
	       text/xml
	       text/javascript
	       application/javascript
	       application/json
	       application/xml
	       application/rss+xml
	       image/svg+xml
	       application/x-font
	       font/otf
	       font/ttf;

	    include /etc/nginx/conf.d/*.conf;
	}
