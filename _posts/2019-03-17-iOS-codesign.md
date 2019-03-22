---
layout: default
title:  "Apple Codesign"
date:   2019-03-16 20:47:22 +0800
categories: "iOS macOS codesign"
---

# Code Signing
	代码签名顾名思义就是对你的代码进行签名（不过这边是对你编译打包后的代码进行签名，还有你的包里所有的资源文件），他的目的是确保你的app没有被不信任的第三方所篡改。还有一个目的，让系统知道2个不同版本的app是相同的，而不需要重复申请比如定位，相册等权限。
# 组成
## Seal
	App各个部分代码的hash值或者校验码的集合。包含了App内包含的所以可执行文件（App主体可执行文件，动态库或者framework）、Info.plist、code requirements等等。通过一种hash算法对各个部分生产hash值。
	app运行的时候内核会重新根据规则生产hash值进行验证。但是hash可以被改变因此下面的数字签名保证了这点的安全。
## Digital Signature
### 原理
	简单的来说是非对称加密，私钥加密，公钥解密。对上面生成的hash值集合进行加解密。只有签名者才拥有私钥，所以想修改hash值重新加密是几乎不可能的。
### 注意点
	每个加密后的数据保存的地方有3个
- 如果是可执行文件，那么签名后的数据保存在可执行文件里。
- 资源文件包括Info.plist 保存在App包的*_CodeSignature/CodeResources*里。
- framework及类似资源相同的道理，保存在framework下的 可执行文件和*_CodeSignature/CodeResources*里
## Code Requirements
代码要求其实就是一套让系统评估签名的规则。签名者可以指定这个规则，但是系统可以选择是否按照该规则执行。

## Entitlements
	Entitlements是一个plist文件，该文件指定了那些系统资源可以被你的应用调用。你可以在Xcode的`Capabilities`里选择应用需要用到的系统功能，然后Xcode会自动帮你生产`.entitlements`文件。
## Provisioning Profiles
	 Provisioning Profiles是一个cms(Cryptographic Message Syntax)格式的文件，他的作用就是绑定签名、Entitlements和sandbox三者的配置文件。里面的内容可以通过命令`$ security cms -D -i example.mobileprovision`读取。内容包含了签名证书信息、Entitlements内容，以及可调试、调试机器列表等内容。
## recodesign

# 参考资料
[Inside Code Signing](https://www.objc.io/issues/17-security/inside-code-signing/)
[macOS Code Signing In Depth](https://developer.apple.com/library/archive/technotes/tn2206/_index.html)
[Code Signing Guide](https://developer.apple.com/library/archive/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40005929-CH1-SW1)
