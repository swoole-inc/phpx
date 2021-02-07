# 自动构建

`PHP-X 2.0`提供了一个全新的命令行工具：`phpx`，可用于编译、打包`phpx`工程。

## 创建工程

在当前目录下创建一个新工程。

```shell
#创建二进制可执行工程
phpx create project_name --bin
#创建PHP扩展工程
phpx create project_name --ext
```

## 目录结构

* `include`：存放`.h`的头文件
* `src`：存放`.cc`或`.cpp`源文件
* `lib`：存放编译好的扩展，或者第三方的`.a`或`.so`二进制库文件
* `bin`：存放编译好的二进制程序

## 直接运行

可以直接运行`phpx`工程，`phpx`会自动编译`c++`源代码，生成二进制文件，并执行。注意程序中必须存在`main`函数否则将无法运行。

!> 只能用于`bin`模式的工程

```shell
phpx run
```

## 编译工程

* `--debug`：编译时使用`-O0`不进行任何优化，默认为关闭`debug`使用`-O2`优化
* `--verbose`：显示详细的编译参数

```shell
phpx build -v
```

## 安装

```shell
phpx install --prefix=/opt
phpx install
```

该指令会检查是否已`build`，未`build`时会自动构建。`--prefix`参数仅对`bin`模式有效。

* `bin`模式：将可执行文件安装到`/usr/local/bin`目录，使用`--prefix`参数可以指定路径，如`/opt`，将会安装到`/opt/bin`目录中
* `ext`模式：将编译好的扩展安装到`PHP`扩展目录中，需要用户自行编辑`php.ini`启用此扩展
