# STL容器
## 目录 or TODO
- [ ] 1.bitset
- [ ] 2.priority_queue
- [ ] 3.map 与 set
- [ ] 4.string
## 正文

### 1. bitset



### 2. priority_queue

#### 2.1. 基本概念

队列中元素按照优先级排列，由堆实现，没有迭代器

定义：`priority_queue<Type, Container, Functional>`

`Type` ——数据类型，`Container`——容器类型（必须是用数组实现的容器，比如vector,deque等等，但不能用 list，默认是vector），`Functional` ——比较方式

当需要用自定义的数据类型时才需要传入这三个参数，使用基本数据类型时，只需要传入数据类型，**默认是大顶堆**。

#### 2.2. 用法

##### 2.2.1. 大顶堆、小顶堆

STL默认都是使用`()`比较的，默认比较使用`less`（即'<'运算符），如`sort(a,a+n)`，默认递增排序；但是优先队列是反常识的。

顶就是`q.top()` 队列的第一个元素

```c++
//升序队列，小顶堆
priority_queue <int,vector<int>,greater<int> > q;
//降序队列，大顶堆
priority_queue <int,vector<int>,less<int> >q;
```

`greater` 和`less`是`std`实现的两个仿函数。

##### 2.2.2. 基本类型例子

```c++
priority_queue<int> a; 
string wrds[] { "one", "two", "three", "four"};
// 初始化
priority_queue<string> b {begin(wrds), end(wrds)};
// 先比较第一个元素，第一个相等比较第二个
priority_queue<pair<int, int> > a;
```

##### 2.2.3. 自定义比较函数

###### 2.2.3.1. 运算符重载

**需要常量修饰符修饰和引用修饰参数，并且函数外面也需要const**，因为`less`仿函数的定义如此

```c++
struct less {
    _CXX17_DEPRECATE_ADAPTOR_TYPEDEFS typedef _Ty _FIRST_ARGUMENT_TYPE_NAME;
    _CXX17_DEPRECATE_ADAPTOR_TYPEDEFS typedef _Ty _SECOND_ARGUMENT_TYPE_NAME;
    _CXX17_DEPRECATE_ADAPTOR_TYPEDEFS typedef bool _RESULT_TYPE_NAME;

    _NODISCARD constexpr bool operator()(const _Ty& _Left, const _Ty& _Right) const {
        return _Left < _Right;
    }
};
```

```c++
struct tmp1 {
	int x;
	tmp1(int a) :x(a) {};
    // 默认是less方法，所以重载<; 如果用greater，需要重载>
	bool operator<(const tmp1& a)const {
        // 看的是返回值，返回的是 x 较大的值
		return x < a.x;	// 大顶堆
	}
}; 
// 运算符重载也可以写在结构体外面
bool operator<(tmp1 a, tmp1 b){}
bool operator<(const tmp1 a,const tmp1 b){}
bool operator<(const tmp1 &a,const tmp1 &b){}
// 但是这种方式不可以 bool operator < (node &a, node &b){}
priority_queue<tmp1> d;
```

###### 2.2.3.2. 仿函数重写

**当用class的形式定义时，需要将重载函数声明为public**

```c++
struct tmp2 {
	bool operator()(tmp1 a, tmp1 b) {
		return a.x < b.x;	// 大顶堆
	}
};
bool operator()(tmp1& a，tmp1& b);
bool operator()(const tmp1& a, const tmp1& b);
bool operator()(tmp1 a, tmp1 b); 
priority_queue<tmp1, vector<tmp1>, tmp2> f;
```

#### 2.3. 参考资料

1. [C++ priority_queue(STL priority_queue)用法详解](http://c.biancheng.net/view/480.html)；
2. [关于优先队列priority_queue自定义比较函数用法整理](https://blog.csdn.net/bat67/article/details/77585312);

### 3. map 与 set

#### 3.1. set

在C++中，`set` 和 `map` 分别提供以下三种数据结构，其底层实现以及优劣如下表所示：

| 集合                 | 底层实现 | 是否有序 | 数值是否可以重复 | 能否更改数值 | 查询效率 | 增删效率 |
| -------------------- | -------- | -------- | ---------------- | ------------ | -------- | -------- |
| `std::set`           | 红黑树   | 有序     | 否               | 否           | O(logn)  | O(logn)  |
| `std::multiset`      | 红黑树   | 有序     | 是               | 否           | O(logn)  | O(logn)  |
| `std::unordered_set` | 哈希表   | 无序     | 否               | 否           | O(1)     | O(1)     |

- 红黑树是一种平衡二叉搜索树，所以key值是有序的，但key不可以修改，改动key值会导致整棵树的错乱，所以**只能删除和增加key**。
- 虽然`std::set`、`std::multiset` 的底层实现是红黑树，不是哈希表，但是`std::set`、`std::multiset` 依然使用哈希函数来做映射，只不过底层的符号表使用了红黑树来存储数据。

#### 3.2. map 

| 映射                 | 底层实现 | 是否有序 | 数值是否可以重复 | 能否更改数值 | 查询效率 | 增删效率 |
| -------------------- | -------- | -------- | ---------------- | ------------ | -------- | -------- |
| `std::map`           | 红黑树   | key有序  | key不可重复      | key不可修改  | O(logn)  | O(logn)  |
| `std::multimap`      | 红黑树   | key有序  | key可重复        | key不可修改  | O(logn)  | O(logn)  |
| `std::unordered_map` | 哈希表   | key无序  | key不可重复      | key不可修改  | O(1)     | O(1)     |

`count()`函数检查的是key在其中出现的个数，然而因为key不可重复，所以返回值只有0和1；

1. `map`空间占用率高，因为其内部实现了红黑树，虽然提高了运行效率（低于`unorder_map`），但是因为每一个节点都需要额外保存父节点、孩子节点和红/黑性质，使得每一个节点都占用大量的空间（但占用的内存比`unorder_map`低）
2. `unorder_map`因为内部实现了哈希表，因此其查找速度非常的快；
3. `unorder_map`哈希表的建立比较耗费时间；



#### 3.3. unordered_map

1. ##### 扩容

```c++
for(int i=0; i<190; i++){
    ump.insert(pair<int, string>(i, "amy"));
    cout << "插入 " << i << " - ";
    cout << "桶数量" << ump.bucket_count() << " - ";
    cout << "最大桶数量" << ump.bucket_count() << endl;
}
```

桶数会以大致 **bucket[n] = 2\*bucket[n-1] + 奇数** （1， 3， 5， 9 ...）

```
3 = 2 * 0 + 3
7 = 2 * 3 + 1 
17 = 2 * 7 + 3
37 = 2 * 17 + 3
79 = 2 * 37 + 5
167 = 2 * 79 + 9
337 = 2 * 167 + 5
```



#### 3.4. 参考链接

1. 

### 4. string

#### 4.1. 参考资料

1. [C++ string中的find()函数 - 王陸 - 博客园 (cnblogs.com)](https://www.cnblogs.com/wkfvawl/p/9429128.html)
2. [C++中String类的字符串分割实现 - 小金乌会发光－Z&M - 博客园 (cnblogs.com)](https://www.cnblogs.com/carsonzhu/p/5859552.html)
