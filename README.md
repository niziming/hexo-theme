# Hexo 博客搭建
个人博客快速搭建模板
## 版权
- Beantech 
- 胡伟煌
## 安装Hexo
npm install hexo-cli -g
如果下载速度慢的可以将npm的下载源换成国内镜像,推荐阿里巴巴的镜像.

## 使用博客模板
### 初始化
~~~ git 
git clone 链接
cd hexo-theme
npm install
~~~

### 基本的Hexo语法
~~~ git
hexo new post "<post name>" # 新建一篇博客
hexo clean && hexo generate # 清理并且生成项目
hexo server # 在本地运行hexo服务器
hexo deploy # 部署在远端
~~~

