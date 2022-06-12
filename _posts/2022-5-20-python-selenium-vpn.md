---
layout: post
title:  python selenium实现vpn
categories: [python,Code,selenium]

---

# 利用vpn软件注册漏洞实现无限vpn会员续费

### 代码：

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service 
from selenium.webdriver.common.by import By
wd=webdriver.Chrome(service=Service('/Users/ashuai/shuai/py/chromedriver'))
a=input("请输入一个基础字符串6位英文以内")
b=input("请输入你要邀请注册的人数")
i=1
while(i<int(b)):
   x=str(i)
   name_f=a
   name_e="@kgail.com"
   wd.get('http://wg19t.dkjiasu.cc:5659/invite?code=TCAOKAgW')
   wd.find_element(By.ID,'email').send_keys(name_f+x+name_e)
   wd.find_element(By.ID,'passwd').send_keys('112233aB')
   wd.find_element(By.ID,'repasswd').send_keys('112233aB')
   wd.find_element(By.ID,'register-confirm').click()
   i=i+1

```

### 原理解释：

作者无意间发现有款vpn软件在使用时可以通过邀请新人注册来实现会员充值，只需要一个新用户通过分享链接注册成功后，邀请人就会获得相应奖励，包括账户返现，会员续费。从普通用户升级为会员后，vpn网速明显提升，每邀请一个新人注册成功大概会随机返现0.1-0.2元不等，用户账户满20即可提现。令人欣喜的是该软件在注册时没有设置邮箱验证和任何人机验证功能，意味着作者使用自动化工具可以注册任意数量的用户，理论上只要用户够多，就可以无限返现。然而很遗憾，在测试过程中当作者反复测试后，网站似乎修改了规则，导致一个用户在邀请100人后就会停止奖励。作者只在修改规则之前拿到了奖励，并被封号处理了。不过此软件依然没有邮箱验证和任何人机验证，上述自动化脚本可以正常执行。

### 代码讲解：

自动化操作完成注册任务，首先需要输入一个任意的字符串a，a是接下来构建邮箱串的基础，在a的基础上加数字和邮箱名@kgail.com一起构成了用户名串。当然这个操作是为了伪装成一个邮箱，只要合乎邮箱名规则，我们可以任意构建这个串。wd.get()函数用来打开需要访问的注册网页，这里应该是作者vpn账户的邀请链接，每一个用户都有一个自己的邀请链接。密码可以自己设置此处为“112233aB”，需要注意的是有些网站在注册时需要检测密码安全等级，为了成功注册，我们应该将密码写的难一点。b参数代表使用者想一次邀请注册的人数，因为i是从1开始，因此如果你想一次注册50人，b应该为51，随后的while循环会将chrome浏览器打开并一个又一个的注册用户。由于此网站没有相应安全防御措施，进程会很顺利的完成。在遇到其他网站时，需要增加close函数来删除上一次注册的信息残留，保证不会在下一次注册时自动登录上一次的账户。