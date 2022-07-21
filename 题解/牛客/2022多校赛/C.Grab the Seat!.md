# C.Grab the Seat!
题目链接 https://ac.nowcoder.com/acm/contest/33186/C

## 题目大意

1.二维平面中，(0,1) - (0,m)为屏幕

2.有n行m列座位（1 <= x <= n, 1 <= y <= m)

3.有k个座位有人，求不被人遮挡屏幕视线的座位数

4.有q次询问，求出每次询问后的答案

## 解题思路
1.对于y = 1和 y = m的行只会挡住后面的人
2.对于1 < y < m的范围，每个点都会挡住从（0，1）和（0，m）到该点射线所成的区域。如下图分界线构成一个折线图，每一行都对应一个t作为分界点。每一行合法的位置为t - 1
3.对于维护这个折线图，我们可以把(0,1)和(0,m)出发的分开处理。可以发现斜率绝对值大的线一旦出现就会覆盖斜率小的线，所以只需要关注同一行中，x较小的点就行。
4.所以对于每个y，要找到最小的合法x，并取斜率绝对值较大的线段，每次得出该行的线段的答案。把从(0,1)和(0,m)出发的取最小就是答案

<img src="https://uploadfiles.nowcoder.com/images/20220718/0_1658115296270/4A47A0DB6E60853DEDFCFDF08A5CA249" style="zoom:120%;" />




## 代码实现

```
int a[N], b[N], ans[N], p[N];
int n, m, k, q;
//a坐标 b为y坐标 q为每一行对应x的最小值
void solve() {
	fill(p,p + m + 1, n + 1);
	for(int i = 1; i <= k; i++) p[b[i]] = min(p[b[i]], a[i]);
	for(int i = 1; i <= m; i++) ans[i] = p[i] - 1;
	
	// 计算(0,1)出发每一行合法的座位数
	int x = INF, y = 1;
	for(int i = 2; i <= m; i++) {
		int x1 = p[i], y1 = i - 1;
		
		if(x != INF) ans[i] = min(ans[i],(x * y1 - 1) / y);
		if(y1 * x >= x1 * y) x = x1, y = y1;
	}
	
	// 计算(0,m)出发每一行合法的座位数
	x = INF, y = 1;
	for(int i = m - 1; i; i--) {
		int x1 = p[i], y1 = m - i;
		
		if(x != INF) ans[i] = min(ans[i],(x * y1 - 1) / y);
		if(y1 * x >= x1 * y) x = x1, y = y1;
	}
	
	int res = 0;
	for(int i = 1; i <= m; i++) res += ans[i];
	cout << res << endl;
}
```

