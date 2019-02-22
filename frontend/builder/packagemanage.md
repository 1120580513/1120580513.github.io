# packagemanage

> 包管理工具

---
## 常用命令
||npm|yarn|pnmp|
|---|---|---|---|
|---#常用命令|
|初始化项目|npm init|yarn init|
|从package.json安装依赖|npm install|yarn|
|安装包|npm install package[@version] [--save、--save-dev]|yarn add package[@version] [--dev、-D]|
|重新下载所有包|npm rebuild|yarn install --force|
|卸载包|npm uninstall package|yarn remove package|
|升级包|rm -rf node_modules && npm install|yarn upgrade|
|---#config|
|列出配置项|npm config ls|yarn config list|
|---#registry|
|登录/登出/发布|npm login/logout/publish|yarn login/logout/publish|
|初始化项目|npm init|yarn c|

## npm 设置淘宝镜像
```shell
npm config set registry https://registry.npm.taobao.org
```

## cnpm
```shell
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
