# 启动器

可以使用`phpx`启动服务。

## 普通 Web 项目

基于`Swoole`和`php-cli-server`实现的多进程`Web`服务器，运行模式与`php-fpm`完全一致，短生命周期，支持热重载，修改`php`代码立即生效。

进程管理部分基于`Swoole\Process\Pool`实现。

```shell
phpx start --web --host=0.0.0.0 --port=9001 --count=100
```

* `--host`：监听的地址，默认为`0.0.0.0`表示监听所有地址
* `--port`：监听的端口
* `--count`：启动的进程数量
* `--daemon`：设置为守护进程