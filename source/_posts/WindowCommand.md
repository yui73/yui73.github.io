---
title: Windows命令集合
date: 2022-02-10 15:31:00
tags: Windows
---
# Windows命令集合
> 对一些不熟悉的Windows命令做一个汇总
- 清屏：`clear`
- 删除文件夹：`rd/s/q 文件路径` `/s参数为子目录` `/q参数为无需询问`
- 删除子文件夹：`del/s/q 文件路径` `/s参数为子目录` `/q参数为无需询问`
- 查看PowerShell的执行策略权限列表：`Get-ExecutionPolicy -List`
- 更改PowerShell的执行策略权限（例如改成remotesigned）：`set-executionpolicy remotesigned`
- 升级为管理员权限：`Start-Process powershell -Verb runAs`（不过是会弹出一个由管理员权限的新窗口）

## 1 将win11右键菜单改为win10样式

- win11 -> win10
在终端（管理员）

​	`reg.exe add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve`

- win10 -> win11
恢复Win11新右键菜单的方法

　还是一样打开 windows 终端(管理员)点击进入之后，直接输入这串代码 ：　

​	`reg.exe delete "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /va /f`

　　然后显示操作成功，重启之后，就可以恢复了。