---
layout:       post
title:        "Pandoc将markdown转pdf"
subtitle:     "Pandoc converts markdown to pdf"
date:         2025-03-14 23:30:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - it
---

询问ChatGPT、Grok和Deepseek等AI工具十几个小时，来来回回折腾，也解决不了用Pandoc将markdown转pdf总是出错的问题。

最后还是谷歌搜索中文文章，才解决了，被chatGPT等折腾死了，不断换各种方法试验，也没用，反反复复地重复错误。

其实就是简单一个命令：

```bash
pandoc test.md -o output.pdf --pdf-engine=xelatex -V CJKmainfont="微软雅黑" -V mainfont="Times New Roman"
```
赶紧记下来，不然以后又不知道了！
