## Codeforces Round #778. C
C.Alice and the Cake
### 原题链接 https://codeforces.com/contest/1654/problem/C

### 题目描述
Alice对一个重量为W的蛋糕进行操作：
每次分为两部分，w向上取整和w向下取整。
最后得到一个序列a,
问序列a是否能有一个蛋糕切得。
##### DFS
这个题目的蛋糕如果存在，其质量一定是a序列的和，
因此对a的和开始进行dfs。
用map记录a序列的每种质量及其出现次数
如果切完后的质量在a序列中存在，则map中减少一次，并且返回上一层。
如果不存在就直接进行下一次切蛋糕。
当w为1时无法进行切割次数若map中还有元素，为no。

#### 时间复杂度O(nlogn)


#### C++ 代码
```
#include <iostream>
#include <cstring>
#include <algorithm>
#include <cstdio>
#include <map>
using namespace std;
typedef long long LL;
const int N = 2e5 + 10;
bool flag;          
int m;
map<int,int> mp;

int a[N];

void dfs(LL u){
	if(!flag) return;
	if(mp[u] > 0){
		mp[u]--;
		m--;
		return;
	} else {
		if(u == 1){
			flag = false;
			return;
		} else {
			dfs(u / 2);
			dfs((u + 1) / 2);
		}
	}
}

void solve(){
	mp.clear();                   //一定要记得清空
	int n;
	cin >> n;
	LL sum  = 0;			      //一定要记得开long long
	for(int i = 0;i < n; i++){
		cin >> a[i];
		sum += a[i];
		mp[a[i]]++;
	} 
	if(n == 1) {
		cout << "YES" << endl;    //当 n == 1时毫无疑问输出YES
 		return ;
	}
	flag = true;
	m = n;
	dfs(sum);
	if((!m) && flag) cout << "YES" << endl;
	else cout << "NO" << endl;
	
}

int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
	int T;
	cin >> T;
	while(T--){
		solve();
	}
}
```

----------