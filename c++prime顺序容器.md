## 顺序容器

### 迭代器范围

一个迭代器范围由一对迭代器表示，两个迭代器分别指向同一个容器中的元素或者尾元素之后的位置，这两个迭代器通常被称为begin和end；（一般第二个迭代器并不指向容器的最后一个元素，而是最后一个元素的下一个位置），这种元素范围称为**左闭合区间**

使用左闭合范围蕴含的编程假定：

​	1、如果begin和end相等，则范围为空

​	2、如果begin和end不等，则范围至少包含一个元素，且begin指向改范围中的第一个元素

​	3、我们可以对begin递增若干多次，使得begin==end

### 赋值和swap

赋值运算符将其左边容器中的全部元素替换为右边容器中元素的拷贝

```c++
c1=c2;  //将c1的内容替换为c2中元素的拷贝，如果两个容器的原来的大小不同，赋值运算符后两者的大小都与右边容器的原大小相同
c1 ={a,b,c};  //赋值后，c1大小为3
```

### 使用assign（仅顺序容器）

赋值运算符要求左边和右边的运算对象具有相同的类型，它将右边运算的所有元素拷贝到左边运算对象中。顺序容器还定义了一个名为assign的成员，允许我们从一个不同但相容的类型赋值，或者从容器的一个子序列赋值，assign操作用参数所指定的元素（的拷贝）替换左边容器中的所有元素

```c++
list<string> names;
vector<const char *>oldstyle;
names=oldstyle;  //错误，容器类型不匹配
names.assign(oldstyle.cbegin(),oldstyle.cend());  //正确，可以将const char * 转换为string
```

assign还可以定义指定数目且具有相同给定值的元素替换容器中原有的元素

```c++
list<string>slist1(1);  //一个元素，为空string
slist1.assign(10,"hiya!");  //10个元素，每个都是hiya
```

### swap

swap交换两个相同类型容器的内容

```c++
vector<string>svec1(10);  //10个元素的vector
vector<string>svec2(24);//24个元素的vector
swap(svec1,svec2);  //交换的仅仅是两个的数据结构
```

**除array外，swap不会对任何元素进行拷贝、删除或者插入操作，因此可以保证在常数时间内完成**

### vector是如何增长的

当我们向容器中添加元素时，因为元素是连续存储的，不是简单的在后面添加元素，而是将已有元素从旧位置移动到新空间中，然后添加新元素，释放旧空间。

