# Problem 2013 - Detect Squares - 检测正方形

2022.01.26

> 给你一个在 X-Y 平面上的点构成的数据流。设计一个满足下述要求的算法：
>
> **添加** 一个在数据流中的新点到某个数据结构中。可以添加 **重复** 的点，并会视作不同的点进行处理。
> 给你一个查询点，请你从数据结构中选出三个点，使这三个点和查询点一同构成一个 **面积为正** 的 **轴对齐正方形** ，统计 满足该要求的方案数目。
> **轴对齐正方形** 是一个正方形，除四条边长度相同外，还满足每条边都与 x-轴 或 y-轴 平行或垂直。
>
> 实现 `DetectSquares` 类：
>
> `DetectSquares()` 使用空数据结构初始化对象
> `void add(int[] point)` 向数据结构添加一个新的点 `point = [x, y]`
> `int count(int[] point)` 统计按上述方式与点 `point = [x, y]` 共同构造 **轴对齐正方形** 的方案数。

```
输入：
["DetectSquares", "add", "add", "add", "count", "count", "add", "count"]
[[], [[3, 10]], [[11, 2]], [[3, 2]], [[11, 10]], [[14, 8]], [[11, 2]], [[11, 10]]]
输出：
[null, null, null, null, 1, 0, null, 2]

解释：
DetectSquares detectSquares = new DetectSquares();
detectSquares.add([3, 10]);
detectSquares.add([11, 2]);
detectSquares.add([3, 2]);
detectSquares.count([11, 10]); // 返回 1 。你可以选择：
                               //   - 第一个，第二个，和第三个点
detectSquares.count([14, 8]);  // 返回 0 。查询点无法与数据结构中的这些点构成正方形。
detectSquares.add([11, 2]);    // 允许添加重复的点。
detectSquares.count([11, 10]); // 返回 2 。你可以选择：
                               //   - 第一个，第二个，和第三个点
                               //   - 第一个，第三个，和第四个点
```

![img](https://assets.leetcode.com/uploads/2021/09/01/image.png)

## Code - Hash Map

不要总想着树/二分，那些带 Log 的又是并不好写并不快

```cpp
class DetectSquares {
public:
    static const int M = 5009;

    struct Node {
        int x;
        int y;
        int cnt;
    } hashmap[M];

    int nodeCnt;

    DetectSquares() {
        for (int i = 0; i < M; i++) {
            hashmap[i] = (Node){-1, -1, 0};
        }
    }

    Node find(int x, int y) {
        if (y < 0 || y >= M) return (Node){-1, -1, 0};
        int posi = y;
        while (hashmap[posi].cnt) {
            if (x == hashmap[posi].x && y == hashmap[posi].y) 
                return hashmap[posi];
            posi++;
            if (posi == M) posi = 0;
        }
        return (Node){-1, -1, 0};
    }
    
    void add(vector<int> point) {
        int x = point[0], y = point[1], posi = y;
        while (hashmap[posi].cnt) {
            if (hashmap[posi].x == x && hashmap[posi].y == y) {
                hashmap[posi].cnt++;
                return;
            }
            posi++;
            if (posi == M) posi = 0;
        }
        hashmap[posi] = (Node){x, y, 1};
    }
    
    int count(vector<int> point) {
        int x = point[0], y = point[1], posi = y;
        int ret = 0;
        while (hashmap[posi].cnt) {
            if (hashmap[posi].y == y) {
                int sep = abs(x - hashmap[posi].x);
                if (sep > 0) {
                    int findY = y + sep;
                    Node p1 = find(x, findY);
                    Node p2 = find(hashmap[posi].x, findY);
                    ret += hashmap[posi].cnt * p1.cnt * p2.cnt;

                    findY = y - sep;
                    p1 = find(x, findY);
                    p2 = find(hashmap[posi].x, findY);
                    ret += hashmap[posi].cnt * p1.cnt * p2.cnt;
                }
            }
            posi++;
            if (posi == M) posi = 0;
        }
        return ret;
    }
};

/**
 * Your DetectSquares object will be instantiated and called as such:
 * DetectSquares* obj = new DetectSquares();
 * obj->add(point);
 * int param_2 = obj->count(point);
 */
```

