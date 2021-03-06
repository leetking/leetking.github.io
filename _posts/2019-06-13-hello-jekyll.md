---
title: Hello Jekyll
create_date: 2019-06-13
modify_date: 2019-08-30
categories: [jekyll, liquid, mathjax, note]
permalink: /hello-jekyll
---

{% assign open_tag = '{%' %}

搭建基于 jekyll 博客时所做的笔记和遇见的一些问题。

jekyll 采用的模板语言是 Liquid，Markdown 处理引擎默认是 kramdown


## jekyll 的渲染流程

```text
[Markdown和Liquid混合页面] --Liquid模板处理--> [Markdown页面] --Markdown渲染--> [HTML页面] --MathJax渲染公式--> [最终网页]
```

这也就是为何即便是 Markdown 中的「代码块」如果包含 {% raw %}`{{`、`}}`、`{%` 或 `%}`{% endraw %} 的话，也需要用 Liquid 的
```liquid
{{ open_tag }} raw %}
  bala bala
{{ open_tag }} endraw %}
```
包裹的原因。


## 模板语言 Liquid

jekyll 采用 `Liquid` 作为「模板语言」，包含了三个部分：[objects](#objects)、[tags](#tags) 和 [filters](#filters).

### Objects

Objects 直接采用 {% raw %}`{{`{% endraw %} 和 {% raw %}`}}`{% endraw %} 包裹需要输出的变量即可。
例如输出变量要 `page.title` 的值，使用 {% raw %}`{{ page.site }}`{% endraw %}.

* 常用变量列表

| Objects       | 解释                                                  |
|---------------|-------------------------------------------------------|
| site          |                                                       |
| site.data     | 默认使用 `_data` 文件夹下的 yaml 文件                 |
| site.posts    | 整个网站内的文章 (page) 列表                          |
| page          | 一篇文章                                              |
| page.url      |                                                       |
| page.layout   | 本文章面采用 `_layouts/` 下的哪一个布局文件           |
| page.date     |                                                       |
| page.title    | 本文章的标题，默认是文件名，可被头信息的 title 覆盖   |
| page.excerpt  | 文章的第一自然段                                      |
| content       | 一篇文章最终渲染成的 HTML                             |

### Tags

Tags 是由 {% raw %}`{%`{% endraw %} 和 {% raw %}`%}`{% endraw %} 包裹的「逻辑表达」，例如：
{% raw %}
```liquid
{% if page.show_sidebar %}
  <div class="sidebar">
    sidebar content
  </div>
{% endif %}
```
{% endraw %}

* 常用 Tags

| Tags              | 功能                              |
|-------------------|-----------------------------------|
| if                | 判断条件是否满足                  |
| for «v» in «list» | 迭代 «list»                       |
| raw               | 原样输出内容                      |
| include           | 把 `_includes/` 下的文件包含进来  |
| assign            | 定义变量                          |

### Filters

Filters 能改变变量的输出，由 `|` 和变量连接。例如：
{% raw %}
```liquid
{{ "hello" | capitalize }}
```
{% endraw %}
能把 `hello` 转换为首字母大写的 `Hello`.

* 常用的 Filters

| Filters           | 功能                      |
|-------------------|---------------------------|
| capitalize        | 大写首字母                |
| downcase          | 转为小写                  |
| upcase            | 转为大写                  |
| first             | 输出列表的第一个          |
| date_to_string    | 转换日期为字符串形式      |
| markdownify       | 渲染 Markdown 内容为 HTML |
| where             | ?                         |


## 头信息

用于定义**本页**的变量，所有变量读存放在 `page` 变量里。例如：
```liquid
---
name: value
---
```
通过 `{% raw %}{{ page.name }}{% endraw %}` 来引用。

TODO 保留变量
page.layout


## 数据文件

jekyll 的数据文件位于 `_data/` 下，并且支持 `YAML`、`JSON` 和 `CSV` 作为数据文件。
通过 `site.data.file_name` 访问这些数据。例如：

* nav.yml

```yaml
- name: Home
  link: /
- name: About
  link: /about.html
```

通过 `for` 标签迭代 `nav.yaml` 的内容
{% raw %}
```liquid
{% for it in site.data.nav %}
  <a href="{{ it.link }}">{{ it.name }}</a>
{% endfor %}
```
{% endraw %}
生成这样的输出：
```html
<a href="/">Home</a>
<a href="/about.html">About</a>
```


## Collections

Collections 是用于定义类似于 `_posts/` 目录的技术。首先我们在 `_config.yml` 中如下配置：
```yaml
collections:
  authors:
    output: true
```
那么就可以在网站的根目录下添加目录 `_authors/`，然后在其中的文件类似于 posts 里的文章，通过 `site.authors` 就能遍历它们了。
`output: true` 能让 `_authors/` 目录中的内容输出，那么就拥有了属性 `url`.

NOTE 思考一下这样能不能做到文件不需要 `YYYY-mm-dd-title.md` 的格式呢？


## jekyll 命令手记

```shell
$ jekyll new            # 创建网站并自带一个基于 Gemfile 的主题
$ jekyll new-theme      # 创建新主题
$ jekyll build          # jekyll b 构建网站并输出到 _site/
$ jekyll build --drafts # 草稿也一起构建
$ jekyll serve          # jekyll s 构建网站并启动本地服务器 http://localhost:4000
$ jekyll serve --watch  # 监视文件改动
$ jekyll clean
```


## 添加代码高亮

使用 highlight 来高亮代码

* 添加 highlight.js
* 启用 highlight.js
```javascript
hljs.initHighlightingOnLoad();
```

## 添加数学公式支持

使用 MathJax 来支持 LaTex 语法的数学公式

* 在页面中添加
```javascript
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
```

* 配置 MathJax 以支持 \$ 为行内公式
```javascript
MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
```

* 配置 MathJax 支持 mediawiki-texvc，如果想支持 `\Omicron` 之类大写希腊字母就需要开启
```javascript
TeX: { extensions: ["mediawiki-texvc.js"] }
```


## 添加 ToC

采用插件 `jekyll-toc` 来实现


## 功能配置测试

### 代码高亮

- C 语言
  ```c
  #include <stdio.h>

  int main(int argc, char **argv)
  {
      printf("Hello Jekyll\n");
      return 0;
  }
  ```

- Java 语言
  ```java
  package com.cwnu.foo;

  public class Main {
      public void main(String[] args) {
          System.out.println("Hello World!\n");
      }
  }
  ```

- Lua 语言
  ```lua
  local rest = require("test")
  test.print("Hello World!\n")
  ```

- Python 语言
  ```python
  import sys
  from os import path

  def hello_word():
      return "Hello World!"

  if __name__ == '__main__':
      print(hello_word())
  ```

### 显示数学公式

- 行内公式

  质能守恒 $ E = mc^2 $

- 行间公式

  记场 $ \vec F = (P, Q, R) $，$\Gamma$ 为光滑曲线，则场 $\vec F$ 在上 $\Gamma$ 的第二型曲线积分为：

  $$ W = \int_\Gamma \vec F\, \mathrm{d} \vec \tau
  = \int_\Gamma (P, Q, R) \cdot (\cos \alpha, \cos \beta, \cos \gamma)\, \mathrm{d} \Gamma $$

  $\vec \tau$ 为 $\Gamma$ 的方向向量


## TODO

1. TOC，采用 `jekyll-toc` 插件
2. 章节的标号，这是 Kramdown 的语法支持
3. 引用的标记，由 Kramdown 语法支持
4. 参考 jekyll 官网如何配置，能支持 Liquid 代码的高亮


## 参考资料

1. [jekyll 文档](https://jekyllrb.com/docs/)
2. [jekyllcn 文档](https://jekyllcn.com/docs/home/)
3. [Liquid 文档](https://shopify.github.io/liquid/)
4. [kramdown 文档](https://kramdown.gettalong.org/index.html)
5. [highlight](https://highlightjs.org)
6. [处理代码块中的 `{{ open_tag }} raw %}`](https://rmaicle.github.io/posts/71xXldY8KeKXWgw)
