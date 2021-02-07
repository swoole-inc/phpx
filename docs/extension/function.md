# 内置函数

本文主要介绍`PHP-X`内置函数的使用，在`PHP`扩展开发中，会经常用到这些内置函数，`PHP-X`的封装，使得调用这些函数像`PHP`代码一样简单。

## echo

在扩展中需要输出一些内容，可以使用`echo`函数。`echo`的使用方法与`C`语言的`printf`是完全一致的。具体请参考`printf`相关文章。

- 在命令行环境（cli），`echo`会打印屏幕
- 在`php-fpm`或`apache`中，`echo`会输出内容到浏览器客户端

```cpp
PHPX_FUNCTION(cpp_test)
{
    echo("a=%d, b=%f, c=%s.\n", args[0].toInt(), args[1].toFloat(), args[2].toCString());
}
```

## var_dump

开发调试`PHP`程序时，经常需要打印一些变量的值。`PHP`提供了`var_dump`函数来打印变量。在`PHP-X`中也可以使用`var_dump`，这个函数接受一个`Variant`对象。

```cpp
PHPX_FUNCTION(cpp_test)
{
    var_dump(args[0]);
}
```

## include

包含`PHP`文件。注意文件不存在会抛出致命错误。正确加载后，此`PHP`文件中的代码将被执行。可以使用`include`在扩展中引入`PHP`代码实现的类和函数。

```cpp
PHPX_FUNCTION(cpp_test)
{
    include("/data/php/library/Autoloader.php");
}
```

## error

打印`PHP`错误日志，相当于`PHP`的`trigger_error`函数。此函数与`echo`很相似，唯一不同的插入了第一个参数，来接受错误等级，如`E_ERROR`或`E_WARNING`。

```cpp
PHPX_FUNCTION(cpp_test)
{
    error(E_ERROR, "error: a=%d, b=%f, c=%s.\n", args[0].toInt(), args[1].toFloat(), args[2].toCString());
}
```

## constant

获取常量的值。此函数可以用于获取`define`定义的常量以及`const`定义的类常量。

```cpp
PHPX_FUNCTION(cpp_test)
{
    auto a = constant("PHP_VERSION");
    auto b = constant("PDO::VERSION");
    var_dump(a);
    var_dump(b);
}
```

## global

获取全局变量的值。包括`PHP`的超全局变量和其他`PHP`代码使用`global`关键词声明的全局变量。

```cpp
PHPX_FUNCTION(cpp_test)
{
    //相当于 $_GET
    auto a = global("_GET");
    //相当于 global $config
    auto b = global("config");
    var_dump(a);
    var_dump(b);
}
```