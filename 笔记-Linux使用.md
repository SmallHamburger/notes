---
title: Linux基本使用
tags: [Linux,Linux基本使用]
date: 2016/9/1
categories: 技术类
---
# 基本命令

## ls 查询目录中的内容
		ls [选项] [文件或者目录]
			-a 所有文件，包括隐藏文件
			-l 详细信息
			-h 人性化显示，byte --> KB
			-d 查看目录属性
			-i 显示inode值
		ll=ls -l

## mkdir 创建目录
		mkdir [选项] 目录
			-p 创建多级目录，递归创建

## cd 切换目录
		cd [目录]
			~/空值 进入当前用户目录
			- 进入上次进入的目录
			.. 进入上级目录
			. 进入当前目录
			tab键 自动补全（点击一下）或者提示所有类似文件（点击两下）

## rmdir 删除一个空白目录，目录不能有任何内容
		rmdir 空白目录

## rm 删除目录或者文件
		rm [选项] 目录或者文件名
			-r 删除目录
			-f 强制删除，不会提醒删除
		rm -rf / 谨慎使用，删除根目录所有文件

## cp 复制
		cp [选项] 源文件或者源目录 目标文件或者目标目录
			-r 复制目录
			-p 连带文件属性一同复制
			-d 若源文件位链接文件，则复制链接属性
			-a 复制所有属性，相当于-pdr

## mv 剪切或者改名，同一目录下剪切就是改名字，不同目录下就是剪切
		mv 源文件或者目录 目标文件或者目录

## ln 生成链接文件
		ln [选项] 源文件 目标文件
			-s 创建软链接

## / 一级目录作用
	/bin 通用命令
	/sbin 超级用户命令
	/boot 启动
	/dev 硬件文件
	/etc 配置文件
	/home 普通用户家目录
	/root 超级用户家目录
	/lib 函数库
	/mnt 硬盘，光盘等存储设备挂载点
	/media 光盘挂载点
	/misc 磁带之类挂载点
	/proc 内存挂在点
	/sys 内存挂载点
	/tmp 临时文件目录
	/usr 系统软件资源目录
	/usr/bin 用户系统命令（通用）
	/usr/sbin 用户系统命令（超级）
	/var 系统的可变文档目录

## locate 在后台数据库中按文件名搜索，搜索速度快，只能按照文件名搜索
	locate 文件名 
		/etc/updatedb.conf 配置文件
	updatedb 更新数据库（默认一天一搜索） 
		/var/lib/mlocate/mlocate.db

## whereis 搜索命令所在的路径及帮助文档所在位置，找到的命令不是shell自带的
	whereis 命令名
		-b 只查找可执行文件
		-m 只查找帮助文件

	which 同whereis 但会增显别名（若存在），不显示帮助文档，无参数，不常用

	echo $PATH 环境变量

## find 搜索文件
	find [搜索范围] [搜索条件，可以有多个] 搜索文件名
		文件名作为过滤条件
			-name 以文件名进行搜索
			-iname 以文件名（不区分大小写）进行搜索
		用户所有者作为过滤条件
			-user 按所有者搜索
				例：find /root -user root
			-nouser 查找没有所有者的文件
		时间作为过滤条件
			-mtime 内容改变的时间
			-ctime 属性改变的时间
			-atime 最近访问的时间
				例：find ./ -mtime +10(-10或者10) 查找10天前（10天内或者第10天前）的文件 
		大小作为过滤条件
			-size 文件占用空间大小，大小的单位默认位数据块，扇区，可选的大小为：b（小写，B）k（小写，KB），M（大写，MB），G（大写，GB）
				例：find ./ -size 25k
		节点信息作为过滤条件
			-inum inode的节点信息
				例：find ./ inum 26421
	通配符
		* 匹配任意字符
		? 匹配任意一个字符
		[abc] [a-z] [0-9]匹配[]内的任意一个字符
		[^a-z] 匹配的内容不包含[]的任意一个字符
	条件逻辑
		-a 代表and，并且
		-o 代表or，或者
			find ./ -size +20k -a -mtime -10
	结果处理
		-exec 需要执行的命令 {} \;
			find ./ -size +20k -a -mtime -10 -exec ls -l {} \;

