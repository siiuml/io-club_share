## 前提：一些编程相关基础

- **文件**：
以二进制形式存储在硬盘等数据持久化的存储设备的东西。
有些被程序以**二进制**（**bin**）形式处理（如 .exe），有些被程序在以某种编码格式（如 utf-8）解码（**decode**）后以**文本**形式读取（如 .txt）。
- **源代码**（**source code**（**src**））：文本格式的文件，由文本编辑器或 **IDE** （**集成开发环境**）读写。IDE能提供
- **解释器**（**intepreter**）、**编译器**（**compiler**）等：处理**源代码**使其能够被执行的东西
- **库**（**library**）：一个代码文件中能用的其他代码或能用的东西，如**包**（**package**），**模块**（**module**）等，其中第三方库和自己的库都需要让处理器、编译器知道去哪找到它们，如 C/C++ 中设置的 include_path
- **语法**：为了让解释器或编译器能正确处理代码而制定的规范
  - **关键字**（**keyword**）：语法中的一些词句（如 `if`），对编译器而言和`=`、`{}`等符号一样重要；
  一般无法像变量一样被用户定义，少数如 Python 3.10 新加入的 `match` 可以被用作变量名（可能是为了向下兼容性）
- **标准库**（**standard library**（**std lib** | **std**））：不是**关键字**，而是你的编程工具给你提前准备好可以直接用的库。
如 Python 中的 `print`，你可以用`_py_print = print`然后`print = *args, **kwargs: _py_print("My printer:", *args, **kwargs)`来重新定义它输出`My printer: <原输出内容>`
其中少数常用的可以像`print`一样直接用，多数需要用`import`语句导入。
但你不需要像第三方库般额外下载它们，一般也不需要额外向解释器或编译器设置它们的路径。
许多标准库是用更低级的语言创造的；但也有许多标准库就是用自身语言写的，或者提供了用自身语言的实现方式（如 CPython 的 `threading` 中提供了对 `threading.RLock` 的 Python 实现，但出于效率考虑在 C 实现可用的状况下会为用户提供 C 实现而非 Python 实现）。
Python 作为有 https://docs.python.org/ 作参考资料的语言，读标准库，对其文档说明、实现效果与源代码进行比对、分析、理解甚至评估，是学习 Python 的很好的入门方式。

---

<br><br>

# Python 面向对象基础

## 简介：编程中的 *面向对象*

我也说不清一些细节，不过了解大概就足够了。

### *面向对象* 是什么

*面向对象* 是区别于 *面向过程* 的一种编程范式。
搜索面向对象的相关信息，可以找到它的三个基本特征是：**封装**、**继承**和**多态**。
一些数据会以**成员**（**member**）的形式被被封装成**类**（**class**），类的**对象**（**object**）一般称作类的**实例**（**instance**）。
简略地，C 语言的代码一般就是面向过程的典型（但用 C 也不是不能实现面向对象的特征，只是麻烦点）；
而C++，Java，Python 等项目中更常用的语言的代码一般都是面向对象的。

### *面向对象* 的意义

*面向对象* 相比 *面向过程* 更贴近人的思维模式。
总地说，面向对象是将现实抽象为代码的重要方式。

### *面向对象* 的代码的语法特点

面向对象的代码的明显标志是常见的关键字 `class`。
`public`、`private`、`protected` 等关键字用来控制封装对象的**成员**的可见性。
在 Java 中可以用 `final` 禁用类的**继承**或禁用某个方法的**多态**（Java: `@Override`）；
在 C++ 中，启用**多态**则需要 `virtual` 关键字。
Python 的面向对象是独特的，以上关键字中仅能用 `class`。

## 函数与方法

```
def func(a, b):
    return a + b

class A:
    def func(self, a, b):
        return a + b
```

`func` 是**函数**（**function**），类里的 `A.func` 是**方法**（**method**）。
和类有关系的一般就是方法。
两者区别微妙，但这种区别能用上，尤其是在给一些工具起名的时候（例如 `types.FunctionType`，`types.MethodType` 两个名字让人明白这是两个易于理解且用途不同的东西）。

## 最简单的类与实例

```
"""demo_1.py"""


class Person:
    """A simple person class."""

    def __init__(self, name):
        # doc here maybe
        self.name = name

    def say_name(self):
        """Print the name of the person."""
        print(f"My name is {self.name}")


if __name__ == "__main__":
    my_name = "Pythoner"
    me = Person(my_name)
    print(f"Produced an instance of Person named {my_name}")
    me.say_name()
```

输出：

```
Produced an instance of Person named Pythoner
My name is Pythoner
```

*示例 1* 中定义了一个类 `Person` 并构造了一个对象 `me`。代码顶部是该文件的文档注释。`__name__` 是 Python 在运行时提供的一个值。当它等于 `"__main__"` 时，该文件是被 Python 解释器直接运行的，而非被其他 Python 文件作为库调用的。

