# std::array<> 

## std::array<> 的能力

1. **内部实现**：
    元素存储在静态 C 风格数组中，是一个有序集合，支持随机访问。
2. **迭代器支持**：
    提供随机访问迭代器，可用于所有 STL 算法。
3. **性能最佳**：
    固定大小序列使用 `std::array<>` 性能最佳，内存分配在栈上，无需重新分配，支持随机访问。

------

### 初始化

1. **默认初始化**：
    默认构造的基本类型元素值未定义。

   ```cpp
   std::array<int, 4> x; // 未定义值
   ```

2. **显式初始化为零**：
    使用空的初始化列表初始化为零。

   ```cpp
   std::array<int, 4> x = {}; // 所有元素初始化为 0
   ```

3. **列表初始化**：
    满足聚合类型要求，可使用列表初始化。

   ```cpp
   std::array<int, 5> coll = {42, 377, 611, 21, 44};
   ```

4. **部分初始化**：
    列表不足时，剩余元素使用默认构造函数初始化。

   ```cpp
   std::array<int, 10> c2 = {42}; // 一个元素为 42，其余元素为 0
   ```

5. **超出大小限制**：
    初始化列表长度超过数组大小会报错。

   ```cpp
   std::array<int, 5> c3 = {1, 2, 3, 4, 5, 6}; // 错误
   ```

6. **不能使用括号初始化**：

   ```cpp
   std::array<int, 5> a({1, 2, 3, 4, 5, 6}); // 错误
   std::vector<int> v({1, 2, 3, 4, 5, 6});   // 正确
   ```

------

### `swap()` 和移动语义

1. **`swap()`**：
    提供 `swap()`，但无法通过交换指针实现，复杂度为线性。

   ```cpp
   std::array<int, 5> a1, a2;
   std::swap(a1, a2); // 线性复杂度
   ```

2. **移动语义**：
    支持隐式移动语义。

   ```cpp
   std::array<std::string, 10> as1, as2;
   as1 = std::move(as2);
   ```

------

### 特殊情况：大小为 0 的数组

1. **空数组**：
    可创建大小为 0 的数组，但访问元素或 `front()`、`back()` 会导致运行时错误。

   ```cpp
   std::array<int, 0> coll; // 空数组
   coll[5] = 42; // 运行时错误
   ```

2. **`data()` 返回值**：
    调用 `data()` 返回未指定值，但不能解引用。

## `std::array<>` 的创建、复制和销毁

以下是 `std::array<>` 的构造函数和析构函数的功能概述：

------

### 默认构造和初始化

- 默认构造函数会

  默认初始化

  所有元素。

  - 对于基本类型，默认初始化的值是**未定义的**。
  - 如果提供初始化列表但元素不足，剩余的元素将通过默认构造函数初始化（对于基本类型为 `0`）。

**示例**：

```cpp
std::array<int, 5> arr1;            // 元素未定义
std::array<int, 5> arr2 = {1, 2};  // 剩余元素初始化为 0
```

构造函数概览


| **操作**                      | **效果**                                                 | **示例**                                                     |
| ----------------------------- | -------------------------------------------------------- | ------------------------------------------------------------ |
| `array<Elem, N> c`            | 默认构造函数，创建默认初始化的数组。                     | `std::array<int, 3> c; // 未定义值（基本类型）`              |
| `array<Elem, N> c(c2)`        | 复制构造函数，创建相同类型数组的副本（所有元素被复制）。 | `std::array<int, 3> c2 = {1, 2, 3}; std::array<int, 3> c(c2);` |
| `array<Elem, N> c = c2`       | 复制构造函数，创建相同类型数组的副本（所有元素被复制）。 | `std::array<int, 3> c = c2;`                                 |
| `array<Elem, N> c(rv)`        | 移动构造函数（C++11），从右值 `rv` 中转移元素的内容。    | `std::array<int, 3> rv = {1, 2, 3}; std::array<int, 3> c(std::move(rv));` |
| `array<Elem, N> c = rv`       | 移动构造函数（C++11），从右值 `rv` 中转移元素的内容。    | `std::array<int, 3> c = std::move(rv);`                      |
| `array<Elem, N> c = initlist` | 使用初始化列表创建数组，数组元素与初始化列表一致。       | `std::array<int, 3> c = {1, 2, 3};`                          |

