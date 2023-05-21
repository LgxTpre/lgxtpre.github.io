```cpp
namespace Prime_Check
{
    int mix,seed,step;
    static const int P[]={2,3,5,7,11,13,17,19,23,29,31,37};
    static const int Pcnt=12;
    inline int qmul(int a,int b,int mod) {int ans=a*b-(int)((long double)a*b/mod+0.5)*mod; return ans<0?ans+mod:ans;}
    inline int qpow(int a,int b,int mod) {int res=1; while(b) {if(b&1) res=qmul(res,a,mod); a=qmul(a,a,mod); b>>=1;} return res;}
    inline int f(int x,int mod) {return (qmul(x,x,mod)+seed)%mod;}
    inline int randoom(int l,int r)
    {
        static mt19937_64 sd(20070707);
        static uniform_int_distribution<int> range(l,r);
        return range(sd);
    }
    inline bool Miller_Robin(int x)
    {
        if(x<=2) return x==2;
        int y_=x-1; while(!(y_&1)) y_>>=1;
        for(int i=0;i<Pcnt;++i)
        {
            if(x==P[i]) return 1;
            int flag=0,y=y_,z=qpow(P[i],y,x);
            if(z==1) flag=1;
            else while(y<x-1)
            {
                if(z==x-1) {flag=1; break;}
                y<<=1,z=qmul(z,z,x);
            }
            if(!flag) return 0;
        }
        return 1;
    }
    inline int floyd(int x)
    {
        seed=randoom(0,x-1);
        int fast,slow,res=1; fast=slow=randoom(0,x-1);
        fast=f(fast,x);
        for(int i=0;slow!=fast;++i)
        {
            res=qmul(res,(fast-slow)%x+x,x);
            if(!res) res=(fast-slow)%x+x;
            if(i%step==0) {int g=gcd(res,x); if(g!=1) return g; res=1;}
            slow=f(slow,x),fast=f(f(fast,x),x);
        }
        return gcd(res,x);
    }
    inline void Pollard_Rho(int x)
    {
        if(x==1) return;
        if(Miller_Robin(x)) return cmax(mix,x);
        int k=1;
        step=((int)log(x))<<1|1;
        while(k==1) k=floyd(x);
        Pollard_Rho(k),Pollard_Rho(x/k);
    }
    inline int solve(int n)
    {
        mix=0; Pollard_Rho(n);
        if(mix==n) return -1; else return mix;
    }
}
```