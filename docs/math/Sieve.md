1. 线性筛素数。
2. 筛法求欧拉函数。
3. 筛法求莫比乌斯函数。
4. 筛法求约数个数：$d_i$ 表示 $i$ 的约数个数，$num_i$ 表示 $i$ 的最小质因子出现次数。
5. 筛法求约数和：$f_i$ 表示 $i$ 的约数和，$g_i$ 表示 $i$ 的最小质因子和。

```cpp
namespace Sieve
{
	static const int N=100000;
	static const int MAX=10010;
	int vis[N+10],P[MAX],Pcnt;
	inline void Linear_Sieve()
	{
		for(int i=2;i<=N;++i)
		{
			if(!vis[i]) P[++Pcnt]=i;
			for(int j=1;j<=Pcnt&&i*P[j]<=N;++j)
			{
				vis[i*P[j]]=1;
				if(!i%P[j]) break;
			}
		}
		return;
	}
	
	int phi[N+10];
	inline void Phi_Sieve()
	{
		phi[1]=1;
		for(int i=2;i<=N;++i)
		{
			if(!vis[i]) P[++Pcnt]=i,phi[i]=i-1;
			for(int j=1;j<=Pcnt&&i*P[j]<=N;++j)
			{
				vis[i*P[j]]=1;
				if(i%P[j]) phi[i*P[j]]=phi[i]*phi[P[j]];
				else      {phi[i*P[j]]=phi[i]*P[j]; break;}
			}
		}
		return;
	}
	
	int mu[N+10];
	inline void Mu_Sieve()
	{
		mu[1]=1;
		for(int i=2;i<=N;++i)
		{
			if(!vis[i]) P[++Pcnt]=i,mu[i]=-1;
			for(int j=1;j<=Pcnt&&i*P[j]<=N;++j)
			{
				vis[i*P[j]]=1;
				if(i%P[j]) mu[i*P[j]]=-mu[i];
				else      {mu[i*P[j]]=0; break;}
			}
		}
		return;
	}
	
	int d[N+10],num[N+10];
	inline void D_Sieve()
	{
		d[1]=1;
		for(int i=2;i<=N;++i)
		{
			if(!vis[i]) P[++Pcnt]=i,d[i]=2,num[i]=1;
			for(int j=1;j<=Pcnt&&i*P[j]<=N;++j)
			{
				vis[i*P[j]]=1;
				if(i%P[j]) num[i*P[j]]=1,d[i*P[j]]=d[i]*2;
				else      {num[i*P[j]]=num[i]+1,d[i*P[j]]=d[i]/num[i*P[j]]*(num[i*P[j]]+1); break;}
			}
		}
		return;
	}
	
	int f[N+10],g[N+10];
	inline void Sum_Sieve()
	{
		f[1]=g[1]=1;
		for(int i=2;i<=N;++i)
		{
			if(!vis[i]) P[++Pcnt]=i,f[i]=g[i]=i+1;
			for(int j=1;j<=Pcnt&&i*P[j]<=N;++j)
			{
				vis[i*P[j]]=1;
				if(i%P[j]) f[i*P[j]]=f[i]*f[P[j]],g[i*P[j]]=P[j]+1;
				else      {f[i*P[j]]=f[i]/g[i]*(g[i]*P[j]+1),g[i*P[j]]=g[i]*P[j]+1; break;}
			}
		}
		return;
	}
}
```