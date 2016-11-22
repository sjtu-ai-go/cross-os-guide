# cross-os-guide
本教程主要针对跨系统运行Go AI程序的问题

## Gogui在Windows上
> 本段介绍如何在Windows的Gogui上对战来自Windows或Linux的AI

将以下文件放在Gogui所在的Windows电脑上的一个目录中
 - 本项目`socat-win/`下的所有文件
 - （Win下选手的ai-win.exe）
 
同时，Linux选手在Gogui的同一子网的另一台Linux电脑上，安装`socat`软件（`sudo apt-get install socat`)，之后运行：
```bash
socat -ddd -v TCP4-LISTEN:5556,fork,reuseaddr \ 
       SYSTEM:"启动你的程序的命令"
```
然后运行`ip addr`来查看本机IP地址

之后，在Gogui的Program - New Program中，填写：
```
command = 
"C:\Program Files\GoGui\gogui-twogtp.exe" -black ai-win.exe -white "socat.exe STDIO tcp-connect:Linux的IP地址:5556" -verbose

working dir=
存放ai-win.exe的目录
```

之后即可让ai-win作为黑，ai-linux作为白对战。同理也可以两个ai-linux对战，只要把`-black ai-win.exe`换成`-black "socat.exe ..."`即可

原理图：
```
                                            /<---- [ Black commands ] --[ STDIO ] ---> ai-win.exe
Gogui <----[ STDIO ] ----> gogui-twogtp.exe
                                            \<---- [ White commands ] --[ STDIO ] ---> socat.exe STDIO tcp-connect <-- [ SOCKET ] -- > socat TCP4-LISTEN SYSTEM <---[ STDIO ]---> ai-linux
```

