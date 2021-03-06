# C++结构体
## 目录 or TODO
- [x] 1.总览
- [x] 2.C中的struct与C++中struct的区别
- [x] 3.C++中struct与class的区别
## 正文

### 1. 总览

<img src="https://images.961110.xyz/images/2021/09/27/classstruct.png" alt="class和struct的区别" style="zoom:50%;" />

### 2. C中的struct与C++中struct的区别

|          | C                  | C++                                                    |
| -------- | ------------------ | ------------------------------------------------------ |
| 成员     | 只有数据，没有函数 | 允许同时有函数和数据出现，允许通过构造函数进行初始化等 |
| 访问权限 | 没有访问权限的设定 | private、protected、public三种。默认的访问权限是public |
| 继承     | 不存在继承的关系   | 可以继承class和struct，也可以被class和struct继承       |

### 3. C++中struct与class的区别

在C++中，为了保持对于C的兼容，保留了struct，并在此基础上对struct进行了扩充。C++中的struct可以包含成员函数，**可以继承，可以实现多态**，但是在C++中的struct还是与class有一些区别的：
struct是一个数据结构的实现体；而class更像是一个对象的实现体。

|                  | struct                          | class                            |
| ---------------- | ------------------------------- | -------------------------------- |
| 默认访问权限     | public                          | private                          |
| 默认继承访问权限 | public，可以继承于struct、class | private，可以继承于struct、class |
| 定义模板参数不能 | 不能                            | 可以用于定义模板参数             |

struct做子类的时候，默认是public的；class做子类的时候，默认是private的。与基类无关。