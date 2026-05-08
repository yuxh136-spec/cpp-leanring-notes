# 第一阶段复盘：结构体数组与函数求平均值

这次代码对应第一个 PPT 结束后的作业：用**面向过程**的方法，计算多个人的身高、体重和三围的平均值。

这个版本暂时不使用类和对象，主要练习的是：

1. `struct` 结构体
2. 结构体数组
3. 函数参数传递
4. `for` 循环求和
5. 从键盘输入数据
6. 用函数拆分程序逻辑

---

## 1. 题目分析

题目要求计算：

```text
平均身高
平均体重
平均胸围
平均腰围
平均臀围
```

其中“三围”一般指：

```text
胸围 chest
腰围 waist
臀围 hip
```

所以每个人需要保存 5 个数据：

```cpp
height  // 身高
weight  // 体重
chest   // 胸围
waist   // 腰围
hip     // 臀围
```

这道题的核心算法并不复杂，本质都是：

```text
平均值 = 总和 / 人数
```

真正需要掌握的是：如何把多个人的数据组织起来，并把这些数据传给函数处理。

---

## 2. 为什么要用结构体

如果不用结构体，可能要写很多零散变量：

```cpp
double height1, weight1, chest1, waist1, hip1;
double height2, weight2, chest2, waist2, hip2;
```

这样写的问题是：

- 变量数量很多，不好管理；
- 数据之间的关系不明显；
- 如果人数变多，代码会很乱；
- 后面传给函数时也不方便。

所以我用结构体把一个人的五项数据打包起来：

```cpp
struct personData
{
    double height;
    double weight;
    double chest;
    double waist;
    double hip;
};
```

这里的 `personData` 是我自己定义的一种数据类型，它表示“一个人的身体数据”。

> 小提醒：按照比较常见的 C++ 命名习惯，结构体类型名也可以写成 `PersonData`，首字母大写更像类型名。不过我这次代码里用了 `personData`，也是可以正常运行的。

---

## 3. 结构体变量和结构体数组

定义一个结构体变量：

```cpp
personData p1;
```

表示创建一个人的数据。

访问结构体成员时使用点号 `.`：

```cpp
p1.height
p1.weight
p1.chest
```

如果要保存多个人的数据，就可以定义结构体数组：

```cpp
personData p[100];
```

它表示最多可以保存 100 个人的数据。

其中：

```cpp
p[0]  // 第 1 个人
p[1]  // 第 2 个人
p[2]  // 第 3 个人
```

访问第 `i + 1` 个人的身高：

```cpp
p[i].height
```

访问第 `i + 1` 个人的体重：

```cpp
p[i].weight
```

访问第 `i + 1` 个人的胸围：

```cpp
p[i].chest
```

---

## 4. 用户输入数据的过程

程序中先定义结构体数组：

```cpp
personData p[100];
```

再输入人数：

```cpp
int num;
cout << "请输入人数：" << endl;
cin >> num;
```

然后用循环逐个输入每个人的数据：

```cpp
for (int i = 0; i < num; i++) {
    cout << "请输入第" << i + 1 << "个人的身高、体重、三围：" << endl;
    cin >> p[i].height
        >> p[i].weight
        >> p[i].chest
        >> p[i].waist
        >> p[i].hip;
}
```

这里的 `i` 从 0 开始，所以显示给用户看时写 `i + 1`。

假设 `i = 0`，输入的数据会存入：

```cpp
p[0].height
p[0].weight
p[0].chest
p[0].waist
p[0].hip
```

假设 `i = 1`，输入的数据会存入：

```cpp
p[1].height
p[1].weight
p[1].chest
p[1].waist
p[1].hip
```

---

## 5. 函数如何获得结构体数组中的数据

以求平均身高为例：

```cpp
double getHeightAvg(personData p[], int n) {
    double sum = 0;

    for (int i = 0; i < n; i++) {
        sum = sum + p[i].height;
    }

    return sum / n;
}
```

函数头：

```cpp
double getHeightAvg(personData p[], int n)
```

可以理解为：

- `double`：函数最后返回一个小数；
- `getHeightAvg`：函数名，表示求平均身高；
- `personData p[]`：接收一个结构体数组；
- `int n`：接收人数。

在 `main` 函数里调用：

```cpp
getHeightAvg(p, num)
```

