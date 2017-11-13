---
layout: post
title: APP接口
date: 2017-11-13 13:32:20 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: i-rest.jpg # Add image post (optional)
tags: [Holidays, Hawaii]
---
# 获取数据库列表 #

## 接口地址: ##
### （输入地址）+ '/web/database/list' ###
## 格式： ##
### json ###
## 参数： ##
### 空 ###
## 返回值： ##
### 数据库名称列表，形如： [u'0810', u'1.0.5', u'1.0.6'] ###
## DEMO ##
<pre>
import json
import requests

url='http://localhost:8079/web/database/list'
data = json.dumps({})
headers = {'Content-type': 'application/json'}
request = requests.post(url,data=data,headers=headers)

结果：
>>> request
<Response [200]>
>>> request.json()
{u'jsonrpc': u'2.0', u'id': None, u'result': [u'0810', u'1.0.5', u'1.0.6', u'1.0.6-security', u'1.0.6-xiao']}
</pre>

# 登录用户验证 #
## 接口地址: ##
### （输入地址）+ '/web/session/authenticate' ###
## 格式： ##
### json ###
## 参数： ##
### db: 数据库名称，login：用户名，password：密码 ###
## 返回值： ##
### 字典dict，形如：###
<pre>
{
u'username': u'admin', 
u'user_context': {u'lang': u'zh_CN', u'tz': False, u'uid': 1}, 
u'web.base.url': u'http://localhost:8079', 
u'currencies': {u'1': {u'digits': [69, 2], u'position': u'after', u'symbol': u'\u20ac'}, u'8': {u'digits': [69, 2], u'position': u'after', u'symbol': u'\xa5'}, u'3': {u'digits': [69, 2], u'position': u'before', u'symbol': u'$'}},
u'uid': 1, 
u'partner_id': 3,
u'web_tours': [], 
u'db': u'1.0.6_T6', 
u'company_id': 1, 
u'session_id': u'763d3e9b5e4e085cb2098f404cdd583f54aa94fc', 
u'is_superuser': True, 
u'is_admin': True, 
u'user_companies': False, 
u'server_version_info': [10, 0, 0, u'final', 0, u'e'], 
u'server_version': u'10.0+e', 
u'name': u'Administrator'
}
</pre>
## DEMO ##
<pre>
import json
import requests
db = '1.0.6_T6'
user = 'admin'
password = '1'
data = json.dumps({'jsonrpc': '2.0','params': {'db': db,'login': user,'password': password}})
headers = {'Content-type': 'application/json'}
request = requests.post('http://localhost:8079/web/session/authenticate',data,headers=headers)

结果
>>> request
<Response [200]>
>>> request.json()
{u'jsonrpc': u'2.0', u'id': None, u'result': {u'username': u'admin', u'user_context': {u'lang': u'zh_CN', u'tz': False, u'uid': 1}, u'web.base.url': u'http://localhost:8079', u'currencies': {u'1': {u'digits': [69, 2], u'position': u'after', u'symbol': u'\u20ac'}, u'8': {u'digits': [69, 2], u'position': u'after', u'symbol': u'\xa5'}, u'3': {u'digits': [69, 2], u'position': u'before', u'symbol': u'$'}}, u'uid': 1, u'partner_id': 3, u'web_tours': [], u'db': u'1.0.6_T6', u'company_id': 1, u'session_id': u'763d3e9b5e4e085cb2098f404cdd583f54aa94fc', u'is_superuser': True, u'is_admin': True, u'user_companies': False, u'server_version_info': [10, 0, 0, u'final', 0, u'e'], u'server_version': u'10.0+e', u'name': u'Administrator'}}
</pre>
# 页面访问 #
## 接口地址: ##
### （输入地址）+ '/web' ###
## 请求方式： ##
### http get ###
## 参数： ##
### cookies 增加 session_id  ###
### 空 ###
## 返回值： ##
###  ###
## DEMO ##
<pre>
import json
import requests
db = '1.0.6_T6'
user = 'admin'
password = '1'
data = json.dumps({'jsonrpc': '2.0','params': {'db': db,'login': user,'password': password}})
headers = {'Content-type': 'application/json'}
request = requests.post('http://localhost:8079/web/session/authenticate',data,headers=headers)
session_id = request.json()['result']['session_id']
cookies = {'session_id': session_id}
web = requests.get('http://localhost:8079/web', cookies=cookies)

结果：
>>> web
<Response [200]>
>>> web.headers
{'date': 'Fri, 03 Nov 2017 02:06:58 GMT', 'set-cookie': 'session_id=73394e94eff0bed05076dd10f2b29b0bc2da4174; Path=/', 'content-length': '133077', 'content-type': 'text/html; charset=utf-8', 'server': 'Werkzeug/0.9.6 Python/2.7.9'}

</pre>
