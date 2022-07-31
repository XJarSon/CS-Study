# K	NIO's Sword
原题链接：https://ac.nowcoder.com/acm/contest/33189/K
暴力题没想到暴力
## 题目大意
构造一串数A,A初始值为0,A每次可以进行10 * A+x的操作使得 A ≡ i (mod n).求i从1~n变化的最小的操作数

## 解题思路
从0开始模拟，每次i+1后A mod n 的余数必是 i,所以每次初始值k设为i - 1。
A ≡ i（mod n）可以转化为 (A - i) = n * k，及 （A - i) % n = k。令k的取值为9，99，999可以得到最小的操作数,当小于k成立时一定可以出现A ≡ i（mod n）

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
#define IOS ios::sync_with_stdio(false), cin.tie(0)
#define YES cout << "Yes" << endl
#define NO cout << "No" << endl
#define se second
#define fi first
typedef pair<double, double> pdd;
typedef pair<int, double> pid;
typedef pair<double, int> pdi;
typedef pair<int, int> pii;
typedef long long ll;
const int N = 1e5 + 10, INF = 0x3f3f3f3f;

signed main() {
    int n;
    cin >> n;
    if (n == 1)  cout << '0';
	else {
        int ans = 1;
        for (int i = 2 ; i <= n ; ++i) {
            ll k = i - 1, kk = 0;
            while (true) {
                k = k * 10 + 9;
                kk = kk * 10 + 9;
                ++ans;
                if ((k - i) % n <= kk) break;  
            }
        }
        cout << ans;
    }
    return 0;
}
```