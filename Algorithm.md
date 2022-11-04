# 动态规划

## 数字三角形模型

### [Acwing898 数字三角形](https://www.acwing.com/problem/content/900/)

------

#### 题目描述

------

- 给定一个如下图所示的数字三角形，从顶部出发，在每一结点可以选择移动至其左下方的结点或移动至其右下方的结点，一直走到底层，要求找出一条路径，使路径上的数字的和最大
- 输入格式：第一行包含整数$n$，表示数字三角形的层数。接下来$n$行，每行包含若干整数，其中第$i$行表示数字三角形第$i$层包含的整数
- 输出格式：输出一个整数，表示最大的路径数字和
- 数据范围：$1 \le n \le 500, -10000 \le 三角形中的数字 \le 10000$
- 时空限制：1s/64MB

#### 解题思路

------

考虑状态：$f[i, j]$

1. 集合：以$[i, j]$为终点的所有路径的集合
2. 属性：路径数字和的最大值
3. 计算：$[i, j]$这个点可以由$[i-1,j]$或者$[i - 1, j - 1]$转换而来，即有$f[i,j] = \max(f[i-1,j], f[i-1,j-1]) + a[i,j]$，最后在最后一行求最大值即可

时间复杂度：$O(n^2)$

#### 代码实现

------

```c++
#include <iostream>
using namespace std;
const int N = 505, minValue = -0x3f3f3f3f;
int a[N][N], f[N][N];
int n;

int main()
{
    cin >> n;
    int res = minValue;
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= i; j++)
                cin >> a[i][j];
    for(int i = 1;i <= n; i++)           
        for(int j = 0;j <= i + 1; j++)         //因为有负数，所以应该将两边也设为-INF
            f[i][j] = minValue;
    f[1][1] = a[1][1];
    for(int i = 2; i <= n; i++)
        for(int j = 1; j <= i; j++)
            f[i][j] = max(f[i - 1][j], f[i - 1][j - 1]) + a[i][j];
    for(int i = 1; i <= n; i++)
        res = max(res, f[n][i]); // 到达最后一行，求个最大值
    cout << res << endl;
}
```

#### 优化

------

可以在上面的基础上进行优化，在计算$f[i,j] = \max(f[i-1,j], f[i-1,j-1]) + a[i,j]$这一步时，对于有的点来说$[i-1, j]$或者$[i-1,j-1]$可能不在三角形之中，所以需要将边界都设为负无穷，在最后一行还需要枚举求最大值。我们从最后一行开始走，走到第一行。所以$f[i,j] = \max(f[i+1,j], f[i+1,j+1]) + a[i,j]$，这样在计算的过程中，$[i + 1, j]$和$[i+1,j+1]$都在三角形中的，不需要考虑边界，最后达到$[1,1]$就是答案，也不需要枚举

#### 优化代码实现

