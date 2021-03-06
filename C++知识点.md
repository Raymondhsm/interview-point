### 1、智能指针模板

* auto_ptr和shared_ptr 只能用于new

* unique_ptr能用于new 和 new[]

* auto_ptr不安全

* unique_ptr只有一个指针拥有内存块的拥有权，把其赋值给unique_ptr会报错

* shared_ptr使用计数，有一个指向就加一，直到计数为零时才调用delete

#### 

### 2、C++中提供了四种类型转换符：static_cast、dynamic_cast、const_cast、reinterpret_cast

* static_cast可以完全替代c的类型转换，而且在对对象指针之间的类型转换时，可以将父类指针转换成子类指针，也可以将子类指针转换成父类指针，但是如果两个类不相关则无法相互转换。 需注意的是，如果父类指针指向一个父类对象，此时将父类指针转换成子类指针虽然可以通过static_cast实现，但是这种转换很不安全；如果父类指针本身就指向子类指针则不存在安全问题。
* const_cast转换过程中增加或删除const属性。
* reinterpret_cast: （interpret是解释的意思，reinterpret即为重新解释，此标识符的意思即为数据的二进制形式重新解释，但是不改变其值。）有着和C风格的强制转换同样的能力。它可以转化任何内置的数据类型为其他任何的数据类型，也可以转化任何指针类型为其他的类型。它甚至可以转化内置的数据类型为指针，无须考虑类型安全或者常量的情形。不到万不得已绝对不用。
* dynamic_cast 只能用于对象指针之间的类型转换，可以将父类指针转换成子类指针，也可以将子类指针转换成父类指针，转换结果也可以是引用，但是dynamic_cast不等同于static_cast。dynamic_cast在将父类指针转换为子类指针的过程中，需要对其背后的对象类型进行检查，以保证类型完全匹配，而static_cast不会。只有当一个父类指针指向一个子类对象，且父类中包含虚函数时，使用dynamic_cast将父类指针转换成子类指针才会成功，否则返回空指针，如果是引用则抛出异常。
  1. 其他三种都是编译时完成的，dynamic_cast是运行时处理的，运行时要进行类型检查。
  2. 不能用于内置的基本数据类型的强制转换。
  3. dynamic_cast转换如果成功的话返回的是指向类的指针或引用，转换失败的话则会返回NULL。
  4. 使用dynamic_cast进行转换的，基类中一定要有虚函数，否则编译不通过。

#### 

### 3、malloc和new区别

* __属性__：new/delete是C++关键字，需要编译器支持。malloc/free是库函数，需要头文件支持。
* __参数__：使用new操作符申请内存分配时无须指定内存块的大小，编译器会根据类型信息自行计算。而malloc则需要显式地指出所需内存的尺寸。
* __返回类型__：new操作符内存分配成功时，返回的是对象类型的指针，类型严格与对象匹配，无须进行类型转换，故new是符合类型安全性的操作符。而malloc内存分配成功则是返回void \* ，需要通过强制类型转换将void\*指针转换成我们需要的类型。
* __分配失败__：new内存分配失败时，会抛出bac_alloc异常。malloc分配内存失败时返回NULL。
* __自定义类型__：new会先调用operator new函数，申请足够的内存（通常底层使用malloc实现）。然后调用类型的构造函数，初始化成员变量，最后返回自定义类型指针。delete先调用析构函数，然后调用operator delete函数释放内存（通常底层使用free实现）。malloc/free是库函数，只能动态的申请和释放内存，无法强制要求其做自定义类型对象构造和析构工作。
* __重载__：C++允许重载new/delete操作符，特别的，布局new的就不需要为对象分配内存，而是指定了一个地址作为内存起始区域，new在这段内存上为对象调用构造函数完成初始化工作，并返回此地址。而malloc不允许重载。
* __内存区域__：new操作符从自由存储区（free store）上为对象动态分配内存空间，而malloc函数从堆上动态分配内存。自由存储区是C++基于new操作符的一个抽象概念，凡是通过new操作符进行内存申请，该内存即为自由存储区。而堆是操作系统中的术语，是操作系统所维护的一块特殊内存，用于程序的内存动态分配，C语言使用malloc从堆上分配内存，使用free释放已分配的对应内存。自由存储区不等于堆，如上所述，布局new就可以不位于堆中。  

#### 

### 4、static关键字总结（作用域、生命周期）

* 修饰函数内局部变量
  
    __变量生命周期和程序一致，多次调用仅会初始化一次，存储在静态存储区__

* 修饰全局变量或函数
  
    __限制全局变量或函数的作用域在本文件__

* 修饰成员变量或成员函数
  
    __多个实例共用一个变量，无需实例化便可以使用静态变量或静态函数__

#### 

