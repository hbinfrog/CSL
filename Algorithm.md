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

**时间复杂度：$O(n^2)$**

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

**时间复杂度：$O(n^3)$**

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

**时间复杂度：$O(n^2)$**

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

#### 相似题目

[Acwing1016 最大上升子序列和](https://www.acwing.com/problem/content/description/1018/)

### [Acwing896 最长上升子序列II](https://www.acwing.com/problem/content/898/)——数据加强

------

#### 题目描述

------

- 数据范围：$1 \le N \le 100000, -10^{9} \le 数列的数 \le 10^9$（其他的和[Acwing895 最长上升子序列](https://www.acwing.com/problem/content/description/897/)一样）

#### 解题思路

------

考虑状态：$q[i]$表示所有长度为$i$的上升子序列的末尾数字中的最小值。可以证明$q[i]$是随着$i$单调递增的。

- 证明：假设存在$i,j(i \lt j)$使得$q[i] \ge q[j]$，则在长度为$j$的上升子序列中可以选取前$i$个数，设最后一个数为$x$，则有$x \lt q[j] \le q[i]$，即可以在长度为$i$的上升子序列中找到一个更小的$x$作为$q[i]$，这与假设的$q[i]$是长度为$i$的上升子序列的末尾数字中的最小值矛盾

所以对于每一个数$a[i]$需要找到它对应的$q$的下标，即在$q$中找到一个小于$a[i]$的最大值$q[x]$，将$q[x + 1]$的值更新为$a[i]$（长度为$x+1$的上升子序列的末尾数字中的最小值发生了变化），这一步可以使用二分。同时需要记录一个当前的上升子序列的最大长度，即$q$值不为0的最大下标，作为二分的右边界

**时间复杂度：$O(n\log n)$**

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

### [Acwing1017 怪盗基德的滑翔翼](https://www.acwing.com/problem/content/description/1019/)——一端上升，一端下降

------

#### 题目描述

------

- 假设城市中一共有$N$幢建筑排成一条线，每幢建筑的高度各不相同。初始时，怪盗基德可以在任何一幢建筑的顶端。他可以选择一个方向逃跑，但是不能中途改变方向（因为中森警部会在后面追击）。因为滑翔翼动力装置受损，他只能往下滑行（即：只能从较高的建筑滑翔到较低的建筑）。他希望尽可能多地经过不同建筑的顶部，这样可以减缓下降时的冲击力，减少受伤的可能性。请问，他最多可以经过多少幢不同建筑的顶部(包含初始时的建筑)？
- 输入格式：输入数据第一行是一个整数$K$，代表有$K$组测试数据。每组测试数据包含两行：第一行是一个整数$N$，代表有$N$幢建筑。第二行包含$N$个不同的整数，每一个对应一幢建筑的高度h，按照建筑的排列顺序给出。
- 输出格式：对于每一组测试数据，输出一行，包含一个整数，代表怪盗基德最多可以经过的建筑数量。
- 数据范围：$1 \le N \le 100, 1 \le K \le 100, 0 \lt h \lt 10000$
- 时空限制：1s/64MB

#### 解题思路

------

考虑$f[i]$和$g[i]$，$f[i]$表示以第$i$个数结尾的最长上升子序列的长度即以第$i$个数开始向前走，$g[i]$表示以第$i$个数开始的最长下降子序列的长度即以第$i$个数开始往后走。$f[i]$和$g[i]$可以由[Acwing895 最长上升子序列](https://www.acwing.com/problem/content/description/897/)的方法来求，最后的结果是求$\max(f[i], g[i])$的最大值。

**时间复杂度：$O(n^2 * K)$**

#### 代码实现

------

```c++
#include <iostream>
#include <cstring>
using namespace std;
const int N = 105;
int a[N], f[N], g[N];
int n;

int main()
{
    int t;
    cin >> t;
    while(t--)
    {
        memset(f, 0, sizeof(f));
        memset(g, 0, sizeof(g));
        cin >> n;
        for(int i = 1; i <= n; i++){
            cin >> a[i];
        }
        for(int i = 1; i <= n; i++)
        {
            f[i] = 1;
            for(int j = 0; j < n; j++)
                if(a[j] < a[i]) f[i] = max(f[i], f[j] + 1);
        }
        for(int i = n; i >= 1; i--)
        {
            g[i] = 1;
            for(int j = n; j > i; j--)
                if(a[j] < a[i]) g[i] = max(g[i], g[j] + 1);
        }
        int res = 0;
        for(int i = 1; i <= n; i++) res = max(res, max(f[i], g[i]));
        cout << res << endl;
        
    }
}
```

#### 相似题目

[Acwing1014 登山](https://www.acwing.com/problem/content/description/1016/)

[Acwing482 合唱队形](https://www.acwing.com/problem/content/description/484/)

### [Acwing1012 友好城市](https://www.acwing.com/problem/content/description/1014/)——如何转换到最长上升子序列问题

------

#### 题目描述

------

- Palmia国有一条横贯东西的大河，河有笔直的南北两岸，岸上各有位置各不相同的$N$个城市。北岸的每个城市有且仅有一个友好城市在南岸，而且不同城市的友好城市不相同。每对友好城市都向政府申请在河上开辟一条直线航道连接两个城市，但是由于河上雾太大，政府决定避免任意两条航道交叉，以避免事故。编程帮助政府做出一些批准和拒绝申请的决定，使得在保证任意两条航线不相交的情况下，被批准的申请尽量多。
- 输入格式：第1行，一个整数$n$，表示城市数。第2行到第$n+1$行，每行两个整数，中间用1个空格隔开，分别表示南岸和北岸的一对友好城市的坐标。
- 输出格式：仅一行，输出一个整数，表示政府所能批准的最多申请数。
- 数据范围：$1 \le n \le 5000, 0 \le x_i \le 10000$
- 时空限制：1s/64MB

#### 解题思路

------

对于任意的合法方案，任何的连线都是没有交集的，所以当某一端的坐标从小到大排列的时候，在这个合法方案中，另一端的坐标也必须是从小到大排列的，所以问题可以转换为求另一端坐标的最长上升子序列的长度，详情见[Acwing895 最长上升子序列](https://www.acwing.com/problem/content/description/897/)

**时间复杂度：$O(n^2)$**

#### 代码实现

------

```c++
#include <iostream>
#include <algorithm>
using namespace std;
typedef pair<int, int> PII;
const int N = 5010;
PII city[N]; 
int f[N];
int n;
int main()
{
    cin >> n;
    for(int i = 0; i < n; i++)
        cin >> city[i].first >> city[i].second;
    sort(city, city + n);
    for(int i = 0; i < n; i++)
    {
        f[i] = 1;
        for(int j = 0; j < i; j++)
            if(city[j].second < city[i].second)
                f[i] = max(f[i], f[j] + 1);
    }
    int res = 0;
    for(int i = 0; i < n; i++) {
        res = max(res, f[i]);
    }
    cout << res << endl;
}
```

### [Acwing1010 拦截导弹](https://www.acwing.com/problem/content/1012/)

------

#### 题目描述

------

- 某国为了防御敌国的导弹袭击，发展出一种导弹拦截系统。但是这种导弹拦截系统有一个缺陷：虽然它的第一发炮弹能够到达任意的高度，但是以后每一发炮弹都不能高于前一发的高度。某天，雷达捕捉到敌国的导弹来袭。由于该系统还在试用阶段，所以只有一套系统，因此有可能不能拦截所有的导弹。输入导弹依次飞来的高度（雷达给出的高度数据是不大于30000的正整数，导弹数不超过1000），计算这套系统最多能拦截多少导弹，如果要拦截所有导弹最少要配备多少套这种导弹拦截系统。
- 输入格式：共一行，输入导弹依次飞来的高度。
- 输出格式：第一行包含一个整数，表示最多能拦截的导弹数。第二行包含一个整数，表示要拦截所有导弹最少要配备的系统数。
- 数据范围：$1 \le n \le 1000, 0 \le h_i \le 30000$
- 时空限制：1s/64MB

#### 解题思路

------

1. 第一问比较简单，就是求最长不上升子序列

2. 第二问使用贪心的解法，从前往后依次扫描每个数：

- 如果现有子序列的末尾数都小于当前数，则创建一个新的子序列
- 如果存在现有子序列的末尾数大于等于当前数，则将当前数放在末尾数大于等于当前数且末尾数最小的子序列后面

将全部序列的末尾数记为一个新的序列并从小到大排列，每来一个数，则需要找到大于等于它的最小数，然后将这个数更新为它，（也可以说是找到小于它的最大值，将这个数后面一个数更新为它）这和[Acwing896 最长上升子序列II](https://www.acwing.com/problem/content/898/)的求解过程是一致的，所以第二问的答案就是求最长上升子序列

**时间复杂度：$O(n^2)$**

#### 代码实现

------

```c++
#include <iostream>
using namespace std;
const int N = 1010;
int a[N], f[N], g[N];
int n;
int main()
{ 
    while(cin >> a[n]) n++;
    for(int i = n - 1; i >= 0; i--)
    {
        f[i] = 1;
        g[i] = 1;
        for(int j = n -1; j > i; j--)
            if(a[j] <= a[i]) f[i] = max(f[i], f[j] + 1);  
            else g[i] = max(g[i], g[j] + 1);
    }
    int res1 = 0, res2 = 0;
    for(int i = 0; i < n; i++) {
        res1 = max(res1, f[i]);
        res2 = max(res2, g[i]);
    }
    cout << res1 << endl;
    cout << res2 << endl;
    
}
```

### [Acwing187 导弹拦截系统](https://www.acwing.com/problem/content/189/)——[Acwing1010 拦截导弹](https://www.acwing.com/problem/content/1012/)的变形

------

#### 题目描述

------

- 为了对抗附近恶意国家的威胁，R 国更新了他们的导弹防御系统。一套防御系统的导弹拦截高度要么一直**严格单调**上升要么一直**严格单调**下降。给定即将袭来的一系列导弹的高度，请你求出至少需要多少套防御系统，就可以将它们全部击落。
- 输入格式：输入包含多组测试用例。对于每个测试用例，第一行包含整数 n，表示来袭导弹数量。第二行包含 n 个**不同的**整数，表示每个导弹的高度。当输入测试用例 n=0 时，表示输入终止，且该用例无需处理。
- 输出格式：对于每个测试用例，输出一个占据一行的整数，表示所需的防御系统数量。
- 数据范围：$1 \le n \le 50$
- 时空限制：3s/64MB

#### 解题思路

------

在[Acwing1010 拦截导弹](https://www.acwing.com/problem/content/1012/)的基础上，每来一个数，需要决定这个数是放入上升子序列还是下降子序列，因此题的数据范围很小，所以可以使用暴力搜索的方法来解决，将某个数先放入上升子序列再放入下降子序列。

时间复杂度：$O(n\cdot 2^n)$

#### 代码实现

------

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 55;
int q[N], up[N], down[N];
int n, res;

void dfs(int u, int su, int sd) {
    if(su + sd >= res) return;
    if(u == n) { // 所有数字都处理完毕了
        res = su + sd;   
        return;
    }
    // 放入上升子序列
    int k = 0;
    while(k < su && up[k] >= q[u]) k++; // 线性查找，使用二分会更快
    int t = up[k];
    up[k] = q[u];
    if(k < su) dfs(u + 1, su, sd); // 没有创建新的序列
    else dfs(u + 1, su + 1, sd); // 创建了一个新的上升子序列
    up[k] = t; // 恢复原状
    // 放入下降子序列
    k = 0;
    while(k < sd && down[k] <= q[u]) k++; // 线性查找，使用二分会更快
    t = down[k];
    down[k] = q[u];
    if(k < sd) dfs(u + 1, su, sd); // 没有创建新的序列
    else dfs(u + 1, su, sd + 1); // 创建了一个新的下降子序列
    down[k] = t; // 恢复原状
}

int main()
{
    while(cin >> n, n) {
        for(int i = 0; i < n; i++) cin >> q[i];
        res = n;
        dfs(0, 0, 0);
        cout << res << endl;
    }   
}
```



### [Acwing897 最长公共子序列](https://www.acwing.com/problem/content/description/899/)

------

#### 题目描述

------

- 给定两个长度分别为$n$和$m$的字符串A和B，求既是A的子序列又是B的子序列的字符串长度最长是多少
- 输入格式：第一行包含两个整数$n$和$m$。第二行包含一个长度为$n$的字符串，表示字符串A。第三行包含一个长度为$m$的字符串，表示字符串B。字符串均由小写字母构成
- 输出格式：输出一个整数，表示最大长度
- 数据范围：$1 \le n, m \le 1000$
- 时空限制：1s/64MB

#### 解题思路

------

考虑状态$f[i, j]$，

- 集合：表示第一个序列的前$i$个字母和第二个序列的前$j$个字母构成的所有公共子序列的集合
- 属性：求集合中的子序列的长度的最大值
- 计算：将集合划分为四种部分：1.A[i]和B[j]都不选；2.A[i]选，B[j]不选；3.A[i]不选，B[j]选；4.A[i]和B[j]都选
  - 1和4分别对应$f[i-1,j-1]$和$f[i - 1, j - 1] + 1$
  - 对于2：$f[i, j - 1]$表示的是第一个序列的前$i$个字母和第二个序列的前$j-1$个字母构成的所有公共子序列的集合；而2对应的是第一个序列的前$i$个字母和第二个序列的前$j$个字母构成的所有公共子序列的集合中A[i]一定出现，B[j]一定不出现的部分；$f[i, j - 1]$中A[i]是不一定存在的，所以说**$f[i, j - 1]$是包含2的；同理$f[i-1, j]$是包含3的**
  - 由于此题求的是$f[i, j]$的最大值，同时第一种情况是完全包含在$f[i, j - 1]$和$f[i - 1, j]$中的（如下图），即只需要对三种情况求最大值即可

<img src="images/pic2.svg" alt="pic" style="zoom: 25%;" />

时间复杂度：$O(n^2)$

#### 代码实现

------

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 1010;
int n, m;
int f[N][N];
char a[N], b[N];
int main()
{
    scanf("%d%d", &n, &m);
    scanf("%s%s", a + 1, b + 1);
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= m; j++) {
            f[i][j] = max(f[i - 1][j], f[i][j - 1]);
            if(a[i] == b[j]) f[i][j] = max(f[i][j], f[i - 1][j - 1] + 1); // 11不一定存在
        }
    cout << f[n][m] << endl;
    
}
```

### [Acwing272 最长公共上升子序列](https://www.acwing.com/problem/content/description/274/)

------

#### 题目描述

------

- 对于两个数列A和B，如果它们都包含一段位置不一定连续的数，且数值是严格递增的，那么称这一段数是两个数列的公共上升子序列，而所有的公共上升子序列中最长的就是最长公共上升子序列了。求两个数列的最长公共上升子序列。
- 输入格式：第一行包含一个整数$n$，表示数列A，B的长度。第二行包含$n$个整数，表示数列A。第三行包含$n$个整数，表示数列 B。
- 输出格式：输出一个整数，表示最长公共上升子序列的长度。
- 数据范围：$1 \le n \le 3000$
- 时空限制：1s/64MB

#### 解题思路

------

考虑状态$f[i, j]$

- 集合：所有由第一个序列的前$i$个数和第二个序列的前$j$个数构成的，且以$b[j]$结尾的公共上升子序列组成的集合
- 属性：集合中的序列的长度的最大值
- 计算：将集合分成两个部分：1.不包含$a[i]$ 2.包含$a[i]$
  - 1对应的是$f[i - 1, j]$
  - 对于2，继续划分集合，分别假设倒数第二个数为$b[1], b[2] ... b[j - 1]$，则对于任一一种情况$b[k]$，集合可以描述为所有以第一个序列的前$i-1$个数和第二个序列的前$k$个数构成的，且以$b[k]$结尾的公共上升子序列，再加上末尾一个相同的数字，即$f[i -1, k] + 1$。可能存在空的情况，此时长度为1

#### 代码实现

------

```c++
#include <cstdio>
#include <iostream>
#include <algorithm>
using namespace std;

const int N = 3010;

int n;
int a[N], b[N];
int f[N][N];

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &b[i]);

    for (int i = 1; i <= n; i ++ )
    {
        int maxv = 1;
        for (int j = 1; j <= n; j ++ )
        {
            f[i][j] = f[i - 1][j];
            if (a[i] == b[j]) f[i][j] = max(f[i][j], maxv);
            if (a[i] > b[j]) maxv = max(maxv, f[i - 1][j] + 1);
        }
    }

    int res = 0;
    for (int i = 1; i <= n; i ++ ) res = max(res, f[n][i]);
    printf("%d\n", res);
}
```



## 背包模型

### [Acwing2 01背包问题](https://www.acwing.com/problem/content/2/)（每个背包最多用一次）

------

#### 题目描述

------

- 有$N$件物品和一个容量是$V$的背包。每件物品只能使用一次。第$i$件物品的体积是$v_i$，价值是$w_i$。求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。输出最大价值。
- 输入格式：第一行两个整数$N, V$，用空格隔开，分别表示物品数量和背包容积。接下来有$N$行，每行两个整数$v_i, w_i$，用空格隔开，分别表示第$i$件物品的体积和价值。
- 输出格式：输出一个整数，表示最大价值。
- 数据范围：$1 \le N, V \le 1000, 0 \le v_i, w_i \le 1000$
- 时空限制：1s/64MB

#### 解题思路

------

考虑状态$f[i,j]$：

- 集合：放下前$i$个物品，且体积不超过$j$的所有方案的集合
- 属性：求集合里所有方案的价值最大值
- 计算：分为两种情况：1.放入了第$i$个物品 2.没有放入第$i$个物品
  - 情况1，可以看做是放下前$i$个物品，且体积不超过$j-v_i$的所有方案加上第$i$件物品的价值，即$f[i-1, j - v_i] + w[i]$
  - 情况2，显然$f[i-1,j]$
  - 综上可得：$f[i, j] = max(f[i-1,j], f[i -1, j - v_i] + w_i)$
  - 最后的答案为$f[n, v]$

时间复杂度为$O(n^2)$

#### 代码实现

------

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 1010;
int f[N][N], v[N], w[N];
int n, m;
int main()
{
    cin >> n >> m;
    for(int i = 1; i <= n; i++) {
        cin >> v[i] >> w[i];
    }
    for(int i = 1; i <= n; i++)
        for(int j = 0; j <= m; j++) {
            f[i][j] = f[i - 1][j]; // 必然存在
            if(j >= v[i]) f[i][j] = max(f[i][j], f[i - 1][j - v[i]] + w[i]); // 只有在j >= v_i的时候才可能会有情况1
        }
    cout << f[n][m] << endl;
    
}
```

#### 代码优化

------

看下面的代码

```c++
f[i][j] = f[i - 1][j]; 
if(j >= v[i]) f[i][j] = max(f[i][j], f[i - 1][j - v[i]] + w[i]);
```

可以看出$f[i, j]$中的$i$参数只与$i - 1$有关，所以这一维的参数可以修改

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 1010;
int f[N], v[N], w[N];
int n, m;
int main()
{
    cin >> n >> m;
    for(int i = 1; i <= n; i++) {
        cin >> v[i] >> w[i];
    }
    for(int i = 1; i <= n; i++)
        for(int j = m; j >= v[i]; j--) { 
            /*
            这里如果不交换顺序，则f[j - v[i]]在第f[j]之前计算，相当于在第i层进行了计算，即等价于
            f[i][j] = max(f[i][j], f[i][j - v[i]] + w[i]);和原来的等式是不一样的。将顺序交换之后
            f[j - v[i]]在第f[j]之后计算，此时f[j - v[i]]代表的就是f[i - 1][j - v[i]]
            */
            f[j] = max(f[j], f[j - v[i]] + w[i]);
        }
    cout << f[m] << endl;
    
}
```



### [Acwing3 完全背包问题](https://www.acwing.com/problem/content/3/)（每个背包可以无限用）

------

#### 题目描述

------

- 有$N$件物品和一个容量是$V$的背包。每件物品都有无限件可用。第$i$件物品的体积是$v_i$，价值是$w_i$。求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。输出最大价值。
- 输入格式：第一行两个整数$N, V$，用空格隔开，分别表示物品数量和背包容积。接下来有$N$行，每行两个整数$v_i, w_i$，用空格隔开，分别表示第$i$件物品的体积和价值。
- 输出格式：输出一个整数，表示最大价值。
- 数据范围：$1 \le N, V \le 1000, 0 \le v_i, w_i \le 1000$
- 时空限制：1s/64MB

#### 解题思路

------

考虑状态$f[i,j]$：

- 集合：放下前$i$个物品，且体积不超过$j$的所有方案的集合
- 属性：求集合里所有方案的价值最大值
- 计算：分为很多情况：第$i$个物品放了0，1，2 ...
  - 第$i$个物品放$k$个，则可以表示为$f[i-1, j - k*v_i] + k * w_i$
  - 对所有的情况求最大值即可
  - 最后的答案为$f[n, v]$

时间复杂度为$O(n^3)$

#### 代码实现

------

```c++
/*
这个代码会TLE
*/
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 1010;
int f[N][N], w[N], v[N];
int n, m;
int main()
{
    cin >> n >> m;
    for(int i = 1; i <= n; i++) cin >> v[i] >> w[i];
    for(int i = 1; i <= n; i++)
        for(int j = 0; j <= m; j++)
            for(int k = 0; k * v[i] <= j; k++) {
                f[i][j] = max(f[i][j], f[i - 1][j - k * v[i]] + k * w[i]);
            }
    cout << f[n][m] << endl;
}
```

#### 代码优化

------

$f[i, j] = max(f[i - 1, j], f[i - 1, j - v] + w, f[i-1,j - 2*v] + 2* w, ...)$

$f[i, j - v] = max(f[i - 1, j - v], f[i-1,j - 2*v] +  w,...)$，其中$f[i, j - v]$比$f[i, j]$少一项，后面的项相差一个$w$，所以有
$$
f[i,j] = max(f[i - 1, j], f[i, j - v] + w)
$$
即可以把$k$给优化掉，时间复杂度为$O(n ^ 2)$

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 1010;
int f[N][N], w[N], v[N];
int n, m;
int main()
{
    cin >> n >> m;
    for(int i = 1; i <= n; i++) cin >> v[i] >> w[i];
    for(int i = 1; i <= n; i++)
        for(int j = 0; j <= m; j++)
            {
                f[i][j] = f[i - 1][j];
                if(j >= v[i])f[i][j] = max(f[i - 1][j], f[i][j - v[i]] + w[i]);
            }
    cout << f[n][m] << endl;
}
```

然后再做类似于[Acwing2 01背包问题](https://www.acwing.com/problem/content/2/)的空间优化，可得

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 1010;
int f[N], w[N], v[N];
int n, m;
int main()
{
    cin >> n >> m;
    for(int i = 1; i <= n; i++) cin >> v[i] >> w[i];
    for(int i = 1; i <= n; i++)
        for(int j = v[i]; j <= m; j++)
            /*
            这里从小到大枚举，则f[j - v[i]]在第f[j]之前计算，相当于在第i层进行了计算，即等价于
            f[i][j] = max(f[i][j], f[i][j - v[i]] + w[i]);和原来的等式是一样的。
            */
            f[j] = max(f[j], f[j - v[i]] + w[i]);
    cout << f[m] << endl;
    
}
```

### [Acwing4 多重背包问题](https://www.acwing.com/problem/content/4/)（限制背包数量）

------

#### 题目描述

------

- 有$N$件物品和一个容量是$V$的背包。每件物品最多有$s_i$可用。第$i$件物品的体积是$v_i$，价值是$w_i$。求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。输出最大价值。
- 输入格式：第一行两个整数$N, V$，用空格隔开，分别表示物品数量和背包容积。接下来有$N$行，每行三个整数$v_i, w_i, s_i$，用空格隔开，分别表示第$i$件物品的体积，价值和数量。
- 输出格式：输出一个整数，表示最大价值。
- 数据范围：$1 \le N, V \le 100, 0 \le v_i, w_i \le 100$
- 时空限制：1s/64MB

#### 解题思路

------

和[Acwing3 完全背包问题](https://www.acwing.com/problem/content/3/)思路完全一样，需要注意的是，背包的数量是小于$s_i$和$j / v[i]$的最小值

时间复杂度为$O(n^3)$

#### 代码实现

------

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 105;
int f[N][N], w[N], v[N], s[N];
int n, m;

int main()
{
    cin >> n >> m;
    for(int i = 1; i <= n; i++) cin >> v[i] >> w[i] >> s[i];
    for(int i = 1; i <= n; i++)
        for(int j = 0; j <= m; j++)
            for(int k = 0; k <= min(s[i], j / v[i]); k++)
                f[i][j] = max(f[i][j], f[i - 1][j - k * v[i]] + k * w[i]);
    cout << f[n][m] << endl;
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



## 数位dp

