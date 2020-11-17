---
layout: post
title: "AutomaticChecking"
categories: code
author:
- JW Chae
meta: "Springfield"
---

자가진단 자동화 프로그램.
(Last update : 2020-11-18)

## Dependency

{% highlight python %}
'''
selenium module (with + chromedriver.exe)
time module
'''
from selenium import webdriver
import time
{% endhighlight %}

## Source Code (Python)

{% highlight python %}
class Info:
    def __init__(self, _name, _yymmdd, _pw):
        self.name = _name
        self.yymmdd = _yymmdd
        self.pw = _pw
        return None

    def getInfo(self):
        return [self.name, self.yymmdd, self.pw]

def felem(driver, _xpath):
    return driver.find_element_by_xpath(_xpath)

def check(driver, name, yymmdd, pw):
    driver.get('https://hcs.eduro.go.kr')
    
    try:
        time.sleep(1.0)
        felem(driver, '//*[@id="secondaryPwForm"]/table/tbody/tr/td/input').send_keys(pw)
        felem(driver, '//*[@id="btnConfirm"]').click()
    except:
        felem(driver, '//*[@id="btnConfirm2"]').click()
        felem(driver, '//*[@id="WriteInfoForm"]/table/tbody/tr[1]/td/button').click()
        felem(driver, '//*[@id="softBoardListLayer"]/div[2]/div[1]/table/tbody/tr[1]/td/select/option[8]').click()
        felem(driver, '//*[@id="softBoardListLayer"]/div[2]/div[1]/table/tbody/tr[2]/td/select/option[5]').click()
        felem(driver, '//*[@id="softBoardListLayer"]/div[2]/div[1]/table/tbody/tr[3]/td[1]/input').send_keys('울산과학고등학교')
        felem(driver, '//*[@id="softBoardListLayer"]/div[2]/div[1]/table/tbody/tr[3]/td[2]/button').click()
        time.sleep(0.5)
        felem(driver, '//*[@id="softBoardListLayer"]/div[2]/div[1]/ul/li').click()
        felem(driver, '//*[@id="softBoardListLayer"]/div[2]/div[2]/input').click()
        felem(driver, '//*[@id="WriteInfoForm"]/table/tbody/tr[2]/td/input').send_keys(name)
        felem(driver, '//*[@id="WriteInfoForm"]/table/tbody/tr[3]/td/input').send_keys(yymmdd)
        felem(driver, '//*[@id="btnConfirm"]').click()
        time.sleep(1.0)
        felem(driver, '//*[@id="WriteInfoForm"]/table/tbody/tr/td/input').send_keys(pw)
        felem(driver, '//*[@id="btnConfirm"]').click()
    
    time.sleep(2.0)
    felem(driver, '//*[@id="container"]/div/section[2]/div[2]/ul/li/a/button').click()
    time.sleep(1.0)
    felem(driver, '//*[@id="container"]/div/div/div[2]/div[2]/dl[1]/dd/ul/li[1]/label').click()
    felem(driver, '//*[@id="container"]/div/div/div[2]/div[2]/dl[2]/dd/ul/li[1]/label').click()
    felem(driver, '//*[@id="container"]/div/div/div[2]/div[2]/dl[3]/dd/ul/li[1]/label').click()
    felem(driver, '//*[@id="btnConfirm"]').click()
    time.sleep(1.0)

def main(infos):
    for i in infos:
        driver = webdriver.Chrome('chromedriver.exe')
        f = i.getInfo()
        try:
            check(driver, f[0], f[1], f[2])
        except:
            print(f'Failed to check (name:{f[0]})')
        driver.quit()
{% endhighlight %}