如果有使用过其他语言，会发现变量好像缺少声明并且缺少类型。
运行 Python 并不需要显式标明这些。
虽然这很方便，但在程序体量增大时类型注解会变得十分有必要。

```
"""demo_2.py"""


class Person:
    """A simple person class."""

    name: str

    def __init__(self, name: str) -> None:
        # doc here maybe
        self.name = name

    def say_name(self) -> None:
        """Print the name of the person."""
        print(f"My name is {self.name}")


if __name__ == "__main__":
    my_name = "Pythoner"
    me = Person(my_name)
    print(f"Produced an instance of Person named {my_name}")
    me.say_name()
```

*示例 2* 中，方法外的 `name: str` 注明此类的公有属性 `name`，函数的参数和行尾":"之间靠 `-> None` 注明此构造方法无返回值。最下方的代码中定义的变量不会被复用，且其类型都十分明确，一般不注解。同理，方法中的局部变量一般不注解。

`class`关键字用于定义一个类。`def` 关键字用于定义一个函数或方法。`f"is {name}"` 用于输出格式化的字符串，其值等于 `"is " + name`。

`Person.__init__` 是类的构造方法，在使用`Person(name: str)` 将实例**构造后** Python 会自动以相同的 `name` 为参数调用该构造方法。用`.`访问类里面定义的公有的东西。

## Python 特色面向对象

### 实例方法与静态方法

`Person.__init__` 和 `Person.say_name` 有二、一个参数，但调用的时候都似乎只用了一、零个参数。
这是因为它们都是**实例方法**。
实例方法在调用时必须传入实例自身作为参数 1 `self`。
一般用 `me.say_name()` 的形式调用，也可以用 `Person.say_name(me)`。`Person.__init__` 同理，只是它是在 `self` 对象被隐式的 `Person.__new__` 返回后由 Python 自动传入 `self` 及其他参数并调用的。

也有不需要依赖于类的实例即可调用的方法，例如 Java 中的主函数：

```
public static void main(String[] args) {
}
```

`static` 关键字指明了这是个**静态方法**。Python 虽然没有 `static`，但有 装饰器 `staticmethod` 和 `classmethod`（用于定义**类方法**）。可以像这样使用它：

```
"""demo_3.py"""

from typing import Self


class Person:
    """A simple person class."""

    name: str

    @staticmethod
    def say_name_by_class(name: str) -> None:
        """Print name by class."""
        print(f"Class: The name is {name}")

    @classmethod
    def from_name(cls, name: str) -> Self:
        """Construct a person from a name."""
        return cls(name)

    def __init__(self, name: str) -> None:
        # doc here maybe
        self.name = name


if __name__ == "__main__":
    me = Person.from_name("Pythoner")
    Person.say_name_by_class(me.name)
```

输出：

```
Class: The name is Pythoner
```

静态方法和类方法都可以通过类名直接调用。其中类方法接受类自身作为参数 1 `cls`，静态方法不需要额外的参数 1。

*示例 3* 中用 `Person.from_name` 构造了 `me`，该类方法用 `typing.Self` 注明返回值为 `Person` 的实例。然后用 `Person.say_name_by_class` 直接输出 `me.name`。

装饰器是以其他函数作为参数的函数，也可以像这样用：

```
class A:

    def __f(x: int) -> int:
        return x + 1

    f = staticmethod(__f)
```

### 可见性与属性

前文提及了 `public`、`protected`和`private` 三个常见而重要的关键字，分别用于定义 **公有**、**受保护**和**私有**的成员，但 Python 里没有它们。
Python 里，类的所有属性、方法都是可以被外部访问的，几乎相当于全为公有属性。
但向其他人或 IDE 说明某个东西是**受保护**或**私有**的，是很容易的事，就像这样：

```
"""demo_4.py"""


class Person:
    """A simple person class."""

    __name: str
    __age: int

    def __init__(self, name: str) -> None:
        self.__name = name
        self.__age = 0

    def say_fake_age(self) -> None:
        """Say a wrong age."""
        print(self.__age + 100)

    def _say_true_age(self) -> None:
        """Say the true age."""
        print(self.__age)

    def get_name(self) -> str:
        """Return the person's name."""
        return self.__name

    def get_age(self) -> int:
        """Return the person's age."""
        return self.__age

    def set_age(self, age: int) -> None:
        """Set the person's age."""
        self.__age = age


if __name__ == "__main__":
    me = Person("Pythoner")
    me.set_age(1)

    print(f"{me.get_name()} says:")
    me.say_fake_age()

    # print(f"Then {me.__name} says:")        # error
    print(f"Then {me._Person__name} says:")   # fine
    me._say_true_age()
```

输出：

```
Pythoner says:
101
Then Pythoner says:
1
```

*示例 4* 中为 Person 设置了两个私有属性（或私有**字段**（**field**）），同时提供了经典风格的 **getter** 和 **setter**。它们是外部访问属性 `name` 和 `age` 的正确方式。

