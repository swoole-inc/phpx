# Extension

`Extension`类是PHP扩展的管理类，扩展入口函数需要返回`Extension`对象的指针。可以使用`PHPX_EXTENSION`宏实现扩展的入口函数。

```cpp
PHPX_EXTENSION()
{
    Extension *ext = new Extension("test", "0.0.1");

    return ext;
}
```

构造方法接受2个参数。

* 第一个参数为扩展的名称，使用`php -m`查看
* 第二个参数为扩展的版本号，使用`php --ri $ext_name`查看

## 回调函数

一个PHP扩展可以设置4个回调函数，分别是：

* `MINIT` 扩展初始化时调用
* `MSHUTDOWN` 扩展销毁时调用
* `RINIT` 请求到来前调用
* `RSHUTDOWN` 请求结束后调用

在`PHP-X`的`Extension`对象上设置属性为C++匿名函数来注册扩展回调函数。

```cpp
extension->onStart = [extension] () {
	//onStart执行的代码
};
```

### `PHP-X`扩展函数对应关系

* `MINIT`  = `onStart`
* `RINIT`  = `onBeforeRequest`
* `MSHUTDOWN`  = `onShutdown`
* `RSHUTDOWN`  = `onAfterRequest`

## 注册函数

扩展内可以调用`registerFunction`来注册内置函数到`PHP`中。需要注意`Zend`有限制，必须在`Extension`对象创建时注册函数。这与类的注册不同，扩展类必须在`onStart`回调中注册。

```cpp
PHPX_EXTENSION()
{
    Extension *ext = new Extension("test", "0.0.1");
	ext->registerFunction(PHPX_FN(test_func1));
	ext->registerFunction(PHPX_FN(test_func2));
    return ext;
}
```

## 注册类

`PHP-X`扩展中可以调用`Extension::registerClass`方法来注册PHP内置类。可以使用`PHPX_ME`宏实现参数简化，使用方法：

```cpp
Class c = new Class("CppClass");
/**
* 注册构造方法
*/
c->addMethod("__construct", CppClass_construct, CONSTRUCT);
/**
* 普通方法
*/
c->addMethod(PHPX_ME(CppClass, test1));
/**
* 静态方法
*/
c->addMethod(PHPX_ME(CppClass, test2), STATIC);
/**
* 添加默认属性
*/
c->addProperty("name", 1234);
/**
* 添加常量
*/
c->addConstant("VERSION", "1.9.0");
/**
* 注册到ZendVM中
*/
PHP::registerClass(c);
```

* `addMethod`添加类的方法，接受3个参数，分别是方法名称、函数、方法的修饰符，可以是`PUBLIC`、`PROTECTED`、`PRIVATE`、`STATIC`、`CONSTRUCT`、`DESTRUCT`等
* `addProperty`添加类的属性
* `addConstant`添加类的常量
* 调用`extends`方法设置继承的父类
* 调用`implements`方法设置类实现的接口
* 添加完相关的方法、属性、常量后，调用`Extension::registerClass`函数将类`CppClass`注册到`ZendVM`

> 如果要添加的类、方法、属性、常量已存在，会返回false，添加失败  
> 类激活后不能再继续添加方法或进行其他修改操作

### 方法函数格式

类的方法必须以下列方式定义，否则将编译失败。

```cpp
void CppClass_construct(Object &_this, Args &args, Variant &retval)
{
    printf("%s _construct\n", _this.getClassName().c_str());
    Array arr;
    arr.append(1234);
    _this.set("name", arr);
}
```

* 类方法接收3个参数，`_this`为对象实例，`args`是PHP传入的参数数组，`retval`是给PHP的返回值默认为`null`
* 如果类方法声明为静态`STATIC`，`_this`将是`UNDEF`未定义的类型
* 使用`_this.call()`方法调用对象的方法
* 使用`_this.get()`和`_this.set()`读写对象的属性

## 依赖关系

编写的PHP扩展需要需要依赖另外一个扩展，在`PHP-X`中可以调用`Extension->require`来实现。

```cpp
PHPX_EXTENSION()
{
    Extension *ext = new Extension("test", "0.0.1");
	ext->require("swoole");
	ext->require("sockets");
    return ext;
}
```

* `require`方法传入所依赖扩展的名称
* 可以多次调用`require`实现多个扩展依赖关系
* 使用`require`后，如果PHP未加载所依赖的扩展，底层会抛出致命错误
* 使用`require`后，可以忽略`php.ini`中扩展配置的顺序，`ZendVM`底层会自动进行排序，先加载依赖的扩展

### 引入头文件

引入其他扩展的头文件时，需要使用`extern C`的方式。

```cpp
extern "C" {
	#include "ext/swoole/php_swoole.h"
}
```

### 使用其他扩展

* 扩展内直接使用`php::exec`执行其他扩展的函数，或者`php::newObject`创建其他扩展提供的类对象
* 其他扩展提供的`C/C++`函数，需要引入相关的头文件，然后调用即可
