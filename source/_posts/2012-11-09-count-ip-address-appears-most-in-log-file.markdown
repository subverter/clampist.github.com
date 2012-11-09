---
layout: post
title: "Count IP Address Appears Most in Log File"
date: 2012-11-09 10:32
comments: true
categories: [Linux, Python]
---

1.Problem
---

之前做运维的笔试题，有一道很经典的统计日志文件中出现最多的 IP 地址及次数的题。其日志格式为

    122.224.6.43 - - [30/Oct/2012:21:29:42 +0800] "GET /phpMyAdmin/scripts/setup.php HTTP/1.1" 404 162 "-" "ZmEu"
    122.224.6.43 - - [30/Oct/2012:21:29:42 +0800] "GET /MyAdmin/scripts/setup.php HTTP/1.1" 404 162 "-" "ZmEu"
    122.224.6.43 - - [30/Oct/2012:21:29:42 +0800] "GET /pma/scripts/setup.php HTTP/1.1" 404 162 "-" "ZmEu"
    122.224.6.43 - - [30/Oct/2012:21:29:42 +0800] "GET /myadmin/scripts/setup.php HTTP/1.1" 404 162 "-" "ZmEu"
    58.251.60.248 - - [30/Oct/2012:23:05:46 +0800] "GET http://www.baidu.com/ HTTP/1.1" 200 612 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; QQDownload 551; .NET CLR 2.0.50727; CIBA)"
    58.251.60.248 - - [30/Oct/2012:23:06:37 +0800] "" 400 0 "-" "-"

这是 `nginx` 日志 `/var/log/nginx/access.log` 中的一个片段。


2.用 Shell 解决
---

#####思路一： awk 取出第一列，然后利用 sort -u 将 IP unique 化输出到文件，再从文件按行读入再计数显示出来

具体如下：

    cd /var/log/nginx

    sudo zcat access.log.1.gz | awk '{print $1}' | sort -u > /tmp/ip

    while read line; do
        let a++
        echo `sudo zcat access.log.1.gz | awk '{print $1}' | grep $line -c` $line
    done < /tmp/ip

    rm /tmp/ip

其中用的是 `grep` 计数的，而且会产生临时文件又需要手动删除，致命点是只能显示出各 IP 次数，但不能列出最大次数及 IP。其中为 access.log.1.gz 文件则用 `zcat`, 如果是普通 access.log 文件则用 `cat`.

#####思路一（改进）：利用 `helpdoc` 则可避免使用临时文件

    cd /var/log/nginx

    while read line; do
        let a++
        echo `sudo zcat access.log.1.gz | awk '{print $1}' | grep $line -c` $line
    done << EOF

    `sudo zcat access.log.1.gz | awk '{print $1}' | sort -u`

    EOF

这里用到 here document, here document 往往用于需要输出一大段文本的地方，例如脚本的 help 函数。两个 EOF 之间产生的内容用于上面循环的输入。

#####思路二：上面所用的 unique 化和 count 方法太拙劣，寻求其他的便捷方法，果然找到了

之前用了循环将问题复杂化了，用 `uniq` 这个命令会很 cool.

    cd /var/log/nginx

    sudo cat access.log | awk '{print $1}'| sort | uniq -c | sort -nr | head -1

其中 `uniq -c` 就是 unique 化 AND count, `sort -n` 表示把字符串中的数字当做数字比较，`-r` 为倒序。

注意：这个方法可能只能显示那些相同次数的 IP 的最前面的一个，所以适当加大为 `head -3` 或 `head -5` 等适可而止，无需再去判断那些相同次数来增加复杂性，这样很好记的。

3.用 Python 来做
---
先将日志文件中所有 IP 读入一个 list, 然后将 IP 和次数统计入一个 dict, 最后输出次数最大的 IP 及次数。

    li = []

    with open('/var/log/nginx/access.log') as f:
        for line in f:
            li.append(line.split(' ')[0])

    dic = {}

    for element in li: 
        dic[element] = dic.get(element, 0) + 1 
    #print(map.items())
    print [(k, v) for k, v in dic.iteritems() if v == max(dic.values())]

会将所有次数最大的 IP 和次数输出，避免了思路二的问题。

4.SEE ALSO
---
* [简洁的bash编程技巧](http://kodango.me/simple-bash-programming-skills)
* [找出apache日志中访问量最大的IP](http://cyr520.blog.51cto.com/714067/768707)
* [找出list中重复最多的N个元素（Python v2.6）](http://hi.baidu.com/jimmy1029/item/2cebc59f7f8498b4cd80e530)
* [python 得到一个list中元素重复出现的次数](http://www.stackenqueue.com/questions/761/python-%E5%BE%97%E5%88%B0%E4%B8%80%E4%B8%AAlist%E4%B8%AD%E5%85%83%E7%B4%A0%E9%87%8D%E5%A4%8D%E5%87%BA%E7%8E%B0%E7%9A%84%E6%AC%A1%E6%95%B0)
