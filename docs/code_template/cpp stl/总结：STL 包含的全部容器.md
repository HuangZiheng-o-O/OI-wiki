### **总结：STL 包含的全部容器**

总共 **14种容器**，包括顺序容器、关联容器、无序容器和适配器容器：

### 1. **顺序容器（Sequence Containers）**

这些容器中的元素按插入顺序存储。主要用于线性数据结构的实现。

#### **顺序容器类型**

| 容器名             | 描述                                     |
| ------------------ | ---------------------------------------- |
| **`vector`**       | 动态数组，支持随机访问和快速尾部插入。   |
| **`deque`**        | 双端队列，支持两端的快速插入和删除。     |
| **`list`**         | 双向链表，支持快速插入和删除。           |
| **`forward_list`** | 单向链表，仅支持从头部到尾部的单向遍历。 |

#### **新成员**

**`forward_list`** 是 C++11 引入的容器，资源使用更少，适合内存受限的场景。

------

### 2. **关联容器（Associative Containers）**

这些容器按键值存储元素，内部通常实现为平衡二叉搜索树（红黑树），以确保快速访问。

#### **关联容器类型**

| 容器名         | 描述                                     |
| -------------- | ---------------------------------------- |
| **`set`**      | 集合，不允许重复元素，按键值排序存储。   |
| **`multiset`** | 多重集合，允许重复元素，按键值排序存储。 |
| **`map`**      | 键值对，按键排序存储。                   |
| **`multimap`** | 多重键值对，允许重复键，按键排序存储。   |

------

### 3. **无序容器（Unordered Containers）**

这些容器基于哈希表实现，元素的存储顺序不固定，但访问效率在平均情况下为 O(1)O(1)。

#### **无序容器类型**

| 容器名                   | 描述                         |
| ------------------------ | ---------------------------- |
| **`unordered_set`**      | 无序集合，不允许重复元素。   |
| **`unordered_multiset`** | 无序多重集合，允许重复元素。 |
| **`unordered_map`**      | 无序键值对，键唯一。         |
| **`unordered_multimap`** | 无序多重键值对，允许重复键。 |

------

### **补充：适配器容器（Container Adapters）**

适配器容器提供了一些封装功能，它们基于其他容器实现特定的数据结构（如栈、队列、优先队列）。

#### **适配器容器类型**

| 容器名               | 描述                       |
| -------------------- | -------------------------- |
| **`stack`**          | 栈，后进先出（LIFO）。     |
| **`queue`**          | 队列，先进先出（FIFO）。   |
| **`priority_queue`** | 优先队列，大根堆或小根堆。 |

------



#### **顺序容器：**

1. `vector`
2. `deque`
3. `list`
4. `forward_list`

#### **关联容器：**

1. `set`
2. `multiset`
3. `map`
4. `multimap`

#### **无序容器：**

1. `unordered_set`
2. `unordered_multiset`
3. `unordered_map`
4. `unordered_multimap`

#### **适配器容器：**

1. `stack`
2. `queue`
3. `priority_queue`

------

### **如何选择容器？**

| 场景                        | 推荐容器                            |
| --------------------------- | ----------------------------------- |
| **随机访问性能优先**        | `vector`                            |
| **需要双端操作**            | `deque`                             |
| **频繁插入/删除非尾部元素** | `list` / `forward_list`             |
| **需要排序和无重复元素**    | `set`                               |
| **需要排序和允许重复元素**  | `multiset`                          |
| **键值对映射且按键排序**    | `map`                               |
| **键值对映射且键无序**      | `unordered_map`                     |
| **需要后进先出的栈操作**    | `stack`（基于 `deque` 或 `vector`） |
| **需要先进先出的队列操作**  | `queue`                             |
| **需要动态调整堆的优先级**  | `priority_queue`                    |

以下是对您所列容器操作及其特性的一部分详细解释。这些操作和特性广泛适用于C++标准模板库(STL)中的容器，如`vector`、`deque`、`list`等。

------

### **构造函数**

1. **`ContType c` (默认构造函数)**

   - **功能**: 创建一个空的容器，不包含任何元素。对于`array<>`类型，创建时会初始化默认值。

   - 示例

     :

     ```cpp
     std::vector<int> v; // 创建一个空的vector
     std::array<int, 3> a; // 默认初始化数组的元素（如int为0）
     ```

2. **`ContType c(c2)` (复制构造函数)**

   - **功能**: 创建一个容器`c`，并将`c2`的所有元素逐一复制到`c`中。

   - 示例

     :

     ```cpp
     std::vector<int> v1 = {1, 2, 3};
     std::vector<int> v2(v1); // v2为v1的副本，包含{1, 2, 3}
     ```

