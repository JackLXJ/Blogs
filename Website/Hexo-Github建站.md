---
title: Hexo+Github建站
date: 2018-09-10 14:24:27
tags: [Hexo,git,node.js,网站建设]
categories: 网站建设
copyright: true
summary_img: https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2506730831,3252862282&fm=27&gp=0.jpg
---

## 前言

gitHub是一个面向开源及私有软件项目的托管平台，也是版本控制库因为只支持git 作为唯一的版本库格式进行托管，故名gitHub。此后，2018年6月4日，微软宣布，通过75亿美元的股票交易收购代码托管平台GitHub。
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

官网：
Github：[https://github.com/](https://github.com/)
Hexo：[https://hexo.io/zh-cn/docs/](https://hexo.io/zh-cn/docs/)

> 以上摘自官方解释

作为一位Coder，一直想找个安静的地方沉淀一下自己，记录自己学习的过程并分享所走过的坑。网上也有各种各样的建站方法，例如[WordPress](https://cn.wordpress.org/)、[emlog](http://www.emlog.net/)、[Typecho](http://typecho.org/)等等平台。但是绝大多数的平台的使用都避免不了备案等一系列的困扰，对于懒癌患者来说无疑是一大痛病，通过海量信息的层层筛选，鄙人最终发现Hexo+Github能够很好的满足大多数人的要求，既简单又美观，使用它来搭建属于自己的个人博客再好不过了。如果你有也有建站的想法的话，那么以下内容将记录了我搭建过程所走的坑，或许能够帮助到你，久而久之，你还会发现其中还有很多有意思的美化操作。

---

> 以下的搭建过程是针对小白所实现的，例如github仓库的创建、环境变量的配置、git终端等一些基础操作都有较为详细的说明。由于鄙人语言功底不行，如果有拗口、错别字、歧义或者不解的地方可在文章末端或右方的Gitalk留言，博主看到会第一时间解释，在此谢过。


## 一、搭建环境

### 环境介绍：
1. windows系统。系统根据自己的需要准备即可，mac、linux皆可，本文以windows系统环境下搭建为例。
2. [git](https://git-scm.com/)。安装之后方便使用各种命令，还能够更好的clone github仓库。
3. [node.js](https://nodejs.org/en/)。一个Javascript运行环境，网站的搭建必须建立在这个框架之上。
4. [Hexo](https://hexo.io/zh-cn/)。使用命令可以直接将Hexo生成的静态资源存储到Github上，然后使用自己的github账户即可访问。

### 安装：

#### Git的安装：
你可在git官网中根据自己的需要进行下载：[https://git-scm.com/](https://git-scm.com/)。打开之后你将看到如下内容：
<img src="https://gss0.baidu.com/-vo3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/b90e7bec54e736d1daf6c64f96504fc2d4626994.jpg" width="500"></img>

将其下载到指定的磁盘，然后傻瓜式安装即可。安装好后打开cmd终端，执行`git --version`，若出现`git version 2.19.2.windows.1`之类的输出则说明已经成功安装。

#### node.js的安装：

node.js的安装和Git的安装如出一辙，同样的操作下载node.js并安装即可，安装好后我们在cmd终端使用`node -v`命令，如出现`v10.13.0`类似输出，则说明已经成功安装。
node.js下载：https://nodejs.org/en/
<img src="https://gss0.baidu.com/9fo3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/b999a9014c086e068f8d8a0c0f087bf40ad1cb54.jpg" width="500"></img>

#### github注册

进入github的注册页面：https://github.com/
<img src="https://gss0.baidu.com/-Po3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/9358d109b3de9c82caa8b3c86181800a18d843a3.jpg" width="500"></img>

然后根据流程填写相应的信息即可。

## 二、博客搭建

### 创建仓库并部署

注册了github之后，我们需要创建一个仓库来存储我们的网站源码，创建的仓库名也就是我们博客访问的url地址，该url是采用子域名的方式，其一般形式为：
```
XXX.github.io
```
 上面的XXX一般代表着你注册时的github用户名，所以在你注册之后该仓库名一般是固定的，仓库的创建及部署流程如下：

* 进入个人github主页之后点击`New repository`来创建仓库，如下：
<img src="https://gss0.baidu.com/-vo3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/3b292df5e0fe9925b1eb231739a85edf8cb17195.jpg" width="400"></img>
* 之后按照如下内容进行创建
<img src="https://gss0.baidu.com/7Po3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/a71ea8d3fd1f4134b010b039281f95cad1c85e55.jpg" width="500"></img>
* 完成以上操作之后，你就已经成功创建好了自己的仓库。这时我们需要利用git命令来生成秘钥。鼠标右键桌面选择`git bash here`，然后在git终端执行以下命令：
```
ssh-keygen -t rsa -C XXX@XXX.com
```

其中`XXX@XXX.com`指的是你注册github时候使用的邮箱，在命令执行的时候回有一些yes、no的选择，直接默认回车即可，最终你将会看到类似如下内容：
```
Your identification has been saved in /c/Users/you/.ssh/id_rsa.
Your public key has been saved in /c/Users/you/.ssh/id_rsa.pub.
The key fingerprint is:
xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx your_email@example.com
```

之后你将在`c/Users/you/.ssh/id_rsa.pub`路径文件看到生成的秘钥对，这个文件我们暂且打开，之后复制粘贴会用到。

> 补充：打开git bash here之后我们首先需要配置一下个人信息，在git终端分别配置自己的用户名和邮箱。命令如下：
```
git config --global user.name XXX   # XXX表示你github注册时的用户名
git config --global user.email XXX  # XXX表示你github注册时的邮箱
```

* 之后我们需要将ssh key添加到我们的github账户。在个人github主页找到settings，然后点击SSH and GPG keys，之后再出现的页面的中点击`New SSH key`，随后根据如下图操作进行添加ssh key：
<img src="https://gss0.baidu.com/7Po3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/a71ea8d3fd1f4134b010b039281f95cad1c85e55.jpg" width="500"></img>

* 打开`git bash here`，执行`ssh -T git@github.com`，之后会出现一系列的问题，我们只需要回答yes即可，最终会输出如下类似内容：
```
Hi username! You've successfully authenticated
```

* OK，完成以上流程后，你的本机就可以连接github了。

<img src="http://b.hiphotos.baidu.com/image/pic/item/2934349b033b5bb5c8f35e343cd3d539b700bcd7.jpg" width="150"></img>

### Hexo博客框架的搭建

在完成以上操作后，我们就可以来使用Hexo了，你可通过如下操作来进行。

* 在以上操作的基础上，我们首先安装一下hexo。根据自己的需要在磁盘中创建一个名为Hexo文件夹，为了更好的管理文件，鄙人是在E盘的根目录中创建改文件夹的。之后进入该文件并在当前路径下打开`git bash here`，依次运行如下命令来进行创建：
```
npm install hexo-cli -g
npm install hexo --save
```
执行完成之后，你会发现在该目录之下会有个`node_modules`文件夹生成，如此一来，你就已经安装好了Hexo，离终点又近了一点 (*￣rǒ￣)

* 以上的node_modules文件生成之后，我们需要配置一下Hexo的环境变量，以便在cmd中可以直接执行后续博客操作的命令。进入到node_module文件夹下的bin目录，然后复制该bin目录的路径，如下：
<img src="https://gss0.baidu.com/-Po3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/242dd42a2834349bc0fedfc2c4ea15ce37d3bef9.jpg" width="500"></img>

后面的添加环境变量的操作比较简单，所以就描述一些流程，就不贴图了。如果有遇到问题的可联系鄙人。后续操作描述如下：1. ctrl+D切换到桌面。2. 右键此电脑，打开属性。3. 点击左侧的高级系统设置。 4. 点击环境变量。5. 在用户变量或者系统变量中找到Path并双击它（推荐更改用户变量）6. 双击之后点击新建，然后将以上的复制的bin目录粘贴至此。6. 然后一步一步的确定、确定、确定。OK，完成了，是不是很简单 (*￣rǒ￣)。

<img src="http://g.hiphotos.baidu.com/image/pic/item/78310a55b319ebc4092170d98926cffc1e171610.jpg" width="150"></img>

在以上操作完成之后，win+r，打开cmd终端，然后执行`Hexo -v`，若出现如下类似信息，则说明你的Hexo已经成功配置环境变量。
<img src="https://gss0.baidu.com/9vo3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/7aec54e736d12f2e48bb1f2742c2d5628535683f.jpg" width="500">

* 随后，我们需要创建我们的博客站点的主目录，你可使用我推荐的方式进行创建，当然你也可以根据自己的喜好方式进行创建。首先在E盘的根目录下创建ZerosBlog文件夹（自定义），然后进入该文件夹并创建XXX.github.io文件夹（该文件夹名必须与你之前创建的github仓库名一致，固定），进入到该目录右键点击`git bash here`来打开git终端，之后在该终端下根据如下命令一步步进行操作：

初始化hexo：
```
hexo init
```
自动安装网站所需组件：
```
npm install
```

* OK，至此，你已经基本完成了网站的建设，可以说是万事具备，只欠东风了。一个基本的Hexo博客框架已经完成了，我们需要导入自己的喜欢的主题即可正常使用了，主题的引入操作如下。


## 三、主题引入

Hexo中有很多很多很多的主题（一个、两个、三个。。。 Σ( ° △ °|||)︴ 好吧，我不知道有多少个，因为他会被许多的大神更新着，如果好奇的话自行了解即可），其主题官网为：[https://hexo.io/themes/](https://hexo.io/themes/)，你可以在此观摩并使用任意一个来作为你博客的主题，但据统计，绝大多数使用hexo+github来搭建博客的都是使用NexT，它的“精于心，简于形”的简单美受到了许多人的青睐，所以以下将以NexT为例来作为我们主题的引入，当然，你也可以去阅读[NexT的主题文档](http://theme-next.iissnan.com/)。

在Hexo主题页面ctrl+F并输入next查找到NexT主题，然后点击进入到NexT主题的github页面，该页面存储了NexT主题的源码，我们需要将其下载下来为己所用。在前面我们已经提到了git的最为方便之处就是可以随意clone github的资源，在这个操作就可以显露出来了 ┗|｀O′|┛ 嗷~~ ┗|｀O′|┛ 嗷~~ ┗|｀O′|┛ 嗷~~。

根据如下图所示复制出该主题的仓库链接：
<img src="https://gss0.baidu.com/-Po3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/f31fbe096b63f624d0bb73448a44ebf81b4ca397.jpg" width="500"></img>

复制好该链接后我们进入`E:\ZerosBlog\XXX.github.io\themes`文件夹下，右键点击`git bash here`进入git终端，并执行如下命令，其中链接为你上一步所复制的内容
```
git clone https://github.com/theme-next/hexo-theme-next.git
```
如果你累了的话可以喝口茶，稍等片刻之后就会在该目录之下成功下载NexT主题了。
<img src="http://n.sinaimg.cn/sinacn/w580h498/20180304/1532-fxipenm7751606.jpg" width="150"/>

下载NexT主题后，我们需要配置来达到使用该主题的目的，该配置文件是属于站点的，其路径为`E:\ZerosBlog\XXX.github.io\_config.yml`，我们用文本编辑器（notepad、notepad++、sublime text、Vim......）打开它，然后ctrl+f输入theme查找到theme属性，然后将值改为next，如下所示：
<img src="https://gss0.baidu.com/-4o3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/8644ebf81a4c510f657b535d6d59252dd42aa53e.jpg">

在NexT中已经为我们准备了四种博客样式，其配置文件在主题的配置文件中，即`E\ZerosBlog\XXX.github.io\themes\next\_config.yml`文件，我们用文本编辑器（notepad、notepad++、sublime text、Vim......）打开它，然后ctrl+F输入scheme查找到如下内容：
<img src="https://gss0.baidu.com/9fo3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/8694a4c27d1ed21b4c479fa2a06eddc450da3fee.jpg">
可以看见总共有四种主题`Muse、Mist、Pisces、Gemini`，你可以根据自己的喜好选择其中一种，然后将其他三种注释即可，ctrl+s保存然后退出

随后我们来到站点的根目录下，即`E:\ZerosBlog\XXX.github.io`，打开git终端，完成如下三步走命令

```
hexo clean  # 清除缓存
heo g   # 生成静态资源
hexo d  # 部署至github
```
在以上命令执行过程中，可能会遇到一个登陆表单的突然出现，我们只需要根据自己github注册时所填的信息进行登陆即可，命令执行完成之后我们的站点已经完成了部署并请求`https://XXX.github.io/`即可访问到自己的网站了，如下图所示：
<img src="https://gss0.baidu.com/-4o3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/2e2eb9389b504fc221d10142e8dde71191ef6def.jpg" width="500"></img>




## 四、总结

以上的搭建过程是针对小白所实现的，例如github仓库的创建、环境变量的配置、git终端等一些基础操作都有较为详细的说明。由于鄙人语言功底不行，如果有拗口、错别字、歧义或者不解的地方可在文章末端或右方的Gitalk留言，博主看到会第一时间解释，在此谢过。

至此，已经完成了博客的搭建，但是我们左看看、右看看，不管怎么看都似乎显得有点单调，在之后将会介绍Hexo的基本命令和博客的美化，可以引入一些插件，比如像Gitalk在线聊天、APlayer、字数统计等一些插件。

OK，结束了。

<div style="text-align: right">2018-09-10,By Zero</div>