## 常用的linux系统配置的命令

1. pppoe 拨号上网配置
    ``` sh
    # 配置pppoe拨号上网
    sudo pppoeconf
    # 拨号
    sudo pon dsl-provider
    # 断开连接
    sudo poff
    # 查看日志
    plog
    ```
2. 修复apt broken

    ``` sh
    # 错误提示为： You might want to run 'apt --fix-broken install' to correct these.
    sudo apt --fix-broken install
    sudo apt-get update
    sudo apt-get upgrade
    ```
3. autojump
    ``` sh
    autojump 有一个文件（里面存放着所有你去过的目录），你可以根据自己的情况，修改每一个路径权重(权重是根据你使用的频率决定)。
    所以在使用 j dir 之前必须保证你曾经 cd 到过指定的目录
    ```
