# 鸟哥私房菜
## linux语句

```
command  [-options]  parameter1  parameter2 ...
```
中刮號[]表示选项，如-h；有時候會使用選項的完整全名，則選項前帶有 -- 符號，例如 --help；

指令, 選項, 參數等這幾個咚咚中間以空格來區分，不論空幾格 shell 都視為一格；

[Enter]按鍵代表著一行指令的開始啟動。指令太長的時候，可以使用反斜線 \ 來跳脫[Enter]符號，使指令連續到下一行。++注意！反斜線後就立刻接特殊字符，才能跳脫？++

## nano 文本编辑
nano的使用其實很簡單，你可以直接加上檔名就能夠開啟一個舊檔或新檔！

```
^G Get Help          M-D DOS Format       M-A Append           M-B Backup File
^C Cancel            M-M Mac Format       M-P Prepend
```
==^== 代表Ctrl

==M== 代表Alt

### 删除目录 ==慎用==

```
rmdir
```
删除空目录

```
rm -rf 目录名
```
删除目录和里面全部内容

```
rm
```
删除文件


```
mv /home/user/oldname /home/user/newname
```
重命名目录

### 显示绝对路径

```
pwd -P
```
### 检查软件的安装环境
1. find out my linux distribution name and version
> $ lsb_release -a
or
> $ cat /etc/*-release

2. find out if package is installed in linux
for red hat:
> $ rpm -qa | grep {package-name}
or 
> $ rpm -qa | less

3. quit running programe
Ctrl+Shift+C

4. show G++ version (搜索时用小写g！！！)
> $ g++ --help
> $ g++ --version

### 显示时间日期

```
date
date +%H:%M
date +%Y/%m/%d
```

```
cal # 显示日历
cal [month] [year]
cal 08 2016
```
### 计算器

```
bc
scale=3   # 设定小数点后的位数，如不设定默认为整数
3/10
.300 # 3/10的显示结果

quit # 退出
```
### 快捷键
1. 双击Tab键，列出全部命令或文件选项
2. Ctrl + C，终止当前运行程序
3. Ctrl + D，相当于退出系统
4. Shift + pagedown、pageup，linux界面翻页
5. 
## 寻求帮助
### -h 或 --help
### man（ual）page

```
man {commond}
date --help
man date
```
#### man page中括号数字的含义

代號 | 代表內容
---|---
1 | ==使用者在shell環境中可以操作的指令或可執行檔==
2 | 系統核心可呼叫的函數與工具等
3 | 一些常用的函數(function)與函式庫(library)，大部分為C的函式庫(libc)
4 | 裝置檔案的說明，通常在/dev下的檔案
5 | ==設定檔或者是某些檔案的格式==
6 | 遊戲(games)
7 | 慣例與協定等，例如Linux檔案系統、網路協定、ASCII code等等的說明
8 | ==系統管理員可用的管理指令==
9 | 跟kernel有關的文件

==用关键词搜索相关指令==

```
man -f man # 只搜索指令名称
man -k man # 同时搜索指令名称和指令描述信息
man 1 man # 显示man（1）指令man page
man 7 man # 显示man（7）指令man page
```
### info命令

總結上面的三個咚咚(==man, info, /usr/share/doc/==)，請記住喔：

1. 在終端機模式中，如果你知道某個指令，但卻忘記了相關選項與參數，請先善用 --help 的功能來查詢相關資訊；

2. 當有任何你不知道的指令或檔案格式這種玩意兒，但是你想要瞭解他，請趕快使用man或者是info來查詢！

3. 而如果你想要架設一些其他的服務，或想要利用一整組軟體來達成某項功能時，請趕快到/usr/share/doc底下查一查有沒有該服務的說明檔喔！


### 查看文件权限
```
ll
```
或

```
ls -l # 每行显示一个文件
```

```
ls {folder.name} # show files in the folder
```

```
ls -al # 显示隐藏文件并每行输出显示
```

```
ls -l --full-time # 显示文档完整时间
```
![文档权限解释](http://linux.vbird.org/linux_basic/0210filepermission//0210filepermission_3.gif)

第一個字元代表這個檔案是『目錄、檔案或連結檔等等』：

1. 當為[ d ]則是目錄
2. 當為[ - ]則是檔案
3. 若是[ l ]則表示為連結檔(link file)，即快捷方式
4. 若是[ b ]則表示為裝置檔裡面的可供儲存的周邊設備(可隨機存取裝置)
5. 若是[ c ]則表示為裝置檔裡面的序列埠設備，例如鍵盤、滑鼠(一次性讀取裝置)。



### 查看全部历史作业
```
$ history
```
### Convert dos text to unix

```
$ dos2unix dCh_assamble.sh
dos2unix: converting file dCh_assamble.sh to UNIX format
```
### [How to verify your files in Linux with MD5](http://www.techradar.com/news/computing/pc/how-to-verify-your-files-in-linux-with-md5-641436)
![image](http://cdn.mos.techradar.com/Review%20images/Linux%20Format/LXF%20123/LXF123.md5.md5sum_samples-970-80.jpg)
The md5 files are checksums. If you're worried that the file didn't download properly, you can run the md5 program on your own computer (it's on most unixes) on the file, then check to make sure that it's the same as the number on the web. I have never needed to do this. It may be ok to skip the md5sum checks since that could take some time on large files.

### unzip

```
gunzip file.gz
unzip file.zip
tar zxvf FileName.tar.gz

# 压缩打包
tar zcvf filename.tar.gz foldername

```
### 向天河拷贝数据

```
ssh ln1 #进入到连接移动硬盘的节点
kls #？？？
mkdir ../data_from_disk #建立要拷贝数据的文件夹
cp -r PublicDatabase/ ../data_from_disk/ #拷贝数据
```
### 创建并编写一个文件
直接在服务器上创建一个sh脚本文件，[How to Quickly Create a Text File Using the Command Line in Linux](http://www.howtogeek.com/199687/how-to-quickly-create-a-text-file-using-the-command-line-in-linux/)
```
cat > dCh_dmnd.sh
```
粘贴如下内容

```
#!/bin/bash
yhrun -n 1 -N 1 -p free ~/diamond blastx -d /WORK/scsfri_jzjiang_1/PubData/Uniref20160801/uniref50 -q /WORK/scsfri_jzjiang_1/Analysis/JingZhe/dCh_asmb/final.contigs.fa -o /WORK/scsfri_jzjiang_1/Analysis/JingZhe/dCh_dmnd/matches.tab --more-sensitive --block-size 10 --index-chunks 1 --outfmt tab
```
然后按Ctrl+D退出

### 修改该文件权限，使其变成可执行文件
- [x] chgrp ：改變檔案所屬群組
- [x] chown ：改變檔案擁有者
- [x] chmod ：改變檔案的權限
```
chmod	u/g/o/a	+/-/= r/w/x	filename # 标准格式

chmod +x dCh_dmnd.sh # 简化方式
chmod a+x dCh_dmnd.sh # 标准格式
chmod a-w dCh_dmnd.sh # 去掉更改权
chmod u=rwx,go=rx filename # user为rwx权，group和others为rx权

```
也可以进行如下设置：
```
chmod 770 filename # 数字代表三种权限的数值
```
- [x] owner = rwx = 4+2+1 = 7
- [x] group = rwx = 4+2+1 = 7
- [x] others= --- = 0+0+0 = 0

关于目录的权限问题：

```
有個目錄的權限如下所示：
drwxr--r--  3  root  root  4096   Jun 25 08:35   .ssh
系統有個帳號名稱為vbird，這個帳號並沒有支援root群組，請問vbird對這個目錄有何權限？是否可切換到此目錄中？
答：
vbird對此目錄僅具有r的權限，因此vbird可以查詢此目錄下的檔名列表。因為vbird不具有x的權限，亦即 vbird 沒有這個抽屜的鑰匙啦！ 因此vbird並不能切換到此目錄內！(相當重要的概念！)
```
上面這個例題中因為vbird具有r的權限，因為是r乍看之下好像就具有可以進入此目錄的權限，其實那是錯的。==能不能進入某一個目錄，只與該目錄的x權限有關啦！==

==工作目錄對於指令的執行是非常重要的，如果你在某目錄下不具有x的權限， 那麼你就無法切換到該目錄下，也就無法執行該目錄下的任何指令，即使你具有該目錄的r或w的權限。==

要開放目錄給任何人瀏覽時，應該至少也要給予r及x的權限，但w權限不可隨便給！

假設有個帳號名稱為dmtsai，他的家目錄在/home/dmtsai/，dmtsai對此目錄具有[rwx]的權限。若在此目錄下有個名為the_root.data的檔案，該檔案的權限如下：

```
-rwx------ 1 root  root  4365 Sep 19 23:20  the_root.data
```

請問dmtsai對此檔案的權限為何？可否刪除此檔案？

答：
如上所示，由於dmtsai對此檔案來說是『others』的身份，因此這個檔案他無法讀、無法編輯也無法執行， 也就是說，他無法變動這個檔案的內容就是了。

但是由於這個檔案在他的家目錄下， 他在此目錄下具有rwx的完整權限，因此對於the_root.data這個『檔名』來說，他是能夠『刪除』的！ 結論就是，dmtsai這個用戶能夠刪除the_root.data這個檔案！

- [x] 例如你在網路上下載一個可執行檔，但是偏偏在你的 Linux系統中就是無法執行！呵呵！那麼就是可能檔案的屬性被改變了！不要懷疑，==從網路上傳送到你的 Linux系統中，檔案的屬性與權限確實是會被改變的喔！==

### 关于线程和进程的[形象解释](http://www.ruanyifeng.com/blog/2013/04/processes_and_threads.html)


# 关于cpu和内存的分配问题
但程序运行时间较长。为了提高运行效率同时降低运行成本，即将cpu和内存用最合理的配置分配给任务，以关键词*how to assign cpu and memory to a task*检索，了解配置方法。

> [There are at least three ways in which you can control how much CPU time a process gets: ](http://blog.scoutapp.com/articles/2014/11/04/restricting-process-cpu-usage-using-nice-cpulimit-and-cgroups)

> Use the ++nice++ command to manually lower the task's priority.

> Use the ++cpulimit++ command to repeatedly pause the process so that it doesn’t exceed a certain limit.
Use Linux’s built-in ++control groups++, a mechanism which tells the scheduler to limit the amount of resources available to the process.

再用关键词*limit cpu and memory usage for a process*检索

# [Shell脚本编程30分钟入门](https://github.com/qinjx/30min_guides/blob/master/shell.md)

## 运行sh脚本

```
ls -l /bin/*sh # 查看系统内的shell相关文件
chmod +x test.sh
./test.sh
```
注意，一定要写成./test.sh，而不是test.sh，运行其它二进制的程序也一样，直接写test.sh，linux系统会去PATH里寻找有没有叫test.sh的，而只有/bin, /sbin, /usr/bin，/usr/sbin等在PATH里，你的当前目录通常不在PATH里，所以写成test.sh是会找不到命令的，要用./test.sh告诉系统说，就在当前目录找。
### 直接运行解释器，其参数就是shell脚本的文件名

```
/bin/sh test.sh
/bin/php test.php
```
这种方式运行的脚本，不需要在第一行指定解释器信息，写了也没用。

### 定义命令别名

```
alias lm='ls -al' # 输入lm等同于ls -al
```
### 整行操作指令串

```

組合鍵	功能與示範
[ctrl]+u/[ctrl]+k	# 分別是從游標處向前刪除指令串 ([ctrl]+u) 及向後刪除指令串 ([ctrl]+k)。
[ctrl]+a/[ctrl]+e	# 分別是讓游標移動到整個指令串的最前面 ([ctrl]+a) 或最後面 ([ctrl]+e)。
```