## grep 搜索字符串
	grep [选项] 字符串 文件名
		-i 忽略大小写
		-v 排除指定字符串

## man 帮助命令
	man [等级] 命令
	man [选项] 命令
		-f 相当于 whatis 命令 = man -f 命令
		-k 所有含有关键字的命令或者文件
## help 查询
	help 命令
## info 详细的帮助命令
	info 命令

## zip 压缩为zip格式，与windows通用
	zip [选项] 文件名 源文件
		-r 压缩目录
## unzip 解压缩zip格式
	unzip 文件名

## gzip 压缩为gzip格式或解压缩gzip格式文件
	gzip [选项] 源文件 [目标目录]
		-c 保留源文件，需要加重定向符号
			例：gzip -c test > t1
		-r 压缩目录下所有子文件，但不包括目录
		-d 解压缩gzip文件，等同于gunzip  ？？？好像有问题

## bzip 压缩为bzip2格式，不能压缩目录
	bzip [选项] 源文件
		-k 源文件，保留源文件
		-d 解压缩，同bunzip2

## tar 打包命令
	tar [选项] 打包文件名 源文件
		-c 打包，与-x/-t
		-v 显示过程
		-f 指定打包的文件名
			例：tar -cvf long.tar long
		-x 解压缩包，与-c/-t对应
		-z 打包时并压缩为gz格式
		-j 打包时并压缩为bz2格式
		-C 大写C，指定解压缩目录
		-t 查看压缩文件内容，与-x/-c对应

## shutdown 关机重启命令
	shutdown [选项] 时间
		-c 取消前一个关机命令
		-h 关机
		-r 重启
## 关机
	halt
	poweroff
	init 0 关机，不建议使用
## 重启
	reboot/init 0

## init 系统运行级别
	init [level]
		0 关机
		1 单用户，类似于安全模式
		2 不完全多用户，不含NFS服务
		3 完全多用户，默认
		4 未分配
		5 图形界面
		6 重启

## logout 退出登录

## df 查询系统中已经挂载的设备
## mount 查询系统中已经挂载的设备
	mount [-t 文件系统] [-o 特殊选项] 设备名 挂载点 
		-a 依据配置文件/etc/fstab的内容自动挂载
		-t 文件系统，（光盘，iso9660）
		-o 特殊选项 
			exec/noexec：可执行，默认为exec
			rw/ro：读写权限，默认为rw
		例：mount -t iso9660 -o ro /etc/sr0(或/etc/cdrom) /mnt/cdrom
				mount /dev/sr0 /mnt/cdrom
		例：mount -t vfat /dev/sdb1 /mnt/usb
					挂载fat32位格式硬盘

## umount 卸载，卸载时挂载点不在使用状态，也不可在当前目录
	umount 设备文件名或挂载点
	例：umount /mnt/cdrom

## fdisk 查看硬盘设备
	fdisk [选项]
		-l 列出硬盘

## w 查看用户登录信息
	w [用户名]
## who  查看用户登录信息
	who [用户名]
## last 查询当前目录和过去登录的用户信息
	默认是读取/var/log/wtmp文件的数据
## lastlog 查看用户最后一次登录时间

## shell 命令行解释器，配置在/etc/shells，可以通过exit退出
	sh或csh 进入相应shell命令

