---
title: Virtual Box虚拟机设置
date: 2024-07-02 11:00:00
tags: Dev
---

# Virtual Box 虚拟机配置

> 前情提要：无法使用增强模块且只能使用免费版的情况下，对虚拟机进行分辨率的修改。

> 百度了好一会都是只能给出一个使用增强模块的方法，情急之下，使用ChatGPT进行搜索，在合适的Prompt下，真的给出了相当可行的方案，由此进行记录。

## 1 修改虚拟机配置文件

- 关闭虚拟机。
- 找到虚拟机的配置文件（通常是`.vbox`文件）。
- 使用文本编辑器（如Notepad++）打开该文件。
- 在文件中找到类似以下的部分：

    ```xml
    <Display VRAMSize="16"/>
    ```

- 在`<Display>`标签中添加一个新的属性来设置自定义分辨率。例如：

    ```xml
    <Display VRAMSize="16" monitorCount="1">
        <VideoMode width="1920" height="1080" depth="32"/>
    </Display>
    ```

## 2 使用命令行工具打开虚拟机

### 2.1 安装`VBoxManage`工具

- 打开环境变量设定

    新增变量：`VBoxManage`
    值：`C:\Program Files\Oracle\VirtualBox`

- 打开Path

    新增：`%VBoxManage%`

### 2.2 打开虚拟机

- 打开终端或命令提示符（Command Prompt）。
- 使用`VBoxManage`工具来设置分辨率。假设虚拟机名称为"WindowsVM"，分辨率为1920x1080：

    ```sh
    VBoxManage setextradata "WindowsVM" "CustomVideoMode1" "1920x1080x32"
    ```

- 然后启动虚拟机，并在Windows中设置为这个分辨率。
