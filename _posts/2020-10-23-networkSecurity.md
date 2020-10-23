---
layout: post
title: 前端安全
categories: web安全
description: 前端安全
keywords: javascript, xss, web安全, CSRF
---

详细记录一下前端安全的东西，XSS、CSRF。


### 1、同源策略

同源策略是不可避免要接触到的协议方面的内容，大致了解一下：

> 同源策略（Same origin policy）是一种约定，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，则浏览器的正常功能可能都会受到影响。可以说Web是构建在同源策略基础之上的，浏览器只是针对同源策略的一种实现。
> 
> 同源策略是浏览器的行为，是为了保护本地数据不被JavaScript代码获取回来的数据污染，因此拦截的是客户端发出的请求回来的数据接收，即请求发送了，服务器响应了，但是无法被浏览器接收。
>
> 同源指的是：协议相同、域名相同、端口相同。
> 
> 非同源的话，同源策略限制了：Cookie、LocalStorage 和 IndexDB 无法读取、DOM 无法获得、AJAX 请求不能发送。


### 2、XSS

#### 2.1 XSS简介

1. 定义：XSS攻击全称跨站脚本攻击，XSS是一种在web应用中的计算机安全漏洞，它允许恶意web用户将代码植入到提供给其它用户使用的页面中。
 
2. XSS的攻击方式：反射型，存储型。
①反射型：发出请求时，XSS代码出现在URL中，作为输入提交到服务器，服务器解析后响应，XSS代码随响应内容一起传回浏览器，最后浏览器解析执行XSS代码，这个过程就像一次反射，故叫反射型。
②存储型：存储型XSS和反射型XSS的区别在于，提交的代码会存储在服务器端，下次请求页面时不用再提交XSS代码。

3. XSS的防范措施：编码、过滤、矫正
①：编码：对用户输入的数据进行HTML entity编码。
②：过滤：移除用户上传的DOM属性，如onerror等，移除用户上传的style节点，script节点，iframe节点等。
③：矫正：避免直接对HTML Entity解码，使用DOM parse转换，矫正不配对的DOM标签。


#### 2.2 XSS成因

1. 网页劫持、DNS劫持、跳转到假的网站页面，比如本地的hosts文件被恶意修改或者DNS服务器被攻击，导致解析到的IP地址是恶意修改过的。脚本被篡改，比如使用第三方的库、资源不安全。 
 
2. 缓存投毒：访问A站点时，加载了B站点的通用脚本，缓存时间非常长，再访问B站点时中招。 比如在公共场合连wf，WiFi连接成功跳转到一个页面，然而这个页面偷偷请求了很多常用资源，比如jQuery之类的类库，然后自行篡改后超长时间缓存在你本地，当你请求其他网站，其他网站用到了你本地有缓存的类库的时候，就不发送请求直接使用本地缓存的资源，而本地的资源是被恶意篡改过的，这个时候就触发了攻击。

3. 文件投毒：在非官方站点下载了某个库；官方下载地址被攻击；网络加速缓存了错误的文件；引用了不可信的第三方cdn上的资源。

4. 客户端投毒：被安装了恶意插件。

5. 自身安全漏洞，产生反射性和存储性的XSS。





 