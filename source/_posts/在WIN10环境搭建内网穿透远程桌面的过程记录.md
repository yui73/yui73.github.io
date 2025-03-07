---
title: 在WIN10环境搭建内网穿透远程桌面的过程记录
date: 2025-01-22 17:26:00
tags: Dev
---

## 0 前言

### 背景

由于近期事情比较多，需要远程桌面使用家里电脑的需求大大增加。

同等配置的云服务器真的相当贵，贵爆了(* ￣︿￣)。

平时一直是使用`ToDesk`进行连接的，然而由于我长时间远程写代码，居然在月中就把免费额度用超了（120H/month），会员24块连续包月。

好吧╮(╯-╰)╭，研究开源社区的动力来源于平时一直 ~~白嫖~~ 使用的软件要收费了。

### 技术选型

*中间经历了熬夜进行技术选型（因为看Chiikawa看到半夜12点才开始干正事）*

想实现远程家里电脑的办法有非常多，问GPT得到的结果就有很多，下做简记。

#### 1. 端口转发

通过在公网服务器上配置端口转发，将访问公网服务器的特定端口请求转发到内网主机上。

**步骤**：

1. 在公网服务器上设置端口转发，将来自外部的流量（比如80端口，或者其他任意端口）转发到内网主机的相应端口（如8080）。

2. 配置防火墙允许端口转发。

3. 需要公网服务器的IP地址和端口号，在其他公网电脑上进行访问。

**优点**：
- 简单易用，适合没有公网IP的内网主机。

**缺点**：
- 内网主机需要进行端口映射，并且有可能会暴露某些服务，存在一定的安全风险。

#### 2. VPN虚拟专用网络

使用VPN技术将公网服务器与内网主机连接在一起，允许其他电脑通过VPN进入到内网，并访问内网主机。

**步骤**：
    
1. 在公网服务器上配置VPN服务器（如使用OpenVPN、Windows自带的VPN功能等）。

2. 让内网主机和其他公网电脑连接到该VPN网络。

3. 通过VPN连接后，公网电脑就能像直接在内网一样访问内网主机。

**优点**：
- 安全性较高，所有流量经过加密。

**缺点**：
- 配置稍微复杂，需要一定的网络知识。

#### 3. 反向代理（Reverse Proxy）

使用公网服务器作为反向代理，将其他公网电脑的请求转发到内网主机。

**步骤**：
1. 在公网服务器上配置反向代理软件（如Nginx、Apache等）。
2. 将外部请求转发到内网主机的特定端口。
3. 在外部访问公网服务器时，代理会将请求转发给内网主机。

**优点**：
- 不需要直接暴露内网主机的IP地址。
- 可以集成安全措施，如SSL证书。

**缺点**：
- 需要一定的配置和维护。

#### 4. SSH隧道

通过SSH隧道在公网服务器和内网主机之间建立安全通道。

**步骤**：
1. 在公网服务器上配置SSH服务。
2. 使用SSH隧道将公网服务器和内网主机连接起来。你可以在外部通过SSH隧道访问内网主机的服务。
3. 通过SSH隧道进行端口转发。

**优点**：
- 安全性较高，数据传输经过加密。

**缺点**：
- 配置稍微复杂，需要对SSH有一定了解。

#### 5. 零信任网络（Zero Trust Network）

使用零信任网络架构（如Cloudflare Tunnel、Tailscale等）来连接内网主机和公网服务。

**步骤**：
1. 在内网主机上安装相关的零信任网络工具（如Tailscale）。
2. 配置公网服务器和其他公网电脑进行连接。

**优点**：
- 高安全性，适用于现代网络环境。
- 免去传统VPN配置复杂性。

**缺点**：
- 需要依赖外部服务或工具，可能涉及到额外费用。


#### 最后选择

一开始看中的是`零信任网络（Zero Trust Network）`，然而发现这些架构都是服务器在国外的，国内环境稳定性肯定会受到影响。

后来选择研究`SSH隧道`，但和端口转发一样，端口暴露在公网上就有被hack的风险，我对自己的网络安全知识还没这么自信。

