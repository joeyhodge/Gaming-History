# [nginx] nginx + django (fastcgi)

自己唯一用过的就是 django，看看如何和 nginx 结合起来。配置了下，看起来还是挺容易的嘛。
比 Apache 的 mod_python 配置起来简单许多。还是 nginx 的配置文件比较清爽，哈哈。

下面是自己弄的超级简单的 nginx 配置。It works!

```
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    server {
        listen 8080;
        server_name localhost;

        location / {
            fastcgi_pass 127.0.0.1:8801;
        }
    }
}

```

```
$ python manage.py runfcgi method=threaded host=127.0.0.1 port=8801 
```
