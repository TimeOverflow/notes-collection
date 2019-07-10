### Scrapy框架

-------

#### 结构

“5+2”框架，三条数据流路径

+ 模块
  + Engine
  + Spider
  + Scheduler
  + Iterm pipelines
  + Downloader
+ 两个中间件Middleware

![1555066580043](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\1555066580043.png)



#### 框架解析

+ Engine

  不需要修改

+ 二者之间有Downloader Middleware，进行用户配置

+ Downloader

  根据请求下载网页，不需要修改

+ Scheduler

  对所有爬取请求进行调度，不需要修改

+ **<font color=red>Spider</font>**

  用户编写的核心代码

  + 解析Downloader返回的响应（Response）
  + 产生爬取项（scraped item）
  + 产生额外的爬取请求（Request）

+ Spider和Engine之间存在Spider Middleware

+ **<font color=red>Item pipelines</font>**

  流水线方式处理Spider产生的爬取项



#### 常用命令

通用格式：`scrapy <command> [options] [args]`

具体命令：

+ `startproject`：创建一个新工程
+ `genspider`：创建一个爬虫
+  `settings`：获得爬虫配置信息
+ `crawl`：运行一个爬虫
+  `list`：列出工程中所有爬虫
+  `shell` ：启动URL调试命令

