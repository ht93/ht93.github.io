---
layout: post
title: Sublime Text Markdown Support
---

1. Install [MarkdownEditing](https://packagecontrol.io/packages/MarkdownEditing), [Markdown Preview](https://packagecontrol.io/packages/Markdown%20Preview) and [LiveReload](https://packagecontrol.io/packages/LiveReload) by `Shift+Ctrl+P -> Install package`.
2. Open a .md file and set Syntax to `Markdown-Markdown` in right bottom corner of Sublime text.
3. Open `Preferences -> Settings - Syntax Specific` add following line to `Markdown.sublime-settings`:   
```"color_scheme": "Packages/MarkdownEditing/MarkdownEditor-Dark.tmTheme"``` 
4. Use the `Shift+Ctrl+P -> Markdown Preview: Preview in Browser -> Github` to preview the output.