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

如果要将一个文件夹所有markdown文件都拼接起来转化为一个pdf文件输出，可以在windows的PowerShell里用下面的Pandoc命令：
```bash
pandoc (Get-ChildItem -Path . -Filter "*.md" | Sort-Object Name | ForEach-Object { '"' + $_.FullName + '"' }) `
  -o output.pdf --pdf-engine=xelatex `
  --toc --number-sections `
  -V toc-title="目录" `
  -V documentclass=report `
  -V geometry=a4paper `
  -V geometry=margin=1in `
  -V fontsize=12pt `
  -V CJKmainfont="Microsoft YaHei" `
  -V mainfont="Times New Roman" `
  --template=template.tex
```
其中的template.tex内容可以如下：
```
\documentclass{report}
\usepackage{fancyhdr}
\pagestyle{fancy}
\fancyhf{}
\rfoot{\thepage} % 右下角页码
\begin{document}
$body$
\end{document}
```

要打开Windows系统的Powershell，可以用win键+R，输入：Powershell。
