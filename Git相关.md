一些学习网址：

1. http://rogerdudler.github.io/git-guide/index.zh.html
2. http://backlogtool.com/git-guide/cn/contents/
3. http://www.runoob.com/git/git-tutorial.html
4. http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000



# Connect VScode and Github

## 复制Github现有的指令集到本地

- 注册并登陆Github

> 新建一个新的**public** Repository，并勾选Initialize this repository with a README。

- 安装最新版Git软件

> **国内的下载网址**：https://github.com/waylau/git-for-win

- 打开VScode

> 快捷键Ctrl+Shift+c打开windows的命令行窗口（cmd，commond窗口）

- 设置Git：初始化、username和email

> 在想要作为Git目录的位置输入如下口令进行初始化

```bash
git init
git config --global user.name [YOUR NAME]
git config --global user.email [YOUR EMAIL]
```

- 克隆远程github repository到本地电脑

复制刚刚在Github中新建的repository网址：在命令行窗口中输入

```bash
git clone [你的网址]
```

复制完成后可在VScode中见到新的文件夹

- 从我的电脑中找到这个文件夹，==用VScode重新打开==
- 在VScode中修改Readme文件，并保存（快捷键：Ctrl+S）。随后，会在左侧Git图标处显示蓝色1的标注，表示有一处修改
- 点击进入Git界面，在消息框中输入修改的内容简介，使用Ctrl+Enter提交
- 点击左上角附近...图标，push到github服务器上即可。

## 将本地新建的指令集推送到github

> 感觉这样做容易出错，具体原因还没搞明白

1. 用VScode打开想要推送的文件夹
2. Ctrl+Shift+c打开命令窗口
3. 告知github推送网址

```bash
git remote add origin https://github.com/[USER.NAME]
或
git remote add origin https://github.com/[USER.NAME]/[REPOSITORY]
```

用该命令查看是否建立连接

```bash
git remote -v
```

4. 用VScode从新打开该文件夹，在git视窗下，点击左上角附近...图标，推送对应的github网址上即可。

## 更改git origin url

```bash
git remote -v
# View existing remotes
# origin  https://github.com/user/repo.git (fetch)
# origin  https://github.com/user/repo.git (push)

git remote set-url origin https://github.com/user/repo2.git
# Change the 'origin' remote's URL

git remote -v
# Verify new remote URL
# origin  https://github.com/user/repo2.git (fetch)
# origin  https://github.com/user/repo2.git (push)
```

