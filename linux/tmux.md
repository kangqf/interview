## tmux

### tmuxinator 
``` 
tmuxinator new projectName
tmuxinator open projectName
tmuxinator projectName
```

### tmux 配置
配置文件参考[这里](https://github.com/kangqf/config/blob/ubuntu/tmux.conf)

由上面更改的快捷键主要有：
```
1. Prefix: C-a           前缀键
2. Prefix + H/J/K/L      调整窗格大小
3. Prefix + C-s          设置/取消 多窗格同步
4. Prefix + |/-          水平垂直分割窗格
5. Prefix + h/j/k/l      跳到上下左右的窗格
6. Prefix + C-h/l        到左右边的窗口
7. Prefix + r            重新载入配置文件
```

tmux 自带的一些快捷键（省略Prefix）

* 窗格相关
``` 
1. 方向键         切换到上下左右的窗格
2. Ctrl+方向键    调节当前窗格的大小
3. 空格           调节当前窗格的布局
4. o              在不同的窗格之间跳转
5. x              关闭当前窗格
6. q              显示窗格的编号
7. z              休眠其它窗格，只显示当前窗格
8. !              将单前窗格分离到一个新的窗口中
9. {              当前窗格与前一窗格互换
10. }             当前窗格与后一窗格互换
```

* 窗口相关
``` 
c               创建一个窗口
数字            跳到对应数字的窗口
,               重命名当前窗口
n               下一个窗口
p               前一个窗口
w               显示窗口列表
f               查找包含指定字符的窗口
&               关闭当前窗口
'               跳到指定下标的窗口
.               重设当前窗口的下标
```

* 会话相关
```
s              显示会话列表
d              断开当前会话的关联
D              断开指定会话的关联
:              进入命令行模式
t              显示时钟
?              显示快捷键列表
```

* 命令行相关
```
1. tmux new-session -s [name]   新建名为name的会话
2. tmux a                       关联最近的一个会话
3. tmux a -t [name]             关联名为name 的会话
4. tmux kill-session -t [name]  关闭名为name 的会话
```