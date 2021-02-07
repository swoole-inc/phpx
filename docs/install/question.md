# 常见问题

## undefined symbol: json_globals

报错：

```bash
/usr/local/lib/libphpx.so: undefined symbol: json_globals in Unknown on line 0
```

出现这个问题，主要原因是缺少`json`库。`json`扩展未加载，或者加载顺序存在问题。如下列配置：

```ini
extension=test.so
extension=json.so
```

这里你的扩展`test.so`在`json`之前加载，而`phpx`依赖`json`，因此会出现`undefined symbol`。重新调整配置，先加载`json.so`即可解决问题。

```ini
extension=json.so
extension=test.so
```

## libphpx.so: cannot open shared object file: No such file or directory

* 检查系统`lib`目录中是否存在`libphpx.so`，包括`/lib`、`/usr/lib`、`/usr/local/lib`等位置
* 检查`/usr/local/lib`目录是否加入了`ld.so.conf`中，位置在`/etc/ld.so.conf`或者`/etc/ld.so.conf.d/*`
* 执行`sudo ldconfig`