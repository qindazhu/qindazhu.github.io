---
title: 自定义Hexo Theme
date: 2020-01-23 19:59:20
tags:  
- Hexo
- Hexo Theme
- Chic
- Theme Next
categories:
- [Hexo]
---
[Next](https://github.com/theme-next/hexo-theme-next)是现在大家用的比较多的Hexo主题，你也可以在[Hexo 官网](https://hexo.io/themes/)去寻找自己喜欢的主题。我个人因为喜欢比较简洁的风格，所以选择了[Chic](https://github.com/Siricee/hexo-theme-Chic)这个主题。

基本上主题的安装方式都一样，下载或者拷贝整个主题文件夹到`<your-hexo-folder>/themes`文件夹下，然后修改**_config.yml**文件中的**theme**字段为你下载的主题文件夹名。以下就以安装并定制Chic主题为例。

## 下载并安装 Chic
```bash
$ cd your-hexo-blog-folder
$ git clone https://github.com/Siricee/hexo-theme-Chic.git themes/me
```
然后修改**_config.yml**文件中的**theme**字段为me。完了记得
```bash
$ rm -rf themes/me /.git
$ git rm --cached .
```
把Chic下的git记录删除，要不然每次提交用`git submodule`比较麻烦，个人项目没必要。


## 添加动态背景
个人比较喜欢[canvas-nest]( https://github.com/theme-next/theme-next-canvas-nest)这个动效，添加方式如下：
1. 从上述链接处下载`canvas-nest.min.js`（如果不想在手机上显示，可以使用`canvas-nest-nomobile.min.js`文件）文件放在`themes/me/source/js/`下
2. 在**_config.yml**里添加canvas_nest的配置项
```
canvas_nest:
  enable: true
  onmobile: false # Display on mobile or not
  src: js/canvas-nest.min.js
  color: "0,0,0" # RGB values, use `,` to separate
  opacity: 0.5 # The opacity of line: 0~1
  zIndex: -1 # z-index property of the background
  count: 80 # The number of lines
```
3. 在`thems/me/layout/layout.egj`添加canvas-nest的调用代码
{% codeblock lang:egj%}
<!DOCTYPE html>
<html lang="<%= config.language %>">
<head>
    <%- partial('_partial/head',{cache: true}) %>
</head>
<body>
    <div class="wrapper">
        <%- partial('_partial/header',{cache: true}) %>
        <div class="main">
            <%- body %>
        </div>
        <%- partial('_partial/footer',{cache: true}) %>
    </div>
    
    <%#  注意接下来这一段------canvas_nest %>
    <% if(theme.canvas_nest.enable===true){ %>
    <script src="<%- url_for(theme.canvas_nest.src) %>" color="<%- theme.canvas_nest.color %>" opacity="<%- theme.canvas_nest.opacity %>" zIndex="<%- theme.canvas_nest.zIndex %>" count="<%- theme.canvas_nest.count %>" ></script>
    <% } %>

</body>
</html>
{% endcodeblock %}