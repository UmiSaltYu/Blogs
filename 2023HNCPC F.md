## 2023湖南省赛 F题题解

*实际上自己用更多，可能偏向记录而不是教程*

**请注意，这一题我参考了别人的思路，并非纯粹自己想出来的**

#### 思路

题目给了多种将a变成b的方法，把这个问题变成a到b的最短路径，因为存在不能把a直接转成b但可以间接转化的方法
题目所说，要选择一个起点让补贴的价钱最少，这一段用暴力就好

所以，对于题目所给的转化方法，我们使用floyd算法求出每一个ai转化为bi的最小补贴价钱

什么是floyd？
详细请见[# 图-最短路径-Floyd(弗洛伊德)算法](https://www.bilibili.com/video/BV19k4y1Q7Gj/?spm_id_from=333.1391.0.0)

再最后，使用两重for循环暴力解出每个起点对应的总补贴价

这里关于顺逆时针的问题不必考虑，因为其实都一样

#### 特别注意

##### 1

让我卡了很久的地方是输入

注意题目描述————“可能存在多个将ai变为bi的巫术。”

悲伤的故事是，这句话我漏看了，导致我输入的时候没有保留最小值，也是我WA好几次的根本原因

##### 2

在预处理D数组的时候，不是很建议使用-1初始化，而是使用INT_MAX

类似于

```cpp
#include<climits>
......
const int INF = INT_MAX;
```

因为我在用-1的时候总会出一点奇奇怪怪的输出......

#### AC代码

```cpp

#include<bits/stdc++.h>
#include<climits>
using namespace std;

const int INF = INT_MAX;

int main(){
    int D[405][405], P[405][405];
    fill_n(&D[0][0], 405*405, INF);
    // fill_n(&P[0][0], 405*405, -1);

    int n, m, from, to, cost;
    cin >> n >> m;
    int arr1[n], arr2[n];
    for(int i = 0; i < n; i++) cin >> arr1[i];
    for(int i = 0; i < n; i++) cin >> arr2[i];

    for(int i = 0; i < m; i++){
        cin >> from >> to >> cost;
        if(cost < D[from][to]) D[from][to] = cost;
    }
    for(int i = 1; i <= 400; i++) D[i][i] = 0;

    for(int k = 1; k <= 400; k++)
    for(int i = 1; i <= 400; i++)
    for(int j = 1; j <= 400; j++){
        if(D[i][k] == INF || D[k][j] == INF) continue;

        if(D[i][j] == INF)
            D[i][j] = D[i][k] + D[k][j];
        else if(D[i][k] + D[k][j] < D[i][j])
            D[i][j] = D[i][k] + D[k][j];
    }

    // for(int i = 1; i <= 13; i++){
    //     for(int j = 1; j <= 13; j++) cout << D[i][j] << ' ';
    //     cout << endl;
    // }

    int min_n = INF;
    for(int i = 0; i < n; i++) {
        int save = 0, judge = 1;
        
        for(int j = 0; j < n; j++) {
            int a = arr1[j];
            int b = arr2[(i + j) % n];
            int min_cost = min(D[a][b], D[b][a]);
            if(min_cost == INF) {
                judge = 0;
                break;
            }
            save += min_cost;
        }
        if(judge && save < min_n) min_n = save;
    }
    cout << (min_n == INF ? -1 : min_n);
    return 0;
}

```