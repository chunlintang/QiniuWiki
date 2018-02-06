#### 一，linux下查看apache占用端口
```bash
netstatus -anpl | grep 80
```
#### 二，linux常用命令

- 系统安全: sudo,  chmod, su, setfacl
- 进程管理: w, top, ps, kill, pkill, pstree, killall
- 用户管理: id, usermod, useradd, groupadd, userdel
- 文件系统: mount, umount, fsck, df, du
- 系统关机重启: shutdown, reboot
- 网络应用: curl, telnet, mail, elinks
- 网络测试: ping, netstat, host
- 网络配置: hostname, ifconfig
- 常用工具: ssh, screen, clear, who, date
- 软件包管理: yum, rpm, apt-get
- 文件查看比较: locate, find
- 文件内容查看:  more, less, tail, head
- 文件处理: cat, touch, unlink, rename, ln
- 目录操作: cd, mv, rm, pwd, tree, cp, ls
- 文件权限: setfacl, chmod, chown, chgrp
- 解压压缩: gzip/gunzip, zip/unzip, tar, bzip2/bunzip2
- 文件传输: ftp, scp

#### 三，系统定时任务

- crontab

```
crontab -e
* * * * * (命令:分 时 日 月 周)
```

- at
```
at 2:00 tommorrow
at>/home/user_00/do_job
at>Ctrl+D

不是循环执行
```

#### 四，vi/vim编辑器

模式：一般模式、编辑模式、命令行模式

- 一般模式：删除、复制/粘贴操作
- 切换编辑模式: i, I, o, O, a, A, r, R
- 切换命令行模式: :、/、?
- 移动光标: ctrl+f, ctrl+b, 0或者功能键home、$或者功能键End、G、gg、N + enter
- 查找/替换: /word、?word、:n1,n2s/word1/word2/g、:1,$s/word1/word2/g、:1,$s/word1/word2/gc
- 删除/复制粘贴: x,X、dd、ndd、yy、nyy、p、P、ctrl+r、.
- 保存退出: w、q、wq
- 视图模式: v、V、ctrl+v、y、d
- 配置：:setnu、:setnonu

#### 五，shell基础

- 脚本执行方式:

赋予权限，直接执行: chmod + x test.sh;./test.sh

调用脚本解释器使得脚本执行: bash、csh、ash、bsh、ksh等

使用source命令: source test.sh

- 编写基础

开头使用#!指定脚本解释器，例如: #!/bin/sh

编写具体功能

```
#!/bin/sh
if ...;
else
fi
```