### 5、extern关键字总结（全局变量和函数可以使得它们能够跨文件被访问）

* 函数的extern修饰无意义，因为函数本身就是extern
* 局部变量的声明不能有extern的修饰，且局部变量在运行时才在堆栈部分分配内存
* 全局变量在外部使用声明时，extern关键字是必须的，如果变量没有extern修饰且没有显式的初始化，同样成为变量的定义，因此此时必须加extern，而编译器在此标记存储空间在执行时加载内并初始化为0
* extern C 表示使用C来编译连接

#### 

### 6、const关键字总结（作用域限制在本编译模块）

* 修饰变量 
  
    __不能修改内容__

* 修饰函数返回值
  
    __函数返回值只能赋值给对应的const变量__

* 修饰成员函数（int fun() const{}）不允许修饰类非成员函数
  
    __禁止修改成员变量__

#### const可以和extern一起使用，可以作用于外部模块，而static不能与extern一起使用

#### 

### 7、explicit关键字总结

* 防止类构造函数的隐式转换

* 当构造函数只有一个参数，或其他非第一个参数有默认值时支持隐式转换
  
  ```cpp
  class A {
  public:
    //这里用explicit关键词来修饰类构造函数.
    explicit A(int i = 5)
    {
        m_a = i;
    }
  private:
    int m_a;
  };
  ```
  
  ```cpp
  {
      A s;
      //这样直接赋值,会被提示错误,因为explicit抑制隐式转换的进行
      s = 10;//这样会报错!!!如果没有explicit修饰不会报错
      //当然显示转换还是可以的.
      s = A(20);
  
      system("pause");
      return 0;
  }
  ```

#### 

### 8、typedef 和 define 的区别（```typedef int（*funName）（）```）

* __原理__：define不做类型检查，只做替换，是预处理指令；typedef做类型检查，能在一个函数定义里面使用typedef
* __功能__：define可以定义类型别名，常量、变量、编译开关等；typedef一般只定义类型别名
* __作用域__：define本文件；typedef函数外则到文件结尾，函数内则到函数结束
* __指针操作__：

```cpp
#define INTPTR1 int*
typedef int* INTPTR2;
INTPTR1 p1, p2;    //一个指针，一个整型
INTPTR2 p3, p4;    //两个整型
```

```cpp
#define INTPTR1 int*
typedef int* INTPTR2;
int a = 1;
int b = 2;
int c = 3;
const INTPTR1 p1 = &a;    //常量指针
const INTPTR2 p2 = &b;    //指针常量
INTPTR2 const p3 = &c;    //指针常量
```

#### 

### 9. 函数指针

- ##### 格式
  
  - `int (*pfun)(int, int);`
  
  - 因为括号优先级比`*`高，所以要用括号`*pfun`

- ##### 函数指针&指针函数区别
  
  - 函数指针是指向函数的**指针**
    
    > `int (*pfun)(int, int);`
  
  - 指针函数是返回值为指针的**函数**
    
    > `int *pfun (int, int);`

- ##### 复杂情况
  
  - 返回值为函数指针
    
    > `int (*pfun(int))(int,int)`
    > 
    > `pfun`先与`(int)`结合，说明`pfun`是一个参数为`int`的函数，再与`*`结合表示其返回值为`int(*)(int,int)`的函数
  
  - 参数为函数指针
    
    > `int fun(int, void(*)(int))`
    > 
    > 参数中`void(*)(int)`表示一个函数指针
  
  - 返回值和参数都为函数指针
    
    > `int (*pfun(int, void(*)(int)))(int,int)`

- ##### 建议书写
  
  - 使用typedef来定义函数指针，方便返回值，参数传递等
    
    > `typedef void(*PF)(int);`
    > 
    > `PF signal(int, void (*)(int))`

#### 

### 10. union

