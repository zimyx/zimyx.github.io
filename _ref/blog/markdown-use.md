---
title:  "Jeklly支持的Markdown规范"
feature_text: 帮助系列
---

Jeklly 支持的Markdown规范 （使用Typora的方法）

|  样式  |         句法          |  快捷键  |                    用例                    |               输出               |
| :--: | :-----------------: | :---: | :--------------------------------------: | :----------------------------: |
|  粗体  | `** **`  or `__ __` | Ctr+b |         `**This is bold text**`          |            **粗体文本**            |
|  斜体  |   `* *` or `_ _`    | Ctr+i |       `*This text is italicized*`        |             *斜体文本*             |
| 删除线  |       `~~ ~~`       |       |       `~~This was mistaken text~~`       |            ~~错误文本~~            |
| 下划线  |                     | Ctr+u |                                          |          <u>划线文本</u>           |
|  标题  |   `#` ~ `######`    |       |            在标题行开始前加 1到6个符号`#`            |            #####小标题            |
|  引用  |         `>`         |       |               在引用行开始前加上`>`               |             ▍引用文本              |
| 行内代码 |    ``    `  ` ``    |       |              用反引号包裹行中的代码或命令              |             `行内代码`             |
| 代码块  |       ` ``` `       |       |               用三个反引号形成代码块                |                                |
|  链接  |                     | Ctr+k | This is [GitHub](https://pages.github.com/) |                                |
| 无序列表 |    `-`  or  `*`     |       |            行前用一个`-`或`*`  加空格             |             ● list             |
| 有序列表 |         1.          |       |                行前用 1. 加空格                |                                |
| 分割线  |                     |       |           使用`***`或者`---`  加上回车           |                                |
| 表达式  |        `$ $`        |       | 用`$`号包裹, 上标: ^ ^    ^2^    下标:  ~ ~     ~x~ | $lim_{x \to \infty} \ exp(-x)$ |
| 表情符  |                     |       |   `:haha:` `:joy:` `:smiley:`  `:cry:`   |         😆 😂 😀 :cry:         |
| 特殊符号 |                     |       |      版权©、注册商标® 、商标 ™   ￥  ±  ‰   §       |                                |
| 插入表格 |                     | Ctr+t |                                          |                                |
| 转义符  |          \          |       |                                          |                                |
|  目录  |        [toc]        |       |        目录列表Table of Contents（TOC）        |                                |
|  注释  |       `[^ ]`        |       |           可以对某一个词语进行注释[^zzz]。            |                                |

[^zzz]: 注释内容

 ==text==        高亮 ==??==在jeklly无效                 

- [ ] ​        任务列表符无效  

代码块示例

```scss
.highlight table td { padding: 5px; }
.highlight table pre { margin: 0; }
```



加强文本块

**注意**      在文本块之后加上    {: .notice}
{: .notice}

**Primary Notice:**   在文本块换行之后加上   {: .notice--primary}
{: .notice--primary}

**Info Notice:**      在文本块换行之后加上   {: .notice--info}
{: .notice--info}

**Warning Notice:**   在文本块换行之后加上   {: .notice--warning}
{: .notice--warning}

**Danger Notice:**    在文本块换行之后加上   {: .notice--danger}
{: .notice--danger}

**Success Notice:**   在文本块换行之后加上    {: .notice--success}
{: .notice--success}

---

**附件：** 个人定制的base.user.css ，从##开始自动生成TOC和内容序号,参考[typora帮助](http://support.typora.io/)

```css
/**************************************
 * TOC 目录生成序号  从##开始
 **************************************/
 
/* No link underlines in TOC */
.md-toc-inner {
    text-decoration: none;
}
 
.md-toc-content {
    counter-reset: h2toc
}
 
.md-toc-h2 {
    margin-left: 0;
    font-size: 1.5rem;
    counter-reset: h3toc
}
 
.md-toc-h3 {
    font-size: 1.1rem;
    margin-left: 2rem;
    counter-reset: h4toc
}
 
.md-toc-h4 {
    margin-left: 3rem;
    font-size: .9rem;
    counter-reset: h5toc
}
 
.md-toc-h5 {
    margin-left: 4rem;
    font-size: .85rem;
    counter-reset: h6toc
}
 
.md-toc-h6 {
    margin-left: 5rem;
    font-size: .8rem;
}

 
.md-toc-h2:before {
    color: black;
    counter-increment: h2toc;
    content: counter(h2toc) ". "
}
 
.md-toc-h2 .md-toc-inner {
    margin-left: 0;
}
 
.md-toc-h3:before {
    color: black;
    counter-increment: h3toc;
    content: counter(h2toc) ". " counter(h3toc) ". "
}
 
.md-toc-h3 .md-toc-inner {
    margin-left: 0;
}
 
.md-toc-h4:before {
    color: black;
    counter-increment: h4toc;
    content: counter(h2toc) ". " counter(h3toc) ". " counter(h4toc) ". "
}
 
.md-toc-h4 .md-toc-inner {
    margin-left: 0;
}
 
.md-toc-h5:before {
    color: black;
    counter-increment: h5toc;
    content: counter(h2toc) ". " counter(h3toc) ". " counter(h4toc) ". " counter(h5toc) ". "
}
 
.md-toc-h5 .md-toc-inner {
    margin-left: 0;
}
 
.md-toc-h6:before {
    color: black;
    counter-increment: h6toc;
    content: counter(h2toc) ". " counter(h3toc) ". " counter(h4toc) ". " counter(h5toc) ". " counter(h6toc) ". "
}
 
.md-toc-h6 .md-toc-inner {
    margin-left: 0;
}
 
 
/**************************************
 * 内容自动序号  从##开始
 **************************************/
 
/** initialize css counter */
#write {
    counter-reset: h2
}
 
h2 {
    counter-reset: h3
}
 
h3 {
    counter-reset: h4
}
 
h4{
    counter-reset: h5
}
 
h5 {
    counter-reset: h6
}
 

 
/** put counter result into headings */
#write h2:before {
    counter-increment: h2;
    content: counter(h2) ". "
}
 
#write h3:before {
    counter-increment: h3;
    content: counter(h2) "." counter(h3) ". "
}
 
#write h4:before, h4.md-focus.md-heading:before { /*override the default style for focused headings */
    counter-increment: h4;
    content: counter(h2) "." counter(h3) "." counter(h4) ". "
}
 
#write h5:before, h5.md-focus.md-heading:before {
    counter-increment: h5;
    content: counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) ". "
}
 
#write h6:before, h6.md-focus.md-heading:before {
    counter-increment: h6;
    content: counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) "." counter(h6) ". "
}
  
/** override the default style for focused headings */
#write>h3.md-focus:before, #write>h4.md-focus:before, #write>h5.md-focus:before, #write>h6.md-focus:before, h3.md-focus:before, h4.md-focus:before, h5.md-focus:before, h6.md-focus:before {
    color: inherit;
    border: inherit;
    border-radius: inherit;
    position: inherit;
    left: initial;
    float: none;
    top: initial;
    font-size: inherit;
    padding-left: inherit;
    padding-right: inherit;
    vertical-align: inherit;
    font-weight: inherit;
    line-height: inherit;
}
```

