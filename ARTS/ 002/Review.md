#Web Architecture 101



## The basic architecture concepts I wish I knew when I was getting started as a web developer

![img](https://ws4.sinaimg.cn/large/006tNc79gy1ftanh1uji3j30ih0bpta1.jpg)

>上面是Stroryblocks网站的基本结构。下面是响应用户的一系列流程：
>
>* 用户在Google搜索 “Strong Beautiful Fog And Sunbeams In The Forest”，第一个显示的结果是Storyblocks的网站。用户点击链接后，浏览器重定向至该网站。浏览器首先向DNS发起请求，寻找如何连接StroryBlocks，然后再发送请求。
>* 该请求命中负载均衡，随机选择一台服务器，响应该请求。网页服务器首先在缓存中找到该图片，剩下的数据从数据库中获取。我们注意到图片的color profile还没有被计算出来，于是发送一个color profile 的job 进入 job 队列。Job servers 将会异步的处理，更新数据库。
>* 然后，我们想找到一些相似图片，于是发起一个请求给全文搜索服务，输入是该图片的名字。当用户选择登入时，我们在账户服务中找到用户的信息。最后，我们将页面视图事件发送到我们的数据firehose，以便记录在我们的云存储系统上，并最终加载到我们的数据仓库中，分析师用它来帮助回答有关业务的问题。
>* server 将视图渲染成HTML，返回给用户浏览器。首先经过负载均衡。页面中包含Javascript 和CSS 央视，这些着云存储系统中，并且连接到了CDN。因此，用户浏览器直接跟CDN联系，取回这些内容。最后，浏览器渲染出了页面，呈现给用户。

下面对每一个组件进行介绍。

# 1. DNS

DNS，域名服务器，简单来说，提供域名和IP地址的 KV查找。通过域名找到IP地址。

# 2. Load Balancer

水平扩展，意味着增加更多的机器。垂直扩展以为增加CPU RAM 在现有的机器上。

* 在Web开发过程中，会更多的用到水平扩展，因为回出现服务器宕机，网络中断，等扽故障，有更多的机器意味着在在出现故障的时候，应用能够继续服务。

* 其次横向扩展能够让不同部分在不同的机器上运行以最小化耦合。

* 最后，横向扩展能够达到纵向扩展不可能达到的高度。

负载均衡将访问请求路由给众多网络服务器中的一个，这些服务器都是镜像或者克隆，任何一个都能够以同样的方式处理请求。

# 3. Web Application Servers

App server 意味着挑选一门特定的语言以及MVC框架。

# 4. Database Servers

数据库服务用来定义数据结构，完成数据的增删改查。数据库大体上分为两种，SQL和NoSQL。

# 5. 缓存服务

缓存服务提供简单的 kv数据存储，能够在O(1)的时间内完成数据的查找与存储。应用程序通常会使用缓存服务存储一些耗费计算的数据。这样就可以从缓存中读取它们而不需要计算。广泛使用的缓存技术有Redis和Memcache。

# 6. 任务队列和服务器

大多数的web应用需要异步的处理一些任务，这些任务的执行往往在背后，并不与用户的请求相关。

任务队列通常包含两个部分，一个任务队列和一个或者多个任务处理服务。

任务队列存储着一些要被异步执行的任务，最简单事FIFO还有一些优先级有限的系统。

在Storyblocks中，任务队列处理一些幕后的任务。例如图片和视频的编码，处理CSV文件，我们使用的FIFO模式。

# 7. 全文搜索服务

通过倒置索引的方式查找包含某个关键字的文档。

![img](https://cdn-images-1.medium.com/max/800/1*gun_BpdDH9KrNna1NnaocA.png)

# 8. Services

当应用达到一定的数量级后，它们会被分割成一些单独的服务。它们并不会暴露给外部，而应用和其他一些服务能够与他们进行交互。

# 9. Data

一个典型的Data pipeline有以下三个阶段：

* 应用程序发送数据给数据仓库，提供流式的接口，通常这些原始数据会被进行转换并发给另外一个数据仓库。AWS Kinesis 和 Kafka 是主要的两个技术。
* 原始数据保存到云存储中， AWS 的 Kinesis 提供了这些的服务，可以将原始数据存储到S3.
* 转换后的数据会被导入数据仓库用于分析。我们用到了AWS的 Redshift。如果数据集很大的话，类似于Hadoop NoSQL MapReduce 会被用到。

# 10. Cloud Storage

你可以轻松的将数据存储到云存储中，这样就可以通过RESTful接口以及HTTP来进行交互。

# 11. CDN

CDN的意思是内容分发网络，它提供以一种服务的方式，例如静态的HTML，CSS，javascaript 图片等，这样就能更快速的获取他们。





![img](https://cdn-images-1.medium.com/max/800/1*ZkC_5865Hx-Cgph3iPJghw.png)

通常情况下，CDN用来提供CSS，Javascript， images，videos，静态HTML等。

