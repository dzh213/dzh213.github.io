---
title: 解决Homebrew安装太慢问题
date: 2021-07-11 15:51:23
tags:
  - Homebrew
categories:
  - Homebrew  
---

### 介绍

Homebrew是一款Mac OS平台下的软件包管理工具，拥有安装、卸载、更新、查看、搜索等很多实用的功能。简单的一条指令，就可以实现包管理，而不用你关心各种依赖和文件路径的情况，十分方便快捷。

本文主要解决问题：Homebrew常规安装太慢；以及通过brew install安装软件太慢，还有时不时的自动updating巨耗时的问题。

首先安利官网：https://brew.sh/index_zh-cn

### 使用国内源安装

1. 创建安装目录

   ```bash
   sudo mkdir /usr/local/Homebrew
   ```

2. 使用中科大源进行git克隆

   ```bash
   sudo git clone https://mirrors.ustc.edu.cn/brew.git /usr/local/Homebrew
   ```

3. 创建一个快捷方式到/usr/local/bin目录

   ```bash
   sudo ln -s /usr/local/Homebrew/bin/brew /usr/local/bin/brew
   ```

4. 创建homebrew-core文件夹，使用中科大源克隆homebrew-core

   ```bash
   sudo mkdir -p /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core
   
   sudo git clone https://mirrors.ustc.edu.cn/homebrew-core.git /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core
   ```

5. 创建`homebrew-cask`文件夹并使用git克隆

   ```bash
   cd "$(brew --repo)/Library/Taps/homebrew/"
   mkdir homebrew-cask
   git clone git://mirrors.ustc.edu.cn/homebrew-cask.git /usr/local/Homebrew/Library/Taps/homebrew/homebrew-cask
   ```

   

6. 获取权限 并 运行更新

   ```bash
   sudo chown -R $(whoami) /usr/local/Homebrew
   
   brew update
   ```

到此，brew相关组件全部安装完成。



### 常用命令

brew 常用命令

```bash
# 显示帮助
brew –help

# 安装软件
brew install pandoc

# 卸载软件
brew uninstall 软件名

# 搜索软件
brew search git

# 查看经安装软件列表
brew list

# 更新所有软件
brew update

# 更新某具体软件
brew upgrade git

#显示软件内容信息
brew info git

# 查看哪些已安装的程序需要更新
brew outdated
```

cask常用命令，大部分命令与brew命令的使用逻辑相同

```bash
#下载软件
brew cask install cakebrew

#卸载软件
brew cask uninstall skitch
```



### 参考文章

1. [解决国内Homebrew安装太慢问题](https://xixincan.github.io/2020/05/23/Skill/%E8%A7%A3%E5%86%B3%E5%9B%BD%E5%86%85Homebrew%E5%AE%89%E8%A3%85%E5%A4%AA%E6%85%A2%E9%97%AE%E9%A2%98/)
2. [Mac快速安装HomeBrew和HomeBrew-Cask方法](https://blog.csdn.net/m0_46197393/article/details/106752367)
