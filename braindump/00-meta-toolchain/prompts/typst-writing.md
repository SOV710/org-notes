file format: 使用 typst format，format 如下 (没有三横线分割):

---
## 基础结构
- `=` 标题（= 一级，== 二级，=== 三级）
- `*粗体*` 粗体
- `_斜体_` 斜体
- `` `代码` `` 行内代码
- ` ``` ` 代码块
- `-` 无序列表
- `+` / `1.` 有序列表
- `---` 分隔线
- `\` 换行
- `//` 注释

## 函数与设置
- `#let` 定义变量/函数
- `#set` 设置样式规则
- `#show` 修改显示规则
- `#import` 导入模块
- `#include` 包含文件

## 常用函数
- `#text()` 文本样式
- `#box()` 行内容器
- `#block()` 块级容器
- `#page()` 页面设置
- `#par()` 段落设置
- `#align()` 对齐
- `#grid()` / `#table()` 网格/表格
- `#image()` 图片
- `#figure()` 图表
- `#heading()` 标题
- `#link()` 链接
- `#outline()` 目录
- `#pagebreak()` 分页
---

以下是我的 typst 模板，你可以参考这个模板进行实践

```typst
// 通用文档模板 (借鉴 Typst 官方简洁模板)
#let simple-doc(
  title: "",
  author: "",
  date: none,
  toc: false,
  body
) = {
  // 基础页面设置
  set page(
    paper: "a4",
    margin: (x: 2cm, y: 2.5cm),
    numbering: "1",
    number-align: center,
  )
  
  // 文本设置
  set text(
    font: ("Libertinus Serif", "Noto Serif CJK SC"),
    size: 11pt,
    lang: "zh",
    region: "cn",
  )
  
  set par(
    justify: true,
    leading: 0.65em,
    first-line-indent: 2em,
  )
  
  // 标题样式
  set heading(numbering: "1.1")
  
  show heading.where(level: 1): it => {
    set text(size: 20pt, weight: "bold")
    set block(above: 1.5em, below: 1em)
    pagebreak(weak: true)
    it
  }
  
  show heading.where(level: 2): it => {
    set text(size: 16pt, weight: "bold")
    set block(above: 1.2em, below: 0.8em)
    it
  }
  
  show heading.where(level: 3): it => {
    set text(size: 13pt, weight: "bold")
    set block(above: 1em, below: 0.6em)
    it
  }
  
  // 列表样式
  set list(indent: 1em, body-indent: 0.5em)
  set enum(indent: 1em, body-indent: 0.5em)
  
  // 引用块样式
  show quote: it => {
    set text(style: "italic")
    set block(inset: (left: 1em))
    pad(left: 0.5em, block(
      width: 100%,
      fill: luma(250),
      stroke: (left: 2pt + gray),
      inset: 1em,
      it.body
    ))
  }
  
  // 表格样式
  set table(
    stroke: (x, y) => (
      top: if y <= 1 { 1pt } else { 0.5pt },
      bottom: 1pt,
    ),
    fill: (x, y) => if y == 0 { luma(240) },
    inset: 8pt,
  )
  
  // 文档标题
  if title != "" [
    #set par(first-line-indent: 0em)
    #align(center)[
      #block(text(size: 24pt, weight: "bold")[#title])
      
      #v(0.5em)
      
      #if author != "" [
        #text(size: 12pt)[#author]
      ]
      
      #if date != none [
        #text(size: 11pt)[
          #v(0.3em)
          #date
        ]
      ] else if author != "" [
        #text(size: 11pt)[
          #v(0.3em)
          #datetime.today().display()
        ]
      ]
    ]
    
    #v(1.5em)
  ]
  
  // 可选目录
  if toc [
    #set par(first-line-indent: 0em)
    #outline(
      title: [目录],
      indent: 1em,
      depth: 3,
    )
    #v(2em)
  ]
  
  // 正文内容
  body
}

// 使用示例
#show: simple-doc.with(
  title: "我的笔记",
  author: "SOV710",
  toc: true,
)

= 第一章：开始

这是一个通用的文档模板，适合各种日常写作需求。

== 列表示例

基本特性：

- 简洁的排版
- 支持中英文混排
- 自动编号和目录

步骤说明：

1. 准备内容
2. 选择合适的模板
3. 开始写作

== 引用示例

> 这是一段引用文字。引用块会有特殊的样式，使其在文档中更加突出。

== 表格示例

#figure(
  table(
    columns: 3,
    [*项目*], [*描述*], [*状态*],
    [任务1], [完成设计], [✓],
    [任务2], [编写文档], [进行中],
    [任务3], [代码审查], [待开始],
  ),
  caption: [任务清单]
)

= 第二章：进阶

== 代码示例

行内代码：使用 `print()` 函数输出内容。

代码块：

```python
def hello_world():
    print("Hello, Typst!")
    
if __name__ == "__main__":
    hello_world()
```

== 数学公式

行内公式：$x^2 + y^2 = r^2$

独立公式：

$ integral_0^infinity e^(-x^2) dif x = sqrt(pi)/2 $
```

你需要直接输出 typst 代码块，不需要输出其余任何东西
