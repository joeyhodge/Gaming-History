# [python] Simple FTPD

## 开启 ftpd 服务

使用 [pyftpdlib][2]，超级简单。

-w 表示 writable。

```
# python -m pip install pyftpdlib

$ python -m pyftpdlib --help
...

$ python -m pyftpdlib -w
[I 2018-11-15 05:53:38] >>> starting FTP server on 0.0.0.0:2121, pid=91813 <<<
[I 2018-11-15 05:53:38] concurrency model: async
[I 2018-11-15 05:53:38] masquerade (NAT) address: None
[I 2018-11-15 05:53:38] passive ports: None
```

开启账号密码验证

```
$ python -m pyftpdlib -w -u name -p pass
```

在 windows cmd 下使用 ftp client

```
> ftp

ftp> open ip_addr 2121
ftp> binary
ftp> put file_name1          # upload

ftp> get file_name2          # download
```

## 参考资料

* [https://stackoverflow.com/questions/4994638/one-line-ftp-server-in-python][1]

[1]:https://stackoverflow.com/questions/4994638/one-line-ftp-server-in-python
[2]:https://github.com/giampaolo/pyftpdlib