使用`SSH隧道`进行内网穿透有篇文章可以参考：[ssh隧道-内网穿透-转发RDP端口-安全的实现Ipad远程桌面windows](https://zhuanlan.zhihu.com/p/679213737)

最后决定还是使用`VPN虚拟专用网络`（因为真的不太懂网络安全这方面的知识）。

## 1 搭建虚拟网络

搭建虚拟网络的方法也有很多，比如`WireGuard`、`ZeroTier`(国外服务器)、`openp2p`。

在B站冲浪时，看到一个国产工具，叫做`皎月连`，类似于和上述的`ZeroTier`和`openp2p`原理类似，因此做了简单尝试。

{% note info %}
官网地址：[皎月连官网](https://www.natpierce.cn/)

官网文档：[皎月连官方文档](https://www.natpierce.cn/pc/infolist/index.html)

组网方法按照官方文档配置十分简单。
{% endnote %}

## 2 WIN10家庭版远程桌面补丁

WIN10家庭版只支持远程协助，并不支持远程桌面。

### 尝试一：`HEU_KMS_Activator`

> Github地址：[HEU_KMS_Activator](https://github.com/zbezj/HEU_KMS_Activator)

`HEU_KMS_Activator`是大佬开发的Windows破解软件。

然而我的电脑系统版本貌似是刻录在启动芯片里了，并不支持修改系统版本。

### 尝试二：`RDPWrap`

> Github地址：[RDPWrap](https://github.com/stascorp/rdpwrap)

`RDPWrap`是大佬开发的，给Windows系统加远程桌面补丁的程序。

配置教程参考：[CSDN](https://blog.csdn.net/weixin_44786530/article/details/137919431)

#### 配置步骤

1. 下载Release内文件并解压。*(这步浏览器会进行拦截，坚持下载即可)*
2. 以管理员身份运行`install.bat`
3. 以管理员身份运行`RDPConf.exe`


#### 遇到问题

按照上述步骤安装后，出现`Not Listening`的错误。

![报错](\img\img_in_posts\在WIN10环境搭建内网穿透远程桌面的过程记录\2.png)

> 接下来的解决方案全是Copy下面大佬的，此处仅做记录。大佬文档：[CSDN](https://blog.csdn.net/NXY666/article/details/121152969)

**报错原因：**

由于 RDP Wrapper 多年未更新，自带的配置文件不支持新版本的远程桌面服务。因此我们只需更新配置文件即可。

**解决方法**

- 新建一个`bat`文件并输入以下内容：

```bat
@echo off & title 更新RDPWrap.ini
 
set INI_Path="C:\Program Files\RDP Wrapper\rdpwrap.ini"
set INI_Dir="C:\Program Files\RDP Wrapper"
 
::检查权限
setlocal enabledelayedexpansion>nul
net session>nul
if !ERRORLEVEL! EQU 2 (
	set "args=!args: ="^&chr^(32^)^&"%!"
	
	set "args="/C"&chr(32)&chr(34)&chr(94)&chr(34)&"%~f0""
	mshta "vbscript:CreateObject("Shell.Application").ShellExecute("cmd.exe", !args!, NULL, "runas", NULL)(window.close)"&&exit
)
 
echo.正在停止远程桌面服务……
echo Y | net stop UmRdpService
echo Y | net stop TermService
 
::删除旧配置文件
:DeleteFile
del %INI_Path%
if exist %INI_Path% (
	echo.文件 %INI_Path% 仍被占用，请手动关闭占用该文件的程序。
	start "" %INI_Dir%
	pause
	goto :DeleteFile
)
 
echo.正在下载配置文件……
curl "https://raw.gitmirror.com/sebaxakerhtc/rdpwrap.ini/master/rdpwrap.ini">%INI_Path%
 
echo.正在重启远程桌面服务……
C:\WINDOWS\System32\svchost.exe -k NetworkService
net start TermService
 
echo.更新完成！按任意键以结束。
pause>nul
```

- 保存文件并双击运行，运行完成后按任意键结束。

注：若脚本运行时出现中文乱码，请将文件保存为 ASCII 编码。因涉及服务停止和启动，运行途中可能会提示需要管理员权限，请务必授权。若你不知道如何授权，可以通过使用鼠标右键点击文件，选择“以管理员身份运行”直接以管理员身份启动。

- 此时我们可以看到，Listener state 已变更为 Listening [fully supported] 。
![成功运行](\img\img_in_posts\在WIN10环境搭建内网穿透远程桌面的过程记录\1.png)


> 配置完上述内容...远程桌面已经准备就绪。(\*^▽^\*)
