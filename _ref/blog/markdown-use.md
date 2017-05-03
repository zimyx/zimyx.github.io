---
title:  "Jeklly支持的Markdown规范"
feature_text: 帮助系列
---

Jeklly 支持的Markdown规范 （使用Typora的方法）

|  样式  |         句法          |   快捷键   |                    用例                    |               输出               |
| :--: | :-----------------: | :-----: | :--------------------------------------: | :----------------------------: |
|  粗体  | `** **`  or `__ __` | Ctrl+b  |         `**This is bold text**`          |            **粗体文本**            |
|  斜体  |   `* *` or `_ _`    | Ctrl+i  |       `*This text is italicized*`        |             *斜体文本*             |
| 删除线  |       `~~ ~~`       |         |       `~~This was mistaken text~~`       |            ~~错误文本~~            |
| 下划线  |                     | Ctrl+ u |                                          |          <u>划线文本</u>           |
|  标题  |   `#` ~ `######`    |         |            在标题行开始前加 1到6个符号`#`            |            #####小标题            |
|  引用  |         `>`         |         |               在引用行开始前加上`>`               |             ▍引用文本              |
| 行内代码 |    ``    `  ` ``    |         |              用反引号包裹行中的代码或命令              |             `行内代码`             |
| 代码块  | ` ``` ` or ` ~~~ `  |         |               用三个反引号形成代码块                |                                |
|  链接  |                     | Ctrl+ k | This is [GitHub](https://pages.github.com/) |                                |
| 无序列表 |    `-`  or  `*`     |         |            行前用一个`-`或`*`  加空格             |             ● list             |
| 有序列表 |         1.          |         |                行前用 1. 加空格                |                                |
| 分割线  |                     |         |           使用`***`或者`---`  加上回车           |                                |
| 表达式  |        `$ $`        |         | 用`$`号包裹, 上标: ^ ^    ^2^    下标:  ~ ~     ~x~ | $lim_{x \to \infty} \ exp(-x)$ |
| 表情符  |                     |         |   `:haha:` `:joy:` `:smiley:`  `:cry:`   |         😆 😂 😀 :cry:         |
| 特殊符号 |                     |         |      版权©、注册商标® 、商标 ™   ￥  ±  ‰   §       |                                |
| 插入表格 |                     | Ctrl +t |                                          |                                |
| 转义符  |          \          |         |                                          |                                |
|  目录  |        [toc]        |         |        目录列表Table of Contents（TOC）        |                                |
|  注释  |       `[^ ]`        |         |           可以对某一个词语进行注释[^zzz]。            |                                |

[^zzz]: 注释内容

 ==text==        高亮无效                 

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
