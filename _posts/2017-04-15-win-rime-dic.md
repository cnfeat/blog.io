---
layout: post
title: 中州韵（小狼毫，鼠须管）输入法设置，导入搜狗字库
date: 2017-04-15
categories: blog
tags: [windows]
description: 中州韵（小狼毫，鼠须管）输入法设置，导入搜狗字库
---

中州韵（小狼毫，鼠须管）输入法其实经过一番简单的设置，也是可以用得很顺手的，下面我总结一下设置方法(鼠须管0.9.11)

### 设置目录

Windows下是 `%APPDATA%\Rime`，也可以在任务栏图标里面点击`右键-用户文件夹`

Mac下是 `~/Library/Rime`，也可以在任务栏图标里面点击`右键-Settings`

### 外观设置

新建或修改设置目录里面的`weasel.custom.yaml`（Windows)或`squirrel.custom.yaml `(Mac)，内容是（注意缩进和冒号后面的空格）

	patch:
	  "style/color_scheme": luna 
	  "style/font_point": 18

其中color_scheme的选择有

- 碧水 - aqua
- 青天 - azure
- 明月 - luna
- 墨池 - ink
- 孤寺 - lost_temple
- 暗堂 - dark_temple
- 星際爭霸 - starcraft

然后保存，右击任务栏上鼠须管的图标，选择 `Deploy` 或者 `重新部署` 即可。

### 双拼设置

在设置目录里面新建或编辑 `default.custom.yaml` 文件，内容是（注意缩进和冒号后面的空格）

	patch:
	  	schema_list:
		    -schema: double_pinyin_mspy

当然你也可以选择其他的双拼方案，如`double_pinyin_abc`，`double_pinyin_flypy`，`double_pinyin`分别对应 `智能ABC`、`小鹤`和`自然码`的双拼方案。

保存文件之后，右击任务栏上鼠须管的图标，选择 `Deploy` 或者 `重新部署` 即可。

### 双拼默认简体

原作者是台湾人，所以默认是正体字输出，但是其实这个框架也提供了简体字的输出方式，只需要在设置目录里新建或者编辑`double_pinyin_mspy.custom.yaml`这个文件，如果你用的是其他双拼方案就把文件名作对应修改。文件内容增加（注意缩进和冒号后面的空格）

	patch:
	  switches:                  
	    - name: ascii_mode
	      reset: 0               
	      states: [ 中文, 西文 ] 
	    - name: full_shape       
	      states: [ 半角, 全角 ]  
	    - name: simplification
	      reset: 1                
	      states: [ 漢字, 汉字 ]

保存后 重新部署 鼠须管 即可。

### 双拼模糊拼音

同样是修改 `double_pinyin_mspy.custom.yaml` 这个文件，将以下内容附到后面即可，保持 `speller/algebra` 和上面的 `switch` 对齐：

	"speller/algebra":
	  - erase/^xx$/
	  - derive/^([zcs])h/$1/ 
	  - derive/^([zcs])([^h])/$1h$2/ 
	  - derive/^n/l/ 
	  - derive/^l/n/ 
	  - derive/^([bpmf])eng$/$1ong/ 
	  - derive/([ei])n$/$1ng/ 
	  - derive/([ei])ng$/$1n/ 
	  - derive/^([jqxy])u$/$1v/
	  - derive/^([aoe].*)$/o$1/
	  - xform/^([ae])(.*)$/$1$1$2/
	  - xform/iu$/Q/
	  - xform/[iu]a$/W/
	  - xform/er$|[uv]an$/R/
	  - xform/[uv]e$/T/
	  - xform/v$|uai$/Y/
	  - xform/^sh/U/
	  - xform/^ch/I/
	  - xform/^zh/V/
	  - xform/uo$/O/
	  - xform/[uv]n$/P/
	  - xform/i?ong$/S/
	  - xform/[iu]ang$/D/
	  - xform/(.)en$/$1F/
	  - xform/(.)eng$/$1G/
	  - xform/(.)ang$/$1H/
	  - xform/ian$/M/
	  - xform/(.)an$/$1J/
	  - xform/iao$/C/
	  - xform/(.)ao$/$1K/
	  - xform/(.)ai$/$1L/
	  - xform/(.)ei$/$1Z/
	  - xform/ie$/X/
	  - xform/ui$/V/
	  - derive/T$/V/
	  - xform/(.)ou$/$1B/
	  - xform/in$/N/
	  - xform/ing$/;/
	  - xlit/QWRTYUIOPSDFGHMJCKLZXVBN/qwrtyuiopsdfghmjcklzxvbn/

保存后，重新部署即可。

### 词库导入

下载某位仁兄导出的搜狗词库 [http://cl.ly/033g2x3k2J05](http://cl.ly/033g2x3k2J05) 后解压得到 `sogou.txt`文件，(实际发现是dic格式的，不过照样可以导入！)

Windows下右键选择`用户词典管理`，选择`『导入文本码表』`功能，将 `sogou.txt` 导入你的用户词典。

Mac下下载 `用户词典管理工具` 并把 `rime_dict_manager` 解压到 `~/Library/Rime`，执行如下命令

	cd ~/Library/Rime
	killall Squirrel
	./rime_dict_manager --import luna_pinyin 路径/sogou.txt