# Class

对于类操作的封装。

**获取/设置类静态属性**必须在扩展加载并初始化之后才能使用。如在`PHPX_METHOD`或`PHPX_FUNCTION`代码中使用。

## 注册一个扩展类

参照 [Extension](/extension/extension) 部分注册类的文档。必须在扩展初始化时注册类。

## 获取类静态属性

使用`Class::get`静态方法
```cpp
Class::get(className, propertyName);
```

* `className`字符串类型，`PHP`类的名称，可以是扩展内置类也可以用户定义类
* `propertyName`字符串类型，静态属性的名称

## 设置类静态属性

使用`Class::set`静态方法
```cpp
Class::set(className, propertyName, value);
```

* `className`字符串类型，`PHP`类的名称，可以是扩展内置类也可以用户定义类
* `propertyName`字符串类型，静态属性的名称
* `value`必须为`Variant`对象
