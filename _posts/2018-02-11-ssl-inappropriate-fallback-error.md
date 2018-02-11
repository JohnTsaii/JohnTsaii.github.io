---
layout: default
title:  "SSL inappropriate fallback error"
date:   2018-02-11 14:11:00 +0800
categories: "network ssl"
---

# inappropriate fallback  
&emsp;&emsp;在查看接口SSL握手的过程中发现了一个`inappropriate fallback`错误，这个错误的产生在于客户端发起一个**Client Hello**消息用的TLS版本小于实际客户端支持的最大ssl版本，而且服务端支持比这个**Client Hello**消息使用的TLS版本更高版本时候。服务端就会返回这个fatal。  
&emsp;&emsp;通常的TLS协商过程：客户端在**Client Hello**消息有个值来表示客户端支持的最大TLS的版本（跟*Record Layer*的版本无关，*Record Layer*表示当前使用的TLS版本），然后服务端返回一个**Server Hello**消息里面包含了服务端支持的TLS版本（这个版本小于或等于客户端支持的最大版本）。例如：客户端最大支持TLSv1.2版本而服务端支持的最大版本是TLSv1.0，那么客户端SSL握手使用的是TLSv1.2版本，而服务端用的是TLSv1.0版本。如果客户端最大支持TLSv1.0而服务端支持最高是TLSv1.2，那么客户端SSL握手使用的是TLSv1.0版本，而服务端使用的也是TLSv1.0版本。  
&emsp;&emsp;如果客户端使用的版本高于服务端支持的最大版本，在有些服务端上会出问题。因此为了是在这种情况下，服务端也能正常响应，某些客户端会**fallback**，即回退TLS版本，如果是TLSv1.2版本，他会回退到TLSV1.1，如果这个还不行会回退到TLSv1.0版本。  
&emsp;&emsp;上面的方法会被攻击者利用来攻击早期版本的TLS漏洞，攻击者会使**Client Hello**失败，强制客户端回退到有漏洞的版本（客户端也可以不执行**fallback**这个功能）。`TLS_FALLBACK_SCSV`密码套件就是为了解决这个问题才被使用的。当使用比客户端支持的最大版本还低的TLS的时候（因为之前使用过最大版本失败了），客户端会**Client Hello**消息里带上这个密码套件。如果服务端收到这个密码套件，他会进行比较，如果服务端支持的版本这个**Client Hello**消息使用的TLS版本高，那么客户端就没有原因使用这个**fallback**方法，然后就会返回一个**inappropriate fallback**错误。（返回这个错误不一定意味着被攻击，只是意味着使用客户端最高版本的握手的时候失败了，再发起这个请求之后服务端返回这个错误）

# 引用
[ssl inappropriate fallback error thread mail](https://mta.openssl.org/pipermail/openssl-users/2017-June/thread.html#5911)  
[RFC TLSv1.2](https://tools.ietf.org/html/rfc5246#section-6.2)  
[SSL_MODE_SEND_FALLBACK_SCSV wiki](https://wiki.openssl.org/index.php/SSL_MODE_SEND_FALLBACK_SCSV)  
