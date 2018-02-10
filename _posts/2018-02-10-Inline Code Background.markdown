---
title: "Inline Code Block Background Color Changing"
layout: post
tag:
- Github
- Jekyll
- Blog
- Code Block
- Background
- Color
img: indigo/indigo.png
blog: true
author: gemst1
summary: "How to change a color of background of inline code block"
permalink: /:title
---

This posting refered to [Imyeonn's posting](https://imyeonn.github.io/blog/blog/19/) about code block 

### Add Variable

#### 1. Choose backgound color
First, search `#59e3ab` on google and choose your background color and copy the hex code of that color. I choose `#94d8e8` for my background color of inline code block.

#### 2. Add variable to `variable.sass`
Add variable to `_sass/base/variable.sass` as below
```sass
$codebackground: #94d8e8
```

### Change property of code block
We can modify the property at `_sass/base/general.sass`. We can find code about the property as seen below. Here, tt section is a property of the inline code block and pre section is a property of multiline code block. You can change both of them as your style.  공부한 내용들을 가끔씩 심심할때

**Notice**: You should put `code` and `		background: #fff` to change only inline code block

```sass
code,
tt
	padding: 1px 0
	font-family: $fontMonospace
	font-size: 12px
	line-height: 20px
	background: $codebackground
	border-radius: 2px
	border-radius: 2px

pre
	box-sizing: border-box
	margin: 0 0 1.75em 0
	width: 100%
	padding: 5px 10px
	font-family: $fontMonospace
	font-size: 1.2rem
	line-height: 2rem
	overflow: auto
	code
		background: #fff
	border: 1px solid $epsilon
	border-radius: 2px
  ```
