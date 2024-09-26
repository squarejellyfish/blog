---
title: 'Hexo Butterfly魔改：Code Highlighting'
date: 2024-09-26 16:30:00
tags:
- Hexo
- Butterfly
- 魔改
cover: /images/hexo-magic-change.png
---

# 安裝Butterfly

```sh
$ git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
$ npm install hexo-renderer-pug hexo-renderer-stylus
```

## _config.yml:
```yml
theme: butterfly
```

安裝完後，預設可以選淺色/深色模式，其實兩種都還不錯

唯一不滿意的是codeblock的highlighting，預設其實有蠻多選項，但是換成深色模式後，就只有一種

一怒之下，把整個網站改成gruvbox theme！

# 魔改Code highlighting

這應該是最簡單的步驟，[Butterfly官方](https://butterfly.js.org/en/posts/customize-code-coloring/?highlight=code+high)也有教學

## 將`hljs`設置`true`

根目錄下的`_config.yml`:

```yml
highlight:
  enable: true
  hljs: true # 改這兩個就夠
```

主題目錄下的`_coinfig.yml`:

```yml
highlighting_theme: false
```

## 下載想要的主題CSS

到[hljs官方github](https://github.com/highlightjs/highlight.js/tree/main)的`src/styles/`資料夾中找到想要的主題

例如`gruvbox-dark-medium.css`:

```css
pre code.hljs {
  display: block;
  overflow-x: auto;
  padding: 1em;
}

code.hljs {
  padding: 3px 5px;
}

.hljs {
  color: #d5c4a1;
  background: #282828;
}

.hljs::selection,
.hljs ::selection {
  background-color: #504945;
  color: #d5c4a1;
}
...
```

下載下來後放到部落格`/source/self/`資料夾，在檔案裡加上一塊：

```css
:root {
  --hl-color: #d5c4a1; # 文字顏色
  --hl-bg: #262626; # 背景顏色
  --hltools-bg: # optional
  --hltools-color: # optional
  --hlnumber-bg: # optional
  --hlnumber-color: # optional
  --hlscrollbar-bg: # optional
  --hlexpand-bg: # optional
}
```

再來找到`.hljs`區：

```css

/* 原本這樣
.hljs {
  color: #d5c4a1;
  background: #282828;
}
*/

/* 改這樣 */
#article-container figure.highlight .hljs {
  color: #d5c4a1;
  background: #282828;
}
```

## Inject Code

再來我們要inject剛剛寫好的檔案，到主題的`_config.yml`:

```yml
inject:
  head:
    - <link rel="stylesheet" href="/self/gruvbox.css">
```

魔改code highlighting完成！