3. **`ContType c = c2` (复制构造函数)**

   - **功能**: 和`ContType c(c2)`一致，语法不同。通过赋值语句创建一个新的容器并复制。

   - 示例

     :

     ```cpp
     std::vector<int> v1 = {1, 2, 3};
     std::vector<int> v2 = v1; // v2也是v1的副本
     ```

4. **`ContType c(rv)` (移动构造函数，C++11及之后支持)**

   - **功能**: 从右值引用（临时对象）中获取内容，避免不必要的复制。这对于性能至关重要，可以显著提高效率。

   - **限制**: `array<>`不支持移动构造。

   - 示例

     :

     ```cpp
     std::vector<int> createVector() {
         return {1, 2, 3}; // 返回右值
     }
     std::vector<int> v(createVector()); // 通过移动构造函数接管右值
     ```

5. **`ContType c = rv` (移动构造函数，C++11及之后支持)**

   - **功能**: 与`ContType c(rv)`相同，仅语法形式不同。

   - 示例

     :

     ```cpp
     std::vector<int> v = createVector(); // 使用移动构造
     ```
		
		
		
   
6. **`ContType c(beg, end)` (区间构造函数)**

   - **功能**: 创建一个容器`c`，并用区间`[beg, end)`中的元素初始化它。常用于从其他容器的一部分构造新的容器。

   - **限制**: `array<>`不支持。

   - 示例

     :

     ```cpp
     std::vector<int> v1 = {1, 2, 3, 4, 5};
     std::vector<int> v2(v1.begin() + 1, v1.end() - 1); // v2 包含{2, 3, 4}
     ```

7. **`ContType c(initlist)` (初始化列表构造函数，C++11及之后支持)**

   - **功能**: 使用初始化列表`initlist`中的值创建并初始化一个容器。

   - **限制**: `array<>`不支持。

   - 示例

     :

     ```cpp
     std::vector<int> v = {1, 2, 3}; // 使用初始化列表
     ```

8. **`ContType c = initlist` (初始化列表赋值构造)**

   - **功能**: 和`ContType c(initlist)`类似，仅形式不同。

   - 示例

     :

     ```cpp
     std::vector<int> v = {4, 5, 6}; // 使用初始化列表构造
     ```

------

### **析构函数**

1. `c.~ContType()` (析构函数)

   - **功能**: 当容器对象销毁时，释放其内存并删除所有元素。如果可能，还会释放底层分配的资源。

   - 示例

     :

     ```cpp
     {
         std::vector<int> v = {1, 2, 3};
         // 离开作用域后，析构函数自动调用
     } // v被销毁，释放内存
     ```

------

### **基本成员函数**

1. **`c.empty()`**

   - **功能**: 返回容器是否为空。如果容器中没有任何元素，返回`true`，否则返回`false`。

   - 示例

     :

     ```cpp
     std::vector<int> v;
     if (v.empty()) {
         std::cout << "Vector is empty." << std::endl;
     }
     ```

2. **`c.size()`**

   - **功能**: 返回容器中当前的元素数量。

   - **限制**: `forward_list<>`不支持此方法。

   - 示例

     :

     ```cpp
     std::vector<int> v = {1, 2, 3};
     std::cout << "Size: " << v.size() << std::endl; // 输出3
     ```

3. **`c.max_size()`**

   - **功能**: 返回容器理论上可以容纳的最大元素数（取决于系统内存和实现限制）。

   - 示例

     :

     ```cpp
     std::vector<int> v;
     std::cout << "Max size: " << v.max_size() << std::endl;
     ```

------

以下是容器操作相关内容的第二部分，主要涉及**比较操作**、**赋值操作**、**交换操作**。

------

### **比较操作**

1. **`c1 == c2`**

   - 功能

     : 比较两个容器

     ```
     c1
     ```

     和

     ```
     c2
     ```

     是否相等。它们相等的条件是：

     - 容器的大小相同 (`size()`)。
     - 对应位置的元素相等（使用元素类型的`==`运算符进行比较）。

   - 示例

     :

     ```cpp
     std::vector<int> v1 = {1, 2, 3};
     std::vector<int> v2 = {1, 2, 3};
     if (v1 == v2) {
         std::cout << "v1 and v2 are equal." << std::endl;
     }
     ```

