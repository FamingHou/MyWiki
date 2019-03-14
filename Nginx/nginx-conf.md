```console
## version 0.8

daemon off;

user www-data;
worker_processes auto;
pid /run/nginx.pid;

events {
    worker_connections 65535;
    # multi_accept on;
}

http {

    ##
    # Basic Settings
    ##

    charset utf-8;
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;
    # server_tokens off;

    # server_names_hash_bucket_size 64;
    # server_name_in_redirect off;

    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    ##
    # SSL Settings
    ##

    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
    ssl_prefer_server_ciphers on;

    ##
    # Logging Settings
    ##

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    ##
    # Gzip Settings
    ##

    gzip on;
    gzip_disable "msie6";

    # gzip_vary on;
    # gzip_proxied any;
    # gzip_comp_level 6;
    # gzip_buffers 16 8k;
    # gzip_http_version 1.1;
    # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

    # proxy_cache
    proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=goods_cache:10m max_size=10g inactive=60m use_temp_path=off;

    ##
    # Virtual Host Configs
    ##
 
    server { # simple reverse-proxy
        listen       80;
        # server_name  jenkins.${projectname}.co.nz;
        access_log   /var/log/nginx/common-access.log;

        location ~ ^/goods { # regex
            proxy_cache goods_cache;
            proxy_cache_revalidate on; ##
            proxy_cache_min_uses 3;    ##
            proxy_cache_use_stale error timeout updating http_500 http_502 http_503 http_504; ## 
            proxy_cache_valid 200 1m;
            
            proxy_cache_bypass   $http_secret_header;  ## bypass/re-cache on a file by file basis
            add_header X-Cache-Status $upstream_cache_status;  ## from the cache (will return 'HIT') or from the content server (will return 'BYPASS').
            ## to expire/refresh the cached file, use curl or any rest client to make a request to the cached page.
            ## curl http://abcdomain.com/mypage.html -s -I -H "secret-header:true"
            ## this will return a fresh copy of the item and it will also replace what's in cache.
            
            proxy_ignore_headers Vary; 
            proxy_cache_lock on; 
            proxy_buffering on;
            proxy_cache_bypass $cookie_nocache $arg_nocache; ## Punch a Hole Through the Cache
            # proxy_cache_methods GET HEAD POST; // 
            # proxy_pass      https://${projectname}.oss-us-west-1.aliyuncs.com/test.json;
            # proxy_pass http://app-service-v0-1/api/testSpeed/;
            # proxy_pass http://jenkins.${projectname}.co.nz:7788/users.json;
            proxy_pass http://jenkins.${projectname}.co.nz:7788;
        }
        
        location / {
            return 404;
        }
    }

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}

```