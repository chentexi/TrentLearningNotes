# 你觉得HTTPS能防止重放攻击吗?

[Java后端技术](javascript:void(0);) *2022-01-25 09:19*

以下文章来源于孤独烟 ，作者孤独烟

**.孤独烟说java,用来分享行业内的java技术和架构！](https://mp.weixin.qq.com/s/v2HAQ_tE4U2pxtzUsinU_A#)

## **引言** 

老规矩，先来一段面试情景再现~~

> **某日，一求职者来到阿雄公司**
>
> ------
>
> **(一阵项目介绍瞎扯……)**
>
> ------
>
> **阿雄:"网络这块学的如何?"**
>
> ------
>
> **求职者:"只懂一点开发中常用的HTTPS和HTTP协议！"**
>
> ------
>
> **(求职者内心:千万别深问啊，我只背了点八股文！)**
>
> ------
>
> **阿雄:"那说一说HTTPS主流程什么样的？"**
>
> ------
>
> **求职者:"懂的，懂的，客户端携带随机数…(省略1000字)…最后生成了对称密钥，客户端和服务端利用对称密钥进行通信！"**
>
> ------
>
> **阿雄听到这里，嘴角VV一笑，心里想到这怎么听着这么像网上某某文章提到的八股文,于是决定问一点这些文章里都避开的内容！**
>
> ------
>
> **于是阿雄继续问道"如果此刻有一个中间人，监听一次TCP连接中的HTTP请求，然后重放这些http请求，进行重放攻击，HTTPS协议能防止该攻击么？"**
>
> ------
>
> **求职者:"我??我??我？？不知！"**
>
> ------
>
> **阿雄:"可能刚才问题有点难，我换个法子问！你刚才提到，客户端和服务端利用对称密钥进行通信，具体怎么加密的，你知道么？"**
>
> ------
>
> **(求职者内心:"不对啊，八股文都只讲到最后生成了对称的会话密钥进行通信，都没下文了")**



## 正文

### 协议流程

我们先来回忆一下HTTPS的通信流程，HTTPS协议 = HTTP协议 + SSL/TLS协议，摘取一下网上一些八股文的回答(以RSA密钥交换的为例)！

- (1)客户端生成一个随机数client_random,TLS版本号，发送到服务端

- (2)服务端发送自己的随机数server_random，服务器使用的证书，发送到客户端

- (3)客户端利用CA公钥对证书进行验证，取出服务器公钥

- (4)客户端生成随机数pre_master_secret,利用服务器公钥进行加密，传送到服务端

- (5)服务端利用服务器私钥进行解密取出pre_master_secret

- (6)服务端和客户端此时利用随机数client_random,server_random,

  pre_master_secret算出对称密钥(master_secret)，利用对称密钥进行对称加密通信

*画外音:*是不是贼熟悉，有背过网络八股文的，一看就懂！

关键问题就在步骤(6)，怎么进行加密的？很多文章都没有说明，甚至有的人以为，拿client_random+server_random+pre_master_secret直接拼成一个字符串，然后就是对称加密密钥，客户端和服务端拿这个密钥对数据进行加密通信！！

对此，我只能说:"Too young too simple!离谱啊！！"

那正确的过程是怎么样的呢，我们继续往下看！

### 协议分析

我先给本文提到的英文单词，给上我的中文翻译，以防大家混淆:

- client_random 客户端随机数
- server_random 服务端随机数
- pre_master_secret 预备主密钥
- master_secret 主密钥
- key_block 密钥块(有的文章把这个东西称为会话密钥)

先大致有个印象，继续往下阅读

现在我们已经有三个参数client_random，pre_master_secret，server_random，服务端和客户端分别会根据这三个参数，推导出master_secret，一旦master_secret被推导出来，会立刻删除pre_master_secret。(**摘自rfc2246,section8.1**）

当master_secret计算出以后，立刻计算key_block(**摘自rfc2246,section6.3**)，这个密钥块，有的文章里又说他是会话密钥！计算公示如下，

```
 key_block = PRF(master_secret,
         "key expansion",
         server_random +
         client_random)
```

如公式所示，PRF是一个Hash算法，如SHA256这些，具体用哪一个取决于TLS协议的版本！我们得到key_block后，可以基于到key_block继续推导出6个密钥值，分别是

- client_write_MAC_key 客户端消息认证码密钥
- server_write_MAC_key 服务端消息认证码密钥
- client_write_key 客户端对称加密密钥
- server_write_key 服务端对称加密密钥
- client_write_IV 客户端初始化向量
- server_write_IV 服务端初始化向量

整个过程用一张图来说明，注意了这六把密钥是根据key_block推导而出，也就是意味着这六把密钥是服务端和客户端共同持有的！

![图片](https://mmbiz.qpic.cn/mmbiz_png/SYoYmIOcI5qvAnHia1MCcFPIC0C9lgwIkMLHYd4GLJxM8pejgDU42Jth2zq3yqT4EordkIv4RibZFlgVj0AlmpibA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

大家一定也发现了，你的密钥前都带有client或者server前缀，这代表密钥是服务端使用还是客户端使用！例如，客户端用client_write_key进行数据加密，发送数据，服务端收到消息后利用client_write_key进行解密。而后服务端使用server_write_key进行数据加密回复信息，客户端收到消息后用server_write_key解密服务端发来的信息！

OK，我们继续往下看！
现在我们已经有了6把密钥了，已经需要发送的消息data，那么加密流程具体怎么样的呢？
TLS一共有三种加密模式，流加密模式、分组模式、 AEAD 模式，我以流加密模式来进行说明，如下图所示

![图片](https://mmbiz.qpic.cn/mmbiz_png/SYoYmIOcI5qvAnHia1MCcFPIC0C9lgwIkhPOfic79vExkjUWrPjpPqzCyMVY9wFFapSMW55QbxgCrxsa6q80EnOg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我们现在来看上面的第二步，利用write_mac_key对数据加密，加上MAC验证码，利用MAC码来保证完整性。

那么，这个MAC验证码的生成公式又是怎么样的呢？

### MAC验证码

在流加密模式下，MAC验证码公式为(**摘自rfc2246,section6.2.3.1**)

- 

```
HMAC_hash(MAC_write_secret, seq_num + TLSCompressed.type +TLSCompressed.version + TLSCompressed.length +TLSCompressed.fragment))
```

看到入参中的seq_num了么？**这就是数据的序列号，这个序号就是用来防止重放攻击的！**那这个序列号怎么用的呢？
假设，此时服务端和客户端连接成功后
*(1)*客户端会在内存中记录 client_send 和 client_recv，默认值为0.客户端每发送一条消息，client_send 会加1，每接收一条服务端发来的消息，client_recv 会加1。
*(2)*服务端也会在内存中记录 server_send 和 server_recv，作用和客户端的作用一样。服务端每发送一条消息，server_send 会加1，每接收一条客户端发来的消息，server_recv 会加1。
*(3)*客户端发送数据时，以当前client_send作为seq_num,计算mac值，发送给服务端，然后client_send加1。
*(4)*服务端收到消息后，先以当前server_recv值，进行完整校验，校验成功后server_recv加1。然后以server_send为作为seq_num,计算mac值，发送给客户端，然后server_send加1。
*(5)*也就是说，如果发送和接收都正常，那么 client_send = server_recv、client_recv = server_send

假设，客户端和服务端相互通信了4次，client_send = server_recv=3(从0开始，所以是3)，服务端检验完第4次消息后，server_recv加1，此时server_recv=4。攻击者如果想重放第4次消息，第4次消息中的client_send值是3，就会出现校验失败的情况！从而能够抵挡住重放攻击！

OK，讲到这里，基本上能回答最开始提出的问题了！

当然，TLS协议本身的内容比较多，我在这里放上TLS协议的地址，大家有兴趣可以自己去查看:
`https://www.rfc-editor.org/rfc/rfc2246.html`

### 课后思考

假设，我们用符号`[]`表示一次TCP连接，`0,1,2,...`代表数据包序号，根据上面的分析，对于这种形式的重放攻击，`[0,0,0,1,1,2,2,3,3,3….]`，HTTPS协议是能够拦截的！
那如果不是一次请求里的重放攻击呢？例如形式是`[0,1,2,3….]`,`[0,1,2,3….]`，这种形式的重放攻击，HTTPS协议能够拦截么？有答案，可以在留言区进行回复！

**提示**:想一想看，最开始的客户端随机数和服务端随机数的作用!