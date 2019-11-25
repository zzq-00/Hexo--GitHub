---
title: Hexo结合GitHub创建个人博客
time: 2019-11-25
---

# 使用Hexo结合GitHub Pages 搭建静态个人网站
## 1.安装Hexo
- 依赖：
    - Node,js
    - Git
- 全局安装Hexo
```
$ npm install -g hexo-cli
# 确认是否安装成功
$ hexo --version
```
## 2.生成个人网站
```
 $ hexo init 项目名
 # 例如
 $ hexo init first-blog
```
## 3.本地预览
```
# 在网站目录下执行该命令，会启动一个本地 http 服务，用于浏览
hexo server
```
- 根据命行给出的地址，访问
## 4.如何写文章
- 在项目的`source/_posts`中创建MarkDown 文件并写入
```
---
title: 文章标题
time：2019-11-25
---
- 内容
```
## 5.如何部署
- 正确方式
    - 编译构建
    - 将搭建结果推送GitHub仓库
    - 开启GitHub Pages
**每次更新都需要重复上面的流程，太过繁琐，不推荐**
## 6.自动部署：GitHub Actions
- GitHub 新增功能
### 6.1 准备
- 1. 安装vue-cli到项目中
```
npm i -g hexo-cli
```
- 2. 修改网站根目录root
    - 如果你的部署的域名是根路径就不需要做任何修改
    - 如果你的部署的域名是子目录，就把root修改为那个子目录名称
    **在文件目录中的`_config.yml`文件中19行的`root` 修改目录**
- 3. 创建github仓库
- 4. 将项目源码推到github仓库
### 6.2 生成token秘钥
- 1.在当前仓库选择`settings`设置
- 2.在右侧tab栏选择`Developer settings`
- 3. 在`Personal access tokens`中点击`Generate newtoken`
- 4. 下拉 找到`Note`-选择项目名`first-blog`
- 5. 设置权限--构选第一项`repo`--然后确定`Generate token`--生成token
- **切记复制秘钥--保存下来--之后要用--秘钥之后不能查看**
### 6.3 将token秘钥添加到项目的screts中
- 在项目仓库中找到斌一次进 `settings`--`Secrets`
- `Name` 必须是ACCESS_TOKEN 【默认是这个名，也可在脚本中修改】
- `Value` 中 将秘钥复制过来---保存
### 6.4 配置GitHub Actions
- 1. 在仓库中进入Actions并进入
- 2. 点击右上角的`Set a workflow youorself`
- 3. 出现的代码替换成如下代码，然后点击右上角的Start commit
```
name: GitHub Actions Build and Deploy Demo 
on:
    push: 
        branches: 
            master 
jobs: 
    build-and-deploy: 
        runs-on: ubuntu-latest 
        steps:  
        name: Checkout 
            uses: actions/checkout@master 
        name: Build and Deploy 
        uses: JamesIves/github-pages-deploy-action@master 
        env:
            ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }} 
            BRANCH: gh-pages 
            FOLDER: public 
            BUILD_SCRIPT: npm install && npm run build
```