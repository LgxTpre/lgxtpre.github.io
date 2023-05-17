### dijktra 单源最短路：
```cpp
namespace dijkstra
{
	struct edge{int nex,to,val;}e[MAX<<1];
	int head[MAX],cnt;
	inline void add(int x,int y,int z)
	{
		e[++cnt].nex=head[x],head[x]=cnt,e[cnt].to=y,e[cnt].val=z;
		return;
	}
	
	int dis[MAX],vis[MAX];
	priority_queue<pii> q;
	inline void dijkstra()
	{
		memset(dis,0x3f,sizeof dis),memset(vis,0,sizeof vis);
		dis[s]=0,q.push(mp(0,s));
		while(!q.empty())
		{
			int now=q.top().second; q.pop();
			if(vis[now]) continue; vis[now]=1;
			for(int i=head[now];i;i=e[i].nex)
			{
				int to=e[i].to;
				if(dis[to]>dis[now]+e[i].val)
					dis[to]=dis[now]+e[i].val,
					q.push(mp(-dis[to],to));
			}
		}
		return;
	}
}
```

### SPFA 单源最短路：
```cpp
namespace SPFA
{
	struct edge{int nex,to,val;}e[MAX<<1];
	int head[MAX],cnt;
	inline void add(int x,int y,int z)
	{
		e[++cnt].nex=head[x],head[x]=cnt,e[cnt].to=y,e[cnt].val=z;
		return;
	}
	
	int dis[MAX],vis[MAX];
	queue<int> q;
	inline void SPFA()
	{
		memset(dis,0x3f,sizeof dis),memset(vis,0,sizeof vis);
		q.push(s),dis[s]=0,vis[s]=1;
		while(!q.empty())
		{
			int now=q.front(); q.pop();
			vis[now]=0;
			for(int i=head[now];i;i=e[i].nex)
			{
				int to=e[i].to;
				if(dis[to]>dis[now]+e[i].val)
				{
					dis[to]=dis[now]+e[i].val;
					if(!vis[to]) vis[to]=1,q.push(to);
				}
			}
		}
		return;
	}
}
```

### SPFA 判负环：
```cpp
namespace Check_Loop
{
	struct edge{int nex,to,val;}e[MAX<<1];
	int head[MAX],cnt;
	inline void add(int x,int y,int z)
	{
		e[++cnt].nex=head[x],head[x]=cnt,e[cnt].to=y,e[cnt].val=z;
		return;
	}
	
	int dis[MAX],vis[MAX],con[MAX];
	queue<int> q;
	inline bool SPFA()
	{
		memset(dis,0x3f,sizeof dis),memset(vis,0,sizeof vis),memset(con,0,sizeof con);
		q.push(s),dis[s]=0,vis[s]=1;
		while(!q.empty())
		{
			int now=q.front(); q.pop();
			vis[now]=0;
			for(int i=head[now];i;i=e[i].nex)
			{
				int to=e[i].to;
				if(dis[to]>dis[now]+e[i].val)
				{
					dis[to]=dis[now]+e[i].val,con[to]=con[now]+1;
					if(con[now]>=n) return 1;
					if(!vis[to]) vis[to]=1,q.push(to);
				}
			}
		}
		return 0;
	}
}
```

### Johnson全源最短路：
```cpp
namespace Johnson
{
	struct edge{int nex,to,val;}e[MAX<<1];
	int head[MAX],cnt;
	inline void add(int x,int y,int z)
	{
		e[++cnt].nex=head[x],head[x]=cnt,e[cnt].to=y,e[cnt].val=z;
		return;
	}
	
	int ans[MAX][MAX];
	int dis[MAX],vis[MAX],con[MAX]; queue<int> q;
	int Dis[MAX],Vis[MAX];          priority_queue<pii> Q;
	inline bool SPFA(int st)
	{
		while(!q.empty()) q.pop();
		memset(vis,0,sizeof vis),memset(dis,0x3f,sizeof dis),memset(con,0,sizeof con);
		q.push(st),vis[st]=1,dis[st]=0;
		while(!q.empty())
		{
			int now=q.front(); q.pop();
			vis[now]=0;
			for(int i=head[now];i;i=e[i].nex)
			{
				int to=e[i].to;
				if(dis[to]>dis[now]+e[i].val)
				{
					dis[to]=dis[now]+e[i].val;
					con[to]=con[now]+1;
					if(con[now]>=n) return 1;
					if(!vis[to]) vis[to]=1,q.push(to);
				}
			}
		}
		return 0;
	}
	inline void dijkstra(int st)
	{
		memset(Vis,0,sizeof Vis),memset(Dis,0x3f,sizeof Dis);
		Q.push(mp(0,st)),Dis[st]=0;
		while(!Q.empty())
		{
			int now=Q.top().second; Q.pop();
			if(Vis[now]) continue; Vis[now]=1;
			for(int i=head[now];i;i=e[i].nex)
			{
				int to=e[i].to;
				if(Dis[to]>Dis[now]+e[i].val)
					Dis[to]=Dis[now]+e[i].val,
					Q.push(mp(-Dis[to],to));
			}
		}
		return;
	}
	inline bool solve()
	{
		for(int i=1;i<=n;++i) add(0,i,0);
		if(SPFA(0)) return 1;
		for(int i=1;i<=n;++i) for(int j=head[i];j;j=e[j].nex) e[j].val+=dis[i]-dis[e[j].to];
		for(int i=1;i<=n;++i)
		{
			dijkstra(i);
			for(int j=1;j<=n;++j) if(Dis[j]==INF) ans[i][j]=-1; else ans[i][j]=Dis[j]+dis[j]-dis[i];
		}
		return 0;
	}
}
```