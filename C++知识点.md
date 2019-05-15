### 1、智能指针模板
	auto_ptr和shared_ptr 只能用于new
	unique_ptr能用于new 和 new[]
	
	auto_ptr不安全
	unique_ptr只有一个指针拥有内存块的拥有权，把其赋值给unique_ptr会报错
	shared_ptr使用计数，有一个指向就加一，直到计数为零时才调用delete
	
### 2、C++中提供了四种类型转换符：static_cast、dynamic_cast、const_cast、reinterpret_cast
* static_cast可以完全替代c的类型转换，而且在对对象指针之间的类型转换时，可以将父类指针转换成子类指针，也可以将子类指针转换成父类指针，但是如果两个类不相关则无法相互转换。 需注意的是，如果父类指针指向一个父类对象，此时将父类指针转换成子类指针虽然可以通过static_cast实现，但是这种转换很不安全；如果父类指针本身就指向子类指针则不存在安全问题。
* const_cast转换过程中增加或删除const属性。
* reinterpret_cast: （interpret是解释的意思，reinterpret即为重新解释，此标识符的意思即为数据的二进制形式重新解释，但是不改变其值。）有着和C风格的强制转换同样的能力。它可以转化任何内置的数据类型为其他任何的数据类型，也可以转化任何指针类型为其他的类型。它甚至可以转化内置的数据类型为指针，无须考虑类型安全或者常量的情形。不到万不得已绝对不用。
* dynamic_cast 只能用于对象指针之间的类型转换，可以将父类指针转换成子类指针，也可以将子类指针转换成父类指针，转换结果也可以是引用，但是dynamic_cast不等同于static_cast。dynamic_cast在将父类指针转换为子类指针的过程中，需要对其背后的对象类型进行检查，以保证类型完全匹配，而static_cast不会。只有当一个父类指针指向一个子类对象，且父类中包含虚函数时，使用dynamic_cast将父类指针转换成子类指针才会成功，否则返回空指针，如果是引用则抛出异常。
	1. 其他三种都是编译时完成的，dynamic_cast是运行时处理的，运行时要进行类型检查。
	2. 不能用于内置的基本数据类型的强制转换。
	3. dynamic_cast转换如果成功的话返回的是指向类的指针或引用，转换失败的话则会返回NULL。
	4. 使用dynamic_cast进行转换的，基类中一定要有虚函数，否则编译不通过。
	
### 3、malloc和new区别
    https://blog.csdn.net/nie19940803/article/details/76358673
    
    
### 4、static关键字总结（作用域、生命周期）
* 修饰函数内局部变量
	__变量生命周期和程序一致，多次调用仅会初始化一次，存储在静态存储区__
* 修饰全局变量或函数
	__限制全局变量或函数的作用域在本文件__
* 修饰成员变量或成员函数
	__多个实例共用一个变量，无需实例化便可以使用静态变量或静态函数__
    
### 5、extern关键字总结（全局变量和函数可以使得它们能够跨文件被访问）
* 函数的extern修饰无意义，因为函数本身就是extern
* 局部变量的声明不能有extern的修饰，且局部变量在运行时才在堆栈部分分配内存
* 全局变量在外部使用声明时，extern关键字是必须的，如果变量没有extern修饰且没有显式的初始化，同样成为变量的定义，因此此时必须加extern，而编译器在此标记存储空间在执行时加载内并初始化为0
* extern C 表示使用C来编译连接

### 6、const关键字总结（作用域限制在本编译模块）
* 修饰变量 
	__不能修改内容__
* 修饰函数返回值
	__函数返回值只能赋值给对应的constr变量
* 修饰成员函数（int fun() const{}）不允许修饰类非成员函数
	__禁止修改成员变量__
	
#### const可以和extern一起使用，可以作用于外部模块，而static不能与extern一起使用
	
### 7、explicit关键字总结
* 防止类构造函数的隐式转换
* 当构造函数只有一个参数，或其他非第一个参数有默认值时支持隐式转换
```
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

int main()
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

### 8、typeof 和 define 的区别（```typedef int（*funName）（）```）
* __原理__：define不做类型检查，只做替换，是预处理指令；typedef做类型检查，能在一个函数定义里面使用typedef
* __功能__：define可以定义类型别名，常量、变量、编译开关等；typedef一般只定义类型别名
* __作用域__：define全局；typedef不明？？？
* __指针操作__：
```
#define INTPTR1 int*
typedef int* INTPTR2;
INTPTR1 p1, p2;	//一个指针，一个整型
INTPTR2 p3, p4;	//两个整型
```
```
#define INTPTR1 int*
typedef int* INTPTR2;
int a = 1;
int b = 2;
int c = 3;
const INTPTR1 p1 = &a;	//常量指针
const INTPTR2 p2 = &b;	//指针常量
INTPTR2 const p3 = &c;	//指针常量
```

### 9、
