---
layout: post
title: "Latex模板"
date: 2013-01-22 22:11
comments: true
categories: [计算机, latex]
---
<!--more-->
``` tex template.tex
\documentclass[UTF8,a4paper,winfonts,cs4size,oneside]{ctexart}
\defaultfontfeatures{Mapping=tex-text}
\usepackage{pdfpages}
\usepackage{ifpdf}
\usepackage{subfigure}
\usepackage{listings}
\usepackage[%
  pdfstartview=FitH,%
  CJKbookmarks=true,%
  bookmarks=true,%
  bookmarksnumbered=true,%
  bookmarksopen=true,%
  colorlinks=true,%
  citecolor=blue,%
  linkcolor=blue,%
  anchorcolor=green,%
  urlcolor=blue%
]{hyperref}
\usepackage{graphicx}
\usepackage{url}
\usepackage{texnames}
\topmargin -0.5 true cm
\oddsidemargin 0 true cm
\evensidemargin 0 true cm
\textheight 23.5 true cm
\textwidth 16 true cm
\usepackage{lastpage}
\usepackage{fancyhdr}
\pagestyle{fancy}
\renewcommand{\headrulewidth}{0.4pt}
%\renewcommand{\footrulewidth}{0.4pt}
\begin{document}
\lstset{numbers=left,
numberstyle=\tiny,
keywordstyle=\color{blue!70}, commentstyle=\color{red!50!green!50!blue!50},
frame=shadowbox,
rulesepcolor=\color{red!20!green!20!blue!20}}
\lstset{language=C++}%这条命令可以让LaTeX排版时将C++键字突出显示
\lstset{breaklines}%这条命令可以让LaTeX自动将长的代码行换行排版
\lstset{extendedchars=false}%这一条命令可以解决代码跨页时，章节标题，页眉等汉字不显示的问题
\CTEXsetup[name={第, 章}]{section}
\CTEXsetup[number={\chinese{section}}]{section}
 
\end{document}
```
