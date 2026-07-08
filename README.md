# Momo

<div align="center">
    <img src="./doc/images/dark-light.jpg">
    <p>一个极简的Blog模板，使用 <a href="https://github.com/Motues/Momo">Momo项目</a> 搭建</p>
    <small><ins>简体中文</ins></small> <small><a href="./doc/README_en.md">English</a></small> 
</div>



## 🚀 快速开始

1. 克隆本项目
    ```bash
    git clone https://github.com/Motues/Momo.git
    cd Momo
    ```
2. 运行 `pnpm install` 安装依赖（使用 `npm install -g pnpm` 安装 `pnpm`）
3. 运行 `pnpm dev` 启动开发服务器

## 🔧 配置

参考 [配置指南](./doc/config_zh-cn.md)，详细信息可以访问 [Momo](https://momo.motues.top/intro/config)，阅读对应文章获取详细信息。

## 📚 更新

参考 [更新指南](./doc/release_zh-cn.md)，介绍如何更新项目，详细信息可以访问 [Momo](https://momo.motues.top/intro/release)。

## 🍃 分支

下面是一些分支，会不定期进行维护，无法保证与 `main` 分支一致

* `memos`：实现 Memos 卡片功能
* `v6`：将依赖升级到 Astro v6

## ⚡ 指令

以下所有的指令可以在根目录下面执行

| 指令 | 作用 |
| --- | --- |
| `pnpm install` | 安装依赖 |
| `pnpm dev` | 启动本地服务器，运行在 `http://localhost:4321` |
| `pnpm build` | 构建发布版本到 `./dist` 目录下 |
| `pnpm preview` | 预览构建后的发布版本 |
| `pnpm astro ...` | 运行 `astro` 命令，例如 `astro add` |
| `pnpm newpost <path> <lang>` | 创建新文章，例如 `pnpm newpost docs/test.md zh-cn`，语言可以省略，默认为`zh-cn` |


## 📚 参考

* [Astro](https://astro.build/)
* [Fuwari](https://github.com/saicaca/fuwari)
* [Tyndall](https://github.com/moyuin-aka/tyndall-public)