意思是把 `main` 函数里的数组 `p` 和人数 `num` 传给函数。

函数体里通过：

```cpp
p[i].height
```

获得第 `i + 1` 个人的身高。

所以这一句：

```cpp
sum = sum + p[i].height;
```

意思就是把每个人的身高依次加到 `sum` 里面。

最后：

```cpp
return sum / n;
```

表示总身高除以人数，得到平均身高。

---

## 6. 为什么函数参数里还要传 `n`

虽然函数接收了数组：

```cpp
personData p[]
```

但是函数并不知道用户实际输入了几个人。

例如数组最大开了 100 个位置：

```cpp
personData p[100];
```

但用户可能只输入了 4 个人。

所以必须额外传入人数：

```cpp
int n
```

这样函数才知道循环应该执行几次：

```cpp
for (int i = 0; i < n; i++)
```

如果不传 `n`，函数就不知道应该算前 4 个、前 10 个，还是前 100 个。

---

## 7. 五个求平均函数的规律

这几个函数结构都一样，只是访问的成员不同。

平均身高：

```cpp
sum = sum + p[i].height;
```

平均体重：

```cpp
sum = sum + p[i].weight;
```

平均胸围：

```cpp
sum = sum + p[i].chest;
```

平均腰围：

```cpp
sum = sum + p[i].waist;
```

平均臀围：

```cpp
sum = sum + p[i].hip;
```

核心步骤都是：

```text
1. 定义 sum 保存总和
2. 用 for 循环把每个人的某一项数据加起来
3. 返回 sum / n
```

---

## 8. 程序执行流程

这段程序大概按下面的顺序执行：

```text
1. 定义结构体 personData
2. 在 main 函数中定义结构体数组 p[100]
3. 输入人数 num
4. 循环输入每个人的身高、体重、胸围、腰围、臀围
5. 调用 getHeightAvg、getWeightAvg 等函数
6. 每个函数内部循环求和并返回平均值
7. main 函数输出所有平均值
```

可以把数据流理解成：

```text
用户输入数据
    ↓
p[i].height / p[i].weight / p[i].chest / p[i].waist / p[i].hip
    ↓
传给求平均值函数
    ↓
for 循环求和
    ↓
返回平均值
    ↓
cout 输出结果
```

---

## 9. 基础优化后的完整代码

这个版本在原来的基础上做了几个小优化：

1. 加入 `#include <iomanip>`，让输出结果保留两位小数；
2. 用 `const int MAX_NUM = 100;` 表示最多人数，避免代码里直接写死 100；
3. 增加人数检查，防止输入 0、负数或者超过数组容量；
4. 函数参数写成 `const personData p[]`，表示函数只读取数组，不修改数组内容；
5. 输出提示更清楚一些。

```cpp
#include <iostream>
#include <iomanip>
using namespace std;

const int MAX_NUM = 100;

struct personData
{
    double height;
    double weight;
    double chest;
    double waist;
    double hip;
};

double getHeightAvg(const personData p[], int n) {
    double sum = 0;

    for (int i = 0; i < n; i++) {
        sum = sum + p[i].height;
    }

    return sum / n;
}

double getWeightAvg(const personData p[], int n) {
    double sum = 0;

    for (int i = 0; i < n; i++) {
        sum = sum + p[i].weight;
    }

    return sum / n;
}

double getChestAvg(const personData p[], int n) {
    double sum = 0;

    for (int i = 0; i < n; i++) {
        sum = sum + p[i].chest;
    }

    return sum / n;
}

double getWaistAvg(const personData p[], int n) {
    double sum = 0;

    for (int i = 0; i < n; i++) {
        sum = sum + p[i].waist;
    }

    return sum / n;
}

double getHipAvg(const personData p[], int n) {
    double sum = 0;

    for (int i = 0; i < n; i++) {
        sum = sum + p[i].hip;
    }

    return sum / n;
}

int main() {
    personData p[MAX_NUM];

    int num;
    cout << "请输入人数：" << endl;
    cin >> num;

    if (num <= 0 || num > MAX_NUM) {
        cout << "人数输入错误，人数应在 1 到 " << MAX_NUM << " 之间。" << endl;
        return 0;
    }

    for (int i = 0; i < num; i++) {
        cout << "请输入第" << i + 1 << "个人的身高、体重、胸围、腰围、臀围：" << endl;
        cin >> p[i].height
            >> p[i].weight
            >> p[i].chest
            >> p[i].waist
            >> p[i].hip;
    }

    cout << fixed << setprecision(2);

    cout << "平均身高：" << getHeightAvg(p, num) << " cm" << endl;
    cout << "平均体重：" << getWeightAvg(p, num) << " kg" << endl;
    cout << "平均胸围：" << getChestAvg(p, num) << " cm" << endl;
    cout << "平均腰围：" << getWaistAvg(p, num) << " cm" << endl;
    cout << "平均臀围：" << getHipAvg(p, num) << " cm" << endl;

    return 0;
}
```

