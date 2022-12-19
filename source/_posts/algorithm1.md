---
title: 算法模板 
excerpt: 入门用
index_img: /img/algorithm/icpc_logo.png
banner_img: /img/algorithm/icpc_logo.png
date: 2024-05-13 6:45:00
categories: algorithm
---

> 自用模板
```cpp
#pragma GCC optimzie("Ofast", "inline")

#define ioio ios::sync_with_stdio(0);cin.tie(0),cout.tie(0)
#define fst first
#define sed second
#define pb push_back

#include <iostream>
#include <algorithm>
#include <cstring>
#include <vector>
#include <cmath>
#include <map>
using namespace std;

typedef long long LL;
typedef pair<int, int> PII;

int dxy[4][2]={{-1,0}, {1,0}, {0,-1}, {0,1}};
const double PI = acos(-1.0);
const int inf = 0x3f3f3f3f;
const int MOD = 1e9+7;
const int N = 2e5+10;
// ********************************************************

int n, m;
int a[N];

void solve(){
    
    
    
    return ;
}

int main(){
    ioio;
    freopen("in.txt", "r", stdin); freopen("out.txt", "w", stdout); 
    //int T; cin>>T; while(T--)
    solve();
    return 0;
}
```


1. [基础语法](#1)     
    1.1 [排序*](#1.1)   
    1.2 [二分](#1.2)   
    1.3 [双指针](#1.3)   
    1.4 [高精度](#1.4)   
    1.5 [进制转换](#1.5)     
    1.6 [位运算](#1.6)

2. [数据结构](#2)   
    2.1 [二叉树](#2.1)  
    2.2 [栈与队列](#2.2)  
    2.3 [并查集](#2.3)  
    2.4 [线段树与树状数组*](#2.4)

3. [图论](#3)  
    3.1 [DFS与BFS](#3.1)   
    3.2 [最短路](#3.2)  
    3.3 [最小生成（支撑）树](#3.3)

4. [数学](#4)  
    4.1 [质（素）数与约数](#4.1)   
    4.2 [快速幂](#4.2)

5. [动态规划](#5)

6. [字符串](#6)  
    6.1 [KMP](#6.1)   
    6.2 [后缀数组](#6.2) 

7. [计算几何](#7)

8. [其他](#8)










<h3 id=1>1 基础算法</h3>

<h4 id=1.1>1.1 排序</h4>

> `sort自定义排序`
> 对于 a b俩元素，如果返回T，则a在b前面，反之b在a前面
```cpp
bool cmp(int a, int b){
    if(a<b) return 1;
    return 0;
}

sort(a.begin(), a.end(), cmp);
```
> `快速排序`
> x是每轮的基准值
```cpp
void Cswap(int *a, int *b){
    int t=*a;
    *a=*b;
    *b=t;
} // C环境下使用的swap函数

void qsort(int *arr, int l, int r){
    if (l >= r) return;
    int i=l-1, j=r+1, x=arr[l+r>>1];
    while (i < j){ // 基准值比较，左右交换
        do i++; while(arr[i] < x);
        do j--; while(arr[j] > x);
        if (i < j) Cswap(&arr[i], &arr[j]);
    }
    qsort(arr, l, j), qsort(arr, j + 1, r);
}
// a[1]-a[n]排序
sort(a, 1, n);
```
> 归并排序
```cpp

```

<h4 id=1.2>1.2 二分</h4>

>
```cpp
// >=x的第一个元素
int bsearch_1(int l, int r){
    while (l < r){
        int mid = l + r >> 1;
        if (check(mid)) r = mid;
        else l = mid + 1;
    }
    return l;
}
```

```cpp
// <=x的最后一个元素
int bsearch_2(int l, int r){
    while (l < r){
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```

> 为何不用STL？
```cpp
// >=x的第一个元素
auto i=lower_bound(a.begin(), a.end(), x);
*i;          // 值
i-a.begin(); // 位置
// 显然 <x 的最后一个元素是
i-a.begin()-1;
```

```cpp
// >x的第一个元素
auto i=upper_bound(a.begin(), a.end(), x);
// 显然 <=x 的最后一个元素是
i-a.begin()-1;
```
<h4 id=1.3>1.3 双指针</h4>

```cpp
// 左指针 右指针
int l=1, r=1;
// 指针不越界，部分情况下考虑左指针优先右指针
while( r<=n && l<=n){
	// 右指针右移
	while( r<=n && 条件) r++;
	{...} // 更新答案巴拉巴拉
	// 左指针右移
	l++;
}
```


<h4 id=1.4>1.4 高精度</h4>

> 高精度加法
```cpp
string ADD(string A, string B) {  // 正序输入 正序输出
	reverse(A.begin(), A.end()); reverse(B.begin(), B.end());
	string res;
	int add = 0;
	for (int i = 0; i < A.size() || i < B.size(); i++) {
		if (i < A.size()) add += A[i] - '0';
		if (i < B.size()) add += B[i] - '0';// 取值
		res.push_back(add % 10 + '0');      // +
		add /= 10;                          // 进位
	}
	if (add) res.push_back(add + '0');
	reverse(res.begin(), res.end());
	return res;
}
```

> 高精度减法
```cpp
string SUB(string& A, string& B) {  // 正序输入 正序输出 自带负号
	if (!(A.size() > B.size() || (A.size() == B.size() && A >= B)))
		return "-" + SUB(B, A);
	reverse(A.begin(), A.end()); reverse(B.begin(), B.end());
	string res;
	for (int i = 0, t = 0; i < A.size(); i++) {
		int ai = A[i] - '0', bi = 0;
		if (i < B.size()) bi = B[i] - '0';
		t = ai - t;
		if (i < B.size()) t -= bi;
		res.push_back((t + 10) % 10 + '0');
		if (t < 0) t = 1;
		else t = 0;
	}
	while (res.size() > 1 && res.back() == '0') res.pop_back();
	reverse(res.begin(), res.end());
	return res;
}
```

> 高精度乘法
```cpp
string MUL(string A, int b) { // 顺序输入 顺序输出
	reverse(A.begin(), A.end());
	string res;
	int t = 0;
	for (int i = 0; i < A.size() || t; i++) {
		if (i < A.size()) t += (A[i] - '0') * b;
		res.push_back(t % 10 + '0');
		t /= 10;
	}
	// 去前导0
	while (res.size() > 1 && res.back() == '0') res.pop_back();
	reverse(res.begin(), res.end());
	return res;
}
```
> 高精度除法
```cpp
string DIV(string A, int b, int& r) {  // 正序输入 正序输出
	if (b == 0) return "ERROR";         // A / b = res ... r 
	reverse(A.begin(), A.end());
	string res;
	r = 0;
	for (int i = A.size() - 1; i >= 0; i--) {
		int ai = A[i] - '0';
		r = r * 10 + ai;
		res.push_back(r / b + '0');
		r %= b;
	}
	while (res.size() > 1 && res.front() == '0') res.erase(res.begin(), res.begin() + 1);
	return res;
}
```

<h4 id=1.5>1.5 进制转换</h4>

> x(a) -> x(10)
```cpp
string x_10(string x, int a){
    string res="0", p="1";  // 将 a 进制下的 x 转换为 10 进制
    for(int i=x.size()-1; i>=0; i--){
        int t; // 判断当前位数字是多少
        if(x[i]>='0' && x[i]<='9') t=x[i]-'0'; 
        if(x[i]>='A' && x[i]<='Z') t=x[i]-'A'+10;
        if(x[i]>='a' && x[i]<='z') t=x[i]-'a'+10;
        res=ADD(res, MUL(p, t));//res+=t*p; // t*(a^0, a^1, a^2)
        p=MUL(p, a);            //p*=a;     //   (a^0, a^1, a^2)
    }
    return res;
}
```

> x(10) -> x(a)
```cpp
string x_a(string x, int a){
    if(x=="0") return "0";
    string res; // 将 10 进制下的 x 转换为 a 进制
    while(x!="0"){
        int r;
        x=DIV(x, a, r);
        if( r>=0 && r<=9 ) r+='0';
        else r+='A'-10;
        res+=r;
    }
    reverse(res.begin(), res.end());
    return res;
}
```

<h4 id=1.6>1.6 位运算</h4>

> ` & ` 按位与 `全1为1`
```c
0&0=0   1&0=0
0&1=0   1&1=1
```
> ` | ` 按位或 `一1为1`
```c
0|0=0   1|0=1
0|1=1   1|1=1
```
> ` ^ ` 按位异或 `不同为1`
```c
0^0=0   1^0=1
0^1=1   1^1=0
```
> ` lowbit `  x 的二进制表示中，最低位的 1 的位置
> ```cpp
> lowbit(0b10110000) == 0b00010000 == 16
>          ~~~^~~~~
> lowbit(0b11100100) == 0b00000100 == 4
>          ~~~~~^~~ 
> ```
```cpp
int lowbit(int x) {
    return x & -x;
}
```
<h3 id=2>2 数据结构</h3>


<h4 id=2.1>2.1 二叉树</h4>

<h4 id=2.2>2.2 栈与队列</h4>


<h4 id=2.3>2.3 并查集</h4>

> 基础并查集

```cpp
int P[N];   // 存放每个数的祖宗

int find(int x){// 找x的祖宗结点
    if(x!=p[x]) p[x]=find(p[x]);
    return p[x];
}

void solve(){
    /* 初始化 */
    for(int i=1; i<=n; i++) p[i]=i; 
    /* 合并 */
    x=find(x), y=find(y);   // 找到x和y的祖宗
    if(x!=y) p[y]=x;        // 将y的祖宗合并到x
}
```


<h4 id=2.4>2.4 线段树与树状数组</h4>

> 树状数组 二叉索引树 强化版前缀和
```cpp
int lowbit(int x){
    return x&-x;
}

void add(int i, int v){
    for(; i<=n; i+=lowbit(i))
        t[i]+=v;    // 该点后面所有后驱都需要+v
}

int query(int i){   
    int res=0;      // i的前缀和
    for(; i>=1; i-=lowbit(i))
        res+=t[i];  // 累加所有后驱
    return res;
}

void init(){
    for(int i=1; i<=n; i++)
        add(i, a[i]);
}

void init(){  // 这种是一个一个的+
    for(int i=1; i<=n; i++){
        t[i]+=a[i];
        int j=i+lowbit(i);
        if(j<=n) t[j]+=t[i];
    }
}
```

> 线段树 不一定正确

```cpp
struct node {
    int l, r;   // 左右结点
    LL sum;    // 区间和
}tr[4 * N];

void pushup(node& u, node& l, node& r) {
    u.sum = l.sum + r.sum;
}

void build(int u, int l, int r) {
    tr[u] = { l,r };
    if (l == r)
        tr[u].sum = a[r];
    else {
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
        pushup(tr[u], tr[u << 1], tr[u << 1 | 1]);
    }
}

void modify(int u, int i, int v) {   // 单点修改
    if (tr[u].l == i && tr[u].r == i)    // 找到叶子结点
        tr[u] = { i,i,v };              // 修改
    else {
        int mid = tr[u].l + tr[u].r >> 1;
        if (i <= mid) modify(u << 1, i, v);
        else modify(u << 1 | 1, i, v);  // 不在左边就肯定在右边
        pushup(tr[u], tr[u << 1], tr[u << 1 | 1]);
    }
}

node query(int u, int l, int r) {
    if (l <= tr[u].l && tr[u].r <= r)
        return tr[u];
    else {
        int mid = tr[u].l + tr[u].r >> 1;
        if (mid >= r)                      // tr[l]--------[m]--------tr[r]
            return query(u << 1, l, r);    //       l----r         
        else if (mid < l)                  // tr[l]--------[m]--------tr[r]
            return query(u << 1 | 1, l, r);//                  l----r
        else {
            auto left = query(u << 1, l, r);
            auto right = query(u << 1 | 1, l, r);
            node res = { l, r };
            pushup(res, left, right);
            return res;
        }
    }
}
```








<h3 id=3>3 图论</h3>

<h4 id=3.1>3.1 DFS与BFS</h4>

> DFS  
> 一个小tips：枚举状态时候，枚举单个点会爆时间，当从行的角度考虑
```cpp
// 处理当前点u
void dfs(int u){
	if( u满足剪枝条件 )
		return ;	
	if( u>N ) {	    	// 到达边界
		if( check() )   // 如果当前情况合法
			deal();		// 输出或者更新最终答案		
		return ;		// 结束
	}
	
	for(auto i : arr){				// 显然，当一个点有多个状态，需要全部考虑
		if( used[i] ) continue ;	// 如果此状态仅可出现一次，那么该点不考虑此状态
		
		change(u, i);		// 将 u 点状态设置为 i （如果需要，应当同时考虑下 i 的状态）	
		dfs( u++ );			// 进入下一层。应当注意，这里的下一层是指u进入递归层面的下一层 
		change(u, row);		// 恢复现场。通常情况下，u是直接置0
	}
	dfs( u++ );				// 部分时候，还需要考虑该点不作处理的情况	
}

```
> 值得一提，STL中的两个全排列函数 `next_permutation` 和 `prev_permutation`

```cpp
#include <algorithm>

int arr[3]={2, 1, 3};
								// cout
do{								// 2 1 3
	for(int i=0; i<3; i++)		// 2 3 1
		cout<<arr[i]<<" ";		// 3 1 2
	cout<<endl;					// 3 2 1 
}while(next_permutation(arr, arr+3));
// 应当注意，next_permutaion是直接在序列上更新下一个序列
// 并且是在更新完后再检查是否是排名更靠后的序列，是则返回true，否则返回false
// 因此，此时的arr为{ 1, 2, 3 }

// prev_permutation同理
int brr[3]={2, 1, 3};

do{								// cout
	for(int i=0; i<3; i++)      // 1 2 3
		cout<<brr[i]<<" ";      // 1 3 2
	cout<<endl;					// 1 2 3
}while(prev_permutation(brr, brr+3));
// 此时的brr为{ 3, 2, 1 }

```

> BFS

```cpp
void BFS(int sx, sy){
	queue<PII> q;
	q.push( {sx, sy} ); // 入队
	used[sx][sy]=1;     // 标记使用
	step[sx][sy]=0;     // 更新步长
	
	while( q.size() ){
		// 当前步
		int x = q.front().fst;
		int y = q.front().sed; 
		
		for(int i=0; i<4; i++){
			// 下一步
			int nx = x+dxy[i][0];
			int ny = y+dxy[i][1];
			if(nx<0 || nx>=R || ny<0 || ny>=C) continue; 				// 检查越界
			if(used[nx][ny]==1 || g[nx][ny]=='不能到达的点') continue; 	// 检查能否到达（此步是否合法）
			
			q.push( {nx, ny} );        // 入队
			used[nx][ny]=1;            // 标记使用
			step[nx][ny]=step[x][y]+1; // 更新步长

			if(g[nx][ny]=='终点') return;
		}
		
		q.pop();
	}

	return ;
}
```


<h4 id=3.2>3.2 最短路</h4>

> `Dijkstra（非负权边）`
> 1. 将所有点看作未确定最短路
> 2. 将起点的dis[s]=0，其余置为+∞
> 3. 将未确定的点中，选取距离s最近的点进行松弛操作
```cpp
int n, m;   // 点、边
vector<PII> edge[100010];   // edge[x]={y,z} x->y=z
int dis[N]; // 最短路
bool st[N]; // 是否已确定最短路
// O(mlogm)
int dijkstra(int s, int e){
    // fst存dis[i] sed存i
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    // 初始化
    memset(dis, 0x3f, sizeof dis);
    dis[s]=0;
    // 起点入队
    heap.push({dis[s], s});  

    while(heap.size()){
        auto x=heap.top().sed;  // 取最小点
        heap.pop();
        // 在出堆的时候判断、确定是否确定最短路
        if(st[x]) continue;
        st[x]=1; // 确定最短路

        // 对该点进行松弛操作
        for(auto& [y, z]: edge[x])
            if( dis[y]>dis[x]+z ){
                dis[y]=dis[x]+z;
                heap.push({dis[y], y});
            }
    }
    return dis[e];  
}
```


> `bellman_ford（万能）`
> 1. 将起点的dis[s]=0，其余置为+∞
> 2. 将所有边进行松弛操作
> 3. 直到某轮迭代中没有执行松弛操作时，退出，或者执行n次
```cpp
int n, m, k; // 最多经过k条边
vector<PII> edge[N];
int dis[N];  // 最短路
int bkup[N]; // 备份，防止串联
// O(nm)
int bellman_ford(int s, int e){
    memset(dis, 0x3f, sizeof dis);
    dis[s]=0;

    for(int i=1; i<=k; i++){ // 最多迭代 k 次
        bool flg=0;
        memcpy(bkup, dis, sizeof dis);
        for(int x=1; x<=n; x++)     // 遍历所有边
        for(auto& [y, z]: edge[x])  // 对每条边进行松弛操作
            if( dis[y] > bkup[x] + z){
                dis[y] = bkup[x] + z;
                flg=1;
            }
        if(!flg) break;  // 当前没有进行松弛操作
    }
    // 因为存在无穷大引出的边发生松弛操作
    // 而图中存在负权边，因此要做个小小的处理
    if(dis[e]>INF/2) return INF; 
    return dis[e];
}
```


> `spfa（非负权回路）`
> 队列优化版 bellman_ford
> 1. 将起点的dis[s]=0，其余置为+∞
> 2. 将被松弛的点加入队列，进行松弛操作
>     * 通过一个 st[i] 来判断 i 点是否在队列中，防止重复入队
```cpp
int n, m;
vector<PII> edge[N];
int dis[N]; // 最短路
bool st[N]; // 是否在队列中?
O(nm)
int spfa(int s, int e){
    queue<int> q;
    memset(dis, 0x3f, sizeof dis);
    
    dis[s]=0;   // 源点
    st[s]=1;    // 在队列中
    q.push(s);  // 扔进队列

    while(q.size()){
        auto x=q.front();
        q.pop();
        st[x]=0; // 不在队列中

        for(auto& [y, z]: edge[x])
            if( dis[y] > dis[x] + z ){
                dis[y] = dis[x] + z;
                if(!st[y]){ // 如果被松弛的点不在队列中
                    st[y]=1;// 就压入队列
                    q.push(y);
                }
            }
    }
    return dis[e];
}
```

> `Floyd（多源最短路）`
> 1. 将自己到达自己的最短路设置为0，其余设置为+∞
> 2. 任意两点，都可以经过任意一点，尝试松弛操作
```cpp
// 注意，此写法省略了边的存储，及直接将边存储到最短路中
int n, m;
int dis[N][N];
// O(n^3)
void flody(){
    for(int k=1; k<=n; k++)
        for(int i=1; i<=n; i++)
            for(int j=1; j<=n; j++)
                dis[i][j]=min(dis[i][j], dis[i][k]+dis[k][j]);
}
```

<h4 id=3.3>3.3 最小生成树</h4>

> `Prim 稠密图`
> 1. 地图初始化为+∞
> 2. 将所有点到达集合的距离设置为+∞，且标记都不在集合中
> 3. 迭代n次，每次将未在集合中且距离集合最近的点加入集合
> 4. 通过新进入集合的点，更新所有点距离集合的距离
```cpp
int n, m;
int edge[N][N];
int dis[N]; // 每个点到达（最小生成树）集合的最短路
bool st[N]; // 是否已经在集合中
// O(n^2+m)
int prim(){
    int res=0;
    memset(dis, 0x3f, sizeof dis);  // 初始化每个点到集合的距离为inf
    memset(st, 0, sizeof st);       // 初始化每个点都未在集合中

    for(int k=0; k<n; k++){ // 迭代 n 次

        int t=-1;           // 找到未在集合中且距离集合最近的点
        for(int i=1; i<=n; i++)
            if(!st[i] && (t==-1 || dis[t]>dis[i]))
                t=i;
        if(k && dis[t]==INF) return INF;    // 孤岛
        
        if(k) res+=dis[t];                  // 权值和
        st[t]=1;                // 进入集合
        for(int i=1; i<=n; i++) // 通过该点去更新
            dis[i]=min(dis[i], edge[t][i]);
    }
    return res;
}
```

> `Kruskal 稀疏图`
> 1. 将所有边从小到大排序
> 2. 维护一堆集合，查询两个元素是否属于同一集合，合并俩集合

```cpp
int n, m;
int p[N];   // 并查集
pair<int, PII> edge[N];

int find(int x){  // 找x的祖宗
    if(p[x]!=x) p[x]=find(p[x]);
    return p[x];
}

int kruskal(){
    sort(edge+1, edge+1+m); 
    for(int i=1; i<=n; i++) p[i]=i;
    // 排序 初始化并查集
    int res=0, cnt=0;
    // 从小到大枚举所有边
    for(int i=1; i<=m; i++){
        int a=edge[i].sed.fst;
        int b=edge[i].sed.sed;
        int w=edge[i].fst;
        a=find(a), b=find(b);
        if(a!=b){
            p[a]=b; // 合并
            res+=w; // 权值和
            cnt++;  // 判断孤岛
        }
    }

    if(cnt<n-1) return inf;
    return res;
}
```
















<h3 id=4>4 数学</h3>

<h4 id=4.1>4.1 质（素）数与约数</h4>
 
> `gcd` 欧几里得算法（辗转相除法）
```cpp
int gcd(int a, int b){
	return b ? gcd(b, a%b) : a;
}
```
> STL中有个相似的 `__gcd(int x, int y)`

> 裴蜀定理：对于任意整数 a,b 存在一对整数 x,y 满足 ax+by=gcd(a,b)  
`exgcd` 拓展欧几里得算法

```cpp
int exgcd(int a, int b, int &x, int &y){
	if(!b){
		x=1, y=0;
		return a;
	}
	int d = exgcd(b, a%b, y, x);
	y -= (a/b)*x;
	return d; 
}
```
> `LCD`
```cpp
int  lcd(int x, int y){
    return a*b/gcd(a, b);
}
```
> `快筛素数` 先生成(2^16)内的素数 再快判
```cpp
int primes[6555], cnt;  // 2^16中只有6542个素数
bool st[65555];         // 生成的时候只用判断这么多个

void make_primes(int n) {
	for (int i = 2; i <= n; i++) {
		if (!st[i]) primes[cnt++] = i;              // 当前数没被筛过st[i]==0，说明是素数
		for (int j = 0; primes[j] <= n / i; j++) {  // 确保 第j个质数 和 i 相乘不会爆
			st[primes[j] * i] = 1;                  // 被唯一标记过
			if (i % primes[j] == 0) break;          // 此时primes[j]是i的最小质因子，退出避免重复筛
		}
	}
	return ;
}

bool is_p(int x) {
    if(!st[0]) make_primes(65536), st[0]=1;
    if(x<=65536) return !st[x];
	for (int i = 0; primes[i] <= x/primes[i]; i++) 
		if (x % primes[i] == 0)
			return 0;
	return 1;
}
```

<h4 id=4.2>4.2 快速幂</h4>

```cpp
// a^b%MOD
LL quick_pow(LL a, int b){
    LL res=1%MOD;
    for( ; b; b>>=1){
        if(b&1) res=1LL*a*res%MOD; // 决定是否相乘
        a=1ll*a*a%MOD;             // 每个位置上，递推出的二进制位上的值
    }
    return res;
}
```


<h3 id=5>5 动态规划</h3>








<h3 id=6>6 字符串</h3>

<h4 id=6.1>6.1 KMP</h4>

```cpp
// 主串和模式串
// 注意!!!从下标1开始!!!
void KMP(string s, string p){
    int n=s.size()-1, m=p.size()-1;
    int ne[100010]={0}; // 模式串的next串
    //求ne数组
    for(int i = 2, j = 0; i <= m; i++) {
        while(j && p[i] != p[j + 1]) j = ne[j];
        if(p[i] == p[j + 1]) j++;
        ne[i] = j;
    }
    //kmp匹配
    for(int i = 1, j = 0; i <= n; i++) {
        while(j && s[i] != p[j + 1]) j = ne[j];
        if(s[i] == p[j + 1]) j++;
        if(j == m) {
            j = ne[j]; //当匹配成功时继续往下匹配
            // 你的操作!!!
        }
    }
    return ;
}
```





<h3 id=7>7 计算几何</h3>