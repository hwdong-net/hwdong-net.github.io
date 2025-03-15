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

昨天，我满怀期待地召唤了ChatGPT、Grok和Deepseek这些AI小助手，希望它们能化身技术导师，指引我将一本书的众多Markdown文件华丽变身为一个精美的PDF文档。它们齐刷刷地推荐我使用Pandoc这个神器，还耐心地一步步教我如何操作，颇有种“指点迷津”的架势。

然而，理想很丰满，现实却骨感得让人抓狂。转换过程频频出错，仿佛掉进了一个无底的bug深渊。我不甘心，把错误日志一股脑儿丢给这些AI“军师”，它们倒也尽职尽责，接连抛出各种新方案。可我跟着改了又改，试了又试，折腾了整整十几个小时，从清晨到深夜11点，连饭都顾不上吃一口，肚子咕咕叫得像在开演唱会，结果呢？Pandoc还是跟我较劲，Markdown转PDF的任务愣是没完成，错误提示像个甩不掉的跟屁虫，反复蹦出来挑衅。

我几乎要崩溃了，心想这些AI大佬怎么就这么不给力呢？方案换了一个又一个，实验做了一轮又一轮，简直像在跟一台坏掉的复读机较劲。到最后，我实在受够了它们的“车轱辘话”，果断抛弃AI，转而向万能的谷歌求救。在一番中文文章的“考古”后，我终于挖到了救命稻草，问题迎刃而解。回想被ChatGPT等AI折磨得死去活来的过程，真是心累到想掀桌——明明是简单的事，偏偏被它们搞得像解世界级难题一样。

真相其实就藏在这一行命令里，简单得让人想哭：

bash

Collapse

Wrap

Copy
pandoc test.md -o output.pdf --pdf-engine=xelatex -V CJKmainfont="微软雅黑" -V mainfont="Times New Roman"
我赶紧把它记下来，刻在脑子里，生怕哪天又忘了，再次陷入AI的“迷魂阵”。

后来我还琢磨着，如果要把一个文件夹里所有的Markdown文件拼接起来，合成一个完整的PDF，又该怎么办呢？在Windows的PowerShell里，我捣鼓出了下面这个“终极武器”：

bash

Collapse

Wrap

Copy
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
为了让输出的PDF更专业，我还顺手搭配了一个简单的template.tex模板：

tex

Collapse

Wrap

Copy
\documentclass{report}
\usepackage{fancyhdr}
\pagestyle{fancy}
\fancyhf{}
\rfoot{\thepage} % 右下角页码
\begin{document}
$body$
\end{document}
对了，如果你不知道怎么打开PowerShell，别慌，只需按下Win键+R，输入“Powershell”，就能召唤出这个命令行神器。

这番折腾下来，我算是明白了：AI固然聪明，但关键时刻，还是得靠自己和谷歌这对“老搭档”救场啊！
