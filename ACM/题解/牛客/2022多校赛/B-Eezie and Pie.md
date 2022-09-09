# B-Eezie and Pie(待补充)

## 题目链接：
[B-Eezie and Pie_"蔚来杯"2022牛客暑期多校训练营6 ](https://ac.nowcoder.com/acm/contest/33191/B)

## 题目大意
给定一棵有根树，每个节点可以为它的 0 ~ 𝑑[𝑖] 级祖先贡献 1 的价值。求最终每个点的价值。

## 解题思路

本题用树上差分实现

## 代码实现
```c++
vector<int> v[N];
int dep[N], d[N], ans[N];
bool st[N];
int len;
int path[N];
void dfsdeep(int u,int deep) {
	dep[u] = deep;
	for(int i = 0; i < v[u].size(); i++) {
		if(dep[v[u][i]] == -1) dfsdeep(v[u][i], deep + 1);
	}
}

void dfs(int u) {
	path[len++] = u;
	for(int i = 0; i < v[u].size(); i++) {
		int dis = dep[v[u][i]];
        if(dis <= dep[u]) continue;
		dfs(v[u][i]);
	}
	int cnt = 0; 
	for(int i = len - 1; i >= 0; i--) {
		if(cnt > d[u]) break;
		cnt++;
		ans[path[i]]++;
	}
	len--;
}

void solve() {
	int n;
	cin >> n;
	st[1] = true;
	for(int i = 1; i < n; i++) {
		int x, y; cin >> x >> y;
		v[x].pb(y);
		v[y].pb(x);
	}
	
	for(int i = 1; i <= n; i++) {
		cin >> d[i];
		dep[i] = -1;
	}
	dfsdeep(1, 0);
	dfs(1);
	
	for(int i = 1; i <= n; i++) {
		cout << ans[i] << ' ';
	}
}

signed main() {
    IOS;
    solve();
}
```