---
title: Create your personal blog with Hexo and GitHub
date: 2020-01-23 20:12:30
tags:  
- Hexo
- GitHub
categories:
- [Hexo]
---
## Install Hexo
1. Install `Git` and `Node`
2. Intall Hexo `$ npm install -g hexo-cli`

## Set up your blog files
Once Hexo is installed, run the following commands to initialize Hexo in the target `<folder>`
```bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```
## Create a Repo on GitHub
Create a repo on GitHub with repo name as `[your_github_username].github.io`.

## Deploy 
1.  Install deployer tool `npm install hexo-deployer-git --save`
2.  Add the following configurations to **_config.yml** in the folder you created with `hexo init` just now.
```
deploy:
  type: git
  repo: https://github.com/<username>/<username.github.io>
  branch: master
  ```
3.  Run `hexo clean && hexo deploy`
4.  Check the webpage at `username.github.io`

## Host your Source files
Note that if you follow the above steps to deploy your blog, the `master` branch will only host the **published files**. There is another branch `gh-pages` hosting your **source files**.  You may want to host the source files with **Pull Request** approach. One way to do that is clone the repo`<username.github.io>` to locale and copy the `.git` folder to `<my-blog-folder>` created by `hexo init`, then checkout another branch (such as with name `hexo-dev`) as the working branch and create PR to branch to `gh-pages` branch if there is any change.

## Links
[Hexo Documentation](https://hexo.io/docs/index.html)