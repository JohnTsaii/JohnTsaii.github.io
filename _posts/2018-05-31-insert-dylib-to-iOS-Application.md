---
layout: default
title:  "iOS App注入动态库"
date:   2018-05-31 14:11:00 +0800
categories: "iOS reverse dylib"
---


# 目的  
	同事让我向应用注入dylib，来尝试他对dylib注入的防护是否有效。我的第一想法就是不用通过大家用的比较多的手段来注入。因为很容易被针对，所以我想到了直接修改App的可执行文件来达到注入的目的，然后找到了一个[insert_dylib](https://github.com/Tyilo/insert_dylib)工具，这个工具可以通过修改mach-o文件，将你需要加载的dylib生成一条`load_command`插入到`mach-o`文件最后一条的`command_line`里。  
	
# 实践过程  
	接下来我们要做的就很简单了，找到你需要注入的安装包,然后改成.zip文件，解压，进入到`Payload/你的App名字/`的目录，用命令行输入`./insert_dylib 要插入动态库在设备上的路径 要被插入的可执行文件名字 生成新的文件名` 。上面要注意2点，第一点，生成的新的可执行文件需要重签名。第二点，那个dylib需要放入包里面，然后单独对dylib签名，签名的证书要与重新签名app的证书要相同。不然会报签名不对启动不了的问题。  
	
# 需要注意的地方  
	在这个过程中dylib的插入路径我一直不知道怎么写，试了很多都不行，放在系统目录下会报`not found dylib`的错误, 我想到了我们framework里也是dylib的文件，我查看了app可执行文件的load_command那部分，发现有一个路径的变量` @executable_path`，这个路径代表当前可执行文件的路径，因此我把dylib移到与app可执行文件同一目录，insert_dylib的时候 动态库路径就指向` @executable_path/你插入的动态库.dylib`重新打包即可运行了。
