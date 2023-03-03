## 工程结构

```text
├─ node_modules #存放着项目所需的依赖，不需要关心
├─ docs  #该目录下存放 编写的文档
│  └─ theme-reco
│     ├─ api.md
│     ├─ plugin.md
│     ├─ theme.md
│     └─ README.md
├─ blogs #该目录下存放 编写的博客文章
│     ├─ category1
│     │  ├─ 2018
│     │  │  └─ 121501.md
│     │  └─ 2019
│     │     └─ 092101.md
│     ├─ category2
│     │  ├─ 2016
│     │  │  └─ 121501.md
│     │  └─ 2017
│     │     └─ 092101.md
│     └─ other
│        └─ guide.md
├─ .vuepress # 该目录下存放 项目配置文件 与 静态资源
│   ├─ config.js #该文件用于 配置项目
│   └─ public # 该目录下存放网页中所需的 静态资源
│     ├─ hero.png # 首页大图
│     ├─ logo.png # 站点logo
│     ├─ favicon.ico #站点图标
│     └─ avatar.png #头像
├─ package.json #依赖管理文件
└─ README.md #这里存放着博客首页的内容
```