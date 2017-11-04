## [github pages构建失败问题解决](http://blog.xiayf.cn/2013/01/08/fix-github-pages-builds-failed/)

2013-01-08 TueBy [youngsterxyf](http://blog.xiayf.cn/author/youngsterxyf.html)

今天为本博客提交更新后，github pages自动构建始终不成功。原以为是新提交中引入了错误，于是按照[Git操作：强制删除提交到远程版本库的数据与版本记录](http://blog.xiayf.cn/2013/01/08/git-cancel-commits/)的方法取消了所有的更新，但依旧没用。

由于构建的结果邮件中只有这样一段话：

> The page build failed with the following error: page build failed

关于构建失败的原因一丁点都没有告诉我们，所以根本没法调试嘛。

在阅读github的官方帮助文档[Pages don't build: "Unable to run Jekyll"](https://help.github.com/articles/pages-don-t-build-unable-to-run-jekyll)后，决定按照其中Syntax errors部分的内容做如下尝试：

首先，按照[jekyll的官方安装文档](https://github.com/mojombo/jekyll/wiki/install)安装jekyll：
    
    sudo gem install jekyll
    

然后，在博客的根目录下，执行：
    
    jekyll --safe
    

命令会输出详细的信息，如果构建失败，则在输出的信息中查找错误原因，比如本博客构建失败的原因出在文章的头部添加了如下类似的标记：
    
    category:
        - Algorithms
    

导致之后一行的tags标记数据在构建时出错。我直接将每次执行构建 `jekyll --safe` 提示的错误信息中相关的category标记直接删除(也许其后加个空行就可以了，没尝试过)，然后重新执行构建，依次循环，直到构建成功。然后将数据提交到远程版本库。

由于jekyll本地构建时输出的信息也比较隐晦，很难一下子找到问题所在。比如提示 `Algorithms` 元素构建出错，
    
    Not creating a link for ref_id = "0".Liquid Exception: undefined method `gsub' for ["Algorithms"]:Array in post
    

但没说是哪篇文章的哪一行，没办法只好使用grep查找：
    
    grep Algorithms _posts/*
    

找出所有存在Algorithms单词的文章，然后逐个文章打开查看。
  *[2013-01-08 Tue]: 2013-01-08T00:00:00+08:00
