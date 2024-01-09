# NP-Hard

常见的 NP-Hard 问题汇总。

在 `P != NP` 前提下，NP-Hard 问题一定不是 P 问题，即一定不是多项式可解的。遇到后要首先尝试搜索的方案，并尝试剪枝/状压等方式减小常数。

## Max-Cut

最大割，NP-Complete

Defined in a Graph. One wants a subset `S` of the vertex set s.t. the number of edges between `S` and the complementary subset is as large as possible.

## Partition Problem

Weak NP-completeness: polynomial in the dimension of the problem and the magnitudes of the data involved. 

Partition the multiset S into two subsets S1, S2 such that the difference between the sum of elements in S1 and the sum of elements in S2 is minimized.

https://www.ijcai.org/Proceedings/09/Papers/096.pdf

## Knapsack Problem

Weak NP-completeness: polynomial in the dimension of the problem and the magnitudes of the data involved. 




汇总

https://www.csie.ntu.edu.tw/~lyuu/complexity/2015/20151117.pdf