# Resource

资源类型，用于保存`C/C++`指针。

## 注册

```cpp
void string_dtor(zend_resource *res)
{
    String *s = static_cast<String *>(res->ptr);
    delete s;
}

extension->registerResource("ResourceString", string_dtor);
```

* 使用`registerResource`注册资源类型，参数一为资源名称，参数二为资源析构函数，用于释放对应的`C++`指针
* 析构函数接受一个`zend_resource`类型的指针，可使用`static_cast`转为对应类型的指针

## 创建

```cpp
Variant v = newResource("ResourceString", new string("hello world"));
```

* 使用`newResource`创建资源类型的`PHP`变量，第一个参数为资源类型名称，第二个参数为`C++`指针
* 需要在`C++`代码中`new`一个对象，不得使用栈上的对象指针，否则会发生内存错误

## 使用

```cpp
Variant v = args[0];
string *str = v.toResource<string>("ResourceString");
```

* `Variant`变量可以使用`toResource`方法将变量转为`C++`的指针
