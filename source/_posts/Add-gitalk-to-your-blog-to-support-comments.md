---
title: Add gitalk to your blog to support comments
date: 2020-01-23 22:13:58
tags:  
- Hexo
- gitalk
- blog comments
categories:
- [Hexo]
---
[gitalk](https://github.com/gitalk/gitalk) is a modern comment component based on GitHub Issue and Preact. You can integrate gitalk to your blog to support `comments`.

## Create a repo on GitHub to store comments
For example, I create a repo named **blog-comments**. 

Remember to enable `issues` in the setting page of this repo.

## Register an OAuth application on GitHub
This application will be used by gitalk.

Remember to enter you blog url in item **Homepage URL** and **AUthorization callback URL**. 

Copy **Client ID** and **Client Secret**. You can also check your application at https://github.com/settings/developers.

## Configure Hexo 
### Create file  `comment.ejs` in folder `themes/me/layout_partial`
```
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.css">
<script src="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.min.js"></script>
<div id="gitalk-container"></div>
<script type="text/javascript">
    var gitalk = new Gitalk({
        clientID: '<%= theme.gitalk.clientID %>',
        clientSecret: '<%= theme.gitalk.clientSecret %>',
        id: window.location.pathname,
        repo: '<%= theme.gitalk.repo %>',
        owner: '<%= theme.gitalk.owner %>',
        admin: '<%= theme.gitalk.admin %>'
    })
    gitalk.render('gitalk-container')
</script>
```
Note if your theme differs mine, you may need to find proper location to store `comment.ejs`.
### Embed `comment.js` in your `post.ejs` file
```
 <%# gitalk comments %>
    <%if(theme.gitalk.enable){%>
        <%- partial('_partial/comment') %>
    <%}%>
    ```
### Configure gitalk parameters in `_config.yml`
```
# gittalk comments
gitalk:
  enable: true
  owner: your-github-username # github username
  repo: blog-comments # repo name to store comments
  admin: your-github-username # owner of repo above
  clientID: your-client-id  # application id and secret
  clientSecret: your-client-secret
```

After publish, you need to login with your GitHub account at the bottom of the current post, then you'll find you (and other users) can comment your blog.  All comments will be stored under `issues` of your branch `blog-comments`.