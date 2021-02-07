# 安装

## 下载源码

* Gitee(码云)：[https://gitee.com/swoole/PHP-X](https://gitee.com/swoole/PHP-X)
* GitHub：[https://github.com/swoole/phpx](https://github.com/swoole/phpx)

## 环境依赖

* `PHP-7.0`或更高版本
* `g++-4.8`或更高版本或`clang++`，必须支持`C++11`标准
* 仅支持 `Linux/MacOS/Windows` 3种平台
* 仅支持 `x86-64` 架构
* `cmake-2.8`或更高版本

## 编译安装

```shell
cmake .
make -j 4
sudo make install
sudo ldconfig
```

> 可以通过`cmake -DPHP_CONFIG_DIR=/opt/php/bin`指定`php-config`的路径  
> 请检查`libphpx.so`是否存在于`ldconfig -p`中

!> MacOS 编译时需要修改`Makefile`，为g++/clang++增加`-undefined dynamic_lookup`编译参数

## Windows

### 准备

* 需要 PHP 源码
* PHP 5.4：Visual C++ 9.0 (Visual Studio 2008 or Visual C++ 2008)
* PHP 5.5 or 5.6：Visual C++ 11.0 (Visual Studio 2012)
* PHP 7.0+： Visual C++ 14.0 (Visual Studio 2015)
* .NET Framework 3.5

> PHP7也可用`VS2017`

### 编译PHP

!> 参考 [官网WIKI页面](https://wiki.php.net/internals/windows/stepbystepbuild)

* 创建`php-sdk`目录，并进入此目录
* 下载`php-sdk`，地址 [http://windows.php.net/downloads/php-sdk](http://windows.php.net/downloads/php-sdk) ，并解压放到`php-sdk`目录中，正常执行后`php-sdk`目录下应该有`bin`、`share`、`script` 3个子目录
* 创建`php-dev`目录，下载依赖包，地址 [http://windows.php.net/downloads/php-sdk](http://windows.php.net/downloads/php-sdk)

!> 依赖包，必须和`VS`、`PHP`版本完全一致，如 `PHP7.0` + `VS2015`，需要下载`deps-7.0-vc14-x64.7z`

