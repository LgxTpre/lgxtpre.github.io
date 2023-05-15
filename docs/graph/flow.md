### 最大流：
```cpp
namespace MaxFlow
{
	struct edge{int nex,to,val;}e[MAX<<1];
	int head[MAX],cnt=1;
	inline void add(int x,int y,int z)
	{
		e[++cnt].nex=head[x],head[x]=cnt,e[cnt].to=y,e[cnt].val=z;
		e[++cnt].nex=head[y],head[y]=cnt,e[cnt].to=x,e[cnt].val=0;
		return;
	}
	
	int s,t;
	int maxflow,d[MAX],pre[MAX];
	queue<int> q;
	inline bool Layer()
	{
		memset(d,0,sizeof d);
		while(!q.empty()) q.pop();
		d[s]=1,pre[s]=head[s],q.push(s);
		while(!q.empty())
		{
			int now=q.front(); q.pop();
			for(int i=head[now];i;i=e[i].nex)
			{
				int to=e[i].to,val=e[i].val;
				if(!d[to]&&val)
				{
					d[to]=d[now]+1,pre[to]=head[to],q.push(to);
					if(to==t) return 1;
				}
			}
		}
		return 0;
	}
	int Dinic(int now,int flow)
	{
		if(now==t) return flow;
		int rest=flow;
		for(int i=pre[now];i;i=e[i].nex)
		{
			int to=e[i].to,val=e[i].val;
			pre[now]=i;
			if(d[to]==d[now]+1&&val)
			{
				int k=Dinic(to,min(val,rest));
				if(!k) d[to]=0;
				e[i].val-=k,e[i^1].val+=k,rest-=k;
				if(!rest) break;
			}
		}
		return flow-rest;
	}
	inline void solve()
	{
		while(Layer()) maxflow+=Dinic(s,INF);
		return;
	}
}
```