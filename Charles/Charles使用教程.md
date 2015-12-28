# Charles使用教程
###### 2015-12-28  达内纪老师

![small icon](http://7xpk73.com1.z0.glb.clouddn.com/PastedGraphic.png)

**Charles**是一款`收费`抓包修改工具，易于上手，数据请求控制容易，修改简单，抓取数据的开始暂停方便等优势！下面来详细介绍下这款强大好用的抓包工具。

	抓包（packet capture）就是将网络传输发送与接收的数据包进行截获、重发、编辑、转存等操作
	也用来检查网络安全。抓包也经常被用来进行数据截取等
	对于达内学员来说，抓包主要是为了解决网络请求操作，没有专人提供网络接口进行练习的问题
	可以抓取大量已上架AppStore的App的网络请求，进行网络阶段的操作练习

## 前期准备
	以下两个操作是必须的

1. 因为**Charles**不是从AppStore上下载的应用程序，安装和运行都需要修改`系统偏好设置`中的`安全与隐私`选项，修改为允许`任何来源`


![](http://7xpk73.com1.z0.glb.clouddn.com/QQ20151227-7%402x.png)

2. **Charles**的运行需要Mac OS上安装`Java环境`[前往下载安装](http://www.java.com/en/download/mac_download.jsp)

![](http://7xpk73.com1.z0.glb.clouddn.com/QQ20151227-9%402x.png)

## 下载与安装

1. 懒人版：直接下载已破解版本


这里提供一个已破解版本[Charles Version3.11.2 下载](http://pan.baidu.com/s/1nuojEit)`密码:vp6g`，只需要下载以后放入`应用程序`文件夹即可。

2. 正版+破解(适合追求最新版本的处女座同学)


**Charles**是一款收费软件[官方正版下载](http://www.charlesproxy.com/download/)，不过在天朝很少有真的要花钱才能使用的软件，我们可以`破解`解决试用期30天的问题。`不注册的话，每次使用30分钟，工具就会自动关闭`。


如果下载的正版软件那么请自行到百度搜索下载破解文件或者`😊购买正版，支持正版😊`


![](http://7xpk73.com1.z0.glb.clouddn.com/QQ20151227-10%402x.png)


破解方式如下：


![](http://7xpk73.com1.z0.glb.clouddn.com/QQ20151227-1%402x.png)


## 使用篇

1. 打开**Charles**软件。


首次打开会有提示，这里是询问是否允许`Charles`自动配置你的网络，这里选择`Grant Privileges`。


![](http://7xpk73.com1.z0.glb.clouddn.com/QQ20151227.png)


2. 确保iOS设备`iphone或ipad等`与运行**Charles**的Mac电脑处于同一个wifi中


3. 查看Mac电脑的IP地址，`系统偏好设置`->`网络`就可以查看到了。 如图，我的电脑ip地址是`192.168.0.2`


![](http://7xpk73.com1.z0.glb.clouddn.com/QQ20151227-11%402x.png)


4. 打开iOS设备，这里以iPhone为例，设置手机的网络操作由Mac来负责。


`ps：不抓包时，把HTTP代理设置为关闭，否则你的手机无法上网`


![](http://7xpk73.com1.z0.glb.clouddn.com/QQ20151227-12%402x.png)


5. 第四步设置完毕以后，正常情况下Mac电脑上会弹出下方提示框。 表示`Charles`软件检测到一个可供监测的设备。这里有两个选项`Allow（允许）`和`Deny（拒绝）`， 这里当然要选择`Allow（允许）`了。


![](http://7xpk73.com1.z0.glb.clouddn.com/QQ20151227-13%402x.png)


	ps： 如果电脑端没有弹出提示，看看这里，希望能帮到你。

 [错误总结](#error)

## 实战篇

1. 首先我们来看一下`Charles`的界面

![](http://7xpk73.com1.z0.glb.clouddn.com/QQ20151227-16%402x.png)

2.  接下来我们试着抓取`网易新闻`App的请求。

 ![](http://7xpk73.com1.z0.glb.clouddn.com/QQ20151227-18%402x.png)



* 首先让我们先点击一下垃圾桶图标，清空之前的记录。
* 在手机上使用下拉刷新操作，观察**Charles**的反应，可以看到网络请求的地址和类型等数据了。

![](http://7xpk73.com1.z0.glb.clouddn.com/QQ20151227-19%402x.png)

* 让我们来切换到`Response`选项，看看请求的返回值。

![](http://7xpk73.com1.z0.glb.clouddn.com/QQ20151227-20%402x.png)

* 还有更强大的功能，我们可以通过切换到`JSON`标签，查看解析类应该如何编写。

![](http://7xpk73.com1.z0.glb.clouddn.com/QQ20151227-21%402x.png)



## <a name="error"></a>错误总结
1.如果手机设置完成http代理后，电脑端没有弹出提示。检查下下方位置，软件的端口与手机的端口号是否一致。

![](http://7xpk73.com1.z0.glb.clouddn.com/QQ20151227-17%402x.png)


