------

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 505;
int n;
int a[N][N], f[N][N];
int main()
{
    cin >> n;
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= i; j++)
            cin >> a[i][j];
    for(int i = n; i >= 1; i--) 
        for(int j = i; j >= 1; j--)
            f[i][j] = max(f[i + 1][j], f[i + 1][j + 1]) + a[i][j];
    cout << f[1][1] << endl;
}
```

#### 相似题目

------

[Acwing1018 最低通行费](https://www.acwing.com/problem/content/description/1020/)

[Acwing1015 摘花生](https://www.acwing.com/problem/content/1017/)

### [Acwing1027 方格取数](https://www.acwing.com/problem/content/description/1029/)

------

#### 题目描述

------

- 设有$N * N$的方格图，我们在其中的某些方格中填入正整数，而其它的方格中则放入数字0。某人从图中的左上角A出发，可以向下行走，也可以向右行走，直到到达右下角的B点。在走过的路上，他可以取走方格中的数（取走后的方格中将变为数字0）。此人从A点到B点共走了两次，试找出两条这样的路径，使得取得的数字和为最大。
- 输入格式：第一行为一个整数$N$，表示$N*N$​的方格图。接下来的每行有三个整数，第一个为行号数，第二个为列号数，第三个为在该行、该列上所放的数。行和列编号从1开始。一行“0 0 0”表示结束。
- 输出格式：输出一个整数，表示两条路径上取得的最大的和。
- 数据范围：$ N \le 10$
- 时空限制：1s/64MB

#### 解题思路

------

考虑两个人一起走，则任何一个时刻两个人所走的步数是一样的。所以两个人的横纵坐标满足$i_1 + j_1 == i_2 + j_2$。设$k = i_1 + j_1 = i_2 + j_ 2$，定义状态$f[k,i_1,i_2]$

1. 集合：表示所有从$[1,1]$出发分别走到$[i_1, k - i_1], [i_2, k - i_2]$的路径的集合
2. 属性：求路径和的最大值
3. 计算：考虑两个人的上一步怎么走，有四种情况：下下；下右；右下；右右

- 下下：两个点分别由$[i_1-1,k - i_1]$和$[i_2 - 1, k - i_2]$到达；下右：两个点分别由$[i_1-1,k - i_1]$和$[i_2, k - i_2 - 1]$到达；右下：两个点分别由$[i_1,k - i_1 - 1]$和$[i_2 - 1, k - i_2]$到达；右右：两个点分别由$[i_1,k - i_1 - 1]$和$[i_2, k - i_2 - 1]$到达
- 对于以上的四种情况都需要考虑两个点是否重合：重合的时候加$w[i_1, j_1]$，不重合的时候加$w[i_1, j_1] + w[i_2, j_2]$
- 综上：$f[k,i_1, i_2]$为四种情况的最大值

时间复杂度：$O(n^3)$

#### 代码实现

------

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 12;
int w[N][N], f[2 * N][N][N];
int n;
int main()
{
    cin >> n;
    int a, b, c;
    while(cin >> a >> b >> c, a || b || c) w[a][b] = c;
    for(int k = 2; k <= 2 * n; k++)
        for(int i1 = 1; i1 <= n; i1++)
            for(int i2 = 1; i2 <= n; i2++)
            {
                int j1 = k - i1, j2 = k - i2;
                if(j1 >= 1 && j1 <= n && j2 >= 1 && j2 <= n) {
                    int t = w[i1][j1];
                    if(i1 != i2) t += w[i2][j2]; //重合的判定
                    int& x = f[k][i1][i2];
                    x = max(x, f[k - 1][i1][i2 - 1] + t);
                    x = max(x, f[k - 1][i1][i2] + t);
                    x = max(x, f[k - 1][i1 - 1][i2] + t);
                    x = max(x, f[k - 1][i1 - 1][i2 -1] + t); // 对应分析中的四种情况
                }
                
            }
    cout << f[2 * n][n][n] << endl;
}
```

#### 相似题目

------

