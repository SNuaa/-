## HTTP系列

### 如何理解OSI七层模型?

[面试官](https://vue3js.cn/interview/http/OSI.html#%E4%B8%80%E3%80%81%E6%98%AF%E4%BB%80%E4%B9%88)

-----

### 如何理解TCP/IP协议?

[面试官](https://vue3js.cn/interview/http/TCP_IP.html#%E4%B8%80%E3%80%81%E6%98%AF%E4%BB%80%E4%B9%88)

-----

### DNS协议 是什么？说说DNS 完整的查询过程?

#### 一、是什么

`DNS`：将域名翻译成`ip`地址

#### 二、域名

域名是一个具有层次的结构，从上到下一次为根域名、顶级域名、二级域名、三级域名...

例如`www.xxx.com`，`www`为三级域名、`xxx`为二级域名、`com`为顶级域名，系统为用户做了兼容，域名末尾的根域名`.`一般不需要输入

#### 三、查询方式

DNS 查询的方式有两种：

- 递归查询：如果 A 请求 B，那么 B 作为请求的接收者一定要给 A 想要的答案

- 迭代查询：如果接收者 B 没有请求者 A 所需要的准确内容，接收者 B 将告诉请求者 A，如何去获得这个内容，但是自己并不去发出请求

#### 四、域名缓存

在域名服务器解析的时候，使用缓存保存域名和`IP`地址的映射

计算机中`DNS`的记录也分成了两种缓存方式：

- 浏览器缓存：浏览器在获取网站域名的实际 IP 地址后会对其进行缓存，减少网络请求的损耗
- 操作系统缓存：操作系统的缓存其实是用户自己配置的 `hosts` 文件

#### 五、查询过程

解析域名的过程如下：

- 首先搜索浏览器的 DNS 缓存，缓存中维护一张域名与 IP 地址的对应表
- 若没有命中，则继续搜索操作系统的 DNS 缓存
- 若仍然没有命中，则操作系统将域名发送至本地域名服务器，本地域名服务器采用递归查询自己的 DNS 缓存，查找成功则返回结果
- 若本地域名服务器的 DNS 缓存没有命中，则本地域名服务器向上级域名服务器进行迭代查询
  - 首先本地域名服务器向根域名服务器发起请求，根域名服务器返回顶级域名服务器的地址给本地服务器
  - 本地域名服务器拿到这个顶级域名服务器的地址后，就向其发起请求，获取权限域名服务器的地址
  - 本地域名服务器根据权限域名服务器的地址向其发起请求，最终得到该域名对应的 IP 地址
- 本地域名服务器将得到的 IP 地址返回给操作系统，同时自己将 IP 地址缓存起来
- 操作系统将 IP 地址返回给浏览器，同时自己也将 IP 地址缓存起
- 至此，浏览器就得到了域名对应的 IP 地址，并将 IP 地址缓存起

-----

### 说说地址栏输入 URL 敲下回车后发生了什么?

- URL解析

  首先判断你输入的是一个合法的`URL` 还是一个待搜索的关键词，并且根据你输入的内容进行对应操作。对 `URL` 进行解析之后，浏览器确定了 Web 服务器和文件名，接下来就是根据这些信息来生成 HTTP 请求消息了。

  > **URL编码**
  >
  >  `encodeURL /decodeURL`编码：对中文和空格进行编码
  >
  > `encodeURLComponent /decodeURLComponent`编码：对传递的参数信息编码

- 缓存检查

- DNS 查询

  通过浏览器解析 URL 并生成 HTTP 消息后，需要委托操作系统将消息发送给 `Web` 服务器。 在这之前需要获取到了**域名对应的目标服务器`IP`地址**

- TCP 连接

  在确定目标服务器服务器的`IP`地址后，则经历三次握手建立`TCP`连接，保证双方都有发送和接收的能力。

- HTTP 请求

  当建立`tcp`连接之后，就可以在这基础上进行通信，浏览器发送 `http` 请求到目标服务器（请求的内容：请求行，请求头，请求主体）

- 响应请求

  当服务器接收到浏览器的请求之后，就会进行逻辑操作，处理完成之后返回一个`HTTP`响应消息（包括：状态行，响应头，响应正文）

- 页面渲染

  当浏览器接收到服务器响应的资源后，首先会对资源进行解析：

  - 查看响应头的信息，根据不同的指示做对应处理，比如重定向，存储cookie，解压gzip，缓存资源等等

  - 查看响应头的 Content-Type的值，根据不同的资源类型采用不同的解析方式

  关于页面的渲染过程如下：

  - 解析HTML，构建 DOM 树
  - 解析 CSS ，生成 CSS 规则树
  - 合并 DOM 树和 CSS 规则，生成 render 树
  - 布局 render 树（ Layout / reflow ），负责各元素尺寸、位置的计算
  - 绘制 render 树（ paint ），绘制页面像素信息
  - 浏览器会将各层的信息发送给 GPU，GPU 会将各层合成（ composite ），显示在屏幕上

-----

### 为什么说HTTP协议是无状态协议？

​		HTTP协议是一种无状态协议，协议自身不对**请求和响应之间的通信状态**进行保存，即对发送过来的请求和响应都不做持久化处理，把HTTP协议设计的如此简单是为了更快地处理大量事务。

----

### 怎么解决HTTP协议无状态协议?Cookie

​		为了解决HTTP协议不能保存通信状态的问题，引入了Cookie状态管理。

​		Cookie技术通过在请求和响应报文中写入Cookie信息来**控制客户端的状态**。

​		Cookie会根据从服务端发送的响应报文的一个叫`Set-Cookie`的首部字段，通知客户端保存Cookie。
当下次客户端再往该服务端发送请求时，客户端会自动在请求报文中加入Cookie值发送出去，服务端发现客户端发来的Cookie后，会检查是哪一个客户端发来的连接请求，对比服务器上的记录，最后得到之前的状态信息。

----

### HTTP 协议包括哪些请求方法？

- GET：对服务器资源的简单请求
- POST：用于发送包含**用户提交数据**的请求
- PUT：传输文件
- DELETE：发出一个删除指定文档的请求
- HEAD：类似于GET请求，不过返回的响应中没有具体内容，用于获取**报文首部**
- OPTIONS：返回所有可用的方法，检查服务器支持哪些方法
- TRACE：发送一个请求副本，以跟踪其处理进程
- CONNECT：用于ssl隧道的基于代理的请求

-----

### GET与POST的区别？

1. GET是**幂等**的，即读取同一个资源，总是得到相同的数据，POST**不是幂等**的；
2. GET一般用于从**服务器获取资源**，而POST有**可能改变服务器上的资源**；
3. 请求形式上：GET请求的数据附在**URL**之后，在**HTTP请求头**中；POST请求的数据在**请求体中**；
4. 安全性：GET请求可被**缓存、收藏、保留到历史记录**，且其请求数据**明文出现在URL**中。POST的参数不会被保存，安全性相对较高；
5. GET的**长度有限制**（操作系统或者浏览器），而POST数据大小无限制
6. GET只允许`ASCII`字符，POST对数据类型没有要求，也允许二进制数据；

> 幂等：在编程的领域，一般指方法被多次执行所产生的影响均与一次执行的影响相同

-----

### PUT和POST区别

1. PUT请求：如果两个请求相同，后一个请求会把第一个请求覆盖掉。（所以PUT用来改资源）（幂等）

2. POST请求：后一个请求不会把第一个请求覆盖掉。（所以POST用来增资源）

-----

### HTTP请求有哪些常见状态码？

1xx：传递信息。服务器收到请求，需要请求者继续执行操作（实际开发中，很少遇到1xx的状态码)

2xx：成功。请求被服务器成功接收并处理。

- 200：客户端请求成功了
- 201：已创建，请求成功并创建了新的资源。
- 206：（部分内容）服务器成功处理了部分请求（断点续传）

3xx：重定向。需要客户端进一步操作以完成请求

- 301：永久重定向。说明请求的资源被永久的移动到新的URI，返回信息会包括新的URI，浏览器会自动定向到新URI
- 302：临时重定向,说明资源只是临时移动，客户端应继续使用原有的URL
- 304：所请求的资源距上一次访问未修改过，服务器会返回该状态，不会返回任何资源。(这个状态和http的缓存机制密切相关，后续单独出一期,协商缓存)

4xx：客户端出现了某种错误，导致请求失败。

- 400：客户端请求的语法错误，服务器无法理解
- 401：未授权，当前请求需要用户身份验证
- 403：禁止，服务器已经理解请求,但是拒绝执行它
- 404：请求资源不存在，服务器无法根据客户端请求找到资源。(比如输入了错误的url就是这种情况)

5xx：服务器错误，服务器在处理请求的过程中发生了错误

- 500（服务器内部错误）：服务器遇到错误，无法完成请求
- 501（尚未实施）：服务器不具备完成请求的功能。 例如，服务器无法识别请求方法时可能会返回此代码
- 502（错误网关）： 服务器作为网关或代理，从上游服务器收到无效响应
- 503（服务不可用）： 服务器目前无法使用（由于超载或停机维护）
- 504（网关超时）： 服务器作为网关或代理，但是没有及时从上游服务器收到请求
- 505（HTTP 版本不受支持）： 服务器不支持请求中所用的 HTTP 协议版本

**应用场景**

下面给出一些状态码的适用场景：

- 100：客户端在发送POST数据给服务器前，征询服务器情况，看服务器是否处理POST的数据，如果不处理，客户端则不上传POST数据，如果处理，则POST上传数据。常用于POST大数据传输
- 206：一般用来做断点续传，或者是视频文件等大文件的加载
- 301：永久重定向会缓存。新域名替换旧域名，旧的域名不再使用时，用户访问旧域名时用301就重定向到新的域名
- 302：临时重定向不会缓存，常用 于未登陆的用户访问用户中心重定向到登录页面
- 304：协商缓存，告诉客户端有缓存，直接使用缓存中的数据，返回页面的只有头部信息，是没有内容部分
- 400：参数有误，请求无法被服务器识别
- 403：告诉客户端进制访问该站点或者资源，如在外网环境下，然后访问只有内网IP才能访问的时候则返回
- 404：服务器找不到资源时，或者服务器拒绝请求又不想说明理由时
- 503：服务器停机维护时，主动用503响应请求或 nginx 设置限速，超过限速，会返回503
- 504：网关超时

-----

### HTTP如何禁用缓存？如何确认缓存？

HTTP/1.1 通过 Cache-Control 首部字段来控制缓存。

```js
//禁止进行缓存
//no-store 指令规定不能对请求或响应的任何一部分进行缓存。
Cache-Control: no-store
```

```js
//强制确认缓存
//no-cache 指令规定缓存服务器需要先向源服务器验证缓存资源的有效性，只有当缓存资源有效时才能使用该缓存对客户端的请求进行响应。
Cache-Control: no-cache
```

-----

### HTTP缓存

（服务器设置的 客户端根据服务器的标识 浏览器自己做处理）

#### **强制缓存**（状态码200）

当缓存数据库中已有所请求的数据时。客户端直接从缓存数据库中获取数据。当缓存数据库中没有所请求的数据时，客户端的才会从服务端获取数据。

![](https://img-blog.csdnimg.cn/2021052020454134.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk3MjAwOA==,size_16,color_FFFFFF,t_70)

#### **协商缓存**（状态码：304）

在强缓存失效之后，客户端会先从缓存数据库中获取到一个**缓存数据的标识**，得到标识后**请求服务端验证是否失效**，如果没有失效服务端会返回304，此时客户端直接从缓存中获取所请求的数据。如果标识失效，服务端返回200并会返回更新后的数据。

![](https://img-blog.csdnimg.cn/20210520204557391.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk3MjAwOA==,size_16,color_FFFFFF,t_70)

**哪些资源可以被缓存——静态资源（js、css、img）不经常更新的 ajax获取的数据**

![](https://img-blog.csdnimg.cn/4e84936901b24b2d95ad702f9d6feb4a.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk3MjAwOA==,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/2a460989a6044de98c9b07633fcd12ff.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk3MjAwOA==,size_16,color_FFFFFF,t_70)

#### **强制缓存的缺点：**

在过期时间内浏览器的资源被修改了，浏览器就拿不到最新的资源，它依旧读取的本地缓存里面旧的资源

> 1. 服务器更新资源后，让资源名称和之前不一样，这样页面会导入全新的资源 html不做强缓存
>
> 2. 当文件更新之后，我们在html导入的时候，设置一个后缀
>
> 3. 别用强缓存
>
> 4. 引入的时候加一个后缀
>
>    ```js
>     <scrtpt src='index.js?12131'>
>     <scrtpt src='index.js?12qwqw1'>   
>    ```

#### **强制缓存：**

![](https://img-blog.csdnimg.cn/20210712184325869.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk3MjAwOA==,size_16,color_FFFFFF,t_70)

> Cache-Control （Response Headers）(在HTTP/1.1中 优先级高)
>
> - max-age 设置缓存有效时间
> - no-cache 不用本地强制缓存，让服务端处理缓存
> - no-store 不强制缓存，不让服务端处理缓存，直接返回资源
> - private 资源只有客户端可以缓存。
> - public 资源客户端和服务器都可以缓存。
>
> Expires （Response Headers）(Expires是HTTP/1.0的字段)
>
> - 控制缓存过期，已被Cache-Control代替
>
> 注：在无法确定客户端的时间是否与服务端的时间同步的情况下，Cache-Control相比于expires是更好的选择，所以同时存在时，只有Cache-Control生效。

> **浏览器的缓存存放在哪里，如何在浏览器中判断强制缓存是否生效？**
>
> 以博客的请求为例，状态码为灰色的请求则代表使用了强制缓存，请求对应的Size值则代表该缓存存放的位置，分别为`from memory cache` 和 `from disk cache`。`from memory cache`代表使用内存中的缓存，`from disk cache`则代表使用的是硬盘中的缓存，浏览器读取缓存的顺序为`memory –> disk`。
>
> **内存缓存(from memory cache)：**内存缓存具有两个特点，分别是快速读取和时效性：
>
> 1. 快速读取：内存缓存会将编译解析后的文件，直接存入该进程的内存中，占据该进程一定的内存资源，以方便下次运行使用时的快速读取。
>
> 2. 时效性：一旦该进程关闭，则该进程的内存则会清空。
>
> **硬盘缓存(from disk cache)：**硬盘缓存则是直接将缓存写入硬盘文件中，读取缓存需要对该缓存存放的硬盘文件进行I/O操作，然后重新解析该缓存内容，读取复杂，速度比内存缓存慢。
>
> 在浏览器中，浏览器会在js和图片等文件解析执行后直接存入内存缓存中，那么当刷新页面时只需直接从内存缓存中读取(from memory cache)；而css文件则会存入硬盘文件中，所以每次渲染页面都需要从硬盘读取缓存(from disk cache)。

### 协商缓存（对比缓存 状态码：304）

服务端判读客户端资源，是否和服务端资源一样一致返回`304`，否则返回`200`和最新的资源

![](https://img-blog.csdnimg.cn/20210712185746139.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk3MjAwOA==,size_16,color_FFFFFF,t_70)

`Last-Modified` [资源标识] 资源最后修改时间

> ##### `Last-Modified / If-Modified-Since`
>
> `Last-Modified`是服务器响应请求时，返回该资源文件在服务器最后被修改的时间，如下。（HTTP/1.0）
>
> `If-Modified-Since`则是客户端再次发起该请求时，携带上次请求返回的Last-Modified值，通过此字段值告诉服务器该资源上次请求返回的最后被修改时间。服务器收到该请求，发现请求头含有If-Modified-Since字段，则会根据If-Modified-Since的字段值与该资源在服务器的最后被修改时间做对比，若服务器的资源最后被修改时间大于If-Modified-Since的字段值，则重新返回资源，状态码为200；否则则返回304，代表资源无更新，可继续使用缓存文件，如下。（HTTP/1.1）

![](https://img-blog.csdnimg.cn/20210712190429801.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk3MjAwOA==,size_16,color_FFFFFF,t_70)

`Etag` [资源标识] 资源唯一标识（一个字符串，类似人类的指纹）

> ##### `Etag / If-None-Match`
>
> `Etag`是服务器响应请求时，返回当前资源文件的一个唯一标识(由服务器生成)，如下。
>
> `If-None-Match`是客户端再次发起该请求时，携带上次请求返回的唯一标识Etag值，通过此字段值告诉服务器该资源上次请求返回的唯一标识值。服务器收到该请求后，发现该请求头中含有If-None-Match，则会根据If-None-Match的字段值与该资源在服务器的Etag值做对比，一致则返回304，代表资源无更新，继续使用缓存文件；不一致则重新返回资源文件，状态码为200，如下

![](https://img-blog.csdnimg.cn/20210712190547291.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk3MjAwOA==,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/20210713155328568.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk3MjAwOA==,size_16,color_FFFFFF,t_70)

> **Last-Modified与Etag的区别**
>
> 1. 优先使用Etag
>
> 2. Last-Modified 只能精确到秒级
>
> 3. 如果资源被重复生成，而内容不变，则Etag更精确
>
> **刷新页面对缓存的影响**
>
> 1. 正常操作：地址栏输入URL，跳转链接，前进后退等【强制缓存有效，协商缓存有效】
>
> 2. 手动刷新：F5，点击刷新按钮，点击菜单刷新【强制缓存失效，协商缓存有效】
>
> 3. 强制刷新：CRTL+F5【强制缓存失效，协商缓存失效】

> 强制缓存优先于协商缓存进行，若强制缓存(Expires和Cache-Control)生效则直接使用缓存，若不生效则进行协商缓存(Last-Modified 与 If-Modified-Since和Etag 与 If-None-Match 是否相等)，协商缓存由服务器决定是否使用缓存，若协商缓存失效，那么代表该请求的缓存失效，重新获取请求结果，再存入浏览器缓存中；生效则返回304，继续使用缓存，主要过程如下:

### 总结

![](https://img-blog.csdnimg.cn/20210713160033748.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk3MjAwOA==,size_16,color_FFFFFF,t_70)



-----

### 说说 HTTP 常见的请求头，响应头有哪些? 作用？

**常见Request Headers**

1. Accept 浏览器可接收的数据格式 如，text/plain

2. Accept-Encoding 浏览器可接收能够接受的编码方式列表，如gzip

3. Accept-Languange 浏览器可接收的语言，如zh-CN

4. Connection: keep-alive 一次TCP连接重复使用

5. cookie

6. Host 请求域名

7. User-Agent 浏览器的浏览器身份标识字符串

8. Content-type 发送数据的格式，如application/json

**常见 Response Headers**

1. Content-type 发送数据的格式，如application/json
2. Content-length 返回数据的大小，多少字节
3. Content-Encoding 返回数据的压缩算法，如gzip
4. Set-Cookie 设置Cookie
5. Cache-Control 缓存控制
6. Expires 控制缓存过期
7. Last-Modified 资源最后修改时间
8. Etag 资源唯一标识

-----

### HTTP的缺点有哪些？

1. 使用明文进行通信，内容可能会被窃听；
2. 不验证通信方的身份，通信方的身份有可能遭遇伪装；
3. 无法证明报文的完整性，报文有可能遭篡改。

----

### HTTPS的工作原理

用户通过浏览器请求HTTPS网站，服务器收到请求，选择浏览器支持的加密和hash算法，同时返回数字证书给浏览器，包含颁发机构、网址、公钥、证书有效期等信息。

浏览器对证书的内容进行校验，如果有问题，则会有一个提示警告。

否则，就生成一个随机数X，同时使用证书中的公钥进行加密，并且发送给服务器。

服务器收到之后，使用私钥解密，得到随机数X，然后使用X对网页内容进行加密，返回给浏览器浏览器则使用X和之前约定的加密算法进行解密，得到最终的网页内容
![](https://img-blog.csdnimg.cn/20210508135718974.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk3MjAwOA==,size_16,color_FFFFFF,t_70)

-----

### HTTPS采用的加密方式有哪些？是对称还是非对称？

HTTPS 采用**混合**的加密机制，使用**非对称密钥**加密用于传输**对称密钥**来保证传输过程的安全性，之后使用对称密钥加密进行通信来保证通信过程的效率。

![](https://img-blog.csdnimg.cn/20210508140250139.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk3MjAwOA==,size_16,color_FFFFFF,t_70)



确保传输安全过程（其实就是rsa原理）：
Client给出协议版本号、一个客户端生成的随机数（Client random），以及客户端支持的加密方法。
Server确认双方使用的加密方法，并给出数字证书、以及一个服务器生成的随机数（Server random）。
Client确认数字证书有效，然后生成呀一个新的随机数（Premaster secret），并使用数字证书中的公钥，加密这个随机数，发给Server。
Server使用自己的私钥，获取Client发来的随机数（Premaster secret）。
Client和Server根据约定的加密方法，使用前面的三个随机数，生成”对话密钥”（session key），用来加密接下来的整个对话过程。

-----

### HTTP和HTTPS的区别

1. HTTP，也就是**超文本传输协议**，它是以**明文方式**发送信息的，所以如果有不法分子截取了 Web浏览器和服务器之间的传输报文，就可以直接获得信息，可想而知这样是不安全。

   HTTPS可以认为是HTTP的**安全版**，是在HTTP的基础上加入了**SSL协议**，这一层协议加在了**传输层和应用层**之间，它可以**对通信数据进行加密，且能够验证身份**，使得通信更加安全。当然不是绝对安全，只是会增加不法分子的破坏成本。

2. 连接方式不同，**HTTPS的端口是443**，而**HTTP的端口是80**;

3. **HTTP协议速度比HTTPS更快**，因为较HTTPS而言，它不需要经过复杂的SSL握手。而HTTPS协议需要经过一些复杂的安全过程,页面响应速度会来得慢。

4. **HTTPS要申请ca证书**，一般免费证书较少，所以需要一定的费用，而HTTP不需要。

-----

### HTTPS的缺点

（1）除了和TCP连接，发送HTTP请求外，还必须和SSL通信，因此通信慢；
（2）SSL必须进行加密处理，在服务器和客户端都需要进行加密和解密的运算处理，因此更多地消耗硬件资源，导致负载增强；
（3）申请SSL证书需要费用。

-----

### SSL中的认证中的证书是什么？

通过使用 证书 来对通信方进行认证。

数字证书认证机构（CA，Certificate Authority）是客户端与服务器双方都可信赖的第三方机构。

服务器的运营人员向 CA 提出公开密钥的申请，CA 在判明提出申请者的身份之后，会对已申请的公开密钥做数字签名，然后分配这个已签名的公开密钥，并将该公开密钥放入公开密钥证书后绑定在一起。

进行 HTTPS 通信时，服务器会把证书发送给客户端。

客户端取得其中的公开密钥之后，先使用数字签名进行验证，如果验证通过，就可以开始通信了。

-----

### 为什么有的时候刷新页面不需要重新建立 SSL 连接？

TCP 连接有的时候会被**浏览器和服务端维持**一段时间，TCP 不需要重新建立，SSL 自然也会用之前的。

-----

### http的请求头中 Keep-Alive 有什么用

`Keep-Alive` 是一个 **HTTP 请求头**，它用于指定 **TCP 连接**应**保持打开状态多长时间**以及可以**重复使用多少次**（主要与 HTTP/1.1 协议中的持久连接相关）

`Keep-Alive` 请求头可以包含两个参数：

1. `timeout`：表示连接应保持打开状态的最长时间（以秒为单位）。在这段时间内，服务器应当等待客户端发送新的请求。
2. `max`：表示在关闭连接之前，允许在该连接上发送的最大请求数。

```js
Keep-Alive: timeout=5, max=100
```

在 HTTP/2 及更高版本中，持久连接已成为默认行为

-----

### 为什么说HTTPS比HTTP安全? HTTPS是如何保证安全的？



-----

### TCP/UDP的区别

两者都是通信协议，也就是传输层的协议，但是他们的通信机制和应用场景不同。

1. TCP 是**面向连接**的协议，建立连接3次握手、断开连接四次挥手，UDP是**面向无连接**，数据传输前后不连接连接，发送端只负责将数据发送到网络，接收端从消息队列读取

2. TCP 提供**可靠**的服务，传输过程采用**流量控制、编号与确认、计时器**等手段确保数据无差错，不丢失。UDP 则**尽可能传递数据**，但不保证传递交付给对

3. TCP **面向字节流**，将应用层报文看成一串无结构的字节流，分解为多个TCP报文段传输后，在目的站重新装配。UDP协议**面向报文**，不拆分应用层报文，只保留报文边界，一次发送一个报文，接收方去除报文首部后，原封不动将报文交给上层应用

4. TCP 只能点对点**全双工通信**。UDP 支持一对一、一对多、多对一和多对多的交互通信

​	**应用场景：**

1. TCP：对于通信数据的**完整性**和**准确性**要求较高的情况，如重要报文传输，邮件发送
2. UDP：对通信**速度**要求较高，对数据信息的**安全性和完整性要求相对较低**的情况，如电话网络，视频会议，实时播报等情况

-----

### 三次握手

> - **seq序号：**用来标识TCP源端向目的端发送的字节流，发起方发送数据时对此进行标记
> - **ack确认号：**只有ACK标志位为1时，确认序号字段才有效 ack=seq+1
> - **标志位：**
>
> ​        + ACK：确认序号有效 
>
> ​		 + RST：重新连接 
>
> ​		 + SYN：发起一个新连接 
>
> ​		 + FIN：释放一个连接

三次握手（Three-way Handshake）其实就是指建立一个TCP连接，建立通道时，需要客户端和服务器总共发送3个包

主要作用就是为了确认**双方的接收能力和发送能力**是否正常、指定自己的**初始化序列号**为后面的可靠性传送做准备

![TCP三次握手](E:\Front\八股文\My八股文\八股图\TCP三次握手.webp)

**过程如下：**

- 一开始，客户端和服务端都处于 `CLOSE` 状态。先是服务端主动监听某个端口，处于 `LISTEN` 状态

- 第一次握手：客户端给服务端发一个 `SYN` 报文。客户端会随机初始化序号`seq`（`client_isn`），同时把 `SYN` 标志位置为 `1`，表示 `SYN` 报文。接着把第一个 `SYN` 报文发送给服务端，表示向服务端发起连接，该报文不包含应用层数据，之后客户端处于 `SYN-SENT` 状态。

  ![format,png-20230309230500953](E:\Front\八股文\My八股文\八股图\format,png-20230309230500953.webp)

- 第二次握手：服务器收到客户端的 SYN 报文之后，首先服务端也随机初始化自己的`seq`序号（`server_isn`），将此序号填入 TCP 首部的「序号」字段中，其次把 TCP 首部的「确认应答号`ack`」字段填入 `client_isn + 1`, 接着把 `SYN` 和 `ACK` 标志位置为 `1`。最后把该报文发给客户端，该报文也不包含应用层数据，之后服务端处于 `SYN-RCVD` 状态。

  ![format,png-20230309230504118](E:\Front\八股文\My八股文\八股图\format,png-20230309230504118.webp)

- 第三次握手：客户端收到服务端报文后，还要向服务端回应最后一个应答报文。首先该应答报文 TCP 首部 `ACK` 标志位置为 `1` ，其次「确认应答号`ack`」字段填入 `server_isn + 1` ，最后把报文发送给服务端，这次报文可以携带客户到服务端的数据，之后客户端处于 `ESTABLISHED` 状态。

  ![format,png-20230309230508297](E:\Front\八股文\My八股文\八股图\format,png-20230309230508297.webp)

- 服务端收到客户端的应答报文后，也进入 `ESTABLISHED` 状态。

从上面的过程可以发现**第三次握手是可以携带数据的，前两次握手是不可以携带数据的**，这也是面试常问的题。

一旦完成三次握手，双方都处于 `ESTABLISHED` 状态，此时连接就已建立完成，客户端和服务端就可以相互发送数据了

![](https://static.vue-js.com/fb489fc0-beb9-11eb-85f6-6fac77c0c9b3.png)

**作用：**

第一次握手：客户端发送网络包，服务端收到了。这样服务端就能得出结论：客户端的发送能力、服务端的接收能力是正常的。
第二次握手：服务端发包，客户端收到了。这样客户端就能得出结论：服务端的接收、发送能力，客户端的接收、发送能力是正常的。不过此时服务器并不能确认客户端的接收能力是否正常。
第三次握手：客户端发包，服务端收到了。这样服务端就能得出结论：客户端的接收、发送能力正常，服务器自己的发送、接收能力也正常。 因此，需要三次握手才能确认双方的接收与发送能力是否正常

-----

###  为什么要三次握手?

如果是两次握手，发送端可以确定自己发送的信息能对方能收到，也能确定对方发的包自己能收到，但接收端只能确定对方发的包自己能收到，无法确定自己发的包对方能收到

并且两次握手的话, 客户端有可能因为**网络阻塞**等原因会发送多个请求报文，**延时到达的请求又会与服务器建立连接**，浪费掉许多服务器的资源

-----

### 四次挥手

`tcp`终止一个连接，需要经过四次挥手

**过程如下：**

​		![format,png-20230309230614791](E:\Front\八股文\My八股文\八股图\format,png-20230309230614791.webp)

- 第一次挥手：客户端发送一个 `FIN` 报文，报文中会指定一个序列号`seq`。此时客户端处于 `FIN_WAIT1` 状态，停止发送数据，等待服务端的确认
- 第二次挥手：服务端收到 `FIN` 之后，会发送 `ACK` 报文，且把客户端的序列号值 +1 作为 `ack` 报文的序列号值，表明已经收到客户端的报文了，此时服务端处于 CLOSE_WAIT状态
- 第三次挥手：如果服务端也想断开连接了，和客户端的第一次挥手一样，发给 `FIN` 报文，且指定一个序列号。此时服务端处于 `LAST_ACK` 的状态
- 第四次挥手：客户端收到 FIN 之后，一样发送一个 `ACK` 报文作为应答，且把服务端的序列号值 +1 作为自己 ACK 报文的序列号值，此时客户端处于 TIME_WAIT状态。需要过一阵子以确保服务端收到自己的 ACK 报文之后才会进入 CLOSED 状态，服务端收到 ACK 报文之后，就处于关闭连接了，处于 CLOSED 状态

![](https://static.vue-js.com/0a3ebb90-beba-11eb-85f6-6fac77c0c9b3.png)

----

### 四次挥手原因

可能**存在未发送完毕**的数据

因为服务器收到客户端断开连接的请求时，可能还有一些数据没有发完，这时先回复ACK，表示接收到了断开连接的请求。等到数据发完之后再发FIN，断开服务器到客户端的数据传送。

----

### TCP怎么保证传输过程的可靠性？

1. **校验和**：发送方在发送数据之前计算校验和，接收方收到数据后同样计算，如果不一致，那么传输有误
2. 确认应答，序列号：TCP进行传输时数据都进行了编号，每次接收方返回ACK都有确认序列号。
3. **超时重传**：如果发送方发送数据一段时间后没有收到ACK，那么就重发数据。连接管理：三次握手和四次挥手的过程。
4. **流量控制**：TCP协议报头包含16位的窗口大小，接收方会在返回ACK时同时把自己的即时窗口填入，发送方就根据报文中窗口的大小控制发送速度。
5. **拥塞控制**：刚开始发送数据的时候，拥塞窗口是1，以后每次收到ACK，则拥塞窗口+1，然后将拥塞窗口和收到的窗口取较小值作为实际发送的窗口，如果发生超时重传，拥塞窗口重置为1。这样做的目的就是为了保证传输过程的高效性和可靠性。

-----

### TCP对应的典型的应用层协议

- FTP：文件传输协议；
- SSH：远程登录协议；
- HTTP：web服务器传输超文本到本地浏览器的超文本传输协议。
- UDP对应的典型的应用层协议：
- DNS：域名解析协议；
- TFTP：简单文件传输协议；
- SNMP：简单网络管理协议。

-----

### IP地址分为哪几类？简单说一下各个分类

![](https://img-blog.csdnimg.cn/20210508134304857.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk3MjAwOA==,size_16,color_FFFFFF,t_70)

-----

### WebSocket协议



-----

### Cookie 和 Session 的区别

**我先解释一下**

http协议本身是一个无状态协议，也就是说服务端并不知道客户端发送过来的多次请求是属于同一个用户，服务器与浏览器为了进行会话跟踪，就必须主动的去维护一个状态，这个状态用于告知服务端前后两个请求是否来自同一浏览器。而这个状态需要通过 cookie 或者 session 去实现。

cookie，它是客户端浏览器用来保存服务端数据的一种机制，当我们通过浏览器去进行网页访问的时候，服务器会某些状态数据以key-value的形式写入到cookie里面，存储到客户端浏览器，然后客户端下一次再访问服务器的时候，我们可以携带这些状态数据，发送到服务器端，服务器端可以根据cookie里面携带的内容去识别使用者。

而session表示一个会话，它是属于服务器端的一个对象，记录服务器和客户端会话状态的机制，session 是基于 cookie 实现的，session 存储在服务器端，sessionId 会被存储到客户端的cookie 中

简单来说服务器端用session来存储客户端在同一个会话里面产生的多次请求的记录，那么基于服务器端的session的存储机制，在结合客户端的cookie机制，我们就可以实现有状态的http协议

**工作原理：**

1. 客户端第一次访问服务器端的时候，服务器端会针对这次请求，创建一个会话session
2. 生成一个唯一的sessionid来标注这个会话，并返回给浏览器
3. 客户端接收到服务器返回的 SessionID 信息后，会将此信息存入到 Cookie 中，同时 Cookie 记录此 SessionID 属于哪个域名
4. 当用户第二次访问服务器的时候，请求会自动判断此域名下是否存在 Cookie 信息，如果存在自动将 Cookie 信息也发送给服务端，服务端会从 Cookie 中获取 SessionID，再根据 SessionID 查找对应的 Session 信息，如果没有找到说明用户没有登录或者登录失效，如果找到 Session 证明用户已经登录可执行后面操作。

![Session](E:\Front\八股文\My八股文\八股图\session.png)

**总的来说：**

cookie是客户端的存储机制，session是服务器端的存储机制，SessionId 是连接 Cookie 和 Session 的一道桥梁

**以上就是我对这个问题的理解**

**区别：**

1. cookie是**客户端的存储机制**，session是**服务器端的存储机制**。session更安全

2. Cookie 一般用来**保存用户信息**。Session 的主要作用就是**通过服务端记录用户的状态**。

   > **Cookie 一般用来保存用户信息。** 比如①我们在 Cookie 中保存已经登录过的用户信息，下次访问网站的时候，页面可以自动帮你把登录的一些基本信息给填了；②一般的网站都会有保持登录的操作，也就是说下次你再访问网站的时候就不需要重新登录了，这是因为用户登录的时候我们可以存放了一个 Token 在 Cookie 中，下次登录的时候只需要根据 Token 值来查找用户即可(为了安全考虑，重新登录一般要将 Token 重写)；③登录一次网站后访问网站其他页面不需要重新登录。
   >
   > **Session 的主要作用就是通过服务端记录用户的状态。** 典型的场景是购物车，当你要添加商品到购物车的时候，系统不知道是哪个用户操作的，因为 HTTP 协议是无状态的。服务端给特定的用户创建特定的 Session 之后就可以标识这个用户并且跟踪这个用户了。

3. **存取值的类型不同**：Cookie 只支持**存字符串数据**，想要设置其他类型的数据，需要将其转换成字符串，Session 可以存**任意数据类型**。

4. **有效期不同：** Cookie 可设置为**长时间保持**，比如我们经常使用的默认登录功能，Session 一般失效时间较短，**客户端关闭（**默认情况下）或者 Session 超时都会失效。

5. **存储大小不同**： 单个 Cookie 保存的数据不能超过 **4K**，Session 可存储数据远高于 Cookie，但是当访问量过多，会占用过多的服务器资源。

**其他：**

- **cookie 是不可跨域的：**每个 cookie 都会绑定单一的域名，无法在别的域名下获取使用，**一级域名和二级域名之间是允许共享使用的**（**靠的是 domain）**。

- > 在JS中处理cooki比较麻烦，因为接口过于简单，只有BOM的`document.cookie`属性。名和值进行URL编码
  >
  > ```js
  > document.cookie = encodeURIComponent("name") + "=" + encodeURIComponent("surui") + "; domain=.badui.com; path=/"
  > ```
  >
  > 可以通过辅助函数来简化相应的操作 `CookieUtil`工具
  >
  > ```js
  > CookieUtil.set()
  > CookieUtil.get()
  > CookieUtil.unset()
  > ```

- > 1. `name=value` //键值对，设置 Cookie 的名称及相对应的值，都必须是字符串类型
  > 2. `domain` //指定  Cookie 可以送达的主机名，默认是当前域名
  > 3. `path` //指定 cookie 在哪个路径（路由）下生效，默认是 '/'。
  > 4. `maxAge` //cookie 失效的时间，单位秒。如果为整数，则该 cookie 在 maxAge 秒后失效。如果为负数，该 cookie 为临时 cookie ，关闭浏览器即失效，浏览器也不会以任何形式保存该 cookie 。如果为 0，表示删除该 cookie 。默认为 -1。- 比 expires 好用。
  > 5. `expires` //	过期时间，在设置的某个时间点后该 cookie 就会失效。一般浏览器的 cookie 都是默认储存的，当关闭浏览器结束这个会话的时候，这个 cookie 也就会被删除
  > 6. `secure` //该 cookie 是否仅被使用安全协议传输。安全协议有 HTTPS，SSL等，在网络上传输数据之前先将数据加密。默认为false。当 secure 值为 true 时，cookie 在 HTTP 中是无效，在 HTTPS 中才有效。
  > 7. `httpOnly` //如果给某个 cookie 设置了 httpOnly 属性，则无法通过 JS 脚本 读取到该 cookie 的信息，但还是能通过 Application 中手动修改 cookie，所以只是在一定程度上可以防止 XSS 攻击，不是绝对的安全
  > 8. 这些参数在Set-Cookie头部使用分号加空格隔开，参数标志只是用于告诉浏览器在上面情况下应该在请求中包含cookie，这些参数并不会随着请求发送给服务器，实际发送的只有cookie的名/值对
  
  


-----

### cookie、localStorage和sessionStorage（Javascript本地存储的方式有哪些？区别及应用场景？）

**区别**

- 存储大小：`cookie`数据大小不能超过`4k`，`sessionStorage`和`localStorage`虽然也有存储大小的限制，但比`cookie`大得多，可以达到5M或更大
- 有效时间：`cookie`设置的`cookie`过期时间之前一直有效，即使窗口或浏览器关闭; `localStorage`存储持久数据，浏览器关闭后数据不丢失除非主动删除数据； `sessionStorage`数据在当前浏览器窗口关闭后自动删除
- 数据与服务器之间的交互方式，`cookie`的数据会自动的传递到服务器，服务器端也可以写`cookie`到客户端(每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题)； `sessionStorage`和`localStorage`不会自动把数据发给服务器，仅在本地保存

**应用场景**

- `cookie`：标记用户与跟踪用户行为的情况
- `localStorage`：适合长期保存在本地的数据（令牌）
- `sessionStorage`：敏感账号一次性登录，推荐使用
- `indexedDB`：存储大量数据的情况、在线文档（富文本编辑器）保存编辑历史的情况

> - `localStorage`本质上是对字符串的读取，如果存储内容多的话会消耗内存空间，会导致页面变卡
> - 受同源策略的限制
>
> ```js
> localStorage.setItem('username','cfangxu');
> localStorage.getItem('username')
> localStorage.key(0) //获取第一个键名
> localStorage.removeItem('username')
> localStorage.clear()
> ```

-----

### Token 和 Session 的区别

Session 是一种**记录服务器和客户端会话状态的机制，使服务端有状态化，可以记录会话信息**。

 Token 是**令牌，访问资源接口（API）时所需要的资源凭证**。Token **使服务端无状态化，不会存储会话信息**。

Session 和 Token 并不矛盾，作为身份认证 Token 安全性比 Session 好，因为每一个请求都有签名还能防止监听以及重放攻击，而 Session 就必须依赖链路层来保障通讯安全了。**如果你需要实现有状态的会话，仍然可以增加 Session 来在服务器端保存一些状态**。

所谓 Session 认证只是简单的把 User 信息存储到 Session 里，因为 SessionID 的不可预测性，暂且认为是安全的。而 Token ，如果指的是 OAuth Token 或类似的机制的话，提供的是 认证 和 授权 ，认证是针对用户，授权是针对 App 。其目的是让某 App 有权利访问某用户的信息。这里的 Token 是唯一的。不可以转移到其它 App上，也不可以转到其它用户上。Session 只提供一种简单的认证，即只要有此 SessionID ，即认为有此 User 的全部权利。是需要严格保密的，这个数据应该只保存在站方，不应该共享给其它网站或者第三方 App。所以简单来说：**如果你的用户数据可能需要和第三方共享，或者允许第三方调用 API 接口，用 Token 。如果永远只是自己的网站，自己的 App，用什么就无所谓了。**

-----

### JWT

[阮一峰的网络日志](http://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html)

#### 是什么

​	一种基于`token`的认证授权机制

**JWT 认证流程：**

- 用户输入用户名/密码登录，服务端认证成功后，会返回给客户端一个 JWT
- 客户端将 token 保存到本地（通常使用 localstorage，也可以使用 cookie）
- 当用户希望访问一个受保护的路由或者资源的时候，需要请求头的 Authorization 字段中使用Bearer 模式添加 JWT，其内容看起来是下面这样

```js
Authorization: Bearer <token>
```

**JWT 使用方式：**

**方式一**

- 当用户希望访问一个受保护的路由或者资源的时候，可以把它放在 Cookie 里面自动发送，但是这样不能跨域，所以更好的做法是放在 HTTP 请求头信息的 Authorization 字段里，使用 Bearer 模式添加 JWT。

  ```dts
  dts复制代码GET /calendar/v1/events
  Host: api.example.com
  Authorization: Bearer <token>
  ```

  - 用户的状态不会存储在服务端的内存中，这是一种 **无状态的认证机制**
  - 服务端的保护路由将会检查请求头 Authorization 中的 JWT 信息，如果合法，则允许用户的行为。
  - 由于 JWT 是自包含的，因此减少了需要查询数据库的需要
  - JWT 的这些特性使得我们可以完全依赖其无状态的特性提供数据 API 服务，甚至是创建一个下载流服务。
  - 因为 JWT 并不使用 Cookie ，所以你可以使用任何域名提供你的 API 服务而**不需要担心跨域资源共享问题**（CORS）

**方式二：**

- 跨域的时候，可以把 JWT 放在 POST 请求的数据体里。

**方式三：**

- 通过 URL 传输

```awk
awk
复制代码http://www.example.com/user?token=xxx
```

![jwt](E:\Front\八股文\My八股文\八股图\jwt.png)

#### **组成**：`Header.Payload.Signature`

- **头部**：

  Header 部分是一个 JSON 对象，描述 JWT 的元数据。

  JSON 对象使`Base64URL` 算法转成字符串。

  ```js
  {
    "alg": "HS256",//签名算法
    "typ": "JWT"// token的类型
  }
  ```

- **负载**：

  `Payload` 部分也是一个 `JSON` 对象，用来存放实际需要传递的数据。

  `JSON` 对象也要使用 `Base64URL` 算法转成字符串

  `JWT` 规定了7个官方字段。

  ```js
  iss (issuer)：签发人
  exp (expiration time)：过期时间
  sub (subject)：主题
  aud (audience)：受众
  nbf (Not Before)：生效时间
  iat (Issued At)：签发时间
  jti (JWT ID)：编号
  ```

  除了官方字段，你还可以在这个部分定义私有字段，下面就是一个例子。

  ```js
  {
    "sub": "1234567890",
    "name": "John Doe",
    "admin": true
  }
  ```

  JWT 默认是不加密的，任何人都可以读到，所以不要把秘密信息放在这个部分。

- **签名**

  `Signature` 部分是对前两部分的签名，防止数据篡改。

  首先，需要指定一个密钥（secret）。这个密钥只有服务器才知道，不能泄露给用户。然后，使用 Header 里面指定的签名算法（默认是 HMAC SHA256），按照下面的公式产生签名。

  ```ja
  HMACSHA256(
    base64UrlEncode(header) + "." +
    base64UrlEncode(payload),
    secret)
  ```

  算出签名以后，把 `Header`、`Payload`、`Signature` 三个部分拼成一个字符串，每个部分之间用"点"（`.`）分隔，就可以返回给用户。

#### Base64URL

前面提到，Header 和 Payload 串型化的算法是 Base64URL。这个算法跟 Base64 算法基本类似，但有一些小的不同。

JWT 作为一个令牌（token），有些场合可能会放到 URL（比如 api.example.com/?token=xxx）。Base64 有三个字符`+`、`/`和`=`，在 URL 里面有特殊含义，所以要被替换掉：`=`被省略、`+`替换成`-`，`/`替换成`_` 。这就是 Base64URL 算法。

#### JWT 的使用方式

客户端收到服务器返回的 JWT，可以储存在 Cookie 里面，也可以储存在 localStorage。

此后，客户端每次与服务器通信，都要带上这个 JWT。你可以把它放在 Cookie 里面自动发送，但是这样不能跨域，所以更好的做法是放在 HTTP 请求的头信息`Authorization`字段里面。

```js
Authorization: Bearer <token>
```

另一种做法是，跨域的时候，JWT 就放在 POST 请求的数据体里面。

#### JWT 的几个特点

（1）JWT 默认是不加密，但也是可以加密的。生成原始 Token 以后，可以用密钥再加密一次。

（2）JWT 不加密的情况下，不能将秘密数据写入 JWT。

（3）JWT 不仅可以用于认证，也可以用于交换信息。有效使用 JWT，可以降低服务器查询数据库的次数。

（4）JWT 的最大缺点是，由于服务器不保存 session 状态，因此无法在使用过程中废止某个 token，或者更改 token 的权限。也就是说，一旦 JWT 签发了，在到期之前就会始终有效，除非服务器部署额外的逻辑。

（5）JWT 本身包含了认证信息，一旦泄露，任何人都可以获得该令牌的所有权限。为了减少盗用，JWT 的有效期应该设置得比较短。对于一些比较重要的权限，使用时应该再次对用户进行认证。

（6）为了减少盗用，JWT 不应该使用 HTTP 协议明码传输，要使用 HTTPS 协议传输。

-----

### 什么是Restful API

传统methods

- GET 获取服务器数据

- POST 向服务器提交数据

现代methods

- GET 获取数据

- POST 新建数据
- PATCH/PUT 更新数据
- DELETE删除数据

传统API：把每个URL当作一个功能
Restful API： 把每个URL当作一个唯一的资源

> 用 URL 定位资源，用 HTTP 动词（GET，POST，DELETE，PUT）描述操作
> 尽量不用URL参数，用method表示操作类型

① url参数

- 传统API ：/api/list?pageIndex=2

- Restful API ：/api/list/2

② method表示操作类型

传统API
- POST请求 /api/create-blog

- POST请求 /api/update-blog?id=100

- GET请求 /api/get-blog?id=100

Restful API

- POST请求 /api/blog

- PATCH请求 /api/blog/100

- GET请求 /api/blog/100

-----

### 协议英文全称

> - **HTTP：**HyperText Transfer Protocol 超文本传输协议
> - **HTTPS：**HTTP Secure 超文本传输安全协议 HTTP over SSL
> - **URL：**Uniform Resource Locator 统一资源定位符
> - **URI：**Uniform Resource Identifier 统一资源标识符
> - **FTP：**File Transfer Protocol 文件传输协议
> - **DNS：**Domain Name System 域名系统
> - **TCP：**Transmission Control Protocol 传输控制协议
> - **UDP：**User Data Protocol 用户数据报协议
> - **NIC：**Network Interface Card 网络适配器，网卡
> - **IP：**Internet Protocol 网际协议
> - **MAC：**Media Access Control Address 媒体存取控制位址
> - **ARP：**Address Resolution Protocol 地址解析协议
> - **SSL：**Secure Socket Layer 安全套接层
> - **TLS：**Transport Layer Security，安全传输层协议
