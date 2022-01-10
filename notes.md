## C++ Primer 5 笔记

### 2

引用自身不是对象，只是对象名字的别名

除了一些特殊情况外，引用类型必须和与之绑定的对象严格匹配，且只能绑定在对象上，不能绑定在某个字面值或某个表达式的计算结果（非常量引用的初始值必须为左值）。注意，引用一旦定义，就无法令其再绑定到其他对象

引用只是变量的别名，如下面的例子打印结果为 10 10

```c++
int main() {
    int i, &ri = i;
    i = 5; ri = 10;
    std::cout << i << " " << ri << std::endl;
    return 0;
}
```

获取对象的地址使用取地址符&，int *pi = &val，如果指针指向了一个对象，可以使用解引用符来访问该对象

```c++
int main() {
    int i = 5;
    int *p = &i;
    cout << p << endl;   //地址
    cout << *p << endl;   // 5
    i = 6;
    cout << *p << endl;   // 6
    *p = 7;
    cout << i << endl;   // 7
    cout << *p << endl;   // 7
    return 0;
}
```

可以简单的认为赋值永远改变的是等号左边的对象
```c++
int *pi = nullptr;
int ival = 0;
pi = &ival;   // 改变的是pi
*pi = 1;   // 改变的是ival
```

可以把引用绑定在const对象上，对常量的引用不能用作修改他所绑定的对象
```c++
const int ci = 1024;
const int &r1 = ci;   // true
r1 = 42;   // false
int &r2 = ci;   // false
return 0;
```
常量引用的初始值可以是非常量的对象，字面值，甚至表达式（即可以是非左值），常量引用引用一个非const对象合法，但是不能通过其改变引用对象的值，对于指针同理

指针本身是不是常量（top-level const）和指针所指的是不是一个常量（low-level const）是两个相互独立的问题

```c++
int i = 0;
int *const p1 = &i;   // 指向int类型的常量指针，不能改变p1的值
const int ci = 42;   // int类型的常量
const int *p2 = &ci;   // 指向int类型的常量的指针，可以改变p2的值
```

```c++
typedef int integer;
using integer = int;
```

auto一般会忽略顶层const，保留底层const
```c++
int i = 1;
const int ci = i, &cr = ci;
auto b = ci;   // int b
auto c = cr;   // int c
auto d = &i;   // int *d
auto e = &ci;   // const int *e
```

头文件保护符
```c++
# ifndef FILE_NAME_H
# define FINE_NAME_H
# endif FINE_NAME_H
```

### 3

使用范围for语句改变字符串中的字符
```c++
for (auto &c: s) {   // c被绑定到序列的每一个元素上
    ...
}
```
vector赋值，注意区分花括号和圆括号

比较复杂的数组声明
```c++
int *ptr[10];   // 含有10个整形指针的数组
int (*Parray)[10] = &arr;   // 指向一个含有10个整数的数组的指针
int (&arrRef)[10] = arr;   // 引用一个含有10个整数的数组
```

在大多数表达式中，使用数组类型的对象其实是使用一个指向该数组首元素的指针
```c++
int main() {
    int a[10] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    int *p = &a[0]   // 等价于 int *p = a;
    auto ia(a);   // ia是指针，指向a第一个元素
    cout << *a << endl;   // 0
    return 0;
}
```

事实上，指针也可以看作迭代器，c++11还定义了begin()和end()函数
```c++
int main() {
    int a[10] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    int *e = &a[10];
    for (int *p = a; p != e; p++) {
        cout << *p << endl;
    }
    return 0;
}
```

多维数组
```c++
int main() {
    int a[3][4];
    int cnt = 0;
    for (auto &row: a) {
        for (auto &col: row) {
            col = cnt;
            cnt++;
        }
    }
    for (auto p = a; p != a + 3; p++) {
        for (auto q = *p; q != *p + 4; q++) {
            cout << *q << endl;
        }
    }
    return 0;
}
```