### 析构函数

- 因为 `std::array<>` 存储的元素是静态分配的（在栈上），析构函数在对象超出作用域时自动销毁。
- 如果数组中的元素类型具有析构函数（例如指针或自定义类），其析构函数会在 `std::array<>` 被销毁时自动调用。

------

### 关键特点总结

1. **默认构造函数**：未初始化的基本类型值可能是未定义的。
2. **列表初始化**：允许部分初始化，剩余元素使用默认构造函数。
3. **禁止括号初始化**：只能使用花括号。
4. **复制和移动**：支持复制构造、复制赋值以及移动构造、移动赋值。

示例代码

```cpp
#include <array>
#include <iostream>
#include <string>

int main() {
    // 默认构造
    std::array<int, 5> arr1;       // 未定义值
    std::array<int, 5> arr2 = {}; // 全部初始化为 0

    // 列表初始化
    std::array<int, 5> arr3 = {1, 2, 3}; // 剩余元素初始化为 0

    // 复制构造
    std::array<int, 5> arr4 = arr3;

    // 移动构造
    std::array<int, 5> arr5 = std::move(arr4);

    // 输出数组内容
    for (const auto& elem : arr3) {
        std::cout << elem << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

### `std::array<>` 的赋值操作

以下是 `std::array<>` 提供的赋值操作及其效果：

------

| **操作**       | **效果**                                                     |
| -------------- | ------------------------------------------------------------ |
| `c = c2`       | 将数组 `c2` 的所有元素赋值给数组 `c`（逐元素赋值）。         |
| `c = rv`       | 从右值 `rv` 中移动所有元素到数组 `c`（C++11 起支持移动赋值）。 |
| `c.fill(val)`  | 将值 `val` 赋给数组 `c` 的每个元素。                         |
| `c1.swap(c2)`  | 交换数组 `c1` 和 `c2` 的数据内容（两数组类型和大小必须相同）。 |
| `swap(c1, c2)` | 交换数组 `c1` 和 `c2` 的数据内容（与 `c1.swap(c2)` 等效）。  |

------

### 特点与注意事项

1. **`=` 赋值**：

   - 支持复制赋值和移动赋值。
   - 对每个元素调用其类型的赋值操作符。

   示例：

   ```cpp
   std::array<int, 3> arr1 = {1, 2, 3};
   std::array<int, 3> arr2;
   arr2 = arr1; // 复制赋值
   ```

2. **`fill(val)`**：

   - 为所有元素赋同一个值，简单高效。
   - 调用元素类型的赋值操作符。

   示例：

   ```cpp
   std::array<int, 5> arr;
   arr.fill(42); // 所有元素都被赋值为 42
   ```

3. **`swap()`**：

   - 交换两个数组的内容。
   - 因为 `std::array<>` 是静态分配的，`swap()` 无法像动态容器那样只交换指针。
   - **复杂度**：线性（O(n)），每个元素的值会被逐一交换。

   示例：

   ```cpp
   std::array<int, 3> arr1 = {1, 2, 3};
   std::array<int, 3> arr2 = {4, 5, 6};
   arr1.swap(arr2); // 交换内容
   ```

4. **内部实现**：

   - 所有这些操作都会调用元素类型的赋值操作符。
   - 如果元素类型是自定义类型，需确保其赋值操作符已正确实现。

示例代码

```cpp
#include <array>
#include <iostream>

