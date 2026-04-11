Self Modifying Code（动态代码加密技术）,指通过修改代码和数据，阻止别人直接静态分析，然后在动态运行程序时对代码进行解密，使程序正常运行

但是.text一般是没有写权限的，在进行自修改和自解密之前，程序需要修改目标内存的权限
Linux系统中，可以使用mprotect函数实现内存权限的修改
Windows系统中，可以使用VirtualProtect函数实现内存权限的修改
![[Pasted image 20251204201212.png]]

解决方法
![[Pasted image 20251204201233.png]]

动态解密：动态调试目标程序，待SMC代码解密后直接使用IDA分析，也可以使用Dump功能将解密后的SMC字节数据DUMP出来，对密文进行静态替换，再使用IDA进行分析
法一：动态调试

接着往下看，可以看到encrypt，并且encrypt函数打开出错了。 ![](https://i-blog.csdnimg.cn/blog_migrate/477cc1e23a30e3bfe34da5c4b4b3c0b3.png)

可以判断是在encrypt处进行了SMC加密，随便下个断点，用 Local windows debugger调试

F7单步走到00401833的call处，

![](https://i-blog.csdnimg.cn/blog_migrate/3a78bd6070b80513cff9f53e16b3a491.png)

单步进入\_Z7函数内 ![](https://i-blog.csdnimg.cn/blog_migrate/ab84aefce025c3c2b6329e7bc6620c5c.png)

点击\_\_Z7encryptPc 先用U将其设为无定义 ![](https://i-blog.csdnimg.cn/blog_migrate/adc067db653e376fc8559a029dbbf8f7.png)

接着选中下面所有数据内容 ![](https://i-blog.csdnimg.cn/blog_migrate/da386e831fd20a1388a228ed3bb33a14.png)

按C转换为代码。 ![](https://i-blog.csdnimg.cn/blog_migrate/41cdb6ceb342f9606f522b3fb2f87a68.png)

之后再点击\_Z7进行P定义为函数， ![](https://i-blog.csdnimg.cn/blog_migrate/fe5fc14a3478440b450db38f20335f11.png)

再用F5反编译得到encrypt函数 ![](https://i-blog.csdnimg.cn/blog_migrate/bca32de89c6d227e76f9cc95ae967173.png)

下面还有一个函数不要忽略了， ![](https://i-blog.csdnimg.cn/blog_migrate/ef5169082405b31445b0c93164a54813.png)

同样P定义为函数， ![](https://i-blog.csdnimg.cn/blog_migrate/0950aaceae9a339665b122219a5cbe38.png)

再F5反编译 ![](https://i-blog.csdnimg.cn/blog_migrate/56778f392342dea5b8a9036b6cb84255.png)
