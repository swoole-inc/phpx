# Object

`Object`类是对`zend_object`的封装，提供了对`PHP`对象操作的`API`

#### 创建对象

```cpp
Object obj = newObject("class");
```

* `newObject`第一个参数为类名，可以是扩展定义的类，也可以是用户代码定义的类
* 第二个参数起为变长参数，可传入`0-10`个构造方法参数

#### 读取属性

```cpp
Object object(var);
object.get("name");
```

#### 设置属性

```cpp
Object object(var);
object.set("name", 1234);
```

`Object::set`方法原型为

```cpp
Object::set(const char *name, Variant &value);
```

## 调用方法

使用`Object::exec`来调用对象的方法。参数原型：

```cpp
Variant Object::exec(const char *method_name, Variant args ...);
```

* `method_name`是方法的名称，字符串类型
* `args`参数，`args`是变长参数，最多可以接受10个参数
* 如果参数超过10个，请使用更底层的`Object::call`方法

### 使用示例

```cpp
Object redis = newObject("redis");
Variant ret = redis.exec("connect", "127.0.0.1", 9501);
```

## 资源属性

在扩展中经常需要保存C++指针，使用属性操作方法，需要分2条指令实现从属性 -> 资源变量 -> 指针的转换。

`PHP-X`提供了`oGet`和`oSet`两个方法来简化资源属性的操作。

### 设置指针

```cpp
_this.oSet("propertyName", "ResourceType", new CppObject(1, 2));
```

### 获取指针

```cpp
CppObject *ptr = _this.oGet<CppObject>("propertyName", "ResourceType");
```