int main() {
    std::array<int, 3> arr1 = {1, 2, 3};
    std::array<int, 3> arr2;

    // 复制赋值
    arr2 = arr1;
    for (const auto& elem : arr2) {
        std::cout << elem << " "; // 输出: 1 2 3
    }
    std::cout << std::endl;

    // fill()
    arr2.fill(42);
    for (const auto& elem : arr2) {
        std::cout << elem << " "; // 输出: 42 42 42
    }
    std::cout << std::endl;

    // swap()
    std::array<int, 3> arr3 = {7, 8, 9};
    arr2.swap(arr3);
    for (const auto& elem : arr2) {
        std::cout << elem << " "; // 输出: 7 8 9
    }

    return 0;
}
```

------

### 总结

- **复制赋值**和**移动赋值**需要两数组类型和大小相同。
- **`fill(val)`** 高效为所有元素赋同一个值。
- **`swap()`** 无法保证常量复杂度，因为每个元素都需要逐一交换值。



## `std::array<>` 的元素访问

### 直接访问操作

| **操作**    | **效果**                                                     |
| ----------- | ------------------------------------------------------------ |
| `c[idx]`    | 返回索引为 `idx` 的元素，不进行范围检查，索引越界会导致未定义行为。 |
| `c.at(idx)` | 返回索引为 `idx` 的元素，**会进行范围检查**，索引越界时抛出 `std::out_of_range` 异常。 |
| `c.front()` | 返回第一个元素，不检查容器是否为空，空容器调用此函数会导致未定义行为。 |
| `c.back()`  | 返回最后一个元素，不检查容器是否为空，空容器调用此函数会导致未定义行为。 |

------

### 使用范围 for 循环或迭代器

- **范围 for 循环**：
   遍历所有元素的首选方式。

  ```cpp
  for (const auto& elem : arr) {
      std::cout << elem << " ";
  }
  ```

- **迭代器**：

  - `begin()` 和 `end()`：返回可写迭代器。
  - `cbegin()` 和 `cend()`：返回只读迭代器。
  - `rbegin()` 和 `rend()`：返回反向可写迭代器。
  - `crbegin()` 和 `crend()`：返回反向只读迭代器。

### 元组接口访问

`std::array<>` 是 C++ 标准库提供的一个定长数组容器，它兼具数组和容器的特性，并支持多种访问方式。其中，通过 `std::get<>` 提供的元组接口访问元素是一个显著的特点。

1. **通过 `std::get<>` 函数访问元素**

   - `std::get<>` 是标准库提供的一个泛型函数模板，适用于 `std::tuple<>` 和 `std::array<>` 等容器。

   - 对于 `std::array<>`，`std::get<>` 允许以类似访问 `std::tuple<>` 元素的方式，通过索引直接访问 `std::array` 的元素。

   - 如果给定的索引超出 `std::array<>` 的范围，编译器会直接报错，避免运行时访问越界。

     ```cpp
     std::array<int, 3> arr = {10, 20, 30};
     std::cout << std::get<3>(arr); // 错误，索引 3 超出范围
     ```

     这在一定程度上提高了代码的安全性和效率，因为编译期就能捕获错误。

2. **索引必须是编译期常量**

   - `std::get<>` 是一个模板函数，索引值作为模板参数必须在编译期确定。
   - 这意味着你不能在运行时动态传递索引，只能传递静态的整数值。

   ```cpp
   constexpr size_t index = 1; 
   std::cout << std::get<index>(arr); // 允许，因为 index 是编译期常量
   ```

   如果尝试使用非编译期常量的值，则会导致编译错误：

   ```cpp
   size_t index = 1;
   std::cout << std::get<index>(arr); // 错误，index 不是编译期常量
   ```

   >如果你需要在运行时动态传递索引来访问 `std::array` 的元素，`std::get<>` 无法满足需求，因为它的索引必须是编译期常量。要实现运行时动态索引，有以下几种替代方案：
   >
   >**1. 使用 `operator[]`**
   >
   >`std::array` 提供了经典的 `operator[]` 和 `at()` 方法，允许通过运行时索引访问元素：
   >
   >```cpp
   >#include <array>
   >#include <iostream>
   >
   >int main() {
   >    std::array<int, 3> arr = {10, 20, 30};
   >    
   >    size_t runtime_index = 1; // 运行时确定的索引
   >    std::cout << arr[runtime_index] << std::endl; // 输出: 20
   >
   >    return 0;
   >}
   >```
   >
   >- 简洁直接，适合绝大多数场景。
   >- `operator[]` 不执行边界检查，但效率更高。
   >
   >#### 注意事项：
   >
   >- 如果需要边界检查，可以使用 `at()` 方法：
   >
   >```cpp
   >std::cout << arr.at(runtime_index) << std::endl; // 超出范围会抛出 std::out_of_range 异常
   >```
   >
   >**2. 使用 `std::variant` 和 `std::visit`**
   >
   >如果你确实需要动态选择某种行为，且数据结构类似于 `std::tuple` 或 `std::array`，可以结合 `std::variant` 和 `std::visit` 来实现。
   >
   >**3. 模拟动态索引行为**
   >
   >有时需要处理复杂逻辑，可以通过函数指针或映射表来模拟。

   

   >1. **`constexpr` 的作用**
   >   - `constexpr` 关键字声明了一个常量表达式。
   >   - `index` 被标记为 `constexpr`，表明其值在编译期就确定为 `1`，而不是在运行时才决定。
   >2. **编译期常量与模板参数**
   >   - `std::get<>` 是一个模板函数，其参数 `index` 必须是一个编译期常量，因为模板参数需要在编译时确定。
   >   - 由于 `index` 是用 `constexpr` 定义的，所以它的值在编译时是已知的，因此可以被用作模板参数。
   >
   >为什么 `std::get<>` 需要编译期常量**
   >
   >- ```
   >  std::get<>
   >  ```
   >
   >   是通过模板参数指定索引的：
   >
   >  ```cpp
   >  template<std::size_t N, typename Array>
   >  decltype(auto) get(Array&& arr);
   >  ```
   >
   >  - 其中 `N` 是非类型模板参数，要求是编译期常量。
   >  - **模板参数的这种设计允许编译器在编译时完成索引的范围检查，提高代码的安全性和性能。**
   >
   >
   >
   >`size_t` 是 C++ 标准库中定义的一种 **无符号整数类型**，用于表示 **对象大小** 或 **数组索引**。
   >
   >1. **定义**
   >   - `size_t` 是一个类型别名，定义在 `<cstddef>` 或 `<cstdlib>` 头文件中。
   >   - 实际上，它是由编译器实现的，与系统的硬件架构有关。
   >   - 常见定义（平台相关）：
   >     - 32 位系统：`typedef unsigned int size_t;`
   >     - 64 位系统：`typedef unsigned long long size_t;`
   >2. **作用**
   >   - 用于表示大小，例如对象的字节数或数组的下标。
   >   - 保证能够存储系统中 **最大可能的对象大小**，因此比普通的整数类型更可靠。
   >3. **无符号类型**
   >   - 因为 `size_t` 是无符号类型，它的值总是非负的。
   >   - 这是因为大小和数组下标不可能是负数。
   >
   >------
   >
   >### **常见使用场景**
   >
   >1. **表示对象大小**
   >
   >`size_t` 通常用于函数返回值或参数，与内存大小有关：
   >
   >```cpp
   >int arr[10];
   >size_t size = sizeof(arr); // 返回数组占用的字节大小
   >```
   >
   >2. **数组索引**
   >
   >`size_t` 常被用作数组的索引类型：
   >
   >```cpp
   >for (size_t i = 0; i < 10; ++i) {
   >    std::cout << i << std::endl;
   >}
   >```
   >
   >3. **标准库接口**
   >
   >- 许多标准库函数的参数或返回值使用 
   >
   >  ```
   >  size_t
   >  ```
   >
   >  - `std::vector::size()` 返回值是 `size_t`。
   >  - `std::string::length()` 返回值也是 `size_t`。
   >
   >**注意事项**
   >
   >1. **无符号类型的问题**
   >
   >   - 因为 `size_t` 是无符号的，负值会导致意外的行为。
   >   - 当与带符号的整数混用时，可能会引发潜在的类型转换错误。
   >
   >   **示例**：
   >
   >   ```cpp
   >   int a = -1;
   >   size_t b = a; // 隐式转换，b 将成为一个非常大的值
   >   std::cout << b; // 输出一个大值，因为无符号类型存储了 "负数的二进制补码"
   >   ```
   >
   >   **解决方案**：
   >
   >   - 尽量避免混用 `size_t` 和带符号整数。
   >   - 使用明确的类型转换，避免意外行为。
   >
   >2. **范围检查**
   >
   >   - 如果试图索引一个过大的数组，可能会导致问题，即便编译器不报错：
   >
   >     ```cpp
   >     std::array<int, 3> arr = {1, 2, 3};
   >     size_t index = 10; // 编译期不会报错
   >     std::cout << arr[index]; // 未定义行为
   >     ```
   >
   >3. **与其他整数类型的兼容性**
   >
   >   - 由于 `size_t` 是平台相关的，其大小可能与普通的 `int` 或 `long` 不一致，尤其是在跨平台开发时需要注意。
   >
   >**总结**
   >
   >- `size_t` 是一种**专为表示对象大小和数组索引设计的无符号类型。**
   >- 它的主要优点是与平台无关且能表示最大的对象大小。
   >- 在使用时要注意其**无符号特性**，避免与带符号整数混用造成潜在的错误。

**示例代码解释**

```cpp
std::array<int, 3> arr = {10, 20, 30}; // 定义一个长度为 3 的 std::array，元素分别为 10, 20, 30
std::cout << std::get<0>(arr);         // 使用 std::get<> 元组接口访问第 0 个元素，输出 10
```

- `std::get<0>(arr)`：表示访问 `arr` 中的第 0 个元素，等价于 `arr[0]`。
- 索引 `0` 是一个编译期常量，因此代码合法且能通过编译。

### **适用场景**

通过 `std::get<>` 元组接口访问 `std::array<>` 的元素适用于以下场景：

- 索引值在编译期静态确定。
- 需要利用编译期索引检查增强代码的安全性。

相比于传统的 `operator[]` 或迭代器，`std::get<>` 在某些场景下（如模板编程）更具优势，因为它与 `std::tuple<>` 的接口一致，简化了泛型代码的实现和维护。

范围检查与安全性

1. **`at()` 提供范围检查**：

   - 如果索引超出范围，`at()` 会抛出 `std::out_of_range` 异常。

   ```cpp
   try {
       arr.at(5) = 42; // 如果索引越界会抛出异常
   } catch (const std::out_of_range& e) {
       std::cerr << e.what() << std::endl;
   }
   ```

2. **未定义行为的风险**：

   - `operator[]`、`front()` 和 `back()` 不检查范围，调用时需确保索引合法。
   - 空数组（如大小为 0 的 `std::array<>`）调用这些操作会导致未定义行为。

   ```cpp
   std::array<int, 0> arr;
   arr.front(); // 运行时错误，未定义行为
   ```

3. **多线程环境中的问题**：

   - 在多线程环境下，操作前需保证线程安全。例如：

     ```cpp
     template <typename C>
     void foo(C& coll) {
         if (coll.size() > 5) {
             coll.at(5) = ...; // OK，可能抛出异常
         }
     }
     ```

示例代码

```cpp
#include <array>
#include <iostream>
#include <stdexcept>