## echo 输出命令，输出的内容如果含有空格，则需要用双引号包裹
	echo [选项] [输出内容]
		-e 支持反斜线空值的字符转换
			控制字符		作用
				\a				输出警告音
				\b				退格键，也就是一个向左删除键
				\n				换行符
				\r				回车符
				\v				垂直制表符
				\onnn			八进制输出字符
				\xhh			十六进制输出字符
				\t				制表符tab
	输出颜色
		#30m = 黑，31m = 红，32m = 绿，33m = 黄，34m = 蓝，35m = 洋红，36m = 青，37m = 白
	例：echo -e "\e[1;31mhelloworld\e[0m"
		\e 调用颜色
		[1; 开启颜色，\e[1;31m
		[0m 关闭颜色, \e[0m

## 脚本执行
	#代表注释
	#！/bin/bash 代表以下内容为shell脚本，如果不是纯shell语言则必须加
	执行方法：
		1. 赋予执行权限，直接运行
			chmod 755 hello.sh
			./hello.sh
		2. 通过Bash调用执行脚本
			bash shell.sh

## alias 给命令添加别名（关机后消失）
	alias 别名='命令'
		例：alias ls='ls --color=never'
	在~/.bashrc 添加别名，可永久生效，可通过source .bashrc
	命令生效顺序:
		1. 用绝对路径或相对路径执行命令
		2. 别名
		3. Bash的内部命令
		4. 按照$PATH环境变量找到的第一个命令
## unalias 删除别名（临时删除）

## 快捷键
	ctrl + c 终止当前命令
	ctrl + l 清屏
	ctrl + a 光标移至行首
	ctrl + e 光标移至行尾
	ctrl + u 从光标位置删除至行首
	ctrl + z 将命令放置后台
	ctrl + r 在历史命令中搜索

## history 历史命令
	history [选项] [历史命令保存文件]
		-c 清空历史命令
		-w 缓存中的历史命令保存文件 ~/.bash_history

## wc 统计信息
	wc [选项] [文件名] 
		-c 统计字符数
		-w 统计单词数
		-l 统计行数
	例：
		wc -wcl test.txt
		输出内容：
			行数		单词数	字节数	路径

## 标准输入输出
	键盘		/dev/stdin	0
	显示器	/dev/stdout	1
	显示器	/dev/stderr	2

## 输出重定向
	> 覆盖
	>> 追加
	> 只存储命令正确执行时内容
		例：
			ls > test.txt			ls列表内容存入到test.txt文件中，只存命令正确执行时的内容
	2> 只存储命令错误执行内容
		例：
			lss 2> test.txt		ls列表内容存入到test.txt文件中，只存命令错误执行时的内容
	&> 或 2>&1 命令正确或者错误执行后输出内容都保存
		例：
			ls > test.txt 2>&1
			ls &> test.txt
	> 文件一 2> 文件二 命令正确和错误执行后内容分开执行
		例：
			ls > test.txt 2> test_error.txt
## /dev/null 黑洞
	例：
		ls > test.txt 2> /dev/null

## 输入重定向, 不懂
	<
	<<

## 多命令顺序执行


| 多命令执行符 |    格式    |   作用   |
|:------------:|:----------:|----------|
|      ;       |  cmd1;cmd2 |多个命令顺序执行，命令之间无任何逻辑关系，无论哪个cmd出错都会顺序执行下一个|
|      &&      | cmd1&&cmd2 |逻辑与，cmd1正确执行后才会执行cmd2|
|     丨       |cmd1丨丨cmd2|逻辑或，cmd1正确执行后不会执行cmd2，cmd1错误执行后cmd2才会执行|
	例：
		ls && echo success || echo fail
			正确 --> success X-->
			错误 X--> 				--> fail
		ls || echo fail && echo success
		  逻辑混乱，不建议

## | 管道符，管道符前面的命令输出作为后面命令的操作对象
	cmd1 | cmd2
		cmd1的处理结果送于cmd2作为处理对象

## Bash中其他特殊符号


|     符号     |   名称    |                作用                |
|:------------:|:---------:|------------------------------------|
|     ‘’     |   单引号  |在单引号中所有的特殊字符都无特殊含义|
|     “”     |   双引号  |取消特殊含义，$和\                  |
|     $()      |           |变量的值，引用命令，转义符等特殊含义|
|     ··     |   反引号  |同$()， 建议使用$()                 |
|      #       |    井号   |在shell脚本中代表注释               |
|      $       |           |用于调用变量的值，例：$name         |
|      \       |   转义符  |转义其后的字符，相当于用单引号括起来|
