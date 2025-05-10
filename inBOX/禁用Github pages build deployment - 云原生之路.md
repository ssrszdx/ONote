---
title: "禁用Github pages build deployment - 云原生之路"
source: "https://blog.361way.com/2023/10/pages-build-deployment.html"
author:
  - "[[admin]]"
published: 2023-10-14
created: 2025-05-10
description: "禁用Github pages build deployment"
tags:
  - "clippings"
---
|

| reads

## 一、问题

Github pages服务是对小站长提供的一种福利服务，可以满足一般的静态站点的托管需求，可以绑定自定义域名，可以提供免费的https SSL服务。静态网站内容通过git命令提交后，可以配置Github Action实现网站内容的自动更新和发布，比如我使用Gohugo测试时发现，其默认会有一个 `pages-build-deployment` 的workflows，而且其运行时会报错，而查看`.github/workflows` 目录下根本不存在对应的文件。查看其运行的报错内容，会发现类似如下的内容：

```bash
1Error:  Logging at level: debug GitHub Pages: github-pages v228 GitHub Pages: jekyll v3.9.3 Theme: jekyll-theme-primer
```

[![pages build deployment](https://blog.361way.com/wp-content/uploads/2023/10/pages-build-deployment.png)](https://blog.361way.com/wp-content/uploads/2023/10/pages-build-deployment.png)

截图上的 Deploy Hugo site to Pages 才是我真实配置的Action。

## 二、解决方法

点选仓库Settings –> pages –> Build and deployment ，修改source选项，默认是 Deploy from a branch ，修改为Github Actions，见下图：

[![github action](https://blog.361way.com/wp-content/uploads/2023/10/github-actions-hugo.png)](https://blog.361way.com/wp-content/uploads/2023/10/github-actions-hugo.png)

修改完成后，回到Actions界面，删除之前的 pages-build-deployment workflows即可，后面就不会再出现了。这是因为github默认对应的静态应用程序是 jekyll。

#### 捐赠本站(Donate)

[![weixin_pay](https://blog.361way.com/wp-content/uploads/juanzheng/donation-mini.png)](https://blog.361way.com/wp-content/uploads/juanzheng/donation.png)  
如您感觉文章有用，可扫码捐赠本站！(If the article useful, you can scan the QR code to donate))

- **Author:** [shisekong](https://blog.361way.com/)
- **Link:** [https://blog.361way.com/2023/10/pages-build-deployment.html](https://blog.361way.com/2023/10/pages-build-deployment.html)
- **License:** This work is under a [知识共享署名-非商业性使用-禁止演绎 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-nd/4.0/). Kindly fulfill the requirements of the aforementioned License when adapting or creating a derivative of this work.

  

## See Also

- [构建安全的 DevSecOps Pipeline](https://blog.361way.com/devsecops-jenkins-github/8479.html)
- [vercel中指定Hugo版本](https://blog.361way.com/2023/11/vercel-version.html)
- [Huaweicloud CCE 实现容器网络隔离](https://blog.361way.com/2023/10/k8s-network-isolation.html)
- [Linux安装最新版Git](https://blog.361way.com/2023/10/vercel-netlify.html)
- [Mkdocs增加Giscus评论系统](https://blog.361way.com/2023/10/vercel-mkdocs.html)