- 在struct中，所有的成员都有自己的存储空间，而且为了便于寻址和管理，所有的数据成员都要遵循[内存对齐](http://hi.baidu.com/phps/blog/item/f03eb93ee12f49fa838b1365.html "难以理解的《内存对齐与ANSI C中struct型数据的内存布局》")的规则；

- 在union中，所有的成员共用一块存储空间，在操作不同的成员时，编译器依据不同的成员类型，按照不同的方式取值。

- 大小端
  
  - 大端模式（Big_endian）：字数据的高字节存储在低地址中，而字数据的低字节则存放在高地址中。
  
  - 小端模式（Little_endian）：字数据的高字节存储在高地址中，而字数据的低字节则存放在低地址中。

```cpp
#include<stdio.h>
union var{
        char c[4];
        int i;
};
int main(){
        union var data;
        data.c[0] = 0x04;

        data.c[1] = 0x03;

        data.c[2] = 0x02;
        data.c[3] = 0x11;
        //数组中下标低的，地址也低，按地址从低到高，内存内容依次为：04,03,02,11。总共四字节！
        //而把四个字节作为一个整体（不分类型，直接打印十六进制），应该从内存高地址到低地址看，0x11020304，低位04放在低地址上。
        printf("%x\n",data.i);
}

//输出为11020304时，为小端
//输出为04030211时，为大端
```

##### 

### 11. void *的作用

- `void`型指针可以指向任意类型

- 可用任意数据类型的指针对`void`指针赋值，但使用`void`型指针对其他类型指针赋值需要显示的强制转换

- 在`ANSI`规定中，`void`型指针不允许进行算术运算（++等），但在`GNU`中可以，它将`void`型看做`char`型进行操作

- 如果一个函数的参数可以是任意类型的指针，则可以使用`void*`作为形参

##### 

### 12. 友元函数或类

- 有时需要定义一些函数，这些函数不是类的一部分，但又需要频繁地访问类的数据成员，这时可以将这些函数定义为该函数的友元函数。

- **优缺点**
  
  - 友元的作用是提高了程序的运行效率（即减少了类型检查和安全性检查等都需要时间开销），
  
  - 但它破坏了类的封装性和隐藏性，使得非成员函数可以访问类的私有成员。

- **使用**
  
  - 友元函数：在类中声明函数时添加`friend`关键字，`private`区或`public`区都可以
  
  - 友元类：在类中声明`friend class className`

- **特性**
  
  - 友元关系不能被继承。 
  
  - 友元关系是单向的，不具有交换性。若类B是类A的友元，类A不一定是类B的友元，要看在类中是否有相应的声明。
  
  - 友元关系不具有传递性。若类B是类A的友元，类C是B的友元，类C不一定是类A的友元，同样要看类中是否有相应的申明

##### 

### 13. 虚继承

- 虚继承是解决C++多重继承问题的一种手段，从不同途径继承来的同一基类，会在子类中存在多份拷贝。从而造成**浪费空间**和**存在二义性**的问题

- ###### 构造函数
  
  - 为了避免B、C调用A的构造函数造成不一致的构造结果。
  
  - D不仅要负责初始化直接基类B和直接基类C，还要负责初始化间接基类A。
  
  - 编译器总是先调用虚基类的构造函数，再按照出现的顺序调用其它的构造函数；

- ###### 内存
  
  - 虚继承时，对象会保存一个虚基类表得到指针，
  
  - 虚基类表中第一个元素为0，接下来保存指针到基类的**地址偏移**
  
  ```cpp
  class A
  {
      int a;
      int b;
  };
  
  class B :  public A
  {
      int c;
  };
  
  class C : public A
  {
      int d;
  };
  
  class D : public B, public  C
  {
      int e;
  };
  ```
  
  如果不采用虚拟继承的话，内存分布如下：![Image\NotVirtual](Image\NotVirtual.png)
  
  如果采用虚拟继承的话，内存分布如下：![Image\virtual](Image\virtual.png)
  
  ###### 

### 14. 内存对齐

- 在C++中规定了空结构体和空类的内存所占大小为1字节，因为c++中规定，任何不同的对象不能拥有相同的内存地址。
  
  而在C语言中，空的结构体在内存中所占大小为0。(gcc中测试为0，其他编译器不一定)

- **为什么要内存对齐**([ref](https://blog.csdn.net/weixin_40853073/article/details/81451792))
  
  - **平台原因(移植原因)**：不是所有的硬件平台都能访问任意地址上的任意数据的；某些硬件平台只能在某些地址处取某些特定类型的数据，否则抛出硬件异常。 
  
  - **性能原因：**数据结构(尤其是栈)应该尽可能地在自然边界上对齐。原因在于，为了访问未对齐的内存，处理器需要作两次内存访问；而对齐的内存访问仅需要一次访问。

- **C++数据类型大小**
  
  | 类型             | 32位   | 64位   |
  |:--------------:|:-----:|:-----:|
  | bool           | 1     | 1     |
  | char           | 1     | 1     |
  | short          | 2     | 2     |
  | unsigned int   | 4     | 4     |
  | int            | 4     | 4     |
  | long           | 4     | 4     |
  | long long      | 8     | 8     |
  | unsigned long  | 4     | 4     |
  | float          | 4     | 4     |
  | double         | 8     | 8     |
  | long double    | 8     | 8     |
  | **void *(指针)** | **4** | **8** |

- **内存对齐原则**
  
  - 对齐系数`#program pack(n)`
  
  - 原则一:存放的首地址偏移量 % min(当前类型大小,对齐系数) == 0
  
  - 原则二：结构体整体对齐，也称作二次对齐，结构体整体大小 % min(当前类型最大的大小,对齐系数) == 0，不足要补齐
  
  ###### 

### 15. 指针XX & XX指针

- **指针常量 & 常量指针**
  
  - 指针常量
    
    - 指针是常量，指针不能指向别的地方
    
    - `int *const p`
  
  - 常量指针
    
    - 指向常量的指针
    
    - ` const int *p`
    
    - `int const *p`
  
  ###### 

- **指针数组 & 数组指针 (优先级`()>[]>*`)**
  
  - 指针数组
    
    - 一个内容为指针的数组，本质为数组
    
    - `int *p[n]`
  
  - 数组指针
    
    - 指向数组的指针
    
    - `int (*p)[n]`

### 16. C++ 11 新特性

- **auto关键字**
  
  - 通知编译器去根据初始化代码推断所声明变量的真实类型。
  
  - 一般来说，auto不能用来声明函数的返回值
  
  - auto不能用于函数传参以及数组类型推导

- **nullptr**  
  
  - 以前都是用0来表示空指针的，但由于0可以被隐式类型转换为整形，这就会存在一些问题。
  
  - nullptr和任何指针类型以及类成员指针类型的空值之间可以发生隐式类型转换，同样也可以隐式转换为bool型（取值为false）。

- **基于范围的for循环**
  
  - ```cpp
    int arr[] = {1,2,3,4,5};
    for(int& e : arr) 
    {
      e = e*e;
    }
    ```

- `lambda`表达式
  
  - **[capture list] (parameter list) ->return type { function body }**

- 新增`share_ptr & weak_ptr`
  
  - 将一个weak_ptr绑定到一个shared_ptr不会改变shared_ptr的引用计数。
  
  - 不论是否有weak_ptr指向，一旦最后一个指向对象的shared_ptr被销毁，对象就会被释放。

- 初始化列表
  
  - **对象的成员变量的初始化动作发生在进入构造函数本体之前**。
  
  ```cpp
  RatedPlayer::RatedPlayer(int r, const string &fn): 
  TableTennisPlayer(fn) //此为初始化
  
  {
      rating = r;    //此为赋值
  
  }
  ```

- 右值引用

###### 

### 17. 常量引用

- 关于引用的初始化有两点值得注意：
  
  - 当初始化值是一个左值（可以取得地址）时，没有任何问题；
  
  - 当**初始化值不是一个左值**时，则只能对一个**const**  T&（常量引用）赋值。
  
  ```cpp
  double& dr = 1; // 错误：需要左值
  
  const double& cdr = 1; // ok
  
  //第二句实际的过程如下：
  
  double temp = double(1);
  
  const double& cdr = temp;
  ```

- 作为函数参数传递时

```cpp
//使用常量引用就可以这样传
void fun(const int &i){}

fun(1); //调用。如果不是常量引用，由于1不是可修改的左值，会错误
```

- 通过常量引用从函数返回一个局部变量（这么做一般是不对的）

```cpp
//返回值回无效，因为函数结束t会被destory
T & fun(){ T t; return t;}
//这样的话，t的生命周期会一直到达ttt结束为止
const T & fun(){T t; return t;}
const T &ttt = fun();
```

###### 

### 18. 模板函数（泛化与特化）

```cpp
template<typename T>
template<class T>
```

- **特化**
  
  - 模板类或函数需要对某些类型进行特化处理。
  
  - 全特化：全部模板量都特殊化
  
  - 偏特化：部分模板量进行特殊化

```cpp
//模板类
template<class T1, class T2>
class A{}

//类的全特化
template<>
class A<B, C>{}

//类的偏特化
template<class T2>
class A<B, T2>{}

//特殊偏特化
template<class T1, class T2>
class A<vector<T1>,T2>

//模板函数
template<typename T>
void fun(){}

//函数的全特化
template<>
void fun<int>(){}
```

###### 

### 19. 虚构造函数与虚析构函数

- #### 为什么构造函数不为虚
  
  1. 虚函数需要虚表指针，但构造函数负责实例化虚表指针；当构造函数为虚时，利用虚表来寻找对应构造函数并不能实现。
  
  2. 没必要。实例化对象时需要显示指定对应的，不存在利用（指向子类的）基类指针来实例化

- #### 为什么析构函数要为虚
  
  1. 防止内存泄露
  
  2. 当不为虚时，只会调用指针类型的析构函数，造成只释放掉对应的内存；为虚时，会调用指针指向的实际的对象类型的析构函数。

###### 

### 20. 常用运算符总结

- ##### 逗号运算符
  
  - 逗号运算符会顺序执行一系列运算。
  
  - 整个逗号表达式的值是以逗号分隔的列表中的**最后一个表达式**的值。

- ##### 常用运算符优先级
  
  - 
