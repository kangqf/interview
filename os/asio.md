## ASIO

### ASIO 使用套路 

1. 创建一个IO Service
2. 创建一个 EndPoint
3. 将IO Service 与 Endpoint 用 Socket 连接起来

方法是 从 IO Service 创建一个sock，然后sock connect（async_connect()）到EndPoint 以及对应的 hander

4. IOService run