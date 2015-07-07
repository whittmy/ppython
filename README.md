# ppython
Automatically exported from code.google.com/p/ppython

程序结构：
  php_python.py  ： python 的主控文件，启动该文件会开启一个socket，等待连接处理。
  php_python.php ： php的交互文件，用来跟python通讯的模块文件
  process.py  : 核心处理文件，该文件处理php端传送过来的命令并分线程进行处理。




创建一个test.py 内容如下
#!/usr/bin/env python3
#coding=utf-8
import MySQLdb
def go() :
    conn=MySQLdb.connect(host='localhost', user='root', passwd='123456', db='test') ;
    cur=conn.cursor();
    sql="show tables;"
    cur.execute(sql);
    r=cur.fetchall();
                                                                                                                               
    return r
    
## 创建一个test.php 内容如下:
<?php header("Content-Type: text/html; charset=utf-8");
require_once('php_python.php');
$res= ppython("test::go");
echo $res;
?>

## 先启动 python3 php_python.py
## 就可以测试  php test.php


============
php端程序
<?php
  require_once("php_python.php"); //框架提供的程序脚本

  $p1 = 2;     
  $p2 = 3; 

  //"ppython"是框架"php_python.php"提供的函数，用来调用Python端服务
  //调用Python的testModule模块的add函数，并传递2个参数。
  $ret = ppython("testModule::add", $p1, $p2);

  echo "返回信息：".$ret;    //打印 5
?>
------------
Python端程序，文件名testModule.py
# -*- coding: UTF-8 -*-
def add(a, b):
  return a + b
  
