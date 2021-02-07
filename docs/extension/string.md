# String

`String`类是对`zend_string`的封装，提供了类似`std::string`的方法。

```cpp
Variant a = "hello world";
String s(a);
echo("str=%s, length=%d.\n", s.c_str(), s.length());

String s2("hello world");

char *data;
size_t length;
String s3(data, length);
```

> `String`对象是其他`Variant`的引用时，析构方法不会释放内存  
> `String`对象是直接创建的字符串，在对象析构时会自动释放对应的内存  

## 类型转换

`String`可以与整型和浮点型互相转换。

### 整形

#### String转Int

```cpp
String s("1234");
long value = s.toInt();
```

#### Int转String

```cpp
String s(1234);
```

### 浮点型

#### String转Float

```cpp
String s("1234.56");
double value = s.toFloat();
```

#### Float转String

```cpp
String s(1234.56);
```