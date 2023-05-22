环境配置：`-lm -Wall -Wl,--stack=536870912 -O2 -std=c++14`

```cpp
#include<bits/stdc++.h>
#define ld long double
#define ui unsigned int
#define ull unsigned long long
#define int long long
#define eb emplace_back
#define pb pop_back
#define mp make_pair
#define pii pair<int,int>
#define fi first
#define se second
#define power(x) ((x)*(x))
#define gcd(x,y) __gcd(x,y)
#define lcm(x,y) (x*y/gcd(x,y))
#define lg(x,y)  __lg(x,y)
using namespace std;

template<typename T=int> inline T read()
{
	T s=0,w=1; char c=getchar();
	while(!isdigit(c)) {if(c=='-') w=-1; c=getchar();}
	while(isdigit(c)) s=(s<<1)+(s<<3)+(c^48),c=getchar();
	return s*w;
}
template<typename T=int> inline void write(T x,char ch)
{
	if(x<0) x=-x,putchar('-');
	static char stk[25]; int top=0;
	do {stk[top++]=x%10+'0',x/=10;} while(x);
	while(top) putchar(stk[--top]);
	putchar(ch);
	return;
}

namespace MyTool
{
	static const int Mod=998244353;
	template<typename T> inline void Swp(T &a,T &b) {T t=a;a=b;b=t;}
	template<typename T> inline void cmax(T &a,T b) {a=a>b?a:b;}
	template<typename T> inline void cmin(T &a,T b) {a=a<b?a:b;}
	template<typename T> inline void Madd(T &a,T b) {a=a+b>Mod?a+b-Mod:a+b;}
	template<typename T> inline void Mdel(T &a,T b) {a=a-b<0?a-b+Mod:a-b;}
	template<typename T> inline void Mmul(T &a,T b) {a=a*b%Mod;}
	template<typename T> inline void Mmod(T &a)     {a=(a%Mod+Mod)%Mod;}
	template<typename T> inline T    Cadd(T a,T b)  {return a+b>=Mod?a+b-Mod:a+b;}
	template<typename T> inline T    Cdel(T a,T b)  {return a-b<0?a-b+Mod:a-b;}
	template<typename T> inline T    Cmul(T a,T b)  {return a*b%Mod;}
	template<typename T> inline T    Cmod(T a)      {return (a%Mod+Mod)%Mod;}
	inline int qpow(int a,int b) {int res=1; while(b) {if(b&1) Mmul(res,a); Mmul(a,a); b>>=1;} return res;}
	inline int qmul(int a,int b) {int res=0; while(b) {if(b&1) Madd(res,a); Madd(a,a); b>>=1;} return res;}
	inline int Qpow(int a,int b) {int res=1; while(b) {if(b&1) res=qmul(res,a); a=qmul(a,a); b>>=1;} return res;} 
}
using namespace MyTool;

inline void file()
{
	freopen(".in","r",stdin);
	freopen(".out","w",stdout);
	return;
}

bool Mbe;

namespace LgxTpre
{
	static const int MAX=500010;
	static const int inf=2147483647;
	static const int INF=4557430888798830399;
	static const int mod=1e9+7;
	static const int bas=131;
	
	inline void lmy_forever()
	{
		
	}
}

bool Med;

signed main()
{
//	file();
	fprintf(stderr,"%.3lf MB\n",abs(&Med-&Mbe)/1048576.0);
	int Tbe=clock();
	LgxTpre::lmy_forever();
	int Ted=clock();
	cerr<<1e3*(Ted-Tbe)/CLOCKS_PER_SEC<<" ms\n";
	return (0-0);
}
```