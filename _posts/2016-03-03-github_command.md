---
title: Github代码提交常用指令
tags: 正则，参数
grammar_cjkRuby: true

---
首次使用 （意思就是这个文件夹中的代码你还没有向GITHUB提交过代码）

`cd /home/test`(假如 test就是你的用户名)/githubtest(这是个文件夹,你可以提前先建立好,这个文件夹也可以是你要提交代码的项目文件夹)

`git init` //这是初始化在这个文件夹中建立一个空库

git add  //这个命令 你可以直接  git add . 这是把当前文件夹中的所有文件都加入到上传的列表中(注意要有空格),你还可以添加具体的文件 git add 你要添加的文件(test/test/test.txt)

`git commit -m` "说明"   //这个 说明 以你自己随意(注意要加 双引号),还要注意 这个条命令最好这样写,网上的有文章说 只用 git commit 这样不是不可以 这样是可以   这样的命令 系统会自动用一个 默认的应用程序打开一个文件让你输入  说明   ,但如果系统没有默认打开的话那就不能继续往下执行了,反正都是要写 说明  ,本来也没几个字,建议大家 直接 把命令写全,省的给自己找麻烦

`git remote add origin https://github.com/test/testt.git`  //这里说两处地方  origin 这个相当于是个别名  你可以自己随便写也可以写成当前文件夹的名 , 后面的地址是你在GITHUB 刚刚新建的 库 地址, 你建了哪几个库,你到GITHUB找到 你 建的库点进去 就能看到相应的地址.

`git push -u origin master`    //开始上传了  ,然后 会提示你 输入 你在 GITHUB上注册的用户名跟密码 输入正确后就等着上传吧

**更新代码**

`cd /home/test`(假如 test就是你的用户名)/githubtest(这个文件夹是你要提交代码的项目文件夹,前提是你已经用过第一种方法了)

 `git add .`     或者添加具体的文件 git add 你要添加的文件(test/test/test.txt)

`git commit -m` "说明"

`git push -u origin master`     //还记的这个别名吗  origin  这个别名就是你用第一种方法首次 向 GITHUB提交代码 你用的 别名