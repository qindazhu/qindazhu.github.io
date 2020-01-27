---
title: tmux Cheet Sheet
date: 2020-01-27 14:26:16
tags:  
- Linux
- tmux
- Cheet Sheet
categories:
- [Cheet Sheet]
---
## 安装
- 有root权限
`sudo apt-get install tmux`
- 无root权限

## 使用
- `tmux`进入, `exit`退出
-  `tmux new -s <session-name>`启动一个会话，不带-t参数默认是从0开始命名
-  `Ctrl+b d`或者`tmux detach` detach当前会话，这里会退出tmux窗口，但里面的进程依然会在后台运行
-  `tmux ls`列出所有会话
-  `tmux attach -t 0`或者`tmux attach -t <session-name>`接入已有会话
-  `tmux kill-session -t  0/<session-name>`杀死会话
-  `tmux switch -t 0/<session-name>`切换会话
-  `tmux split-window` 上下划分窗口
-  `tmux split-window -h`左右划分窗口
-  `Ctrl+b 箭头`切换窗口