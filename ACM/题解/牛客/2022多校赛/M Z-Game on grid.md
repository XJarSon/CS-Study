# M.Z-Game on grid
## 题目链接：
https://ac.nowcoder.com/acm/contest/33191/M
## 题目大意
Bob和Alice在n * m的网格上轮流移动一个棋子。棋子初始位置为(1,1)，每次只能向由(x,y)移动到(x+1,y)或(x,y+1)。棋盘上有'A'  'B'  '.' 三种情况。若棋子移动到A点处Alice胜利，若棋子移动到B点处Bob胜利，若棋子移动到（n,m)且Alice和Bob都未胜利的话，则平局。
Alice先手，问Alice是否可以必胜、必平局、必输。输出其结果。
## 解题思路
每个棋子只可以向右或者向下移动，所以状态时没用后效性的，我们考虑用dp解决。
对于每种结果只有yes和no两种情况，所以我们考虑使用二进制解题。二进制的每一位表示1表示该种情况可以，0表示不可以。所以在网格上为 'A' 的点dp为001，为 '.' 的010，为 'B'的点为100。
很显然，对于网格上为 ' A ' 或 ' B ' 的位置我们不需要考虑，已经确定了是必输还是必赢。 
对于地图上每个点我们分两种情况考虑Alice走和Bob走。显然，当此时这个点的坐标之和为奇数时，Alice行动；当此时这个点的坐标之和为奇数时，Bob行动。

以必赢为例
在Alice行动时(x+1,y)和(x,y+1)中只要有一个情况 ' A  '，就可以走。而在Bob行动时(x+1,y)和(x,y+1)必须全为 ' A '才行。由此得出状态转移方程
dp(i,j) = dp(i + 1,j) & dp(i,j + 1);   ((i + j) %2 ==  1) 
dp(i,j) = dp(i + 1,j) | dp(i,j + 1);    ((i + j) %2 == 0)
对于其他两种情况也相同

## 代码实现
```c++
char g[N][N];
int dp[N][N];
void solve() {
    int n, m;
    cin >> n >> m;
    memset(dp, 0,sizeof dp);
    for (int i = 1; i <= n; i++) cin >> g[i] + 1;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (g[i][j] == 'A')
                dp[i][j] = 1;
            else if (g[i][j] == 'B')
                dp[i][j] = 4;
            else
                dp[i][j] = 2;
        }
    }
  
    for (int i = n; i; i--) {
        for (int j = m; j; j--) {
        	if (g[i][j] != '.') continue;
            if (j == m && i == n)
                continue;
            else if (i == n)
                dp[i][j] = dp[i][j + 1];
            else if (j == m)
                dp[i][j] = dp[i + 1][j];
            else if ((i + j) & 1)                       // Bob(i,j)从走
                dp[i][j] = dp[i][j + 1] & dp[i + 1][j];
            else                                       // Alice(i,j)从走
                dp[i][j] = dp[i + 1][j] | dp[i][j + 1];
        }
    }
    int k = dp[1][1];

    for (int i = 0; i < 3; i++) {
        if (k & 1)
            cout << "yes ";
        else
            cout << "no ";
        k >>= 1;
    }
}
```