2. **`c1 != c2`**

   - **功能**: 判断两个容器是否不相等。等价于`!(c1 == c2)`。

   - 示例

     :

     ```cpp
     std::vector<int> v1 = {1, 2, 3};
     std::vector<int> v2 = {4, 5, 6};
     if (v1 != v2) {
         std::cout << "v1 and v2 are not equal." << std::endl;
     }
     ```

3. **`c1 < c2`**

   - 功能

     : 按字典序比较容器的元素。

     - 如果`c1`在第一个不相等的元素上小于`c2`，或者`c1`为`c2`的前缀，返回`true`。

   - **限制**: 无序容器（如`unordered_map`）不支持此操作。

   - 示例

     :

     ```cpp
     std::vector<int> v1 = {1, 2, 3};
     std::vector<int> v2 = {1, 2, 4};
     if (v1 < v2) {
         std::cout << "v1 is less than v2." << std::endl;
     }
     ```

4. **`c1 > c2`**

   - **功能**: 判断`c1`是否字典序大于`c2`。等价于`c2 < c1`。

   - 示例

     :

     ```cpp
     if (v2 > v1) {
         std::cout << "v2 is greater than v1." << std::endl;
     }
     ```

5. **`c1 <= c2`**

   - **功能**: 判断`c1`是否小于或等于`c2`，等价于`!(c2 < c1)`。

   - 示例

     :

     ```cpp
     if (v1 <= v2) {
         std::cout << "v1 is less than or equal to v2." << std::endl;
     }
     ```

6. **`c1 >= c2`**

   - **功能**: 判断`c1`是否大于或等于`c2`，等价于`!(c1 < c2)`。

   - 示例

     :

     ```cpp
     if (v2 >= v1) {
         std::cout << "v2 is greater than or equal to v1." << std::endl;
     }
     ```

------

### **赋值操作**

1. **`c = c2`**

   - **功能**: 将容器`c2`的所有元素复制到`c`中。

   - 示例

     :

     ```cpp
     std::vector<int> v1 = {1, 2, 3};
     std::vector<int> v2;
     v2 = v1; // 复制v1到v2
     ```

2. **`c = rv` (移动赋值，C++11及之后支持)**

   - **功能**: 将右值`rv`中的元素移动到`c`中，释放`rv`的资源。相比复制赋值，移动赋值性能更高。

   - **限制**: `array<>`不支持移动赋值。

   - 示例

     :

     ```cpp
     std::vector<int> createVector() {
         return {1, 2, 3}; // 返回一个右值
     }
     std::vector<int> v;
     v = createVector(); // 移动赋值
     ```

3. **`c = initlist` (初始化列表赋值，C++11及之后支持)**

   - **功能**: 用初始化列表中的值替换容器`c`的内容。

   - 示例

     :

     ```cpp
     std::vector<int> v;
     v = {4, 5, 6}; // 通过初始化列表赋值
     ```

------

### **交换操作**

1. **`c1.swap(c2)`**

   - **功能**: 交换容器`c1`和`c2`的数据内容。

   - **优势**: 在需要交换数据时，比复制操作更高效。

   - 示例

     :

     ```cpp
     std::vector<int> v1 = {1, 2, 3};
     std::vector<int> v2 = {4, 5, 6};
     v1.swap(v2); // v1变为{4, 5, 6}，v2变为{1, 2, 3}
     ```

2. **`swap(c1, c2)`**

   - **功能**: 标准库提供的全局函数，用于交换两个容器的内容。

   - 示例

     :

     ```cpp
     std::swap(v1, v2); // 与v1.swap(v2)功能相同
     ```

------

以下是对C++标准模板库(STL)容器的特性以及一些高级操作的详细介绍：

------

### **容器特性**

1. **序列容器与关联容器**
   - **序列容器**: 元素按插入顺序排列，例如`vector`、`deque`、`list`等。
   - **关联容器**: 元素按键值排序或哈希排列，例如`map`、`set`、`unordered_map`、`unordered_set`。
2. **随机访问支持**
   - 容器如`vector`和`deque`支持随机访问，可以使用下标访问元素：`c[i]`。
   - 其他容器如`list`、`map`、`set`仅支持顺序访问。
3. **动态大小**
   - 像`vector`和`deque`支持动态扩展（会分配新内存并移动元素）。
   - `array`是固定大小容器，无法动态调整大小。
4. **高效插入与删除**
   - **头尾插入**: `deque`和`list`在容器的头部或尾部插入删除效率高（O(1)）。
   - **中间插入**: `list`在中间位置的插入效率高（O(1)），而`vector`需要移动元素（O(n)）。
5. **自动排序**
   - `map`和`set`等关联容器会自动对元素排序（基于键值或元素值）。
   - `unordered_map`和`unordered_set`则基于哈希存储，提供常数时间复杂度的查找和插入。
