GPT-LeetCode中常见的算法模板和范式

以下是LeetCode中常见的算法模板和范式，分为以下几类：**二分查找、双指针、动态规划、贪心算法、回溯、滑动窗口、前缀和、分治法、并查集、图论等**，这些模板广泛应用于LeetCode中的经典题目中。

------

### **1. 二分查找模板**

#### 基础二分查找

```cpp
int binary_search(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2; // 防止溢出
        if (nums[mid] == target) 
            return mid;
        else if (nums[mid] < target) 
            left = mid + 1;
        else 
            right = mid - 1;
    }
    return -1; // 没找到
}
```

#### 寻找第一个大于等于目标值的索引

```cpp
int lower_bound(vector<int>& nums, int target) {
    int left = 0, right = nums.size(); // 注意右边界为size
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) 
            left = mid + 1;
        else 
            right = mid;
    }
    return left; // left是第一个>=target的位置
}
```

------

### **2. 双指针**

#### 两数之和（数组已排序）

```cpp
vector<int> two_sum_sorted(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    while (left < right) {
        int sum = nums[left] + nums[right];
        if (sum == target)
            return {left, right};
        else if (sum < target)
            left++;
        else
            right--;
    }
    return {};
}
```

#### 反转字符串

```cpp
void reverse_string(vector<char>& s) {
    int left = 0, right = s.size() - 1;
    while (left < right) {
        swap(s[left], s[right]);
        left++;
        right--;
    }
}
```

------

### **3. 动态规划**

#### 01背包问题

```cpp
int knapsack(vector<int>& weights, vector<int>& values, int capacity) {
    int n = weights.size();
    vector<int> dp(capacity + 1, 0);
    for (int i = 0; i < n; i++) {
        for (int j = capacity; j >= weights[i]; j--) {
            dp[j] = max(dp[j], dp[j - weights[i]] + values[i]);
        }
    }
    return dp[capacity];
}
```

#### 最长递增子序列（LIS）

```cpp
int length_of_LIS(vector<int>& nums) {
    vector<int> dp(nums.size(), 1);
    int max_len = 1;
    for (int i = 1; i < nums.size(); i++) {
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j])
                dp[i] = max(dp[i], dp[j] + 1);
        }
        max_len = max(max_len, dp[i]);
    }
    return max_len;
}
```

------

### **4. 贪心算法**

#### 跳跃游戏（判断是否可达终点）

```cpp
bool can_jump(vector<int>& nums) {
    int max_reach = 0;
    for (int i = 0; i < nums.size(); i++) {
        if (i > max_reach) return false;
        max_reach = max(max_reach, i + nums[i]);
    }
    return true;
}
```

#### 合并区间

```cpp
vector<vector<int>> merge_intervals(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end());
    vector<vector<int>> merged;
    for (auto& interval : intervals) {
        if (merged.empty() || merged.back()[1] < interval[0]) 
            merged.push_back(interval);
        else 
            merged.back()[1] = max(merged.back()[1], interval[1]);
    }
    return merged;
}
```

------

### **5. 回溯**

#### 全排列

```cpp
void backtrack(vector<int>& nums, vector<bool>& used, vector<int>& path, vector<vector<int>>& result) {
    if (path.size() == nums.size()) {
        result.push_back(path);
        return;
    }
    for (int i = 0; i < nums.size(); i++) {
        if (used[i]) continue;
        used[i] = true;
        path.push_back(nums[i]);
        backtrack(nums, used, path, result);
        path.pop_back();
        used[i] = false;
    }
}

vector<vector<int>> permute(vector<int>& nums) {
    vector<vector<int>> result;
    vector<bool> used(nums.size(), false);
    vector<int> path;
    backtrack(nums, used, path, result);
    return result;
}
```

------

### **6. 滑动窗口**

#### 最小覆盖子串

```cpp
string min_window(string s, string t) {
    unordered_map<char, int> need, window;
    for (char c : t) need[c]++;
    int left = 0, right = 0, valid = 0, start = 0, min_len = INT_MAX;
    while (right < s.size()) {
        char c = s[right++];
        if (need.count(c)) {
            window[c]++;
            if (window[c] == need[c])
                valid++;
        }
        while (valid == need.size()) {
            if (right - left < min_len) {
                start = left;
                min_len = right - left;
            }
            char d = s[left++];
            if (need.count(d)) {
                if (window[d] == need[d])
                    valid--;
                window[d]--;
            }
        }
    }
    return min_len == INT_MAX ? "" : s.substr(start, min_len);
}
```

------

### **7. 前缀和**

#### 二维前缀和（区域和查询）

```cpp
class NumMatrix {
private:
    vector<vector<int>> prefix;
public:
    NumMatrix(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        prefix = vector<vector<int>>(m + 1, vector<int>(n + 1, 0));
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                prefix[i][j] = matrix[i - 1][j - 1] + prefix[i - 1][j] + prefix[i][j - 1] - prefix[i - 1][j - 1];
            }
        }
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        return prefix[row2 + 1][col2 + 1] - prefix[row1][col2 + 1] - prefix[row2 + 1][col1] + prefix[row1][col1];
    }
};
```

------

### **8. 并查集**

#### 岛屿的数量

```cpp
class UnionFind {
private:
    vector<int> parent, rank;
public:
    UnionFind(int size) : parent(size), rank(size, 0) {
        iota(parent.begin(), parent.end(), 0);
    }
    int find(int x) {
        if (parent[x] != x)
            parent[x] = find(parent[x]);
        return parent[x];
    }
    void unionSet(int x, int y) {
        int rootX = find(x), rootY = find(y);
        if (rootX != rootY) {
            if (rank[rootX] > rank[rootY])
                parent[rootY] = rootX;
            else if (rank[rootX] < rank[rootY])
                parent[rootX] = rootY;
            else {
                parent[rootY] = rootX;
                rank[rootX]++;
            }
        }
    }
};

int numIslands(vector<vector<char>>& grid) {
    if (grid.empty() || grid[0].empty()) return 0;
    int m = grid.size(), n = grid[0].size();
    UnionFind uf(m * n);
    int directions[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    int count = 0;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == '1') {
                count++;
                grid[i][j] = '0';
                for (auto& dir : directions) {
                    int ni = i + dir[0], nj = j + dir[1];
                    if (ni >= 0 && ni < m && nj >= 0 && nj < n && grid[ni][nj] == '1') {
                        uf.unionSet(i * n + j, ni * n + nj);
                    }
                }
            }
        }
    }
    return count;
}
```

------

这些模板涵盖了LeetCode中最常见的算法问题，具体的变种和扩展可以在题目中灵活应用。需要更多详细模板时可以告诉我！