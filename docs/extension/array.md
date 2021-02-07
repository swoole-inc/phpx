# Array

`Array`类型是ZendVM底层`zend_array`和`HashTable`的封装。提供了非常方便的数组操作API

#### 数字索引数组

```cpp
Array arr;
arr.append(1234);
arr.append(1234.56);
arr.append(false);
arr.append("hello world");
```

#### 关联索引数组

```cpp
Array arr;
arr.set("key1", 1234);
arr.set("key2", 1234.56);
arr.set("key3", "hello world");
arr.set("key4", true);
```

`Array`类实现了迭代器，可使用迭代器对数组进行遍历，如果是数字索引数组，还可以直接使用for循环进行遍历。

## 数字索引数组

```cpp
for(int i = 0; i < array.count(); i++)
{
    php::echo("key=%d, value=%s.\n", i, array[i].toCString());
}
```

## 迭代器

```cpp
for(auto i = array.begin(); i != array.end(); i++)
{
    php::echo("key=%d, value=%s.\n", i.key().toCString(), i.value().toCString());
}
```

* `array.begin()` 返回类型为 `ArrayIterator`
* 迭代器的方法`key()`和`value()`可以获取到数组的`key`和`value`

## 方法列表

数组的方法列表。

### merge

合并数组的值。函数原型：

```cpp
Array::merge(Array &array2);
```

* 使用示例

```cpp
Array arr1;
Array arr2;
arr2.append(123);
arr2.append("hello world");
arr2.append(123.05);
arr1.merge(arr2);
```

### sort

数组排序，函数原型：

```cpp
void Array::sort(void);
```
