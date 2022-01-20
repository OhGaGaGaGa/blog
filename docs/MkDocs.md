# 使用 MkDocs + Github 搭建博客

并未使用 GitHub Pages，只是新建了普通的 GitHub 仓库。

## 主题

MkDocs: https://www.mkdocs.org

Material MkDocs: https://squidfunk.github.io/mkdocs-material

### 常用命令

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.
* `mkdocs gh-deploy` - Publish to \*.github.io/{Repository_NAME}

由于 GitHub 在命令行中同步可能出现代理等问题，不妨使用 GitHub Desktop 进行同步。在 `master` 分支中设置 workflow 后，即可自动将更改更新至网页中。

## 搜索引擎索引

### Google

在 https://search.google.com/search-console 中添加资源。添加时需要验证自己的所有权，采用 HTML 标记的方式。

使用覆盖 HTML Title 的方法完成：https://squidfunk.github.io/mkdocs-material/customization/#overriding-partials

目录：

```
.
├─ overrides/
│  └─ main.html
└─ mkdocs.yml
```

`main.html` 代码：

```html
{% extends "base.html" %}

{% block htmltitle %}
  <title>WJK's Blog</title>
  <meta name="google-site-verification" content="1234567812345678abcdefghijk1234567812345678" />
{% endblock %}
```

### Bing

在 https://www.bing.com/webmasters 中，登录微软账户或谷歌账户后，直接通过 Google Search Console 导入即可。

## 公式

采用 https://cyent.github.io/markdown-with-mkdocs-material/syntax/math_main/ 中的方法

用于TeX数学公式的展示，使用之前要先在docs/里创建javascripts目录，并在目录中放置extra.js

依赖模块: pymdownx.arithmatex

目录：

```
.
├─ docs/
│  └─ js/ 
│     └─ extra.js
└─ mkdocs.yml
```

`extra.js` 代码：

```js
window.MathJax = {
  tex2jax: {
    inlineMath: [ ["\\(","\\)"] ],
    displayMath: [ ["\\[","\\]"] ]
  },
  TeX: {
    TagSide: "right",
    TagIndent: ".8em",
    MultLineWidth: "85%",
    equationNumbers: {
      autoNumber: "AMS",
    },
    unicode: {
      fonts: "STIXGeneral,'Arial Unicode MS'"
    }
  },
  displayAlign: "left",
  showProcessingMessages: false,
  messageStyle: "none"
};
```

`mkdocs.yml` 后添加内容：

```yaml
extra_javascript:
    - 'js/extra.js'
    - 'https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-MML-AM_CHTML'
```

## 参考

https://affectalways.github.io/hugo_seo/

https://cyent.github.io/markdown-with-mkdocs-material/