---
title: 【转】curl的常用用法
date: 2021-10-05 21:28:35
tags: 技术
---


> 因为在机房中往往使用命令行，在测试api或者测试elasticsearch数据库时往往需要大量使用curl命令，在网上找到了阮一峰老师的这篇笔记，里面基本涵盖了日常使用的大部分curl的参数。
> 
> 这些参数的用法建议熟读并背诵。
>
> 本文忽略了一些不常用到的参数说明。

作者：阮一峰

curl 是常用的命令行工具，用来请求 Web 服务器。它的名字就是客户端（client）的 URL 工具的意思。

它的功能非常强大，命令行参数多达几十种。如果熟练的话，完全可以取代 Postman 这一类的图形界面工具。

不带有任何参数时，curl 就是发出 GET 请求。

```shell
$ curl <https://www.example.com>
```

上面命令向 `www.example.com`发出 GET 请求，服务器返回的内容会在命令行输出。

## -d

  `-d`参数用于发送 POST 请求的数据体。

```shell
$ curl -d'login=emma＆password=123'-X POST <https://google.com/login>
# 或者
$ curl -d 'login=emma' -d 'password=123' -X POST  <https://google.com/login>
```

使用 `-d`参数以后，HTTP 请求会自动加上标头 `Content-Type : application/x-www-form-urlencoded`。并且会自动将请求转为 POST 方法，因此可以省略 `-X POST`。

## -F

`-F`参数用来向服务器上传二进制文件。

```shell
$ curl -F 'file=@photo.png' <https://google.com/profile>
```

上面命令会给 HTTP 请求加上标头 `Content-Type: multipart/form-data`，然后将文件 `photo.png`作为 `file`字段上传。

`-F`参数可以指定 MIME 类型。

```shell
$ curl -F 'file=@photo.png;type=image/png' <https://google.com/profile>
```

上面命令指定 MIME 类型为 `image/png`，否则 curl 会把 MIME 类型设为 `application/octet-stream`。

## -H

`-H`参数添加 HTTP 请求的标头。

```shell
$ curl -H 'Accept-Language: en-US' <https://google.com>
```

上面命令添加 HTTP 标头 `Accept-Language: en-US`。

```shell
$ curl -H 'Accept-Language: en-US' -H 'Secret-Message: xyzzy' <https://google.com>
```

上面命令添加两个 HTTP 标头。

```shell
$ curl -d '{"login": "emma", "pass": "123"}' -H 'Content-Type: application/json' <https://google.com/login>
```

上面命令添加 HTTP 请求的标头是 `Content-Type: application/json`，然后用 `-d`参数发送 JSON 数据。

## -i

`-i`参数打印出服务器回应的 HTTP 标头。

```shell
$ curl -i https://www.example.com
```
上面命令收到服务器回应后，先输出服务器回应的标头，然后空一行，再输出网页的源码。

## -I
`-I`参数向服务器发出 HEAD 请求，然会将服务器返回的 HTTP 标头打印出来。

```shell
$ curl -I https://www.example.com
```

上面命令输出服务器对 HEAD 请求的回应。

`--head`参数等同于`-I`。

```shell
$ curl --head https://www.example.com
```

## -k
`-k`参数指定跳过 SSL 检测。

```shell
$ curl -k https://www.example.com
```
上面命令不会检查服务器的 SSL 证书是否正确。

## -L
`-L`参数会让 HTTP 请求跟随服务器的重定向。curl 默认不跟随重定向。

```shell
$ curl -L -d 'tweet=hi' https://api.twitter.com/tweet
```

## --limit-rate
`--limit-rate`用来限制 HTTP 请求和回应的带宽，模拟慢网速的环境。

```shell
$ curl --limit-rate 200k https://google.com
```
上面命令将带宽限制在每秒 200K 字节。

## -o
`-o`参数将服务器的回应保存成文件，等同于wget命令。

```shell
$ curl -o example.html https://www.example.com
```
上面命令将www.example.com保存成example.html。


## -O
`-O`参数将服务器回应保存成文件，并将 URL 的最后部分当作文件名。

```shell
$ curl -O https://www.example.com/foo/bar.html
```
上面命令将服务器回应保存成文件，文件名为bar.html


## -u
`-u`参数用来设置服务器认证的用户名和密码。

```shell
$ curl -u 'bob:12345' https://google.com/login
```
上面命令设置用户名为bob，密码为12345，然后将其转为 HTTP 标头Authorization: Basic Ym9iOjEyMzQ1。

curl 能够识别 URL 里面的用户名和密码。

```shell
$ curl https://bob:12345@google.com/login
```
上面命令能够识别 URL 里面的用户名和密码，将其转为上个例子里面的 HTTP 标头。

```
$ curl -u 'bob' https://google.com/login
```
上面命令只设置了用户名，执行后，curl 会提示用户输入密码。\

## -v


`-v`参数输出通信的整个过程，用于调试。

```
$ curl -v https://www.example.com
```
--trace参数也可以用于调试，还会输出原始的二进制数据。

```
$ curl --trace - https://www.example.com
```


## -x

-x参数指定 HTTP 请求的代理。

```shell
$ curl -x socks5://james:cats@myproxy.com:8080 https://www.example.com
```
上面命令指定 HTTP 请求通过myproxy.com:8080的 socks5 代理发出。

如果没有指定代理协议，默认为 HTTP。

```shell
$ curl -x james:cats@myproxy.com:8080 https://www.example.com
```
上面命令中，请求的代理使用 HTTP 协议.

##  -X

`-X`参数指定 HTTP 请求的方法。

```bash
$ curl -X POST https://www.example.com
```
上面命令对https://www.example.com发出 POST 请求。

## 参考：

1. [curl 的用法指南](https://www.ruanyifeng.com/blog/2019/09/curl-reference.html)
2. [Curl Cookbook](https://catonmat.net/cookbooks/curl)