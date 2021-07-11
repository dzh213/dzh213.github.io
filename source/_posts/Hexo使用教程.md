---
title: Hexo使用教程
toc: true
date: 2019-11-16 13:24:23
categories:
  - Hexo
tags:
  - Hexo
---

### 分支情况
- master hexo生成的静态文件
- source markdown源码

### 拉取代码
``` bash
https://github.com/dzh213/dzh213.github.io.git
```
### 环境安装
**切换到source分支**
**全局安装Hexo(前提已安装node)**
```bash
npm install -g hexo-cli
```
**安装相关依赖**
相关依赖已写入了package.json中，直接安装即可。
```bash
npm i
```
### Hexo 命令使用
**创建一篇文章**
```bash
hexo new "demo"
```
**生成静态文件**
```bash
hexo generate
```
可简写成
```bash
hexo g
```
**本地部署预览**
```bash
hexo server
```
**将静态文件部署到github**
```bash
hexo deploy
```
简写
```bash
hexo d
```
**缓存文件和静态文件清理**
```bash
hexo clean
```
&nbsp;

***编辑完成后需要将source分支上的markdown源码通过git提交，以免丢失。***
