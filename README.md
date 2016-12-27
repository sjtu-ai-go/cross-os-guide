# cross-os-guide
本教程主要针对跨系统运行Go AI程序的问题 （基于洛神的修改版）
## Gogui在Windows上
> 本段介绍如何在Windows的Gogui上对战来自Windows或Linux的AI

将以下文件放在Gogui所在的Windows电脑上的一个目录中
 - 本项目`socat-win/`下的所有文件
 - （Win下选手的ai-win.exe）
 
同时，Linux选手在另一台Linux电脑上，安装`socat`软件（`sudo apt-get install socat`)，之后运行：
```bash
socat -ddd -v TCP4-LISTEN:5556,fork,reuseaddr \ 
       SYSTEM:"启动你的程序的命令"
```
然后运行`ip addr`来查看本机IP地址


## 在 windows 平台上建立一个文本文档另存为 xx.bat
内容：
```
@echo off
set BLACK=D:\bill\socat-win\socat.exe STDIO tcp-connect:192.168.1.146:5566
set WHITE=D:\bill\windowsplayer.exe
set TWOGTP=""D:\bill\OneDrive\Documents\Computer Go\GoAI\GoGui\gogui-twogtp"" -black ""%BLACK%"" -white ""%WHITE%"" -size 19
"D:\bill\OneDrive\Documents\Computer Go\GoAI\GoGui\gogui" -program "%TWOGTP%" -size 19 -computer-both -verbose
```
###解释：
set BLACK 一行设置 执黑者程序地址，如果通过 socat 与 linux 通信则需修改 socat.exe的路径和ip 地址及端口； 
set WHITE 一行设置 执白者程序地址，若是 windows 本地程序，则只需指定路径即可。最后一行需要指定 gogui 的路径，其他参数不用修改。

然后双击 bat 即可进行对战。

####socat原理图：
```
                                            /<---- [ Black commands ] --[ STDIO ] ---> ai-win.exe
Gogui <----[ STDIO ] ----> gogui-twogtp.exe
                                            \<---- [ White commands ] --[ STDIO ] ---> socat.exe STDIO tcp-connect <-- [ SOCKET ] -- > socat TCP4-LISTEN SYSTEM <---[ STDIO ]---> ai-linux
```

