---
layout: post
title: 如何搭建一个独立博客——简明 Github Pages与 jekyll 教程
date: 2014-05-10 13:31:54
categories: blog
tags: [Hexo,github,独立博客]
description: 这是一篇很详尽的独立博客搭建教程，里面介绍了域名注册、DNS设置、github 和 jekyll 设置等过程，这是我写得最长的一篇教程。我想将我搭建独立博客的过程在一篇文章中尽可能详细地写出来，希望能给后来者一个明确的指引，同时用这篇教程开篇，正式开始我的第八大洲之旅。
---


## 重要更新



由于我在2015-07-26换了 mac ,博客平台从 hexo 转移 jekyll.

为什么用 jekyll?因为用 jekyll搭建博客真的好简单.比 hexo 简单多了.

接下来,我将用十步教你搭建博客.


1. 继续用我的教程一直操作:购买域名>注册GitHub>SSH Key配置成功
2. 到"使用GitHub Pages建立博客",你就可以停住了.
3. 到我的[github](https://github.com/cnfeat),找到[我的仓库](https://github.com/cnfeat/cnfeat.github.io).
4. 找到右上角的"fork",点击,将我的博客仓库复制到你的仓库.
5. 回到你的仓库,将"cnfeat.github.io" 改成"你的用户名.github.io"
6. 这样"你的用户名.github.io"就能访问了,不过页面内容是我的,嘿嘿.
7. 在本页面搜"DNS设置",参照我的DNS设置,将godaddy的设置改好.
8. 回到"https://github.com/你的用户名/你的用户名.github.io"找到"CNAME"文件,将"cnfeat.com"改成你的域名,你的域名就拥有我的博客了
9. 博客的文字都放在"https://github.com/你的用户名/你的用户名.github.io"中的"post"里面,你可以将按照里面文章模板去修改,其他的细节自己摸索一下
10. 极简博客教程完成.




### 迭代

- 2015-09-21 01:02:38


------

摘要：这是一篇很详尽的独立博客搭建教程，里面介绍了域名注册、DNS 设置、github 和 jekyll 设置等过程，这是我写得最长的一篇教程。我想将我搭建独立博客的过程在一篇文章中尽可能详细地写出来，希望能给后来者一个明确的指引，同时用这篇教程开篇，正式开始我的[第八大洲之旅](http://www.douban.com/note/523589521/)。

##前言

作为一个技术小白，没有技术基础，看网上的教程也云里雾里，看程序员的教程相当不容易，稍微有些细节描述得不清楚自己就要绕弯路去找答案（善用搜索引擎），所以，在自己的博客搭建完成之后，我决定要将我搭建博客的过程全记录下来，以供后期和我一样的小白参考（是的，我坚信还有很多一样和我一样的人），我会尽可能详细的整理这个教程，其中的资料可能会摘录到其他人的教程，我会在后面列出了参考资料，感谢这些作者们。

为什么要开博客？可以看看我的这篇[《为什么你要写博客？》](http://zhuanlan.zhihu.com/cnfeat/19743861)

也可以看看这篇[《我的博客时代》](http://zhuanlan.zhihu.com/cnfeat/19743255)

以下以我的博客:www.cnfeat.com为例，教大家如何搭建一个独立博客。

##为什么要搭建一个独立博客?

独立的才是自己的。

##小白进入门槛

- 1、非常折腾，需要耐心；
- 2、也需要一定的学习能力和钻研精神；
- 3、懂一些网页基础知识，不懂也重要，参看第二和第三条；

##小白白请看

2014年5月15日更新：发现一个更简单的方法：[用静态页面生成静态博客](http://isnowfy.github.io/about-simple-cn.html) byisnowfy

按此教程操作即可。

##为什么选择GitHub Pages？

很多人用 wordpress，你为什么要用 github pages 来搭建？

- 1、github pages有300M免费空间，资料自己管理，保存可靠；
- 2、学着用 github，享受 github 的便利，上面有很多大牛，眼界会开阔很多；
- 3、顺便看看 github 工作原理，最好的团队协作流程；
- 4、github 是趋势；
- 5、你不觉得一个文科生用 github 很 geek 吗？瞬间跻身技术界；
- 6、就算 github 被墙了，还可以搬到国内的 gitcafe 中去。

![](http://cnfeat.qiniudn.com/bg2012082502.jpg)

##GitHub Pages是什么？

GitHub Pages 本用于介绍托管在 GitHub 的项目， 不过，由于他的空间免费稳定，用来做搭建一个博客再好不过了。

>github Pages可以被认为是用户编写的、托管在github上的静态网页。

![](http://cnfeat.qiniudn.com/1.png)

##购买域名

只推荐上 [godaddy](https://www.godaddy.com/) 购买，安全，而且可以使用支付宝。

教程（截止至2014年5月10日）如下

**1、查你想要的域名**；



![](http://cnfeat.qiniudn.com/2.png)


**2、查到适合的域名之后选择「continue to Cart」**；

![](http://7d9mjz.com1.z0.glb.clouddn.com/goodcnfeat-buy.jpg)

**3、godaddy附加收费服务，不要管，继续「continue to Cart」**；

![](http://cnfeat.qiniudn.com/4.png)

**4、确认购买。**修改购买年限，默认是两年，可以修改成1/2/3/5/10年，随自己喜欢。现在godaddy上com每年的默认费用是12.99美元，附加上ICANN的管理费用就是13.17美元。

![](http://cnfeat.qiniudn.com/5.png)


如果你不是土豪，可以上网搜godaddy的优惠码，一大堆，找一个填进这里，填完之后，一年的费用会变成8.99美元。

![](http://cnfeat.qiniudn.com/61.png)


**说明一下**：一般来讲，使用网上的优惠码第一年收费8.99美元，以后每年的收费是10.99美元，不过在网上可以搜到合适的优惠码，可以每年的收费都是8.99美元，记得多测试自行鉴别。

如图，我买了五年的费用就是45.85美元，随后点击「Proceed to Checkout」

![](http://cnfeat.qiniudn.com/6.png)


**5、结算。**登录或注册界面，填完必要的信息之后，选择用支付宝结算。

![](http://cnfeat.qiniudn.com/7.png)



如果以上的教程如果不够清晰，可以参照这一份[《2013年10月新版godaddy域名注册图文教程》](http://www.admin5.com/article/20131014/527495.shtml)。

**6、检查。**结算后，重新登录，去「My Account」，域名已经显示在你的账户了。

![](http://cnfeat.qiniudn.com/8.png)



**7、补充一些注意事项：**

- 输入优惠码没有优惠或者优惠幅度较低，请清除浏览器cookies再尝试；
- 如果没有支付宝支付选项，有可能是使用的优惠码不支持支付宝，请重新清除浏览器cookies再尝试；
- 注册时用户填写信息时一定要输入正确的邮箱名字，否则修改十分麻烦。
- 买完域名之后一定要记得去自己的邮箱查看激活邮件，否则域名激活不了。

##安装准备软件

依次下载安装。

- 1、[Node.js](http://nodejs.org/)
- 2、[Git](http://git-scm.com/)

##怎么打开Git？

- 1、开始菜单Git Bash。
- 
- ![](http://cnfeat.qiniudn.com/9.jpg)
- 
- 2、鼠标右键打开Git Bash。
- ![](http://cnfeat.qiniudn.com/s10.jpg)



##注册GitHub

访问：http://www.github.com/ 

注册你的username和邮箱，邮箱十分重要，GitHub上很多通知都是通过邮箱的。

注册过程比较简单，详细也可以看：

[一步步在GitHub上创建博客主页 全系列](http://pchou.info/web-build/2013/01/03/build-github-blog-page-01.html) by pchou（推荐）

##配置和使用Github

以下教程主要参考beiyuu的[《使用Github Pages建独立博客》](http://beiyuu.com/github-pages/)写成。

##配置SSH keys

我们如何让本地git项目与远程的github建立联系呢？用SSH keys。

###检查SSH keys的设置

首先我们需要检查你电脑上现有的ssh key：

    $ cd ~/.ssh 检查本机的ssh密钥

如果提示：No such file or directory 说明你是第一次使用git。

###生成新的SSH Key：

    $ ssh-keygen -t rsa -C "邮件地址@youremail.com"
    Generating public/private rsa key pair.
    Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):<回车就好>

注意1: 此处的邮箱地址，你可以输入自己的邮箱地址；注意2: 此处的「-C」的是大写的「C」

然后系统会要你输入密码：

    Enter passphrase (empty for no passphrase):<输入加密串>
    Enter same passphrase again:<再次输入加密串>

在回车中会提示你输入一个密码，这个密码会在你提交项目时使用，如果为空的话提交项目时则不用输入。这个设置是防止别人往你的项目里提交内容。

注意：输入密码的时候没有*字样的，你直接输入就可以了。

最后看到这样的界面，就成功设置ssh key了：

![](http://cnfeat.qiniudn.com/11.png)

###添加SSH Key到GitHub

在本机设置SSH Key之后，需要添加到GitHub上，以完成SSH链接的设置。

- 1、打开本地C:\Documents and Settings\Administrator\.ssh\id_rsa.pub文件。此文件里面内容为刚才生成人密钥。如果看不到这个文件，你需要设置显示隐藏文件。准确的复制这个文件的内容，才能保证设置的成功。

- 2、登陆github系统。点击右上角的 Account Settings--->SSH Public keys ---> add another public keys

- 3、把你本地生成的密钥复制到里面（key文本框中）， 点击 add key 就ok了

![](http://cnfeat.qiniudn.com/s12.jpg)


###测试

可以输入下面的命令，看看设置是否成功，git@github.com的部分不要修改：

    $ ssh -T git@github.com

如果是下面的反馈：

    The authenticity of host 'github.com (207.97.227.239)' can't be established.
    RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
    Are you sure you want to continue connecting (yes/no)?

不要紧张，输入yes就好，然后会看到：

    Hi cnfeat! You've successfully authenticated, but GitHub does not provide shell access.

###设置用户信息

现在你已经可以通过SSH链接到GitHub了，还有一些个人信息需要完善的。

Git会根据用户的名字和邮箱来记录提交。GitHub也是用这些信息来做权限的处理，输入下面的代码进行个人信息的设置，把名称和邮箱替换成你自己的，名字必须是你的真名，而不是GitHub的昵称。

    $ git config --global user.name "cnfeat"//用户名
    $ git config --global user.email  "cnfeat@gmail.com"//填写自己的邮箱

###SSH Key配置成功

本机已成功连接到github。

若有问题，请重新设置。常见错误请参考：

[GitHub Help - Generating SSH Keys](http://help.github.com/articles/generating-ssh-keys)

[GitHub Help - Error Permission denied (publickey)](http://help.github.com/articles/error-permission-denied-publickey)



##将独立域名与GitHub Pages的空间绑定

###GitHub Pages的设置

方法一：在Repository的根目录下面，新建一个名为CNAME的文本文件，里面写入你要绑定的域名，比如cnfeat.com。

方法二：到我的github仓库，点击右下角的「Download ZIP」，下载源文件，解压，找到CNAME文件，用记事本打开，将cnfeat.com修改成你的域名，放进Hexo\source目录下，用hexo命令提交上去。

    $ hexo d -g

###DNS设置

用DNSpod，快，免费，稳定。

注册[DNSpod](www.dnspod.cn)，添加域名，如下图设置。

![](http://cnfeat.qiniudn.com/15.png)



其中A的两条记录指向的ip地址是github Pages的提供的ip

- 192.30.252.153
- 192.30.252.154

如博客不能登录，有可能是github更改了空间服务的ip地址，记得及时到在[GitHub Pages](https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages)查看最新的ip即可

www指定的记录是你在github注册的仓库。

###去Godaddy修改DNS地址

更改godaddy的Nameservers为DNSpod的NameServers。

![](http://cnfeat.qiniudn.com/16.png)

1、点击「My Account」，管理我的域名。

![](http://cnfeat.qiniudn.com/17.jpg)


2、点击域名。

![](http://cnfeat.qiniudn.com/18.jpg)



3、将godaddy的Nameservers更改成f1g1ns1.dnspod.net和f1g1ns2.dnspod.net


![](http://cnfeat.qiniudn.com/19.jpg)

如有不详看可以看[DNSpod提供的官方帮助](https://support.dnspod.cn/Kb/showarticle/tsid/42/)

详细也可以看这里：[一步步在GitHub上创建博客主页(3)](http://pchou.info/web-build/2013/01/05/build-github-blog-page-03.html)

##使用GitHub Pages建立博客

与GitHub建立好链接之后，就可以方便的使用它提供的Pages服务，GitHub Pages分两种，一种是你的GitHub用户名建立的username.github.io这样的用户&组织页（站），另一种是依附项目的pages。

想建立个人博客是用的第一种，形如cnfeat.github.io这样的可访问的站，每个用户名下面只能建立一个。



###github上建立仓库

登录后系统，在github首页，点击页面右下角「New Repository」

![](http://cnfeat.qiniudn.com/13.png)



填写项目信息：

**project name：**cnfeat.github.io  

**description：** Writing 1000 Words a Day Changed My Life

注：Github Pages的Repository名字是特定的，比如我Github账号是cnfeat，那么我Github Pages Repository名字就是cnfeat.github.io。

![](http://cnfeat.qiniudn.com/14.png)



点击「Create Repository」 完成创建。

详细可以看这里：[一步步在GitHub上创建博客主页(2)](http://pchou.info/web-build/2013/01/05/build-github-blog-page-02.html)

##用Hexo克隆主题

###Hexo介绍

Hexo的作者是[tommy351](https://github.com/tommy351/hexo)，根据[官方介绍](http://hexo.io/docs/index.html)，Hexo是一个简单、快速、强大的博客发布工具，支持Markdown格式。

###安装Hexo

打开git。

    $ npm install -g hexo

###部署Hexo

在我的电脑中建立一个名字叫「Hexo」的文件夹，然后在此文件夹中右键打开Git Bash。

    $ hexo init

Hexo随后会自动在目标文件夹建立网站所需要的所有文件。

现在我们已经搭建起本地的hexo博客了，执行以下命令(在H:\hexo)，然后到浏览器输入localhost:4000看看。

    $ hexo g
    $ hexo s



###复制cnfeat的主题

以下进入复制主题环节，如果那一步出现问题，或者修改后没有显示修改的结果，建议来来一个，再看看，可以解决很多问题。

    $ hexo clean
    $ hexo g
    $ hexo s

建立了Hexo文件之后复制wuchong的修改的主题，我的就是复制他的修改的。

    $ git clone https://github.com/wuchong/jacman.git themes/jacman

或者复制yangjian的

    $ git clone https://github.com/A-limon/pacman.git themes/pacman

###启用cnfeat的主题

修改Hexo目录下的config.yml配置文件中的theme属性，将其设置为jacman。

    theme: jacman

注意：Hexo有两个config.yml文件，一个在根目录，一个在theme下，此时修改的是在根目录下的。

###更新主题

    $ cd themes/jacman
    $ git pull

注意：为避免出错，请先备份你的_config.yml 文件后再升级。

###使用与调试
在本地如果需要预览或者调试你的博客，可以启动本地服务器：

    hexo serve

部署到Github前需要配置_config.yml文件。


    deploy:
    type: github
    repository: git@github.com:A-limon/alimon.github.com.git
    branch: master

如果你是为一个项目制作网站，那么需要把branch设置为gh-pages。若要绑定自定义域名也可以参考Hexo或Github Page的帮助文档，制作一个cname文件。

之后执行下列指令即可完成部署，注意部署会覆盖掉你之前在版本库中存放的文件。

    hexo clean
    hexo generate
    hexo deploy

或可加入 —generate 选项，在部署前自动生成文件。

###本地查看调试

    $ hexo g #生成
    $ hexo s #启动本地服务，进行文章预览调试

或者直接作用组合命令

    $ hexo d -g

浏览器输入http://localhost:4000，查看搭建效果。此后的每次变更_config.yml 文件或者上传文件都可以先用此命令调试，非常好用，尤其是当你想调试出自己想要的主题时。

备注：2014年10月9日 笔记本升级，重装系统后，重复以上步骤，Hexo需要重新搭建，_config.yml文件需要重新修改。

##将独立域名与GitHub Pages的空间绑定

###GitHub Pages的设置

方法一：在Repository的根目录下面，新建一个名为CNAME的文本文件，里面写入你要绑定的域名，比如cnfeat.com。

方法二：到我的github仓库，点击右下角的「Download ZIP」，下载源文件，解压，找到CNAME文件，用记事本打开，将cnfeat.com修改成你的域名，放进Hexo\source目录下，用hexo命令提交上去。

    $ hexo d -g

###DNS设置

用DNSpod，快，免费，稳定。

注册[DNSpod](www.dnspod.cn)，添加域名，如下图设置。

![](http://cnfeat.qiniudn.com/15.png)



其中A的两条记录指向的ip地址是github Pages的提供的ip

- 192.30.252.153
- 192.30.252.154

如博客不能登录，有可能是github更改了空间服务的ip地址，记得及时到在[GitHub Pages](https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages)查看最新的ip即可

www指定的记录是你在github注册的仓库。

###去Godaddy修改DNS地址

更改godaddy的Nameservers为DNSpod的NameServers。

![](http://cnfeat.qiniudn.com/16.png)

1、点击「My Account」，管理我的域名。

![](http://cnfeat.qiniudn.com/17.jpg)


2、点击域名。

![](http://cnfeat.qiniudn.com/18.jpg)



3、将godaddy的Nameservers更改成f1g1ns1.dnspod.net和f1g1ns2.dnspod.net


![](http://cnfeat.qiniudn.com/19.jpg)

如有不详看可以看[DNSpod提供的官方帮助](https://support.dnspod.cn/Kb/showarticle/tsid/42/)

详细也可以看这里：[一步步在GitHub上创建博客主页(3)](http://pchou.info/web-build/2013/01/05/build-github-blog-page-03.html)

##搭建完成

至此，独立博客就算搭建完成，如需进步一完善请在参看以下文章或博客下留言。

[Pacman主题介绍](http://yangjian.me/workspace/introducing-pacman-theme/) by yangjian

[使用hexo搭建博客](http://yangjian.me/workspace/building-blog-with-hexo/) by yangjian

[hexo系列教程：（二）搭建hexo博客](http://zipperary.com/2013/05/28/hexo-guide-2/) by zippera（推荐）


[hexo系列教程：（三）hexo博客的配置、使用](http://zipperary.com/2013/05/29/hexo-guide-3/)by zippera（推荐）

##进阶篇：Hexo设置

网站搭建完成后，就可以根据自己爱好来对Hexo生成的网站进行设置了，对整站的设置，只要修改项目目录的_config.yml就可以了，这是我的设置，可供参考。

    # Hexo Configuration
    ## Docs: http://hexo.io/docs/configuration.html
    ## Source: https://github.com/tommy351/hexo/
    
    # Site #整站的基本信息
    title: 1000 words a Day #网站标题
    subtitle: Writing 1000 Words a Day Changes My Life #网站副标题
    description: 学习总结 思考感悟 知识管理 #网站描述
    author:  cnFeat #网站作者，在下方显示
    email: cnFeat@gmail.com #联系邮箱
    language: zh-CN
    
    # URL
    ## If your site is put in a subdirectory
    url: http://www.cnfeat.com #你的域名
    root: /
    permalink: :year/:month/:day/:title/
    tag_dir: tags
    archive_dir: archives
    category_dir: categories
    code_dir: downloads/code
    
    # Directory
    source_dir: source
    public_dir: public
    
    # Writing
    new_post_name: :title.md # File name of new posts
    default_layout: post
    auto_spacing: false # Add spaces between asian characters and western characters
    titlecase: false # Transform title into titlecase
    external_link: true # Open external links in new tab
    max_open_file: 100
    multi_thread: true
    filename_case: 0
    render_drafts: false
    post_asset_folder: false
    highlight:
      enable: true
      line_number: true
      tab_replace:
    
    # Category & Tag
    default_category: uncategorized
    category_map:
    tag_map:
    
    # Archives
    ## 2: Enable pagination
    ## 1: Disable pagination
    ## 0: Fully Disable
    archive: 2
    category: 2
    tag: 2
    
    # Server
    ## Hexo uses Connect as a server
    ## You can customize the logger format as defined in
    ## http://www.senchalabs.org/connect/logger.html
    port: 4000
    server_ip: 0.0.0.0
    logger: false
    logger_format:
    
    # Date / Time format
    ## Hexo uses Moment.js to parse and display date
    ## You can customize the date format as defined in
    ## http://momentjs.com/docs/#/displaying/format/
    date_format: YYYY-MM-DD
    time_format: H:mm:ss
    
    # Pagination
    ## Set per_page to 0 to disable pagination
    per_page: 15 #每页15篇文章
    pagination_dir: page
    
    # Disqus #社会化评论disqus，我使用多说，在主题中配置
    disqus_shortname:
    
    # Extensions
    ## Plugins: https://github.com/tommy351/hexo/wiki/Plugins
    ## Themes: https://github.com/tommy351/hexo/wiki/Themes
    theme: jacman
    exclude_generator:
    Plugins:
    - hexo-generator-feed
    - hexo-generator-sitemap
    
    #sitemap
    sitemap:
      path: sitemap.xml
    
    #Feed Atom
    feed:
      type: atom
      path: atom.xml
      limit: 20
    
    # Markdown
    ## https://github.com/chjj/marked
    markdown:
      gfm: true
      pedantic: false
      sanitize: false
      tables: true
      breaks: true
      smartLists: true
      smartypants: true
    
    # Stylus
    stylus:
      compress: false
    
    # Deployment
    ## Docs: http://hexo.io/docs/deployment.html
    deploy:
      type: github
      repository: https://github.com/cnfeat/cnfeat.github.io.git
      branch: master        

###修改局部页面

页面展现的全部逻辑都在每个主题中控制，源代码在hexo\themes\jacman\中：

    .
    ├── languages  #多语言
    |   ├── default.yml#默认语言
    |   └── zh-CN.yml  #中文语言
    ├── layout #布局，根目录下的*.ejs文件是对主页，分页，存档等的控制
    |   ├── _partial   #局部的布局，此目录下的*.ejs是对头尾等局部的控制
    |   └── _widget#小挂件的布局，页面下方小挂件的控制
    ├── source #源码
    |   ├── css#css源码 
    |   |   ├── _base  #*.styl基础css
    |   |   ├── _partial   #*.styl局部css
    |   |   ├── fonts  #字体
    |   |   ├── images #图片
    |   |   └── style.styl #*.styl引入需要的css源码
    |   ├── fancybox   #fancybox效果源码
    |   └── js #javascript源代码
    ├── _config.yml#主题配置文件
    └── README.md  #用GitHub的都知道


###发表新文章

用hexo发表新文章

    $ hexo n #写文章 

其中my new post为文章标题，执行命令后，会在项目\source\_posts中生成my new post.md文件，用编辑器打开编写即可。

当然，也可以直接在\source\_posts中新建一个md文件，我就是这么做的。

写完后，推送到服务器上，执行

    $ hexo g #生成
    $ hexo d #部署 # 可与hexo g合并为 hexo d -g


###用Hexo发表文章的Markdown语法

使用jacman或pacman主题，建议按此标准语法写：

    title: postName #文章页面上的显示名称，可以任意修改，不会出现在URL中
    date: 2013-12-02 15:30:16 #文章生成时间，一般不改，当然也可以任意修改
    categories: example #分类
    tags: [tag1,tag2,tag3] #文章标签，可空，多标签请用格式，注意:后面有个空格
    description: 附加一段文章摘要，字数最好在140字以内。
    ---

    以下正文


###为什么我的博客有目录？

我用的是Markdown语法，Markdown语法怎么用？

请看这里：[献给写作者的 Markdown 新手指南](http://jianshu.io/p/q81RER)

或者看这里：[Markdown](https://www.google.com.hk/#hl=zh-CN&q=site:jianshu.io+Markdown)

###Hexo命令

常用命令：

    hexo new "postName" #新建文章
    hexo new page "pageName" #新建页面
    hexo generate #生成静态页面至public目录
    hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
    hexo deploy #将.deploy目录部署到GitHub

常用复合命令：

    hexo d -g #生成加部署
    hexo s -g #预览加部署
简写：

    hexo n == hexo new
    hexo g == hexo generate
    hexo s == hexo server
    hexo d == hexo deploy

##安装插件

添加sitemap和feed插件

    $ npm install hexo-generator-sitemap
    $ npm install hexo-generator-feed

修改_config.yml，增加以下内容

    # Extensions
    Plugins:
    - hexo-generator-feed
    - hexo-generator-sitemap
    
    #Feed Atom
    feed:
      type: atom
      path: atom.xml
      limit: 20
     
    #sitemap
    sitemap:
      path: sitemap.xml


##Hexo上传README文件

Github的版本库通常建议同时附上README.md说明文件，但是hexo默认情况下会把所有md文件解析成html文件，所以即使你在线生成了README.md，它也会在你下一次部署时被删去。怎么解决呢？

在执行hexo deploy前把在本地写好的README.md文件复制到.deploy文件夹中，再去执行hexo deploy。


##404页面

GitHub Pages有提供制作404页面的指引：[Custom 404 Pages](https://help.github.com/articles/custom-404-pages)。

直接在根目录下创建自己的404.html或者404.md就可以。但是自定义404页面仅对绑定顶级域名的项目才起作用，GitHub默认分配的二级域名是不起作用的，使用hexo server在本机调试也是不起作用的。

推荐使用[腾讯公益404](http://www.qq.com/404)。

##图床

推荐使用七牛（10G空间，免费）。

##RSS

hexo提供了RSS的生成插件，需要手动安装和设置。步骤如下：

安装RSS插件到本地：

    npm install hexo-generator-feed

开启RSS功能：编辑hexo/_config.yml，添加如下代码：

    plugins:
    - hexo-generator-feed
    - 
然后修改 themes/jacman（如果你的不是这个主题，请自行对应）下的 _config.yml，在 menu section 添加：
    Rss: /atom.xml

最后使用 hexo d -g部署网站即可。

访问hexo/public/atom.xml即可看到xml

##sitemap

同样的，我们使用hexo提供的插件，方法与添加RSS类似。

安装sitemap到本地：

    npm install hexo-generator-sitemap

开启sitemap功能：编辑hexo/_config.yml，添加如下代码：

    plugins:
    - hexo-generator-sitemap
 
访问hexo/public/sitemap.xml即可看到站点地图。

不过，sitemap的初衷是给搜索引擎看的，为了提高搜索引擎对自己站点的收录效果，我们最好手动到google和百度等搜索引擎提交sitemap.xml。


##参考资料：

[1][一步步在GitHub上创建博客主页 全系列](http://pchou.info/web-build/2013/01/03/build-github-blog-page-01.html) by pchou（推荐）

[2]网站优化：[一步步在GitHub上创建博客主页(6)](http://pchou.info/web-build/2013/01/09/build-github-blog-page-06.html) by pchou （推荐）

[3][搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html) by 阮一峰（推荐）

[4][hexo系列教程：（二）搭建hexo博客](http://zipperary.com/2013/05/28/hexo-guide-2/) by zippera（推荐）

[5][hexo系列教程：（三）hexo博客的配置、使用](http://zipperary.com/2013/05/29/hexo-guide-3/)by zippera（推荐）

[6][hexo系列教程：（四）hexo博客的优化技巧](http://zipperary.com/2013/05/30/hexo-guide-4/) by zippera（推荐）

[7][hexo你的博客](http://ibruce.info/2013/11/22/hexo-your-blog/) by ibruce（推荐） 

[8][Pacman主题介绍](http://yangjian.me/workspace/introducing-pacman-theme/) by yangjian（推荐）

[9][使用hexo搭建博客](http://yangjian.me/workspace/building-blog-with-hexo/) by yangjian（推荐）

[10][折腾了个Pacman主题](http://wuchong.me/blog/2014/01/24/change-to-pacman/) by wuchong（推荐）

[11]hexo官方写作教程[「Writing」](http://hexo.io/docs/writing.html)

[12]知乎上的教程：[如何搭建个人独立博客？](http://www.zhihu.com/question/20463581)

[13]在GitHub Pages设置独立域名的官方教程：[[Setting up a custom domain with GitHub Pages]](https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages)

[14][使用Github Pages建独立博客](http://beiyuu.com/github-pages/) by beiyuu

[15][git/github初级运用自如](http://www.cnblogs.com/fnng/archive/2012/01/07/2315685.html) by 虫师

[16][hexo搭建静态博客以及优化](http://code.wileam.com/build-a-hexo-blog-and-optimize/) by Joanna Wu

[17][使用Hexo搭建个人博客](http://c4fun.cn/blog/2014/03/03/use-hexo-blog/) by c4fun

[18][Github Pages与Hexo建个人博客流程](http://kescoode.com/2013/11/17/github-page-and-hexo-guide/) by Kesco

[19][Git push时重复输入用户名密码的问题](http://zipperary.com/2013/05/26/ssh-errors-with-github/) by zippera

[20][hexo文件结构及网站优化](http://www.solarck.com/) by kevin chen


##相关链接

[1][GitHub Pages主页](https://pages.github.com/)

[2][godaddy域名商](http://www.godaddy.com/)

[3][DNSPOD](www.dnspod.cn)

[4][Hexo官方主页](http://hexo.io/)

[5][GotGitHub：GitHub介绍](http://www.worldhello.net/gotgithub/index.html)（推荐）

[5][图标制作网站：faviconer](http://www.faviconer.com/)

[6]本地测试页[localhost:4000](http://localhost:4000/)


##鸣谢

[wuchong](http://wuchong.me/)

[yangjian](http://yangjian.me)

Van Cheng

##[关于我](http://cnfeat.com/about/)

这里有我的个人简介：[关于我](http://cnfeat.com/about/)

如果你想看到我最新的文章，可以关注我的微信公众号「cnfeat」。

![](http://cnfeat.qiniudn.com/1000.png)
