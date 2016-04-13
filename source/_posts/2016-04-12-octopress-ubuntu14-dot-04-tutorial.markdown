---
layout: post
title: "像黑客一样写blog：Ubuntu14.04+Octopress+Github Pages"
date: 2016-04-12 22:05:23 +0800
comments: true
categories: web
---


之前在windows 10 上部署了一套Octopress环境，但是如果要发布博客，就只有在安装了Octopress环境才能发布blog.因为就有了这篇写在Ubuntu14.04系统中的文章。与在windows上安装有些不同，有些需要注意的问题，在这篇文章会指出这些问题，并给出解决办法。希望此篇文章能够为有意在Ubuntu14.04上安装Octopress环境的童鞋提供一个参考操作步骤。如有不准确的地方，还请批评指正。


> - ***基本工具介绍***
> - ***安装依赖***
> - ***安装Octopress***
> - ***发布Github Pages***
> - ***绑定私有域名***
> - ***发布第一篇Blog***
> - ***换台电脑继续写博客***
> - ***参考链接***

<!--more-->

## 基础工具介绍 ##

此次环境部署是在使用VMware12pro创建的Ubuntu14.04虚拟机中创建的，关于如何安装创建Ubuntu虚拟机不在本文讨论范围，读者自行咨询[谷歌老师](https://www.google.com.hk/search?q=vmware+12+%E8%99%9A%E6%8B%9F%E6%9C%BA+ubuntu+14.04&rlz=1C1CHUN_zh-CNCN651CN651&oq=vmware+12+%E8%99%9A%E6%8B%9F%E6%9C%BA+ubuntu+14.04&aqs=chrome..69i57.941j0j9&sourceid=chrome&ie=UTF-8)。在这里简要介绍一下[Octopress](http://octopress.org/)，Markdown，Github Pages和Git。

### Octopress ###

Octopress与Wordpress类似，也是搭建个人博客的工具。本人不熟悉Wordpress在此不再赘述。Octopree非常灵活，可定制性非常强，如果熟悉Markdown(一种简单的标记语言，下文简单介绍)以及简单的Ruby语言，就可以自定义实现诸多功能。Octopress主要有三大优势：

1. Octopress完全免费开源。并且可以将Octopress搭建的博客绑定到你个人的Github Pages(也是个人免费的)上，无需额外购买博客服务器。
2. A blogging framework for hackers. 这是Octopress[官网](http://octopress.org)大标题上的一句话。Octopress是专为黑客而生，能够像黑客写代码一样来写博客。
3. Octopress可以完全定制，可以根据自己的需要充分定制，纯代码生成，同时Octopress拥有庞大的用户社区，如有问题可以在社区中寻求帮助，在搜索引擎中搜索Octopress关键字，可以得到数以亿计的结果，由此可见Octopress是受到诸多人的喜爱和使用的。

### Markdown ###

Markdown是一种纯文本标记语法，非常简单，完全可以使用系统中的纯文本文件编辑器进行编写。虽然简单，但是却可以生成表现力极强的文档。Octopress中发布的每篇博文都是以`.markdown` 为后缀命名的。每篇博文都是一个完整的Markdown文档，Markdown非常简单，即使是非IT专业人士也可以非常容易上手，并很快写出格式规范，精彩的文章。虽然Markdown文档可以使用纯文本编辑器编辑书写，但有很多很方便所见即所得的编辑器。在这里推荐几款非常流行的markdown编辑器，[MarkdownPad2](http://markdownpad.com/)和作业部落出品的[Cmd Markdown](https://www.zybuluo.com/mdeditor)，这些编辑器所即所得并且非常简便易用。在Octopress发布博客时，我们可以使用这些工具编辑我们的`.markdown`文档，然后保存在`/octopress/source/_posts`文件夹中，然后运行Octopress生成部署命令发布出去(参见下文)。

### Github Pages和Git ###

Github Pages和依托Github平台开发出的一款提供静态网站的产品。[Github](https://github.com/)是一个著名的代码托管网站，码农应该没有不知道的。若要使用该产品，首先需要一个[Github](https://github.com/)的一个账号。

在有了Github账号后，需要在这个账号中创建一个仓库`new repository`,此时需要注意，创建的仓库名字格式必须为`yourname.github.io`,这里的`yourname`必须是你在Github上的创建账号时使用的用户名，这是受到Github Pages产品服务所限制约定的，否则无法使用Github Pages服务。Github Pages服务根据你创建的仓库名字自动识别为这是一个用来创建Github Pages仓库，也就是说一个Github账号只能创建一个Github Page仓库。之后使用Octopress上传到这个仓库里面所有博客的源代码以及其他所需文档。Octopress如何上传并绑定这个Github Pages仓库，下文分解。

Git是什么？从名字上看就和Github脱不开关系。简要来说Git是一个分布式版本控制系统，Github本身是一个代码托管平台，它所使用的代码管理系统就是Git。如果稍微熟悉Linux操作系统的人，肯定知道一个非常著名的计算机界的传奇人物Linus，他正是Git的创造人，在此致敬！

当你使用Git管理你自己的代码时，你本地电脑上和远程服务器上都有一个完整的代码版本库。通过Git，团队协作时可以非常方便的彼此推送自己修改的部分代码。

## 安装依赖 ##

下面开始接受Octopress安装过程，本文虽然是在Ubuntu14.04系统之上搭建，但是在其他系统Windows，Mac OS也是非常类似的。

首先安装Octopress的依赖软件：`ruby`，`nodejs`，`git`。

**安装Ruby**

为了安装Ruby，我们使用[Ruby Version Manager](https://rvm.io/) (RVM),使用RVM你可以同时安装多个版本的Ruby，可以在不同版本之间非常容易的进行切换。安装Ruby在命令终端输入：
  
{% codeblock 安装RVM　%}
$ curl -L https://get.rvm.io | bash -s stable --ruby
$ #重启 shell
$ source ~/.rvm/scipts/rvm #实际安装时具体路径可能不同，在命令输出中会有提示，以实际命令输出为准
$ #检查RVM是否安装成功
$ rvm -v
$ #检查Ruby是否安装成功
$ ruby -v
{% endcodeblock %}


如果rvm安装过程中，没有同时安装ruby，可以手动安装

{% codeblock %}
$ rvm install 2.3.0
$ rvm use 2.3.0
$ rvm rubygems latest
{% endcodeblock %}

此时，ruby环境安装完成。

**安装Nodejs**

安装`nodejs`是要给Octopress提供javascrpt编译环境，否则就会出现`javascript runtime error`。

    $ sudo apt-get install nodejs

**安装Git**

只有安装了`git`， Octopress才可以使用git把源代码`clone`到本地并`push`到Github。

    $ sudo apt-get install git

我们的博客源代码是被推送到Github Pages的远程仓库的。Octopress是使用Git工具把源代码文件推送到远程仓库，因此本地的octopress需要和远程仓库之间建立一种信任关系，远程Github Pages远程仓库需要确认推送操作实施者有权限执行推送操作。这种信任关系是通过*ssh key*建立的。简单来说一个*ssh key* 对应一个授权，当你在多台电脑部署了octopress环境，但有希望多个octopress部署发布同一个Github Pages时，可以在Github账号中添加多个*ssh key*。

首先，在本地创建*ssh key*，执行命令：

    $ ssh-keygen -t rsa -C "yourmail@example.com"

特别注意这里的email是你在注册Github账号使用的注册emial地址，方便起见，之后一路回车，不必输入任何东西即可创建一个*ssh key*。 在`~/.ssh`目录中可以看到`id_rsa`和`id_ras.pub`两个文件，分别是私钥和公钥，私钥不要泄露，否则任何人得到私钥就能对你的远程仓库做任何操作。
    
需要把公钥`id_rsa.pub`提交到你的Github账号中。登录你的Github账号，打开`Account settings`中的`SSH keys`页面，点击`Add SSH Key` title可任意填写，在Key的文本框中粘贴`id_rsa.pub`文件中的所有内容然后保存。此后你就可以在本地使用Git工具push代码到你的Github中了。

## 安装Octopress ##

**1)**首先把Octopress源代码从Github上克隆到本地

    $ git clone git://github.com/imathis/octopress.git octopress
执行此命令后就可以看到在当前目录下有个octopress文件夹，里面就是octopress的源代码文件。

***2)**安装Octopress依赖项

    $ gem install bundler
    $ bundle install

至此octopress的相关依赖安装完成。 

运行以下命令可以在本地预览博客效果：

    $ rake install
    $ rake preview

`rake`是`ruby+make`的简写。octopress底层是基于ruby编写的jekyll框架的。与octopress操作的命令都会用到rake。
运行命令`rake preview`后，在浏览器中输入`http://localhost:4000` 可以在本地预览博客效果。此时会看到一个最基本的页面。

## 发布Github Pages ##

此时，Octopress博客只是在你本地的浏览器中能够访问到，但我们的目的是把自己的博客发布到外面的互联网上，这样全世界接入互联网的人都能够看到我们自己的博客了。我们选择Github Pages服务来发布我们的博客，使用Octopress可以非常方便的和Github Pages服务进行绑定。

Octopress发布到Github Pages上本质就是把Octopress在本地生成的源代码和静态页面使用Git分布式版本控制系统推送到Github 的Github Pages的仓库`yourname.github.io`中。 Github 仓库目前同时支持ssh和https协议的url。在你的仓库中，可以看到`git@github.com:yourname/yourname.github.io.git`和`https://github.com/yourname/yourname.github.io.git`类似的url。Octopress可以任选其中之一进行绑定。运行下面命令：

    $ rake setup_github_pages

之后会提示你输入前面提到的url，任选其中一种输入即可。

在执行下面命令：

    $ rake generate
    $ rake deploy

也可执行：

    $ rake gen_deploy

这个命令是相当于你执行上面两条命令，是一种简写方式。

现在任何人都可以在浏览器中输入网址`yourname.github.io` 访问你自己刚刚搭建的博客了。

> 再说说两者之间绑定的原理，在执行`rake setup_github_pages`命令之后，octopress文件夹中会多一个_deploy 目录，在你未修改任何与octopress相关主题或者增加博文之前，里面只有一个`index.html`文件，也就是默认主题的首页。里面的内容都是octopress源码编译之后生成的目标文件，_deploy 目录被当做master分支，请注意，在github中创建的`username/username.github.io`仓库中也有一个默认称为master 的分支，octopress仓库中除了_deploy 目录外，其它都被octopress当做source 分支，关于git技术中分支的概念，可自行查询前文git相关文档介绍。octopress的思想就是在source 分支保存整个框架所需要的文件以及写的博文`.markdown`文件，将每次生成的html文件全部放进_deploy目录中，再将_deploy目录所有的文件推送到github pages仓库中，这时便可通过`username.github.io`访问到生成好的html文件，也就是_deploy目录中的文件，至于其它source 分支文件，也可以推送到github中备份，此过程不影响octopress的生成效果，只是防止本地文件丢失无法恢复带来的不良后果，可执行以下代码完成source分支的备份：
      
 {% codeblock %} 
 $ git add .
 $ git commit -m 'commit message'
 $ git push origin source
 {% endcodeblock %}

> 另外，rake generate 做的实质是把octopress框架生成的文件放进octopress/public 文件夹中，rake deploy 做的实质是将octopress/public 中的文件拷贝到octopress/_deploy 文件夹中并将其中的文件推送到username.github.io 仓库中。


## 绑定私有域名 ##

目前你可以使用`yourname.github.io`这样的二级域名访问你自己的博客。当然你也可以绑定自己购买的域名。具体域名购买方式请咨询[谷歌老师](https://www.google.com.hk/search?q=域名购买&oq=域名购买),这里假设你已经购买了一个域名`yourname.org`
,下面就需要把这个域名地址指向你刚刚搭建好的博客`yourname.github.io`. 首先在本地目录octopress/source/下面新建一个名为`CNAME`的文本文件，写入`yourname.org` ，在执行octopress部署命令`rake gen_deploy` 发布到Github Pages中，此时还无法使用你的域名访问。下一步需要把你的域名解析指向你的Github Pages所在服务器的IP地址。执行下面命令获取IP地址：

     $ ping yourname.github.io

之后登陆你购买域名的管理中心，找到域名解析的配置项。具体细节不再赘述。至此，你就拥有了一个可以使用自己购买域名访问的博客了。


## 发布第一篇Blog ##

执行下面命令：

    $ rake new_post["Post Tile"]

会在`octopress/source/_posts`目录下生产一个名称为`yyyy-mm-dd-Post-Tile.markdown`的Markdown文件。这个文件就是你要发布的博文的具体内容。编辑完成这个文档后执行命令：

    $ rake gen_deploy

再次在浏览器中输入你的域名就能访问你刚刚写的博文了。Exciting！

此时你的远程Github Pages仓库是没有你的本地octopress所有源代码的，如果你的本地电脑因为不当操作造成octopress源文件的损坏，很可能就会无法继续发布博文了，保险起见，执行如下命令把octopress所有的源文件推送到远程仓库中,在octopress目录下

 {% codeblock %}
 $ git add .
 $ git commit -m 'commit message'
 $ git push -u origin source
 {% endcodeblock %}

## 换台电脑继续写博客 ##

如果只是到此为止，那么你发布博文都只能通过当前使用的电脑进行。如果想换一台电脑怎么做呢？这也很简单，详情如下。

1 首先要确保每次修改本地内容后都提交到了Github Pages远程仓库的source分支中。 并且切换后的当前的电脑有octopress的依赖环境，详情请看前文。

2 执行下面命令：


 {% codeblock %}
 $ git clone git@github.com:yourname/yourname.github.io.git
 $ cd yourname.github.io
 $ git checkout source
 {% endcodeblock %}
 
  之后就可以像前文操作一样发布博文了。

3 如果想要换回原来电脑，此时一定要记得每次修改本地内容后都要在octopress顶层目录下执行push操作。

    $ git pull origin source

熟悉Git的童鞋肯定知道上面的操作都是很简单的Git的命令操作。
只需要执行这么简单几个Git命令，就可以在不同的电脑之间来回切换了。

## 参考链接 ##
[Octopress在Mac下的安装配置](http://www.chongh.wiki/blog/2015/12/16/octopress-tutorial/)

[Windows下OctoPress环境搭建](http://www.yebangyu.org/blog/2015/10/17/howtoinstalloctopress/)

上面两篇博文的作者分别是我的好友[邦宇](http://www.yebangyu.org) 和 [冲华](http://www.chongh.wiki),本篇博文参考这两篇文章甚多，受益匪浅。


