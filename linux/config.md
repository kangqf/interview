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
2. 修复apt boken
    ``` sh
    # 错误提示为： You might want to run 'apt --fix-broken install' to correct these.
    sudo apt --fix-broken install
    sudo apt-get update
    sudo apt-get upgrade
    ```
3. 