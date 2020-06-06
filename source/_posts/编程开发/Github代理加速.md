---
title: Github代理加速
date: 2020-06-05
categories: [编程开发]
---



```shell
## 开启代理
git config --global http.proxy 'socks5://127.0.0.1:7891'
git config --global https.proxy 'socks5://127.0.0.1:7891'

## 关闭代理
git config --global --unset http.proxy
git config --global --unset https.proxy

```



同时需要在你的代理软件中开启全局模式：

<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1gfhkh034lhj30m40ceq7k.jpg" alt="image-20200605180018041" style="zoom:50%;" />

注意，socks5的端口号根据代理软件设置的端口号来开(我的是7891)