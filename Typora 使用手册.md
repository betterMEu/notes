# Markdown For Typora

## Overview

**Markdown** is created by [Daring Fireball](http://daringfireball.net/); the original guideline is [here](http://daringfireball.net/projects/markdown/syntax). Its syntax, however, varies between different parsers or editors. **Typora** is using [GitHub Flavored Markdown][GFM].

[toc]

## 块级元素

### 段落和换行符

小换行 `shift` + `return`

换行 `return` | `</br>`

### 标题

Headings use 1-6 hash (`#`) characters at the start of the line, corresponding to heading levels 1-6. For example:

``` markdown
# This is an H1

## This is an H2

###### This is an H6
```

In Typora, input ‘#’s followed by title content, and press `Return` key will create a heading.

### 引用块

输入 `>` 并按下回车

>block queto
>
>>嵌套 

### 列表

Input `* list item 1` will create an unordered list - the `*` symbol can be replace with `+` or `-`.

Input `1. list item 1` will create an ordered list - their markdown source code is as follows:

``` markdown
## un-ordered list
*   Red
*   Green
*   Blue

## ordered list
1.  Red
2. 	Green
3.	Blue
```

### 任务列表

可以点击标志完成 | 未完成，输入 `- [ ] -`

- [x] a task list item
- [ ] list syntax required
- [ ] normal **formatting**, @mentions, #1234 refs
- [ ] incomplete
- [x] completed



### 代码块

在空白行输入 `~~~[编程语言名称]` 或者 ```[编程语言名称] 然后按下回车



### 数学公式

You can render *LaTeX* mathematical expressions using **MathJax**.

To add a mathematical expression, input `$$` and press the 'Return' key. This will trigger an input field which accepts *Tex/LaTex* source. For example:


$$
\mathbf{V}_1 \times \mathbf{V}_2 =  \begin{vmatrix}
\mathbf{i} & \mathbf{j} & \mathbf{k} \\
\frac{\partial X}{\partial u} &  \frac{\partial Y}{\partial u} & 0 \\
\frac{\partial X}{\partial v} &  \frac{\partial Y}{\partial v} & 0 \\
\end{vmatrix}
$$


In the markdown source file, the math block is a *LaTeX* expression wrapped by a pair of ‘$$’ marks:

``` markdown
$$
\mathbf{V}_1 \times \mathbf{V}_2 =  \begin{vmatrix}
\mathbf{i} & \mathbf{j} & \mathbf{k} \\
\frac{\partial X}{\partial u} &  \frac{\partial Y}{\partial u} & 0 \\
\frac{\partial X}{\partial v} &  \frac{\partial Y}{\partial v} & 0 \\
\end{vmatrix}
$$
```

You can find more details [here](https://support.typora.io/Math/).

### 表格

输入 `| First Header  | Second Header |` 并按下 `return`

``` markdown
| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |
```

表格项里可以使用加粗等效果...

使用 `：` 为内容设置居中 | 靠左 | 靠右

``` markdown
| Left-Aligned  | Center Aligned  | Right Aligned |
| :------------ |:---------------:| -----:|
| col 3 is      | some wordy text | $1600 |
| col 2 is      | centered        |   $12 |
| zebra stripes | are neat        |    $1 |
```



### 脚注

``` markdown
You can create footnotes like this[^footnote].

[^footnote]: Here is the *text* of the **footnote**.
```

效果：

You can create footnotes like this[^footnote].

[^footnote]: Here is the *text* of the **footnote**.



### 水平线

输入 `***` 或者 `---` 并按下回车

------



### YAML Front Matter

Typora now supports [YAML Front Matter](http://jekyllrb.com/docs/frontmatter/). Input `---` at the top of the article and then press `Return` to introduce a metadata block. Alternatively, you can insert a metadata block from the top menu of Typora.



### 目录大纲

输入 [toc] 并按下回车



## 行内元素



### 链接

``` markdown
This is [an example](http://example.com/ "Title") inline link.

[This link](http://example.net/) has no title attribute.
```

***Effect：***

This is [an example](http://example.com/ "Title") inline link.

[This link](http://example.net/) has no title attribute.



#### 内部链接

~~~markdown
#块级元素
[我会跳转到块级元素目录噢](#块级元素) 
~~~

使用 Ctrl + `鼠标点击` 跳转 [我会跳转到块级元素目录噢](#块级元素) 

>底层用的是 HTML 标签的 id 属性跳转，也可以通过设置 id 如 `<span id='块级元素'>块级元素</span>` 完成内部链接跳转



#### 引用链接

``` markdown
This is [an example][id] reference-style link.

Then, anywhere in the document, you define your link label on a line by itself like this:

[id]: http://example.com/  "Optional Title Here"
```

效果:

This is [id][] reference-style link.

[id]: http://example.com/	"Optional Title Here"



还有另一种语法，效果一样

``` markdown
[Google][]
And then define the link:

[Google]: http://google.com/
```

[Google][]
And then define the link:

[Google]: http://google.com/



### URLs

Typora allows you to insert URLs as links, wrapped by `<`brackets`>`.

`<i@typora.io>` becomes <i@typora.io>.

Typora will also automatically link standard URLs. e.g: www.google.com.

### Images

Images have similar syntax as links, but they require an additional `!` char before the start of the link. The syntax for inserting an image looks like this:

``` markdown
![Alt text](/path/to/img.jpg)

![Alt text](/path/to/img.jpg "Optional title")
```

You are able to use drag & drop to insert an image from an image file or your web browser. You can modify the markdown source code by clicking on the image. A relative path will be used if the image that is added using drag & drop is in same directory or sub-directory as the document you're currently editing.

If you’re using markdown for building websites, you may specify a URL prefix for the image preview on your local computer with property `typora-root-url` in YAML Front Matters. For example, input `typora-root-url:/User/Abner/Website/typora.io/` in YAML Front Matters, and then `![alt](/blog/img/test.png)` will be treated as `![alt](file:///User/Abner/Website/typora.io/blog/img/test.png)` in Typora.

You can find more details [here](https://support.typora.io/Images/).



### 斜体

``` markdown
*single asterisks*

_single underscores_
```

output:

*single asterisks*

_single underscores_



### 粗体

``` markdown
**double asterisks**

__double underscores__
```

output:

**double asterisks**

__double underscores__



### 代码

``` markdown
Use the `printf()` function.
```

Use the `printf()` function.



### 删除线

GFM adds syntax to create strikethrough text, which is missing from standard Markdown.

`~~Mistaken text.~~` becomes ~~Mistaken text.~~



### 下划线

Underline is powered by raw HTML.

`<u>Underline</u>` becomes <u>Underline</u>.



### Emoji :smile:

Input emoji with syntax `:smile:`.

User can trigger auto-complete suggestions for emoji by pressing `ESC` key, or trigger it automatically after enabling it on preference panel. Also, inputting UTF-8 emoji characters directly is also supported by going to `Edit` -> `Emoji & Symbols` in the menu bar (macOS).

### 数学

To use this feature, please enable it first in the `Preference` Panel -> `Markdown` Tab. Then, use `$` to wrap a TeX command. For example: `$\lim_{x \to \infty} \exp(-x) = 0$` will be rendered as LaTeX command.

To trigger inline preview for inline math: input “$”, then press the `ESC` key, then input a TeX command.

You can find more details [here](https://support.typora.io/Math/).

### 下标

To use this feature, please enable it first in the `Preference` Panel -> `Markdown` Tab. Then, use `~` to wrap subscript content. For example: `H~2~O`, `X~long\ text~`/

### 上标

To use this feature, please enable it first in the `Preference` Panel -> `Markdown` Tab. Then, use `^` to wrap superscript content. For example: `X^2^`.

### 高亮

To use this feature, please enable it first in the `Preference` Panel -> `Markdown` Tab. Then, use `==` to wrap highlight content. For example: `==highlight==`.

## HTML

You can use HTML to style content what pure Markdown does not support. For example, use `<span style="color:red">this text is red</span>` to add text with red color.

### Embed Contents

Some websites provide iframe-based embed code which you can also paste into Typora. For example:

```Markdown
<iframe height='265' scrolling='no' title='Fancy Animated SVG Menu' src='http://codepen.io/jeangontijo/embed/OxVywj/?height=265&theme-id=0&default-tab=css,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'></iframe>
```

### Video

You can use the `<video>` HTML tag to embed videos. For example:

```Markdown
<video src="xxx.mp4" />
```

### Other HTML Support

You can find more details [here](https://support.typora.io/HTML/).

[GFM]: https://help.github.com/articles/github-flavored-markdown/
[an example]: 





# 绘图

