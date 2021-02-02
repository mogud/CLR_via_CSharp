 # 第 16 章 数组

 本章内容

* <a href="#16_1">初始化数组元素</a>
* <a href="#16_2">数组转型</a>
* <a href="#16_3">所有数组都隐式派生自 `System.Array`</a>
* <a href="#16_4">所有数组都隐式实现 `IEnumerable`、`ICollection` 和 `IList`</a>
* <a href="#16_5">数组的传递和返回</a>
* <a href="#16_6">创建下限非零的数组</a>
* <a href="#16_7">数组的内部工作原理</a>
* <a href="#16_8">不安全的数组访问和固定大小的数组</a>

数组是允许将多个数据项作为集合来处理的机制。CLR 支持一维、多维和交错数组(即数组构成的数组)。所有数组类型都隐式地从 `System.Array` 抽象类派生，后者又派生自 `System.Object`。这意味着数组始终是引用类型，是在托管堆上分配的。在应用程序的变量或字段中，包含的是对数组的引用，而不是包含数组本身的元素。下面的代码更清楚地说明了这一点：

```C#
Int32[] myIntegers;                 // 声明一个数组引用
myIntegers = new Int32[100];        // 创建含有 100 个 Int32 的数组
```

第一行代码声明 `myIntegers` 变量，它能指向包含 `Int32` 值的一维数组。`myIntegers` 刚开始设为 `null`，因为当时还没有分配数组。第二行代码分配了含有 100 个 `Int32` 值的数组，所有 `Int32` 都被初始化为 0。由于数组是引用类型，所以会在托管堆上分配容纳 100 个未装箱`Int32`所需的内存块。实际上，除了数组元素，数组对象占据的内存块还包含一个类型对象指针、一个同步块索引和一些额外的成员<sup>①<sup>。该数组的内存块地址被返回并保存到`myIntegers`变量中。

还可创建引用类型的数组：

```C#
Control[] myControls;               // 声明一个数组引用
myControls = new Control[50];       // 创建含有 50 个 Control 引用的数组
```

第一行代码声明`myControls` 变量，它能指向包含 `Control` 引用的一维数组。`myControls` 刚开始被设为 `null`，因为当时还没有分配数组。第二行代码分配了含有 50 个 `Control` 引用的数组，这些引用全被初始化为`null`。由于 `Control` 是引用类型，所以创建数组只是创建了一组引用，此时没有创建实际的对象。这个内存块的地址被返回并保存到 `myControls` 变量中。

图 16-1 展示了值类型的数组和引用类型的数组在托管堆中的情况。

![16_1](../resources/images/16_1.png)  

图 16-1 值类型和引用类型的数组在托管堆中的情况

图 16—1 中，`Control` 数组显示了执行以下各行代码之后的结果：

```C#
myControls[1] = new Button();
myControls[2] = new TextBox();
myControls[3] = myControls[2];
myControls[46] = new DataGrid();
myControls[48] = new ComboBox();
myControls[49] = new Button();
```
