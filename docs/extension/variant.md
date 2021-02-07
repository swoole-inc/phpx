# Variant

`Variant`类型相当于ZendVM的`zval`结构，是对PHP变量的封装。

#### 赋值

`Variant`底层实现了操作符重载，可以直接对其赋值。

```cpp
Variant a = 1234;
Variant b = 1234.56;
Variant c = false;
Variant d = "hello world";
```

`Variant`对象可以直接赋值给另外一个 `Variant`对象。

```cpp
Variant a = "hello world";
Variant v = a;
```

#### 构造方法

`Variant`构造使用了C++多态，可以使用任意整型、浮点型、字符串等类型构造`Variant`对象。

```cpp
Variant a(1234);
Variant b(1234.56);
Variant c(false);
Variant d("hello world");
size_t length;
Variant e(char *data, length);
```

## 类型判断

`Variant`对象的`isXXX`系列方法可以用于判断变量的类型。

### 整型

```cpp
var.isInt();
```

### 浮点型

```cpp
var.isFloat();
```

### 字符串

```cpp
var.isString();
```

### 布尔型

```cpp
var.isBool();
```

### 对象

```cpp
var.isObject();
```

### 数组

```cpp
var.isArray();
```

### NULL类型

```cpp
var.isNull();
```

### 资源类型

```cpp
var.isResource();
```

## 类型转换

`Variant`对象提供的`toXXX`系列函数可以将标量(Scalar)变量转换为其他类型。Zend字符串、数组、对象可直接使用构造方法进行转换，但必须保证传入的`Variant`对象必须为该类型，否则底层会抛出致命错误。

### 转为整型

```cpp
long value = var.toInt();
```

### 转为浮点型

```cpp
double value = var.toFloat();
```

### 转为布尔型

```cpp
bool value = var.toBool();
```

### 转为字符串

```cpp
//C++风格字符串
string value = var.toString();
//C风格字符串
const char* value = var.toCString();
//zend_string
String str(var);
```

### 转为数组

```cpp
Array arr(var);
```

### 转为对象

```cpp
Object obj(var);
```

### 转为资源

```cpp
CppObject *ptr = var.toResource<CppObject>();
```

## 高级接口

### 判断相等

两个`Variant`对象可以使用`Variant::equals`方法判断是否相等。`Variant::equals`函数原型：

```cpp
bool Variant::equals(Variant &other_var, bool strict = false);
```

* `other_var` 是要对比的另外一个变量
* `strict` 是否启用严格模式，使用严格模式时，底层不会自动转换类型，如 `"1234" == 1234` 会判断为不相等

```cpp
Variant a = 1234;
Variant b = "1234";
bool ret = a.equals(b);
```

### 直接判断

除了使用`equals`方法判断相等外，还可以直接使用`==`判断两个`Variant`对象是否相等，但不支持严格模式。

```cpp
if (a == b)
{
	echo("a==b\n");
}
```

### 导出内存

某些情况下需要导出`Variant`对象到内存堆上，可以使用`Variant::dup`方法实现。

```cpp
Variant* Variant::dup(Variant &v);
```

* `dup`方法会使用`new`在堆上申请内存，并复制`Variant`对象的值