6. **内存管理**
   - 容器如`vector`通过容量（`capacity`）和大小（`size`）区分分配的内存和实际使用的内存。
   - 当`size`超过`capacity`时，`vector`会重新分配内存，性能会稍有影响。

------

### **高级操作**

#### 1. **插入操作**

1. **`c.insert(pos, val)`**

   - **功能**: 在`pos`位置插入值`val`。对于某些容器（如`map`），`val`是键值对。

   - 复杂度

     :

     - `vector`/`deque`: O(n)（需要移动元素）。
     - `list`: O(1)（已知位置时）。

   - 示例

     :

     ```cpp
     std::vector<int> v = {1, 2, 4};
     v.insert(v.begin() + 2, 3); // 在索引2插入3，v变为{1, 2, 3, 4}
     ```

2. **`c.insert(pos, n, val)`**

   - **功能**: 在`pos`位置插入`n`个值`val`。

   - 示例

     :

     ```cpp
     v.insert(v.end(), 2, 5); // 在末尾插入两个5，v变为{1, 2, 3, 4, 5, 5}
     ```

3. **`c.insert(pos, first, last)`**

   - **功能**: 将区间`[first, last)`的值插入到`pos`位置。

   - 示例

     :

     ```cpp
     std::vector<int> v1 = {7, 8, 9};
     v.insert(v.end(), v1.begin(), v1.end()); // v变为{1, 2, 3, 4, 5, 5, 7, 8, 9}
     ```

------

#### 2. **删除操作**

1. **`c.erase(pos)`**

   - **功能**: 删除`pos`位置的元素。

   - 复杂度

     :

     - `vector`/`deque`: O(n)。
     - `list`: O(1)。

   - 示例

     :

     ```cpp
     v.erase(v.begin() + 2); // 删除索引2的元素，v变为{1, 2, 4, 5, 5, 7, 8, 9}
     ```

2. **`c.erase(first, last)`**

   - **功能**: 删除区间`[first, last)`中的元素。

   - 示例

     :

     ```cpp
     v.erase(v.begin() + 3, v.end() - 2); // 删除索引3到倒数第2个元素，v变为{1, 2, 4, 8, 9}
     ```

3. **`c.clear()`**

   - **功能**: 删除容器中的所有元素。

   - 示例

     :

     ```cpp
     v.clear(); // 清空v，size()变为0
     ```

------

#### 3. **排序与反转**

1. **`std::sort(first, last)`**

   - **功能**: 对区间`[first, last)`中的元素进行升序排序。

   - **限制**: 容器必须支持随机访问（如`vector`、`deque`）。

   - 示例

     :

     ```cpp
     std::sort(v.begin(), v.end()); // 对v排序
     ```

2. **`std::reverse(first, last)`**

   - **功能**: 反转区间`[first, last)`中的元素。

   - 示例

     :

     ```cpp
     std::reverse(v.begin(), v.end()); // 反转v的元素顺序
     ```

------

#### 4. **查找与计数**

1. **`std::find(first, last, val)`**

   - **功能**: 在区间`[first, last)`中查找值`val`，返回迭代器（若找不到，返回`last`）。

   - 示例

     :

     ```cpp
     auto it = std::find(v.begin(), v.end(), 4);
     if (it != v.end()) {
         std::cout << "Found: " << *it << std::endl;
     }
     ```

2. **`std::count(first, last, val)`**

   - **功能**: 统计区间`[first, last)`中值为`val`的元素个数。

   - 示例

     :

     ```cpp
     int count = std::count(v.begin(), v.end(), 5); // 统计5出现的次数
     ```

------

#### 5. **特定容器的高级操作**

1. **`map::emplace` 和 `unordered_map::emplace`**

   - **功能**: 在`map`或`unordered_map`中插入键值对，避免不必要的复制。

   - 示例

     :

     ```cpp
     std::map<int, std::string> m;
     m.emplace(1, "one"); // 直接构造键值对
     ```

2. **`priority_queue::push` 和 `priority_queue::pop`**

   - 功能

     :

     - `push`: 插入一个元素并调整堆。
     - `pop`: 删除堆顶元素（最大或最小元素）。

   - 示例

     :

     ```cpp
     std::priority_queue<int> pq;
     pq.push(10);
     pq.push(20);
     pq.pop(); // 删除20（最大值）
     ```

------

这些容器特性和高级操作在实际开发中非常重要，尤其是当你需要优化性能或操作复杂数据结构时。希望这部分对您有帮助！