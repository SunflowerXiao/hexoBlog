---
title: markdown语法总结
tags:
  - markdown
categories:
  - 开发语言
comments: true
date: 2019-10-29 11:27:06
---


#### 可以直接写html

```
<ul id="ProjectSubmenu">
    <li><a href="/projects/markdown/" title="Markdown Project Page">Main</a></li>
    <li><a class="selected" title="Markdown Basics">Basics</a></li>
    <li><a href="/projects/markdown/syntax" title="Markdown Syntax Documentation">Syntax</a></li>
    <li><a href="/projects/markdown/license" title="Pricing and License Information">License</a></li>
    <li><a href="/projects/markdown/dingus" title="Online Markdown Web Form">Dingus</a></li>
</ul>

<h3> 这里是文字 </h3>
```

字体的加粗和斜体：

```
*text*    <=>   <em>text</em>
**text**  <=>   <strong>text</strong>
_text_    <=>   <em>text</text>
__text__  <=>   <strong>text</strong>
```

code格式:

```
 `<h1>` <=>  <code>h1</code>
```

段落引用格式：

```
> This is a blockquote.    <=>  <blockquote> <p>This is a blockquote.</p> </blockquote>
> ## this is a blockquote  <=>  <blockquote> <h2>This is a blockquote.</h2> </blockquote>
```

headers标签: 
```
    #   <=>  <h1>
    ##  <=>  <h2>

    Markdown Basics
    ================  <=> <h1>Markdown: Basics</h1>

    Markdown: Basics
    ----------------  <=> <h2>Markdown: Basics</h2>
```

list标签 - ul

```
    *   Candy. 
    *   Gum.
    *   Booze.

    或者 

    +   Candy. 
    +   Gum.
    +   Booze.

    或者

    -   Candy. 

        this is a paragraph

    -   Gum.
    -   Booze.


    解析结果：

    <ul>
        <li>Candy.</li>
        <li>Gum.</li>
        <li>Booze.</li>
    </ul>

    <ul>
        <li><p>Candy.</p><p>this is a paragraph</p></li>
        <li>Gum.</li>
        <li>Booze.</li>
    </ul>

```

list标签 - ol

```
    1.  Red
    2.  Green
    3.  Blue

    解析如下

    <ol>
        <li>Red</li>
        <li>Green</li>
        <li>Blue</li>
    </ol>
```

超链接：

```bash
This is an [example link](http://example.com/). <=>   This is an <a href="http://example.com/">example link</a>

This is an [example link](http://example.com/ "With a Title").  <=>   This is an <a href="http://example.com/" title="With a Title">example link</a>

或者使用标识符

[demo] [s]    <=>  <a href="/projects/markdown/syntax" title="Markdown Syntax">demo</a>
[demo] [d]    <=>  <a href="/projects/markdown/dingus" title="Markdown Dingus">demo</a>
[demo] [src]  <=>  <a href="/projects/markdown/basics.text">demo</a>

[s]: /projects/markdown/syntax  "Markdown Syntax"
[d]: /projects/markdown/dingus  "Markdown Dingus"
[src]: /projects/markdown/basics.text

```

图片：

```
![alt text](/path/to/img.jpg "Title") <=> <img src="/path/to/img.jpg" alt="alt text" title="Title"/>

或者使用标识符

![alt text][d] <=> <img src="/path/to/img.jpg" alt="alt text" title="Title"/>
[d]: /path/to/img.jpg  "Title"
```