#  J.Melborp Elcissalc
## 题目链接：
https://ac.nowcoder.com/acm/contest/33192/J
## 题目大意
给三个数n, k, t。
对于长度的n的数组， 数组中数的指为0 ~ k - 1。对于该数组的所有子数组，数组元素的和能被k整除，即 %k == 0的子数组恰好有t个。求出这样的数组的个数。个数很多，需要对998244353取余。

## 解题思路
这是一道典型的dp问题。
先考虑子问题：对于一个给定的序列，求多少子区间的和mod k 为 0。
用前缀和考虑在mod k 的意义下每个0 ~ k-1的数出现的次数x，每次选择两个相同的数。他们之间的区间原来的数的和为k的倍数。
所以此题可以转化为：求dp满足条件的前缀和数组有多少种
于是我们对集合dp(i,j,k)。i表示考虑了0~i的数，放入了j个位置，满足条件的子区间有k个的方案数。
对于前 j1 个数后填充 j2个 i，每次增加的符合要求的子区间为 2 C j2个（根据子问题的结论，从j2个数中选2个）,根据乘法原理增加方案数为在原理的基础上乘上(n- j1) C j2（在n-j1个位置上放j2个数）。

## 代码实现
```c++
#include <algorithm>
#include <cmath>
#include <cstring>
#include <iostream>
#include <queue>
#include <stack>
#include <vector>

using namespace std;

#define IOS ios_base::sync_with_stdio(false), cin.tie(0)
#define YES cout << "Yes" << endl
#define NO cout << "No" << endl
#define pb push_back
#define se second
#define fi first
typedef pair<double, double> pdd;
typedef pair<int, double> pid;
typedef pair<double, int> pdi;
typedef pair<int, int> pii;
typedef long long ll;
const int MOD = 998244353;
ll qmi(ll x, int y) {
    ll res = 1;
    while (y) {
        if (y & 1) res *= x, res %= MOD;
        x *= x, x %= MOD;
        y >>= 1;
    }
    return res;
}
struct modint {
    int v;
    modint() { v = 0; }
    modint(int x) { v = x % MOD; }
    modint(long long x) { v = x % MOD; }
    modint operator+(const modint &a) const { return modint((v + a.v) % MOD); }
    modint operator+=(const modint &a) { return *this = *this + a; }
    modint operator-(const modint &a) const {
        return modint((v - a.v + MOD) % MOD);
    }
    modint operator-=(const modint &a) { return *this = *this - a; }
    modint operator*(const modint &a) const { return modint(1ll * v * a.v); }
    modint operator*=(const modint &a) { return *this = *this * a; }
    modint operator/(const modint &a) const {
        return modint(1ll * v * qmi(a.v, MOD - 2));
    }
    modint operator/=(const modint &a) { return *this = *this / a; }
    bool operator==(const modint &a) const { return v == a.v; }
} one(1);

modint fac[2100], ifac[2100];
modint C(int n, int m) { return fac[n] * ifac[m] * ifac[n - m]; }
modint dp[70][70][2050];

void init() {
	IOS;
    fac[0] = one;
    for (int i = 1; i <= 2100; i++) fac[i] = fac[i - 1] * i;
    ifac[2100] = one / fac[2100];
    for (int i = 2100; i >= 1; i--) ifac[i - 1] = ifac[i] * i;
}

int main() {
    init();
    int n, k, t;
    cin >> n >> k >> t;
    for (int i = 0; i <= n; i++) {
    	// 对于0特殊处理，因为 0 可以自身成一个区间
        dp[0][i][i * (i + 1) / 2] = C(n, i);  
    }
    for (int i = 1; i <= k - 1; i++) {
    	// i 表示考虑 1 ~ i 的方案
        for (int j1 = 0; j1 <= n; j1++) {
        	// j1 表示前j1个位置在原来的基础上维持不变
            for (int cnt = 0; cnt <= j1 * (j1 + 1) / 2; cnt++) {
            	// cnt 表示贡献度
                for (int j2 = 0; j2 <= n - j1; j2++) {
                	// j2 表示j1以外有j2个位置放 i
                    dp[i][j1 + j2][cnt + j2 * (j2 - 1) / 2] +=
                        dp[i - 1][j1][cnt] * C(n - j1, j2);
                }
            }
        }
    }
    cout << dp[k - 1][n][t].v << endl;
    return 0;
}


```