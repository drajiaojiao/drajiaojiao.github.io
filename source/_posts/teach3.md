---
title: 《算法竞赛进阶指南》 
excerpt: 以此为纲
index_img: /img/teach/3.png
banner_img: /img/teach/3.png
date: 2022-11-22 20:00:00
categories: teach
---

<h1 id=1><font color="green">初窥门径</font></h1>
<details>
<summary>目录</summary>

  1. 基础算法
       * [枚举与模拟](#1.1.1)
       * 递推与递归 [0x02]()
       * 前缀和与差分 [0x03]()
       * 排序 [0x05]()
       * 二分 [0x04]()
       * 双指针
  
  2. 数据结构
       * 栈 [0x11]()
       * 队列 [0x12]() 
       * 哈希表 [0x14]()
       * 堆
  
  3. 搜索
       * DFS [0x22]()
       * BFS [0x25]()
       * 剪枝优化与Flood Fill
    
  4. 字符串
       * KMP [0x15]() 
       * Trie [0x16]()

  5. 动态规划
       * 背包问题 [0x52]()
       * 线性DP [0x51]()
       * 区间DP [0x53]()
  
  6. 数学
       * 进制转换
       * 高精度
       * 快速幂
       * 质（素）数
       * 约数个数
       * 组合计数 [0x36]()

  7. 图论
       * 最短路 [0x61]()
       * 拓扑排序 
       * 最小生成树 [0x62]()
  </details>


<h2 id=1.1><font color="green"> 一、基础算法 </font></h2>

* <h3 id=1.1.1> 枚举与模拟 </h3>

<details> 
<summary>AcWing 466.回文日期</summary>  

* [AcWing 466. 回文日期](https://www.acwing.com/problem/content/468/) 

```cpp
#include <iostream>
using namespace std;

int months[13]={0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
bool dateOK(int ymd){
    months[2]=28; // 恢复现场
    int y=ymd/10000;
    int m=ymd%10000/100;
    int d=ymd%100;
    if((y%4==0 && y%100!=0) || y%400==0) months[2]=29; // 闰年
    if(m<1 || m>12) return 0;
    if(d<1 || d>months[m]) return 0;
    return 1;
}

int main(){
    int date1, date2, ans=0;
    cin>>date1>>date2;
    
    for(int i=date1/10000; i<=date2/10000; i++){
        // 生成回文日期
        // 判断日期是否合法，是否在date区间
        int j=i%10*1000
             +i/10%10*100
             +i/100%10*10
             +i/1000%10;
        int k=i*10000+j;
        
        if(dateOK(k) && k>=date1 && k<=date2)
            ans++;
    }
    
    cout<<ans;
    
    return 0;
}
```
</details>


<details> 
<summary>AcWing 441. 数字统计</summary>

* [AcWing 441. 数字统计](https://www.acwing.com/problem/content/443/)

```cpp
#include <iostream>
using namespace std;

int main(){
    int l, r, ans=0;
    cin>>l>>r;
    for(int i=l; i<=r; i++){
        int j=i;
        while(j){
            if(2 == j%10) ans++;
            j/=10;
        }
    }
    cout<<ans;
    return 0;
}
```

</details>

<details> 
<summary>AcWing 1245. 特别数的和</summary>

* [AcWing 1245. 特别数的和](https://www.acwing.com/problem/content/1247/)

```cpp
#include <iostream>
using namespace std;

int n, res;

bool is(int i){
    while(i){
        int j=i%10;
        if(j==2 || j==0 || j==1 || j==9) return 1;
        i/=10;
    }
    return 0;
}

int main(){
    cin>>n;
    for(int i=1; i<=n; i++)
        if(is(i)) res+=i;
    
    cout<<res;
    
    return 0;
}
```

</details>

<details> 
<summary>AcWing 1210. 连号区间数</summary>  

* [AcWing 1210. 连号区间数](https://www.acwing.com/problem/content/1212/)

```cpp
#include <iostream>
using namespace std;

const int N=1e4+10;
int arr[N], n, res;

int main(){
    cin>>n;
    for(int i=1; i<=n; i++) scanf("%d", &arr[i]);
    
    // 枚举每个区间
    for(int l=1; l<=n; l++){
        int mmax=arr[l];
        int mmin=arr[l];
        for(int r=l; r<=n; r++){
            mmin=min(mmin, arr[r]);
            mmax=max(mmax, arr[r]);
            
            if(r-l==mmax-mmin) res++;
        }
    }
    
    cout<<res;
    
    return 0;
}
```

</details>


<details> 
<summary>AcWing 1204. 错误票据</summary>  

* [AcWing 1204. 错误票据](https://www.acwing.com/problem/content/description/1206/)

```cpp
#include <iostream>
using namespace std;

const int N=1e5+10;

int arr[N], mmin=0x3f3f3f3f, mmax=-0x3f3f3f3f, m, n;

int main(){
    int t; cin>>t;
    while(scanf("%d", &t)!=EOF){
        arr[t]++;
        mmin = mmin<t ? mmin:t;
        mmax = max(mmax, t);
    }    
    
    for(int i=mmin; i<=mmax; i++){
        if(arr[i]==2) n=i;
        if(arr[i]==0) m=i;
    }
    
    cout<<m<<" "<<n;
    
    return 0;
}
```

</details>

<details> 
<summary>AcWing 1236. 递增三元组</summary>  

* [AcWing 1236. 递增三元组](https://www.acwing.com/problem/content/1238/)

```cpp
#pragma G++ optimize("Ofast")
#include <iostream>
#include <algorithm>
using namespace std;

typedef long long LL;

const int N=1e5+10;

LL ans;
int arr[N], brr[N], crr[N], n;

int main(){
    cin>>n;
    for(int i=1; i<=n; i++) scanf("%d", &arr[i]);
    for(int i=1; i<=n; i++) scanf("%d", &brr[i]);
    for(int i=1; i<=n; i++) scanf("%d", &crr[i]);
    
    sort(arr+1, arr+1+n);
    sort(brr+1, brr+1+n);
    sort(crr+1, crr+1+n);
    
    // 枚举b
    for(int i=1; i<=n; i++){
        int a = lower_bound(arr+1, arr+1+n, brr[i])-arr-1;
        int c = upper_bound(crr+1, crr+1+n, brr[i])-crr;
        
        if(a>=1 && c<=n)
            ans+=1LL*a*(n-c+1);
    }
    
    cout<<ans;
    
    return 0;
}
```

</details>



<details> 
<summary>AcWing 1219. 移动距离</summary>  

* [AcWing 1219. 移动距离](https://www.acwing.com/problem/content/1221/)

```cpp
#include <iostream>
using namespace std;

int main(){
    int w, m, n;
    cin>>w>>m>>n;
    
    // 1维变2维
    int x1=(m-1)/w+1;
    int y1=m%w;
    if(y1==0) y1=w;
    
    int x2=(n-1)/w+1;
    int y2=n%w;
    if(y2==0) y2=w;
    
    if(!(x1&1)) y1=w-y1+1;    
    if(!(x2&1)) y2=w-y2+1;
    
    //cout<<x1<<" "<<y1<<"\n"<<x2<<" "<<y2<<"\n";
    
    cout<<abs(x2-x1)+abs(y2-y1);
    
    return 0;
}
```

</details>





* <h3 id=1.1.2> 递推与递归 </h3>

* <h3 id=1.1.3> 前缀和与差分 </h3>

* <h3 id=1.1.4> 排序 </h3>

* <h3 id=1.1.5> 二分 </h3>

* <h3 id=1.1.6> 双指针 </h3>






















<h2 id=1.3><font color="green"> 三、搜索 </font></h2>

* <h3 id=1.3.1> DFS </h3>

* <h3 id=1.3.2> BFS </h3>

<details> 
<summary> AcWing 1101. 献给阿尔吉侬的花束 </summary>  

* [AcWing 1101. 献给阿尔吉侬的花束](https://www.acwing.com/problem/content/1103/)

```cpp
#define fst first
#define sed second
#include <iostream>
#include <cstring>
#include <queue>
using namespace std;

typedef pair<int, int> PII;

const int dxy[4][2]={{-1,0}, {1,0}, {0,-1}, {0,1}};
const int inf=0x3f3f3f3f;
const int N=210;

int g[N][N];
int st[N][N];
int res[N][N];
PII s, e;
int n, m;

void solve(){
    memset(g, 0, sizeof g);
    memset(st, 0, sizeof st);
    memset(res, 0, sizeof res);
    
    cin>>n>>m;
    for(int i=1; i<=n; i++){
        string t; cin>>t;
        for(int j=1; j<=m; j++)
            if(t[j-1]=='S')
                s={i,j};
            else if(t[j-1]=='E')
                e={i,j};
            else if(t[j-1]=='#')
                g[i][j]=1;
    }
    
    queue<PII> q;
    res[s.fst][s.sed]=0;
    q.push(s), st[s.fst][s.sed]=1;
    
    while(q.size()){
        auto u=q.front();
        q.pop();
        
        for(int i=0; i<4; i++){
            int nx=u.fst+dxy[i][0];
            int ny=u.sed+dxy[i][1];
            
            if(g[nx][ny]) continue;
            if(nx<1 || ny<1 || nx>n || ny>m) continue;
            if(st[nx][ny]) continue;
            
            res[nx][ny]=res[u.fst][u.sed]+1;
            q.push({nx, ny}), st[nx][ny]=1;
        }
        if(st[e.fst][e.sed]) break; // 剪枝
    }
    
    if(res[e.fst][e.sed])
        cout<<res[e.fst][e.sed]<<"\n";
    else
        cout<<"oop!\n";
        
    return ;
}

int main(){
    int T; cin>>T; while(T--)
    solve();
    return 0;
}
```

</details>

BFS：武士风度的牛、逃离迷宫、地牢大师、A计划


* <h3 id=1.3.3> 剪枝优化与Flood Fill </h3>

<details> 
<summary> AcWing 1113. 红与黑 </summary>  

* [AcWing 1113. 红与黑](https://www.acwing.com/problem/content/1115/)

```cpp
// dfs 写法
#define fst first
#define sed second
#include <iostream>
#include <cstring>
using namespace std;

typedef pair<int, int> PII;

const int dxy[][2]={{-1,0},{1,0},{0,-1},{0,1}};
const int N=30;

int n, m, cnt;
bool g[N][N];
bool st[N][N];
PII S;

void dfs(int x, int y){
    cnt++;
    for(int i=0; i<4; i++){
        int nx=x+dxy[i][0];
        int ny=y+dxy[i][1];
        
        if(nx<1 || ny<1 || nx>n || ny>m) continue;
        if(g[nx][ny] || st[nx][ny]) continue;
        
        st[nx][ny]=1;
        dfs(nx, ny);
    }
    return ;
}

void solve(){
    cnt=0;
    memset(g, 0, sizeof g);
    memset(st, 0, sizeof st);
    
    for(int i=1; i<=n; i++){
        string s; cin>>s;
        for(int j=1; j<=m; j++)
            if(s[j-1]=='#') g[i][j]=1;
            else if(s[j-1]=='@') S={i,j};
    }
    
    // 进入这个位置，将其能够搜索到的其他位置打上标记
    st[S.fst][S.sed]=1;
    dfs(S.fst, S.sed);
    cout<<cnt<<"\n";
    return ;
}

int main(){
    while(cin>>m>>n)
        if(m!=0 && n!=0) solve();
    return 0;
}
```


```cpp
// bfs 写法
#define fst first
#define sed second
#include <iostream>
#include <cstring>
#include <queue>
using namespace std;

typedef pair<int, int> PII;

const int dxy[][2]={{-1,0},{1,0},{0,-1},{0,1}};
const int N=30;

int n, m, cnt;
bool g[N][N];
bool st[N][N];
PII S;

void bfs(int x, int y){
    
    queue<PII> q;
    q.push({x, y}), st[x][y]=1, cnt++;
    
    while(q.size()){
        auto u=q.front();
        q.pop();
        
        for(int i=0; i<4; i++){
            int nx=u.fst+dxy[i][0];
            int ny=u.sed+dxy[i][1];
            
            if(nx<1 || ny<1 || nx>n || ny>m) continue;
            if(g[nx][ny] || st[nx][ny]) continue;
            
            q.push({nx, ny}), st[nx][ny]=1, cnt++;
        }
    }
}

void solve(){
    cnt=0;
    memset(g, 0, sizeof g);
    memset(st, 0, sizeof st);
    
    for(int i=1; i<=n; i++){
        string s; cin>>s;
        for(int j=1; j<=m; j++)
            if(s[j-1]=='#') g[i][j]=1;
            else if(s[j-1]=='@') S={i,j};
    }
    
    // 进入这个位置，将其能够搜索到的其他位置打上标记
    st[S.fst][S.sed]=1;
    bfs(S.fst, S.sed);
    cout<<cnt<<"\n";
    return ;
}

int main(){
    while(cin>>m>>n)
        if(m!=0 && n!=0) solve();
    return 0;
}
```

</details>



<details> 
<summary> AcWing 1233. 全球变暖 </summary>  

* [AcWing 1233. 全球变暖](https://www.acwing.com/problem/content/1235/)

```cpp
#define fst first
#define sed second
#include <iostream>
#include <cstring>
#include <queue>
using namespace std;

typedef pair<int, int> PII;

const int N=1010;
const int dxy[][2]={{-1,0}, {1,0}, {0,-1}, {0,1}};

bool g[N][N];
bool st[N][N];
int n, res;

// 如果存在一个点，满足其上下左右都是岛屿的情况，那么这个点肯定不会被淹没
bool bfs(int x, int y){
    bool flg=0; 
    
    queue<PII> q;
    q.push({x,y}), st[x][y]=1;
    
    while(q.size()){
        auto u=q.front();
        q.pop();
        
        int cnt=0;
        for(int i=0; i<4; i++){
            int nx=u.fst+dxy[i][0];
            int ny=u.sed+dxy[i][1];
            
            if(g[nx][ny]==1) cnt++;
            
            if(nx<1 || ny<1 || nx>n || ny>n) continue;
            if(g[nx][ny]!=1 || st[nx][ny]) continue;
            
            q.push({nx, ny}), st[nx][ny]=1;
        }
        if(cnt==4) flg=1; // 存在上下左右都有岛屿的情况
    }
    
    return flg==0;
}

int main(){
    cin>>n;
    for(int i=1; i<=n; i++){
        string s; cin>>s;
        for(int j=1; j<=n; j++)
            if(s[j-1]=='#') g[i][j]=1;
    }
    
    for(int i=1; i<=n; i++)
        for(int j=1; j<=n; j++)
            if(g[i][j] && !st[i][j])
                if( bfs(i,j) ) res++;
    
    cout<<res;
    
    return 0;
}
```

</details>



<details> 
<summary> Luogu P1141 01迷宫 </summary>  

* [Luogu P1141 01迷宫](https://www.luogu.com.cn/problem/P1141)

```cpp
#define fst first
#define sed second
#include <iostream>
#include <cstring>
#include <queue>
using namespace std;

typedef pair<int, int> PII;

const int dxy[4][2]={{-1,0}, {1,0}, {0,-1}, {0,1}};
const int N=1e3+10;

int n, m, flg, cnt;

bool g[N][N];
bool st[N][N];
int res[N][N]; // 用res存标记，每个标记对应连接数量
int num[1000000];

void bfs(int ux, int uy){
	
	queue<PII> q;
	q.push({ux, uy}), st[ux][uy]=1, res[ux][uy]=flg;
	
	while(q.size()){
		auto u=q.front();
		q.pop();
		cnt++;
		
		for(int i=0; i<4; i++){
			int nx=u.fst+dxy[i][0];
			int ny=u.sed+dxy[i][1];

			if(nx<1 || ny<1 || nx>n || ny>n) continue;
			if(g[nx][ny]==g[u.fst][u.sed] || st[nx][ny]) continue;

			q.push({nx, ny}), st[nx][ny]=1, res[nx][ny]=flg;
		}
	}
}

int main(){ 
    cin>>n>>m;
	for(int i=1; i<=n; i++){
		string s; cin>>s;
		for(int j=1; j<=n; j++)
			if(s[j-1]=='1') g[i][j]=1;
	}
    
	for(int i=1; i<=n; i++)
		for(int j=1; j<=n; j++)
			if(!st[i][j]) 
				cnt=0, ++flg, bfs(i, j), num[flg]=cnt;
    // 因为每个 连通块 的大小，需要走完才知道
    // 所以我们在进入每一块连通块时，给次连通块每个位置做上唯一flg
    // bfs结束的时候，将cnt赋值给num[flg]，查询时，就可以直接查询了

	for(int i=1; i<=m; i++){
		int a, b; scanf("%d%d", &a, &b);
		cout<<num[res[a][b]]<<"\n";
	}

    return 0;
}
```

</details>

















<h2 id=1.6><font color="green"> 六、数学 </font></h2>

* <h3 id=1.6.4> 质（素）数 </h3>

<details> 
<summary> AcWing 866. 试除法判定质数 </summary>  

* [AcWing 866. 试除法判定质数](https://www.acwing.com/problem/content/868/)

```cpp
#include <iostream>
using namespace std;

bool isP(int n){
    if(n<2) return false;
    for(int i=2; i<=n/i; i++)
        if(n%i==0)
            return false;
    return true;
}

void solve(){
    int n; cin>>n;
    
    if(isP(n))
        puts("Yes");
    else
        puts("No");
    return ;
}

int main(){
    int T; cin>>T; while(T--)
    solve();
    return 0;
}
```

</details>

<details> 
<summary> AcWing 867. 分解质因数 </summary>  

* [AcWing 867. 分解质因数](https://www.acwing.com/problem/content/869/)

```cpp
#include <iostream>
using namespace std;

void solve(){
    int n; cin>>n;
    
    for(int i=2; i<=n/i; i++)
        if(n%i==0){
            // 质因数 i
            
            int cnt=0;
            while(n%i==0){
                n/=i;
                cnt++;
            }
            printf("%d %d\n", i, cnt);
        }
        
    if(n>1) printf("%d 1\n", n);
    puts("");
}

int main(){
    int T; cin>>T; while(T--)
    solve();
    return 0;
}
```

</details>


<details> 
<summary> AcWing 868. 筛质数 </summary>  

* [AcWing 867. 分解质因数](https://www.acwing.com/problem/content/description/870/)



```cpp
// 埃式筛 O(nloglogn)
#include <iostream>
using namespace std;

const int N=1e6+10;

// 素数的数量 近似 N/(2*lgN)

int primes[100000], cnt;  // 素数集合
bool st[N];     // 标记合数

void make_primes(int n){
    st[1]=1;
    for(int i=2; i<=n; i++)
        if(!st[i]){
            primes[++cnt]=i;
            for(int j=i; j<=n; j+=i) st[j]=1;   // 标记合数
        }
}

int main(){
    int n; cin>>n;
    
    make_primes(n);
    
    cout<<cnt;
    
    return 0;
}
```

```cpp
// 欧拉筛 O(n)
#include <iostream>
using namespace std;

const int N=1e6+10;

int primes[100000], cnt;
bool st[N];

void make_primes(int n){
    st[1]=1;
    for(int i=2; i<=n; i++){
        if(!st[i]) primes[++cnt]=i;
        for(int j=1; primes[j]<=n/i; j++){
            st[ primes[j]*i ] = 1;  // 用最小质因子去筛
            if(i%primes[j]==0) break;
        }
    }
}

int main(){
    int n; cin>>n;
    
    make_primes(n);
    
    cout<<cnt;
    
    return 0;
}
```
</details>



<details> 
<summary>Luogu P1835 素数密度 </summary>  

* [Luogu P1835 素数密度](https://www.luogu.com.cn/problem/P1835)


```cpp
#include <iostream>
#include <cmath>
#include <unordered_set>
using namespace std;

typedef long long LL;

const int N=1e6+10;

int primes[100000], cnt;
bool st[N];

LL L, R, ans;
unordered_set <LL> H;    // 如果H=1，说明被筛掉

// 对于 n 而言
// 其质因子的范围是 [ 2-sqrt(n) ]
// 因此我们可以用质因子去将[ L-R ] 中的合数给弄出来
void make_primes(int n){
    st[1]=1;
    for(int i=2; i<=n; i++){
        if(!st[i]) primes[++cnt]=i;
        for(int j=1; primes[j]<=n/i; j++){
            st[ primes[j]*i ] = 1;  // 用最小质因子去筛
            if(i%primes[j]==0) break;
        }
    }
}

int main(){
    make_primes( sqrt(pow(2,31)-1) );
    
    cin>>L>>R;
    for(int i=1; i<=cnt; i++){  // 枚举质数
        LL b = L / primes[i];
        LL j = b * primes[i];
        while(j<L) j+=primes[i];   // 定位到大于L，同时能被primes[i]整除的第一个数
        
        for( ; j<=R; j+=primes[i])
            H.insert(j); // 筛掉再说
    }

    for(LL i=L; i<=R; i++){        
        if(i<sqrt(pow(2,31)-1)) {
            if(st[i]==0) ans++;
            continue;
        }
        if(!H.count(i)) ans++;
    }
    cout<<ans;
    
    return 0;
}
```


</details>





<h2 id=1.7><font color="green"> 七、图论 </font></h2>

* <h3 id=1.7.1> 最短路 </h3>

<details> 
<summary> AcWing 849. Dijkstra求最短路 I <font style="background-color:red", color=black><b>dijkstra[ 模板 ]</b></font></summary>  

* [AcWing 849. Dijkstra求最短路 I](https://www.acwing.com/problem/content/851/)

```cpp
#include <iostream>
#include <cstring>
using namespace std;

const int N=510;
const int inf=0x3f3f3f3f;

int n, m;
int edge[N][N]; // 稠密图
int dis[N]; // 最短路
bool st[N]; // 是否确定最短

void dijkstra(int s, int e){
    memset(dis, 0x3f, sizeof dis);
    
    dis[s]=0;
    
    for(int i=1; i<=n; i++){
        
        int t=-1;
        for(int j=1; j<=n; j++)
            if(!st[j] && (t==-1 ||  dis[t]>dis[j]))
                t=j;    
        
        st[t]=true;
        if(t==e) break;
        
        for(int j=1; j<=n; j++)
            dis[j]=min(dis[j], dis[t]+edge[t][j]);
    }
}

int main(){
    memset(edge, 0x3f, sizeof edge);
    cin>>n>>m;
    for(int i=1; i<=m; i++){
        int x, y, z;
        scanf("%d%d%d", &x, &y, &z);
        edge[x][y]=min(edge[x][y], z);
    }
    
    dijkstra(1, n);
    
    if(dis[n]==inf)
        cout<<"-1";
    else
        cout<<dis[n];
    
    return 0;
}
```

</details>

<details> 
<summary>Luogu P4779 【模板】单源最短路径（标准版） <font style="background-color:red", color=black><b>dijkstra堆优化[ 模板 ]</b></font></summary>  

* [Luogu P4779 【模板】单源最短路径（标准版）](https://www.luogu.com.cn/problem/P4779)

```cpp
#define fst first
#define sed second
#define pb push_back
#include <iostream>
#include <cstring>
#include <queue>
#include <vector>
using namespace std;

typedef pair<int, int> PII;

const int N=1e5+10;
const int inf=0x3f3f3f3f;

vector<vector<PII>> edge(N);    // 邻接表
bool st[N];
int dis[N];

int n, m, S;

void dijkstra(int s){
    memset(dis, 0x3f, sizeof dis);
    priority_queue<PII, vector<PII>, greater<PII>> heap;

    dis[s]=0;
    heap.push({dis[s], s});

    while(heap.size()){
        auto x=heap.top().sed;
        heap.pop();

        if(st[x]) continue;
        st[x]=1;

        for(auto &[y, z]: edge[x])
            if( dis[y] > dis[x]+z ){
                dis[y] = dis[x]+z;
                heap.push({dis[y], y});
            }
    }

}

int main(){
    cin>>n>>m>>S;
    for(int i=1; i<=m; i++){
        int x, y, w;
        scanf("%d%d%d", &x, &y, &w);
        edge[x].pb({y, w});
    }

    dijkstra(S);

    for(int i=1; i<=n; i++) cout<<dis[i]<<" ";

    return 0;
}
```

</details>


<details> 
<summary>AcWing 853. 有边数限制的最短路 <font style="background-color:orange", color=black><b>bellman-ford[ 模板 ]</b></font></summary>  

* [AcWing 853. 有边数限制的最短路](https://www.acwing.com/problem/content/855/)

```cpp
#include <iostream>
#include <cstring>
using namespace std;

const int inf=0x3f3f3f3f;
const int N=510;

int n, m, k;
int edge[N][N];
int dis[N];
int bkup[N];

void bellman_ford(int s, int e){
    memset(dis, 0x3f, sizeof dis);
    
    dis[s]=0;
    
    for(int i=1; i<=k; i++){
        bool flg=0;
        memcpy(bkup, dis, sizeof dis);   
        
        for(int x=1; x<=n; x++)
            for(int y=1; y<=n; y++)
                if( dis[y] > bkup[x]+edge[x][y] ){
                    dis[y] = bkup[x]+edge[x][y];
                    flg=1;
                }
        
        if(!flg) break;
    }
}

int main(){
    memset(edge, 0x3f, sizeof edge);
    cin>>n>>m>>k;
    for(int i=1; i<=m; i++){
        int x, y, z;
        scanf("%d%d%d", &x, &y, &z);
        edge[x][y]=min(edge[x][y], z);
    }
    
    bellman_ford(1, n);
    
    if(dis[n]>inf/2)
        cout<<"impossible";
    else
        cout<<dis[n];
    
    return 0;
}
```

</details>

<details> 
<summary>AcWing 851. spfa求最短路 <font style="background-color:orange", color=black><b>bellman-ford队列优化（spfa）[ 模板 ]</b></font> </summary>  

* [AcWing 851. spfa求最短路](https://www.acwing.com/problem/content/853/)

```cpp
#pragma G++ optimize("Ofast")

#define fst first
#define sed second
#include <iostream>
#include <queue>
#include <cstring>
using namespace std;

typedef pair<int, int> PII;

const int inf=0x3f3f3f3f;
const int N=1e5+10;

int n, m;
vector<PII> edge[N];
int dis[N]; 
bool st[N]; // 是否在队列中

void spfa(int s, int e){
    memset(dis, 0x3f, sizeof dis);
    queue<int> q;
    
    dis[s]=0;
    q.push(s), st[s]=1;
    
    while(q.size()){
        auto x=q.front();
        q.pop(), st[x]=0;
        
        for(auto& [y, z]: edge[x])
            if( dis[y] > dis[x]+z){
                dis[y] = dis[x]+z;
                if(!st[y])
                    q.push(y), st[y]=1;
            }
    }
}

int main(){
    cin>>n>>m;
    for(int i=1; i<=m; i++){
        int x, y, z;
        scanf("%d%d%d", &x, &y, &z);
        edge[x].push_back({y, z});
    }
    
    spfa(1, n);
    
    if(dis[n]==inf)
        cout<<"impossible";
    else
        cout<<dis[n];
    
    return 0;
}
```

</details>

<details> 
<summary> Luogu P3371 【模板】单源最短路径（弱化版） <font style="background-color:orange", color=black><b>spfa??[ 模板 ]</b></font></summary>  

* [Luogu P3371 【模板】单源最短路径（弱化版）](https://www.luogu.com.cn/problem/P3371)

</details>

<details> 
<summary>AcWing 854. Floyd求最短路 <font style="background-color:yellow", color=black><b>Floyd[ 模板 ]</b></font> </summary>  

* [AcWing 854. Floyd求最短路](https://www.acwing.com/problem/content/856/)

```cpp
#include <iostream>
using namespace std;

const int inf=0x3f3f3f3f;
const int N=210;

int n, m, k;
int dis[N][N];

void floyd(){
    for(int k=1; k<=n; k++)
        for(int i=1; i<=n; i++)
            for(int j=1; j<=n; j++)
                dis[i][j]=min(dis[i][j], dis[i][k]+dis[k][j]);
}

int main(){
    cin>>n>>m>>k;
    
    for(int i=1; i<=n; i++)
        for(int j=1; j<=n; j++)
            if(i==j) dis[i][j]=0;
            else dis[i][j]=inf;
                
    for(int i=1; i<=m; i++){
        int x, y, z; 
        scanf("%d%d%d", &x, &y, &z);
        dis[x][y]=min(dis[x][y], z);
    }
    
    floyd();
    
    for(int i=1; i<=k; i++){
        int x, y;
        scanf("%d%d", &x, &y);
        if(dis[x][y]>inf/2) puts("impossible");
        else cout<<dis[x][y]<<"\n";
    }
    
    return 0;
}
```

</details>

<details> 
<summary> Luogu P1629 邮递员送信 <font style="background-color:red", color=black><b>dijkstra堆优化</b></font></summary> 

* [Luogu P1629 邮递员送信](https://www.luogu.com.cn/problem/P1629)

```cpp
#define fst first
#define sed second
#define pb push_back
#include <iostream>
#include <cstring>
#include <queue>
#include <vector>
using namespace std;

typedef long long LL;
typedef pair<int ,int> PII;

const int N=1e3+10;

vector<vector<PII>> edge(N);
vector<vector<PII>> redge(N); // 反向边
bool st[N];
int dis[N]; // 1到每个点的最短路

int n, m;
LL ans;

void dijkstra(int s, vector<vector<PII>> &e){
    memset(st, 0, sizeof st);
    memset(dis, 0x3f, sizeof dis);
    priority_queue<PII, vector<PII>, greater<PII>> heap;

    dis[s]=0;
    heap.push({dis[s], s});

    while(heap.size()){
        auto x=heap.top().sed;
        heap.pop();

        if(st[x]) continue;
        st[x]=1;

        for(auto &[y, z]: e[x])
            if( dis[y] > dis[x]+z ){
                dis[y] = dis[x]+z;
                heap.push({dis[y], y});
            }
    }
}

int main(){
    //freopen("in.txt", "r", stdin); freopen("out.txt", "w", stdout);
    int n, m;
    cin>>n>>m;
    for(int i=1; i<=m; i++){
        int x, y, w;
        scanf("%d%d%d", &x, &y, &w);
        edge[x].pb({y, w});
        redge[y].pb({x, w});
    }
    
    dijkstra(1, edge);
    for(int i=2; i<=n; i++) ans+=dis[i];
    dijkstra(1, redge);
    for(int i=2; i<=n; i++) ans+=dis[i];
    cout<<ans;

    return 0;
}
```

</details>


<details> 
<summary>  Luogu P1119 灾后重建 <font style="background-color:yellow", color=black><b>Floyd</b></font></summary> 

* [Luogu P1119 灾后重建](https://www.luogu.com.cn/problem/P1119)

```cpp
#include <iostream>
#include <cstring>
using namespace std;

const int N=210;

int dis[N][N];
int ti[N];
bool st[N];

int n, m, q;

int floyd(int x, int y, int t){
    for(int k=0; k<n; k++){
        if(ti[k]>t || st[k]) continue;
        st[k]=1;

        for(int i=0; i<n; i++)
            for(int j=0; j<n; j++)
                if( dis[i][j] > dis[i][k] + dis[k][j])
                    dis[j][i] = dis[i][j] = dis[i][k] + dis[k][j];
    }

    if(dis[x][y]==0x3f3f3f3f || ti[x]>t || ti[y]>t) 
        return -1;
    else
        return dis[x][y];
}

int main(){
    freopen("in.txt", "r", stdin); freopen("out.txt", "w", stdout);
    memset(dis, 0x3f, sizeof dis);
    cin>>n>>m;
    for(int i=0; i<n; i++) scanf("%d", &ti[i]);
    for(int i=1; i<=m; i++){
        int x, y, w;
        scanf("%d%d%d", &x, &y, &w);
        dis[x][y]=min(dis[x][y], w);
        dis[y][x]=min(dis[y][x], w);
    }
    for(int i=0; i<n; i++) dis[i][i]=0;

    cin>>q;
    while(q--){
        int x, y, t;
        scanf("%d%d%d", &x, &y, &t);
        cout<<floyd(x, y, t)<<"\n";
    }

    return 0;
}
```

```cpp
#pragma G++ optimize("Ofast")
#define fst first
#define sed second

#include <iostream>
#include <cstring>
#include <queue>
#include <vector>
using namespace std;

typedef pair<int, int> PII;

const int N=210;

int edge[N][N];
int dis[N]; // 最短路
bool st[N]; // 是否确定最短路

int n, m, q;
int ti[N];  // 每个村庄完成重建的时间

int dijkstra(int s, int e, int t){
    memset(dis, 0x3f, sizeof dis);
    memset(st, 0, sizeof st);
    priority_queue<PII, vector<PII>, greater<PII>> heap;

    dis[s]=0;
    heap.push({dis[s], s});
    
    while(heap.size()){
        auto x=heap.top().sed;
        heap.pop();

        if(st[x]) continue;
        st[x]=1;

        edge[s][x]=min(edge[s][x], dis[x]); // 更新最短路

        for(int y=1; y<=n; y++){    // 用邻接表可能会更快
            int z=edge[x][y];

            if( dis[y] > dis[x]+z && ti[x]<=t && ti[y]<=t ){
                dis[y] = dis[x]+z;
                heap.push({dis[y], y});
            }
        }
    }

    if(dis[e]==0x3f3f3f3f)
        return -1;
    else
        return dis[e];
}

int main(){
    //freopen("in.txt", "r", stdin); freopen("out.txt", "w", stdout);
    memset(edge, 0x3f, sizeof edge);
    cin>>n>>m;
    for(int i=1; i<=n; i++) scanf("%d", &ti[i]);    
    for(int i=1; i<=m; i++){
        int x, y, w;
        scanf("%d%d%d", &x, &y, &w);
        x++, y++;
        edge[x][y]=min(edge[x][y], w);
        edge[y][x]=min(edge[y][x], w);
    }

    cin>>q;
    while(q--){
        int x, y, t;
        scanf("%d%d%d", &x, &y, &t);
        x++, y++;
        cout<<dijkstra(x, y, t)<<"\n";
    }
    return 0;
}
```

</details>




<details>
<summary> AcWing 1488. 最短距离 </summary>

* [AcWing 1488. 最短距离](https://www.acwing.com/problem/content/1490/)

```cpp
// 超级源点
#pragma G++ optimize("Ofast")

#define pb push_back
#define fst first
#define sed second

#include <iostream>
#include <queue>
#include <cstring>
#include <vector>
using namespace std;

typedef pair<int, int> PII;

const int inf=0x3f3f3f3f;
const int N=1e5+10;

int n, m, k, q;
vector<PII> edge[N];
int dis[N];
bool st[N];

int dijkstra(int s){
    int ans=inf;
    memset(dis, 0x3f, sizeof dis);
    memset(st, 0, sizeof st);
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    
    dis[s]=0;
    heap.push({dis[s], s});
    
    while(heap.size()){
        auto x=heap.top().sed;
        heap.pop();
        
        if(st[x]) continue;
        st[x]=1;
        
        //cout<<dis[x]<<"\n";
        for(auto &[y, z]: edge[x])
            if( dis[y] > dis[x]+z ){
                dis[y] = dis[x]+z;
                heap.push({dis[y], y});
            }
    }
    
    return ans;
}

int main(){
    cin>>n>>m;
    for(int i=1; i<=m; i++){
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        edge[a].pb({b, c});
        edge[b].pb({a, c});
    }
    cin>>k;
    for(int i=1; i<=k; i++){
        int j;
        scanf("%d", &j);
        edge[n+1].pb({j, 0});
    }
    
    dijkstra(n+1);
    
    cin>>q;
    while(q--){
        int s;
        scanf("%d", &s);
        cout<<dis[s]<<"\n";
    }
    
    return 0;
}
```

</details>

<details>

<summary> AcWing 1507. 旅行计划 </summary>

* [AcWing 1507. 旅行计划](https://www.acwing.com/problem/content/description/1509/)

```cpp
// 记录路径
#pragma G++ optimize("Ofast")

#define pb push_back
#define fst first
#define sed second
#include <iostream>
#include <cstring>
#include <queue>
using namespace std;

typedef pair<int, int> PII;

const int N = 510;

int n, m, S, D;
vector<pair<int, PII>> edge[N]; // x->y 
int pre[N]; // 最短路径中，最短的一条路径的前驱
PII dis[N]; // 最短路径及最小花费
bool st[N]; // 是否确定最小

void dijkstra(int s, int e) {
    memset(dis, 0x3f, sizeof dis);
    priority_queue<pair<PII, int>, vector<pair<PII, int>>, greater<pair<PII, int>>> heap;

    dis[s] = { 0, 0 };
    heap.push({ dis[s], s });

    while (heap.size()) {
        auto x = heap.top().sed;
        heap.pop();

        if (st[x]) continue;
        st[x] = 1;

        for (auto& [y, w] : edge[x]) {
            PII t = { dis[x].fst + w.fst, dis[x].sed + w.sed };
            if (dis[y] > t) {
                dis[y] = t;
                heap.push({ dis[y], y });
                pre[y]=x;   // 通过x点到y
            }
        }
    }
}

int main() {
    cin >> n >> m >> S >> D;
    for (int i = 1; i <= m; i++) {
        int a, b, c, d;
        scanf("%d%d%d%d", &a, &b, &c, &d);
        edge[a].pb({ b, {c, d} });
        edge[b].pb({ a, {c, d} });
    }

    dijkstra(D, S); // 反着来，这样可以直接用pre输出

    int u = S;
    cout<<S<<" ";
    while (u != D) cout << pre[u] << " ", u = pre[u];

    cout << dis[S].fst << " " << dis[S].sed;

    return 0;
}
```

</details>

</details>


































































---
<h1 id=2><font color="blue">牛刀小试</font></h1>

  <details>
  <summary>目录</summary>

  1. 基础算法
       * 位运算 [0x01]()
       * 倍增 [0x39]()
       * 构造 
  
  2. 数据结构
        * 并查集 [0x41]()
        * 树状数组 [0x42]()
        * 线段树 [0x43]()
        * 二叉搜索树与平衡树 [0x46]()
        * 分块
        * 点分治
        * 可持久化数据结构

  3. 搜索
       * 迭代加深
       * 双向DFS或BFS
       * BFS变形
       * A* 与 IDA*
  </details>

---


<h1 id=3><font color="red">略有所成</font></h1>

  <details>
  <summary>目录</summary>

  </details>



