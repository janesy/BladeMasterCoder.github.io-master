---
layout: post
title: 史上最全个人技术博客搭建教程
category: 技术 博客
tags: [blog]
description: 
---

> 断断续续折腾了一个多月的博客终于在昨天全部完工，就等域名实名认证通过就可以上线了。期间搜集了很多的资料，照着做还是会有些疑惑，所以我把制作过程中遇到的难题都一一记下来，今天就给大家整理一篇最为齐全的个人博客搭建教程……

# 首先，准备工程 #

     按我的想法来讲，第一步是选好博客的模板。因为如果你是真心对待这件事情的话，今天创建的博客将跟随你一生，就像男孩选老婆一样，一定要选一个令自己称心如意，看着舒服的模板，这样也就不至于看了一阵日子后就腻了，在看到别的好看的就三心二意想要更换，这个时候估计是很麻烦了，相信你也不会想给自己再添乱了。所以我在选博客框架的时候花费的时间是最多的，中途换了三四个模板，以至于一个多月才将博客上架。所以，当你在看到一个好看的模板的时候，先不要心急，仔细的想想以后你是否会依然喜爱，它是否真的符合你的需求，最好是多找一个从中对比，找到最适合你的，但不一定是最炫的。


到[google-code-prettify](http://code.google.com/p/google-code-prettify/)官网下载代码。

# 2.包含css和js #

在自己的项目中导入`prettify.css`和`prettify.js`，如下

	<link rel="stylesheet" href="/assets/js/prettify/prettify.css">
	<script src="/assets/js/prettify/prettify.js"></script>

调用 `prettify.js` 实现代码高亮，`linenums`表示显示行号，如果不想显示行号，就去掉。

	<script type="text/javascript">
	  $(function(){
	    $("pre").addClass("prettyprint linenums");
	    prettyPrint();
	  });
	</script>
	
这里导入了css和js后，就可以直接用`markdown`的`tab`的方式来导入代码段了，而如果想在行里显示代码高亮，则需要在行里用`（键盘上1左边的符号）包裹代码。


# 3.显示行号 #

默认的每五行显示一个行号，如果想每一行都显示行号，则需要修改`prettify.css`，`list-style-type` 由`none`改为 `decimal`

	li.L0, li.L1, li.L2, li.L3, li.L4, li.L5, li.L6, li.L7, li.L8, li.L9 { list-style-type: decimal;}


# 4.主题样式 #

如果对默认主题不满意，可自定义`prettify.css`。我在prettify的主题`desert`的基础上自定义的`prettify.css`完整如下。

	pre.prettyprint { display: block; background-color: #333;font-size:14px; }
	pre .nocode { background-color: none; color: #000 }
	pre .str { color: #ffa0a0 } /* string  - pink */
	pre .kwd { color: #f0e68c; font-weight: bold }
	pre .com { color: #87ceeb } /* comment - skyblue */
	pre .typ { color: #98fb98 } /* type    - lightgreen */
	pre .lit { color: #cd5c5c } /* literal - darkred */
	pre .pun { color: #fff }    /* punctuation */
	pre .pln { color: #fff }    /* plaintext */
	pre .tag { color: #f0e68c; font-weight: bold } /* html/xml tag    - lightyellow */
	pre .atn { color: #bdb76b; font-weight: bold } /* attribute name  - khaki */
	pre .atv { color: #ffa0a0 } /* attribute value - pink */
	pre .dec { color: #98fb98 } /* decimal         - lightgreen */
	
	/* Specify class=linenums on a pre to get line numbering */
	ol.linenums { margin-top: 5px; margin-bottom: 5px; padding-left:5px;color: #AEAEAE } /* IE indents via margin-left */
	
	li.L0, li.L1, li.L2, li.L3, li.L4, li.L5, li.L6, li.L7, li.L8, li.L9 { list-style-type: decimal;}
	/* Alternate shading for lines */
	li.L1,li.L3,li.L5,li.L7,li.L9 { }
	
	@media print {
	  pre.prettyprint { background-color: none }
	  pre .str, code .str { color: #060 }
	  pre .kwd, code .kwd { color: #006; font-weight: bold }
	  pre .com, code .com { color: #600; font-style: italic }
	  pre .typ, code .typ { color: #404; font-weight: bold }
	  pre .lit, code .lit { color: #044 }
	  pre .pun, code .pun { color: #440 }
	  pre .pln, code .pln { color: #000 }
	  pre .tag, code .tag { color: #006; font-weight: bold }
	  pre .atn, code .atn { color: #404 }
	  pre .atv, code .atv { color: #060 }
	}
	
	
	.prettyprint.linenums ol li, pre.prettyprint.linenums ol li {
		padding-left: 12px;
		color: #bebec5;
		line-height: 20px;
		margin-left: 0;
	}
	.prettyprint.linenums, pre.prettyprint.linenums {
		-webkit-box-shadow: inset 40px 0 0 #3D4C53,inset 41px 0 0 #464741;
		-moz-box-shadow: inset 40px 0 0 #3D4C53,inset 41px 0 0 #464741;
		box-shadow: inset 40px 0 0 #3D4C53,inset 41px 0 0 #464741;
	}
	
	p > code{
		margin: 0 3px;
		background: #ddd;
		border: 1px solid #ccc;
		border-radius: 2px;
		color: rgba(0,0,0,0.6);
		font-family: Menlo, Monaco, "Andale Mono", "lucida console", "Courier New", monospace;
	}
	
	a > code{
		margin: 0 3px;
		background: #ddd;
		border: 1px solid #ccc;
		border-radius: 2px;
		color: #2a7ae2;
	}