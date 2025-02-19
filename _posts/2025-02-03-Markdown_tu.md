---
layout:       post
title:        "Markdown极简入门教程"
subtitle:     "Markdown minimalist introductory tutorial"
date:         2025-02-03 00:00:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - it
---

# Markdown 基础教程

## 什么是 Markdown？
一种轻量级标记语言，用简单符号即可排版文档  
✅ 易读易写 ✅ 兼容纯文本 ✅ 支持转换为HTML/PDF等格式  
常用于写文档、笔记、博客、README文件等

---

## 基础语法

### 1. 标题
```markdown
# 一级标题
## 二级标题
### 三级标题
（最多支持六级标题，每级多一个 # 号）
```

### 2. 文字格式
```markdown
**加粗文本**  
*斜体文本*  
~~删除线~~  
==高亮==（部分编辑器支持）
```

### 3. 列表
#### 无序列表：
```markdown
- 项目1
- 项目2
  - 子项目（缩进2空格）
```
#### 有序列表：
```markdown
1. 第一项
2. 第二项
```
### 4. 链接与图片
```markdown
[链接文字](https://example.com)
![图片描述](图片路径.jpg)
```

### 5. 引用
```markdown
> 引用内容
> 多行引用
```
### 6. 分割线
```markdown
---
或
***
（三个及以上短横线/星号）
```

## 进阶语法

### 1. 代码

```markdown
行内代码：`代码`
代码块：
```编程语言
// 示例（如python/js等）
print("Hello World")
```
```

```cpp
#include <iostream>       
#include <string>         
// std::cout
 // std::string
 int main (){
 std::string s("hello world");
 std::string s2 = s.substr(0,6);
 std::cout<<s2<<"  "<<s2.size()<<std::endl;
 std::cout<<s+s2<<std::endl;
 }
```

### 2. 表格
```markdown
| 标题1 | 标题2 |
|-------|-------|
| 内容1 | 内容2 |
| 左对齐 | 右对齐 |
（冒号决定对齐方式：:---左对齐，---:右对齐，:---:居中）
```

### 3. 任务列表
```markdown
- [x] 已完成任务
- [ ] 未完成任务
```

### 4. 脚注
```markdown
这是需要注释的内容[^1]
[^1]: 这里是脚注说明
```

## 工具推荐

编辑器：VS Code、Typora、Mark Text

在线工具：StackEdit、Markdown Here、

支持平台：GitHub、Notion
