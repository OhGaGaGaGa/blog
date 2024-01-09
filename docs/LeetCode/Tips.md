## vector

### 全局 `vector`

如果想要 `int a[N]`，但是不能确定 `N` 的大小，可以如下。

```cpp
class Solution {
public:
    vector<int> vec;
    vector<vector<int>> permute(vector<int>& nums) {
        int n = nums.size();
        vec = vector<int> (n);
        dfs(0, n, nums);
        return ans;
    }
}
```

### 两维定长 `vector`

```cpp
vector<vector<int> > vec(m, vector<int>(n));
```

元素类型为 `vector<int>` 的 `vector` 容器，初始化为包含 `m` 个 `vector<int>` 对象，每个对象都是一个新创立的 `vector<int>` 对象的拷贝，而这个新创立的 `vector<int>` 对象被初始化为包含 `n` 个 0。

## 读入整行字符串

```cpp
std::istringstream stream("line1\nline2\nline3"); // or cin
while (std::getline(stream, line)) {
    std::cout << line << std::endl;
}
```