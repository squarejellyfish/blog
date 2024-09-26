---
title: Hexo搭建個人部落格
date: 2024-09-25 19:13:09
tags:
- hexo
- blog
cover: /images/Hexo.webp
---

## 需要有

- Node.js
- Git

## 安裝Hexo

```sh
npm install -g hexo-cli
```

## 創建部落格

```sh
hexo init <folder>
cd <folder>
npm install
```

## 創建貼文

```sh
hexo new "貼文名稱"
```

創建完後貼文會在`source/_post/`資料夾中，可以在那裡編輯

```markdown
---
title: Hexo搭建個人部落格
date: 2024-09-25 19:13:09
---

## 需要有

- Node.js
- Git

## 安裝Hexo
...
```

## 建立靜態檔&本機伺服器

```sh
hexo generate
hexo server
```

本機伺服器預設在[localhost:4000](https://youtu.be/dQw4w9WgXcQ?si=uPqVhaFQsc-12SYg) 

## 部署

1. 裝`hexo-deployer-git`插件：

```sh
npm install hexo-deployer-git --save
```

2. 到Github創建一個repo，名字叫做`<username>.github.io`（名字很重要）

3. 然後到`_config.yml`的最下面：

```yml
deploy:
  type: git
  repo: https://github.com/<username>/<username>.github.io.git ## 剛剛的repo
  branch: main ## master 也可以
```

4. 部署吧老鐵

```sh
hexo clean
hexo deploy
```

# 客製化主題

以主題`cactus`來舉例：

## 安裝

1. 在部落格主目錄：
```sh
git clone https://github.com/probberechts/hexo-theme-cactus.git themes/cactus
```

2. 在`_config.yml`中，把`theme`改成：
```yml
theme: cactus
```

3. 重新創建靜態檔：
```sh
hexo clean
hexo generate
```