Python 惯例上，private 成员名以双下划线 `__`开头，protected 成员名以单下划线 `_` 开头。外部实际上可以访问这两种成员。
`Person._set_true_age` 的单下划线表明不推荐外部直接访问它，`Person.__name` 的双下划线表明更不推荐外部直接访问它。

Python 在运行时会重命名私有成员名，例如将 `Person.__age` 重命名为 `Person._Person__age`。
标记了注释 `# error` 的一行在运行时会导致报错（但我的 VSCode 的类型检查插件不会报错），而标记了注释 `# fine` 的一行则能正常运行（但插件会报错）。

可以用 `property` 装饰器为属性生成 Python 规范的 **getter** 和 **setter**：

```
"""demo_5.py"""


class Person:
    """A simple person class."""

    __name: str
    __age: int

    def __init__(self, name: str) -> None:
        self.__name = name
        self.__age = 0

    @property
    def name(self) -> str:
        """Return the person's name."""
        return self.__name

    @property
    def age(self) -> int:
        """Return the person's age."""
        return self.__age

    @age.setter
    def age(self, age: int) -> None:
        self.__age = age


if __name__ == "__main__":
    me = Person("Pythoner")
    me.age = 1

    print(f"{me.name}'s age is {me.age}")
```

输出:

```
Pythoner's age is 1
```

装饰器 `property` 通过实现**描述器**（**descriptor**）接口中的 `__get__`，`__set__` 让 `getter` 和 `setter` 更简洁。

### 继承与多态

#### 基础继承与多态

```
"""demo_6.py"""


class A:

    _a: int

    def __init__(self, a: int) -> None:
        print(f"A.__init__({a})")
        self._a = a

    def func(self, b: int) -> int:
        print(f"A.func({b})")
        return self._a + b

    @property
    def a(self) -> int:
        print("A.a.__get__")
        return self._a


class B1(A):

    @property
    def a(self) -> int:
        print("B1.a.__get__")
        return self._a + 1


class B2(A):

    def __init__(self, a: int) -> None:
        print(f"B2.__init__({a})")
        super().__init__(a + 1)

    @property
    def a(self) -> int:
        print("B1.a.__get__")
        return super().func(self._a)


class C(B1, B2):
    def __init__(self, a: int) -> None:
        print(f"C.__init__({a})")
        B2.__init__(self, a)


INIT_ARG = 3

if __name__ == "__main__":
    print("b1.a", B1(INIT_ARG).a)
    print("b2.a", B2(INIT_ARG).a)
    print("c.a", C(INIT_ARG).a)
```

输出：

```
A.__init__(3)
B1.a.__get__
b1.a == 4
B2.__init__(3)
A.__init__(4)
B1.a.__get__
A.func(4)
b2.a == 8
C.__init__(3)
B2.__init__(3)
A.__init__(4)
B1.a.__get__
c.a == 5
```

用 `isinstance(B1(0), A)` 可以判断 `B1(0)` 是否是 `A` 的实例，这里返回值是 `True`。用 `isinstance(B1(0), (B1, B2))` 可以判断 `B1(0)` 是否是 `B1` 或 `B2` 的实例，这里返回值也是 `True`。用 `issubclass(B1, A)` 可以判断 `B1` 是否是 `A` 的子类，返回值也是 `True`。

#### 多重继承的原理

注意到*示例 6* `C` 中同时继承了 `B1` 和 `B2` 两个类。`A` 至 `C` 形成了菱形继承。Java 是做不到这个的。Python 的类访问成员的方式很特殊。Python 为每个不做特殊说明的类创建了创建了字典 `__dict__`，可以用 `vars(a := A(0))` 访问 对象 `a` 的 `__dict__` 成员。**字典**是标准库中实现的 `collections.abc.MutableMapping[str, object]`，是字符串到成员对象的可变映射，故而当键为字符串值为任何对象时能充当**命名空间**（**namespace**）的功能。由于 Python 中类访问属性一般是这样实现的，所以菱形继承是理所当然能实现的事。此外，这也允许为对象动态添加成员。`__init__` 中赋值操作都是在动态添加成员（`_a: int` 这类语句仅为类型说明并不实际影响运行），类外为对象动态添加成员也是可行的。

#### 使用 `__slots__`

Python 的 `__dict__` 也对性能造成了影响。一般不明显，但有些时候通过禁用 `__dict__` 能提高访问效率。禁止生成 `__dict__` 改用 `__slots__` 可以禁用动态添加字段，将字段的名字、数量在一开始就都限定好，就像 Java 那样。这时，`__slots__` 若冲突，则无法完成多重继承。`__slots__` 为空时完全不会影响继承，所以 Python 中定义接口很适合使用 `__slots__ = ()`（虽然 Python 没有 `interface` 关键字。Java 里的**接口**（**interface**）也是没有字段的，一个 Java 类也能实现（**implements**）多个接口。