[Acwing275 传纸条](https://www.acwing.com/problem/content/description/277/)

## 最长上升子序列模型

### [Acwing895 最长上升子序列](https://www.acwing.com/problem/content/description/897/)

------

#### 题目描述

------

- 给定一个长度为$N$的数列，求数值严格单调递增的子序列的长度最长是多少
- 输入格式：第一行包含整数$N$。第二行包含$N$个整数，表示完整序列
- 输出格式：输出一个整数，表示最大长度
- 数据范围：$1 \le N \le 1000, -10^{9} \le 数列的数 \le 10^9$
- 时空限制：1s/64MB

#### 解题思路

------

考虑状态：$f[i]$

1. 集合：所有以第$i$个数结尾的所有的上升子序列
2. 属性：长度最长
3. 计算：从第$i$个数依次向前找，如果某个数大于或者等于第$i$个数，则直接跳过；如果小于第$i$个数，则有$f[i] = max(f[i], f[j] + 1)$。将所有的$f[i]$求出来，取一个最大值即可

时间复杂度：$O(n^2)$

#### 代码实现

------

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 1010;
int a[N], f[N];
int n;
int main()
{
    cin >> n;
    int res = 0;
    for(int i = 0; i < n; i++) cin >> a[i];
    for(int i = 0; i < n; i++) {
        f[i] = 1;
        for(int j = 0; j < i; j++) {
            if(a[i] > a[j]) f[i] = max(f[i], f[j] + 1);
        }
        res = max(res, f[i]);
    }
    cout << res << endl;
}
```

### 数据加强[Acwing896 最长上升子序列II](https://www.acwing.com/problem/content/898/)

------

#### 题目描述

------

- 数据范围：$1 \le N \le 100000, -10^{9} \le 数列的数 \le 10^9$（其他的和[Acwing895 最长上升子序列](https://www.acwing.com/problem/content/description/897/)一样）

#### 解题思路

------

考虑状态：$q[i]$表示所有长度为$i$的上升子序列的末尾数字中的最小值。可以证明$q[i]$是随着$i$单调递增的。

- 证明：假设存在$i,j(i \lt j)$使得$q[i] \ge q[j]$，则在长度为$j$的上升子序列中可以选取前$i$个数，设最后一个数为$x$，则有$x \lt q[j] \le q[i]$，即可以在长度为$i$的上升子序列中找到一个更小的$x$作为$q[i]$，这与假设的$q[i]$是长度为$i$的上升子序列的末尾数字中的最小值矛盾

所以对于每一个数$a[i]$需要找到它对应的$q$的下标，即在$q$中找到一个小于$a[i]$的最大值$q[x]$，将$q[x + 1]$的值更新为$a[i]$（长度为$x+1$的上升子序列的末尾数字中的最小值发生了变化），这一步可以使用二分。同时需要记录一个当前的上升子序列的最大长度，即$q$值不为0的最大下标，作为二分的右边界

时间复杂度：$O(n\log n)$

#### 代码实现

------

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

const int N = 100010;
int a[N], q[N];
int n;

int main()
{
    cin >> n;
    for(int i = 0; i < n; i++)
        cin >> a[i];
    q[0] = -2e9;
    int len = 0;
    for(int i = 0; i < n; i++) {
        int l = 0, r = len;  // 右边界为len
        while(l < r) { // 二分模板
            int mid = (l + r + 1) >> 1;
            if(q[mid] < a[i]) l = mid;
            else r = mid - 1;
        }
        len = max(len, r + 1); // 更新len
        q[r + 1] = a[i]; // 更新q
        
    }
    cout << len << endl;
}
```



## 状态压缩dp

### [Acwing1064 小国王](https://www.acwing.com/problem/content/1066/)

------

#### 题目描述

------

- 在$n * n$的棋盘上放$k$个国王，国王可攻击相邻的8个格子，求使它们无法互相攻击的方案总数

- 输入格式：共一行，包含两个整数$n$和$k$
- 输出格式：共一行，表示方案总数，若不能够放置则输出0
- 数据范围：$1 \le n \le 10, 0 \le k \le n ^ 2$
- 时空限制：1s/64MB

#### 解题思路

------

状态压缩dp：$f[i, j, s]$

1. 集合：所有只摆在前$i$行，已经摆了$j$个国王，并且第$i$行摆放的状态为$s$的所有方案的集合

2. 属性：求集合的数量
3. 计算：

- 设第$i - 1$行的状态为$b$，第$i$行的状态为$a$，则需要满足$a$和$b$不能有两个相邻的1；两行相同列对应的数不能同时为1，即$(a \& b) == 0$；同时斜边也不能攻击到，即$(a | b)$不能有两个相邻的1
- 在满足上面的条件之后，$f[i, j, a]$可以由所有只摆在前$i-1$行，已经摆了$j-count(a)$个国王，并且第$i - 1$行摆放的状态为$b$的所有方案的集合转换而来，其中$count(a)$为$a$中包含的1的数量，即$f[i, j, a] = f[i, j, a] + f[i -1, j -count(a), b]$

时间复杂度：$O(n*m*k)$

#### 代码实现

------

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <vector>
using namespace std;
const int N = 12, M = 1 << 10, K = 105;

long long f[N][K][M];
int n, m;
vector<int> state;
vector<int> head[M];
int cnt[M];

bool check(int x) {
    for(int i = 0; i < n; i++) { // 不能有两个相邻的1
        if((x >> i) & 1 && (x >> (i + 1)) & 1)
            return false;
    }
    return true;
}

int count(int x) {
    int res = 0;
    for(int i = 0; i < n; i++) {  // 包含的1的数量
        if((x >> i) & 1) res++;
    }
    return res;
}

int main()
{
    
    cin >> n >> m;
    for(int i = 0; i < 1 << n; i++) {
        if(check(i)) {
            state.push_back(i); // 哪些是满足不能有两个相邻的1
            cnt[i] = count(i); // 统计一下1的数量
        }
    }
    for(int i = 0 ; i < state.size(); i++) {
        for(int j = 0; j < state.size(); j++) {
            int a = state[i], b = state[j]; // 满足check条件里面，拿两个数还要满足：两行相同列对应的数不能同时为1；斜边也不能攻击到，即$(a | b)$不能有两个相邻的1
            if((a & b) == 0 && check(a | b)) head[a].push_back(b);
        }
    }
    f[0][0][0] = 1;
    for(int i = 1; i <= n + 1; i++)
        for(int j = 0; j <= m; j++)
            for(int a : state) 
                for(int b : head[a]) { // 找到和a能够满足条件的b
                    int c = cnt[a];
                    if(j >= c) f[i][j][a] += f[i - 1][j - c][b]; // 必须j >= c
                }
                
    cout << f[n + 1][m][0] << endl; // 假装多了一行，则答案是所有只摆在前n + 2行，已经摆了m个国王，并且第n + 1行摆放的状态为0的集合的数量
    
    
}
```

![image-20221026135757259](/Users/hbin/Library/Application Support/typora-user-images/image-20221026135757259.png)



![image-20221026151709119](/Users/hbin/Library/Application Support/typora-user-images/image-20221026151709119.png)

## 数位dp