---

## 10. 测试数据

可以输入：

```text
4
170 60 86 72 90
165 53 82 68 88
178 70 92 78 96
160 50 80 64 86
```

对应含义：

```text
第1个人：身高170，体重60，胸围86，腰围72，臀围90
第2个人：身高165，体重53，胸围82，腰围68，臀围88
第3个人：身高178，体重70，胸围92，腰围78，臀围96
第4个人：身高160，体重50，胸围80，腰围64，臀围86
```

---

## 11. 运行结果参考

```text
平均身高：168.25 cm
平均体重：58.25 kg
平均胸围：85.00 cm
平均腰围：70.50 cm
平均臀围：90.00 cm
```

---

## 12. 这段代码体现的面向过程思想

这段程序是典型的面向过程写法。

它的结构是：

```text
数据：personData 结构体和结构体数组
过程：getHeightAvg、getWeightAvg 等函数
主流程：main 函数负责输入、调用函数、输出结果
```

也就是说，程序把问题拆成了几个步骤：

1. 定义一个人的数据格式；
2. 输入多个人的数据；
3. 用函数分别计算各项平均值；
4. 输出结果。

这里还没有把数据和操作封装到类里面，所以它属于面向过程程序设计。

如果后面改成面向对象版本，就会把这些数据和操作放进类里，例如定义一个 `Person` 类或者 `AverageCounter` 类。

---

## 13. 常见易错点

### 1. 结构体定义后面忘记写分号

错误示例：

```cpp
struct personData
{
    double height;
    double weight;
}
```

正确写法：

```cpp
struct personData
{
    double height;
    double weight;
};
```

结构体定义结束后，最后必须有一个分号。

### 2. 把“三围”写成“三维”

这里应该写“三围”，不是“三维”。

```text
三围 = 胸围 + 腰围 + 臀围
```

### 3. 忘记数组下标从 0 开始

```cpp
p[0]  // 第 1 个人
p[1]  // 第 2 个人
```

所以在提示用户输入时，通常写：

```cpp
cout << "请输入第" << i + 1 << "个人的数据";
```

### 4. 忘记传入人数 `n`

函数接收数组时，最好同时传入人数：

```cpp
double getHeightAvg(const personData p[], int n)
```

否则函数不知道应该处理几个元素。

### 5. 没检查人数是否合法

如果数组只有 100 个位置，而用户输入 200，就会造成数组越界。

所以可以加上：

```cpp
if (num <= 0 || num > MAX_NUM) {
    cout << "人数输入错误" << endl;
    return 0;
}
```

---

## 14. 我这次需要重点记住的点

1. 结构体可以把多个相关变量打包成一个整体。
2. 结构体数组可以保存多组同类型数据。
3. 函数接收结构体数组时，可以写成 `personData p[]`。
4. 如果函数不需要修改数组，可以写成 `const personData p[]`。
5. 访问结构体成员时使用点号 `.`。
6. `p[i].height` 表示第 `i + 1` 个人的身高。
7. 求平均值的基本方法是先循环求和，再除以人数。
8. 面向过程版本的重点是“数据 + 函数”，而不是“类 + 对象”。

---

## 15. 后续可以继续优化的方向

目前这个版本适合第一阶段练习。以后学到更多知识后，还可以继续优化：

1. 用类和对象重写，完成面向对象版本；
2. 用数组或枚举减少五个求平均函数的重复代码；
3. 用 `vector` 代替固定大小数组；
4. 把输入、计算、输出分别拆成更独立的函数；
5. 用文件保存测试数据和运行结果。

这次阶段先重点掌握结构体数组和函数传参即可。
