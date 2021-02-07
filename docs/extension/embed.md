# Embed

在`C++`程序中可以内嵌一个`ZendVM`，实现与`PHP`程序的交互。需要依赖`PHP`官方的`embed`模块。编译`PHP`时需要增加`--enable-embed`参数。

## 编写程序

```cpp
#include "phpx_embed.h"
#include <iostream>

using namespace php;
using namespace std;

int main(int argc, char * argv[])
{
    VM vm(argc, argv);
    vm.eval("echo 'Hello World!';");
    vm.include("embed.php");
	
	auto a = constant("SWOOLE_BASE");
    cout << "SWOOLE_BASE = " << a.toInt() << endl;

    auto obj = newObject("Test");
    auto ret = obj.call("getName");

    cout << ret.toString() << endl;

    obj.set("name", "Tianfeng");

    auto ret2 = obj.call("getName");
    cout << ret2.toString() << endl;
	
    return 0;
}
```

* `VM`类就是`ZendVM`虚拟机环境，构造方法中自动初始化`VM`。由于`ZendVM`使用了大量全局变量，一个进程内只能创建一个VM
* `VM`对象析构时自动销毁`ZendVM`
* `vm`对象有2个`API`，`eval`方法可以执行任意的`PHP`代码，`include`方法用于加载一个PHP脚本并执行
* `VM`初始化后，可以在`C++`程序中调用`PHP-X`中提供的接口与PHP进行交互

## 编译程序

```shell
PHP_INCLUDE = `php-config --includes`
PHP_LIBS = `php-config --libs`
PHP_LDFLAGS = `php-config --ldflags`

all: embed.cpp
	c++ -DHAVE_CONFIG_H -g -o embed -O0 embed.cpp -std=c++11  ${PHP_INCLUDE} ${PHP_LDFLAGS} -lphp7 -lphpx ${PHP_LIBS}
```

* `-lphp7`载入`PHP7`的动态连接库
* `-lphpx`载入`PHP-X`动态连接库
