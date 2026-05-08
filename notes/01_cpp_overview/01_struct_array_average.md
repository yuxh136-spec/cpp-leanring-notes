# 第一阶段复盘：结构体数组与函数求平均值

这次代码对应的是第一个 PPT 结束后的作业：用面向过程的方法，计算多个人的身高、体重和三围的平均值。

这个版本没有使用类和对象，主要练习的是：

1. `struct` 结构体
2. 结构体数组
3. 函数参数传递
4. `for` 循环求和
5. 从键盘输入数据

---

## 1. 为什么要用结构体

题目中每个人都有 5 个数据：

```cpp
身高 height
体重 weight
胸围 chest
腰围 waist
臀围 hip
```

如果不用结构体，就要写很多零散变量，比如：

```cpp
double height1, weight1, chest1, waist1, hip1;
double height2, weight2, chest2, waist2, hip2;
```

这样数据一多就很乱。

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

这里的 `personData` 是我自己定义出来的一种数据类型。它表示“一个人的身体数据”。

---

## 2. 结构体变量和结构体数组

定义一个结构体变量：

```cpp
personData p1;
```

表示创建一个人的数据。

访问成员时用点号 `.`：

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

如果要访问第 `i + 1` 个人的身高，就写：

```cpp
p[i].height
```

如果要访问第 `i + 1` 个人的体重，就写：

```cpp
p[i].weight
```

---

## 3. 用户输入数据的过程

程序中先定义一个数组：

```cpp
personData p[100];
```

然后输入人数：

```cpp
int num;
cout << "请输入人数： " << endl;
cin >> num;
```

再用循环逐个输入每个人的数据：

```cpp
for (int i = 0; i < num; i++) {
    cout << "请输入第" << i + 1 << "个人的身高、体重、三围 " << endl;
    cin >> p[i].height
        >> p[i].weight
        >> p[i].chest
        >> p[i].waist
        >> p[i].hip;
}
```

这里的 `i` 从 0 开始，所以输出时写 `i + 1`，这样显示给用户看的就是第 1 个人、第 2 个人，而不是第 0 个人。

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

## 4. 函数如何获得结构体数组中的数据

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

- `double`：函数最后返回一个小数
- `getHeightAvg`：函数名，表示求平均身高
- `personData p[]`：接收一个结构体数组
- `int n`：接收人数

调用时：

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

## 5. 五个求平均函数的规律

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

本质都是：

```cpp
平均值 = 总和 / 人数
```

---

## 6. 完整代码

```cpp
#include<iostream>
using namespace std;

struct personData
{
    double height;
    double weight;
    double chest;
    double waist;
    double hip;
};

double getHeightAvg(personData p[], int n) {
    double sum = 0;

    for (int i = 0; i < n; i++) {
        sum = sum + p[i].height;
    }

    return sum / n;
}

double getWeightAvg(personData p[], int n) {
    double sum = 0;

    for (int i = 0; i < n; i++) {
        sum = sum + p[i].weight;
    }

    return sum / n;
}

double getChestAvg(personData p[], int n) {
    double sum = 0;

    for (int i = 0; i < n; i++) {
        sum = sum + p[i].chest;
    }

    return sum / n;
}

double getWaistAvg(personData p[], int n) {
    double sum = 0;

    for (int i = 0; i < n; i++) {
        sum = sum + p[i].waist;
    }

    return sum / n;
}

double getHipAvg(personData p[], int n) {
    double sum = 0;

    for (int i = 0; i < n; i++) {
        sum = sum + p[i].hip;
    }

    return sum / n;
}

int main() {
    personData p[100];

    int num;
    cout << "请输入人数： " << endl;
    cin >> num;

    for (int i = 0; i < num; i++) {
        cout << "请输入第" << i + 1 << "个人的身高、体重、三围 " << endl;
        cin >> p[i].height
            >> p[i].weight
            >> p[i].chest
            >> p[i].waist
            >> p[i].hip;
    }

    cout << "平均身高： " << getHeightAvg(p, num) << endl;
    cout << "平均体重： " << getWeightAvg(p, num) << endl;
    cout << "平均胸围： " << getChestAvg(p, num) << endl;
    cout << "平均腰围： " << getWaistAvg(p, num) << endl;
    cout << "平均臀围： " << getHipAvg(p, num) << endl;

    return 0;
}
```

---

## 7. 测试数据

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

## 8. 运行结果参考

```text
平均身高： 168.25
平均体重： 58.25
平均胸围： 85
平均腰围： 70.5
平均臀围： 90
```

如果想让结果保留两位小数，可以额外加上：

```cpp
#include<iomanip>
```

并在 `main` 函数开始处写：

```cpp
cout << fixed << setprecision(2);
```

---

## 9. 这段代码体现的面向过程思想

这段程序是典型的面向过程写法。

它的结构是：

```text
数据：personData 结构体和结构体数组
过程：getHeightAvg、getWeightAvg 等函数
主流程：main 函数负责输入、调用函数、输出结果
```

也就是说，程序把问题拆成了几个步骤：

1. 定义一个人的数据格式
2. 输入多个人的数据
3. 用函数分别计算各项平均值
4. 输出结果

这里还没有把数据和操作封装到类里面，所以它属于面向过程程序设计。

---

## 10. 我这次需要重点记住的点

1. 结构体可以把多个相关变量打包成一个整体。
2. 结构体数组可以保存多组同类型数据。
3. 函数接收结构体数组时，可以写成 `personData p[]`。
4. 访问结构体成员时使用点号 `.`。
5. `p[i].height` 表示第 `i + 1` 个人的身高。
6. 求平均值的基本方法是先循环求和，再除以人数。
7. 面向过程版本的重点是“数据 + 函数”，而不是“类 + 对象”。
