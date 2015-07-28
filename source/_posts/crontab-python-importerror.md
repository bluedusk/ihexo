title: 使用crontab定时运行python脚本ImportError错误.md
date: 2015-07-28 08:18:35
tags: 
- python

---


记录下crontab运行python脚本报错ImportError的解决。

<!-- more -->

用python写了个操作MongoDB的脚本，用crontab做定时任务。脚本如下：

```
*/1 * * * * python ~/MyWork/mongo.py
```
默认情况下，运行情况会发mail，使用mail命令输入对应的mail号码查看。如果想输出到文件可以：

```
*/1 * * * * python ~/MyWork/mongo.py  >> ~/hi.log 2>&1
```

以上crontab运行后报错，提示为找不到pymongo包：

```
> ImportError: No module named pymongo
```

但是命令行明明可以运行：

```
$~  python ~/MyWork/mongo.py
```
查了一些资料后发现可能是python多版本的问题，crontab用到的默认版本没有安装pymongo. 那么就排查下：

``` python

python
#显示为python2.7.10是应该使用的默认版本

/usr/bin/pytohn
/usr/bin/pytohn2.7
#以上两个都显示为python2.7.6

which python
#找到PATH中python用的是哪个
#输出为
/usr/local/bin/python
```

那么问题就找到了， crontab调用了/usr/bin/python，导致ImportError，解决方法是在crontab中加入路径信息：

```
*/1 * * * */usr/local/bin/python ~/MyWork/mongo.py  >> ~/hi.log 2>&1
```

OK， 可以正常运行了。