int main() {
    std::array<int, 4> arr = {1, 2, 3, 4};

    // 使用范围 for 循环
    for (const auto& elem : arr) {
        std::cout << elem << " "; // 输出: 1 2 3 4
    }
    std::cout << std::endl;

    // 使用索引访问
    std::cout << arr[2] << std::endl;  // 输出: 3
    try {
        std::cout << arr.at(5) << std::endl; // 抛出 std::out_of_range 异常
    } catch (const std::out_of_range& e) {
        std::cerr << "Exception: " << e.what() << std::endl;
    }

    // 使用元组接口访问
    std::cout << std::get<0>(arr) << std::endl; // 输出: 1

    // 使用迭代器
    for (auto it = arr.begin(); it != arr.end(); ++it) {
        std::cout << *it << " "; // 输出: 1 2 3 4
    }

    return 0;
}
```

### 总结

- `std::array<>` 支持通过索引、范围 for 循环、迭代器和元组接口访问元素。
- **安全性优先时使用 `at()`**，它提供范围检查。
- 空数组调用 `front()`、`back()` 或越界访问会导致未定义行为，需谨慎操作。
- 多线程场景下需同步，避免容器在检查后被修改。

## `std::array<>` 的迭代器操作

以下是 `std::array<>` 的所有迭代器操作及其效果：

------

| **操作**      | **效果**                                                     |
| ------------- | ------------------------------------------------------------ |
| `c.begin()`   | 返回指向第一个元素的随机访问迭代器。                         |
| `c.end()`     | 返回指向最后一个元素之后位置的随机访问迭代器。               |
| `c.cbegin()`  | 返回指向第一个元素的常量随机访问迭代器（C++11 起）。         |
| `c.cend()`    | 返回指向最后一个元素之后位置的常量随机访问迭代器（C++11 起）。 |
| `c.rbegin()`  | 返回反向迭代的第一个元素的反向迭代器。                       |
| `c.rend()`    | 返回反向迭代的最后一个元素之后位置的反向迭代器。             |
| `c.crbegin()` | 返回反向迭代的第一个元素的常量反向迭代器（C++11 起）。       |
| `c.crend()`   | 返回反向迭代的最后一个元素之后位置的常量反向迭代器（C++11 起）。 |

------

### 关键特点与注意事项

1. **随机访问迭代器**：
   - 迭代器类型实现了随机访问迭代器的接口，可以执行快速索引、指针算术等操作。
2. **迭代器类型**：
   - 对于 `std::array<>`，`begin()`、`end()` 等操作通常返回普通指针，因为其底层是 C 风格数组。
   - 在某些实现（如安全 STL 版本）中，迭代器可能是一个辅助类，而非普通指针，因此不能完全依赖其为指针。
3. **迭代器有效性**：
   - 只要数组本身有效，迭代器也始终有效。
   - 特殊情况：`swap()` 会为数组赋新值，此时迭代器仍然指向原数组，但指向的元素值可能已更改。

------

### 示例代码

```cpp
#include <array>
#include <iostream>
#include <iterator>

int main() {
    std::array<int, 5> arr = {10, 20, 30, 40, 50};

    // 正向迭代
    std::cout << "Forward iteration: ";
    for (auto it = arr.begin(); it != arr.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    // 常量正向迭代
    std::cout << "Constant forward iteration: ";
    for (auto it = arr.cbegin(); it != arr.cend(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    // 反向迭代
    std::cout << "Reverse iteration: ";
    for (auto it = arr.rbegin(); it != arr.rend(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    // 常量反向迭代
    std::cout << "Constant reverse iteration: ";
    for (auto it = arr.crbegin(); it != arr.crend(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

------

### 示例输出

```text
Forward iteration: 10 20 30 40 50 
Constant forward iteration: 10 20 30 40 50 
Reverse iteration: 50 40 30 20 10 
Constant reverse iteration: 50 40 30 20 10
```

------

### 总结

- **迭代器接口**允许灵活的元素访问（正向和反向）。
- **常量迭代器**保证元素不会被修改（`cbegin()`/`cend()`、`crbegin()`/`crend()`）。
- 在特殊实现中，迭代器可能不是普通指针，需注意类型的差异性。
- `swap()` 会影响迭代器指向的内容值，但不会使迭代器失效。