---
title: Android strings.xml多语言翻译脚本
date: 2018-04-01 23:56:39
tags: python
---

## 前言

在开发应用时需要用到多语言，而strings资源比较多，查漏补缺检查起来也很麻烦，需要有个工具能够快速翻译港澳繁体、台湾繁体、英文等。

因此用python写了个脚本。

## 思路

1. 编写正则匹配

2. 读取源strings.xml文件

3. 按行正则解析，请求网络转换内容

4. 将转换后的内容写进新xml

5. 完成转换

##进阶

1. 添加参数，指定要转换的语言
2. 对比查找目标文件缺失的string

## 实现

- 需要用到**正则**、**文件读写**、**json解析**、**urllib网络请求** 
- 目前只实现了简体到港澳繁体、台湾繁体的转换。如果需要翻译英文，可以使用有道的API来实现，参考代码中的trans_content方法添加即可。
- 以下为实现代码，已放在[github](https://github.com/chenjindiao/PythonTools)上

~~~python
# -*- coding:utf-8 -*-

"""
StringsTranslate.py
该脚本用于将Android strings.xml自动翻译为目标语言，输出新的xml文件
仅作交流学习之用，
使用了https://tool.lu/zhconvert这个网站进行繁体翻译
@autor: chenjindiao
@date: 2018-03-30
"""

import json
import re
import sys
import time
import urllib
import urllib2


# 从每行string中，解析出值
def find_content(line):
    # line = '  <string name="app_name">contnet</string>  '
    match_obj = re.match(r'.*?<string name="(.*?)">(.*?)</string>.*?', line, )
    if match_obj:
        # print 'find_content: ' + match_obj.group(2)
        return match_obj.group(2)
    else:
        return None


# 翻译，将内容转换为需要的语言，默认翻译为港澳繁体
# 这里使用https://tool.lu/zhconvert这个网站进行翻译
def trans_content(content, locale="zh-hk"):
    url = "https://tool.lu/zhconvert/ajax.html"
    headers = {
        "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_2) AppleWebKit/537.36",
        "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8"
    }
    form_data = {
        "code": content,
        "operate": locale
    }
    request = urllib2.Request(url, data=urllib.urlencode(form_data), headers=headers)
    response = None
    try:
        response = urllib2.urlopen(request).read()
    except urllib2.HTTPError, e:
        print e.msg
        # urllib2.HTTPError: HTTP Error 429: Too Many Requests
        if e.code == 429:
            print 'sleep 15...'
            time.sleep(15)
            response = urllib2.urlopen(request).read()
    # print response
    if response:
        data = json.loads(response)
        return data['text']
    else:
        return None


# 读取文件按行翻译，不需要翻译的行就按原内容输出到新文件
def trans_file(path, locale):
    new_path = path + "_" + locale + ".xml"
    xf = open(path)
    out = open(new_path, "w")
    for line in xf:
        print 'line: ' + line
        value = find_content(line)
        if value:
            # print 'old value: ' + value
            new_value = trans_content(value, locale)
            # print 'new_value: ' + new_value
            new_line = line.replace(value, new_value)
            print 'new_line: ' + new_line
            out.write(new_line)
        else:
            out.write(line)
    xf.close()
    out.close()


def main():
    # 解决文件编码报错问题
    reload(sys)
    sys.setdefaultencoding('utf-8')

    # 命令使用
    print 'Use command line: python StringsTranslate.py sourcePath targetLocale'
    print 'targetLocale: zh-tw  zh-hk'

    path = sys.argv[1]
    locale = sys.argv[2]
    # print path + ' ' + locale

    print 'begin trans file: ' + path
    print trans_file(path, locale)
    print 'finish!'


if __name__ == '__main__':
    main()

~~~

- 目前使用了网络请求转换繁体，还有看到一些python库可以用，如[opencc-python](https://pypi.python.org/pypi/opencc-python/) 、[zhtools](https://github.com/skydark/nstools/tree/master/zhtools) 但好像只是简单的单字转换，目前没有实际测试过。