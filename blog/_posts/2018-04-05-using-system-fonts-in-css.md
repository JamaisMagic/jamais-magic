---
title: "CSS使用系统字体"
description: "CSS使用系统字体"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  - css
tags:
  - css
  - font-family
  - system-ui
---

今天看到的一片文章，使用系统字体，记录一下，直接引用文章内容。

> In many cases, you’ll also have to take backward compatibility into account — not every visitor has the latest & greatest browser. This CSS should cover all the bases:

```css
font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
    "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans",
    "Droid Sans", "Helvetica Neue", sans-serif;
```

### 参考资料

> <site><a target="_blank" href="https://furbo.org/2018/03/28/system-fonts-in-css/?utm_source=CSS-Weekly&utm_campaign=Issue-309&utm_medium=email">System Fonts in CSS</a></site>
