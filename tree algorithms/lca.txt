#include<bits/stdc++.h>
#include<ext/pb_ds/assoc_container.hpp>
#include<ext/pb_ds/tree_policy.hpp>
#include <sstream>
#define fast_io ios_base::sync_with_stdio(false);cin.tie(NULL)
#define MOD 1000000007
#define MOD1 998244353
#define INF 1e18
#define nline "\n"
#define pb push_back
#define pf push_front
#define ppb pop_back
#define mp make_pair
#define ff first
#define ss second
#define PI 3.141592653589793238462
#define set_bits __builtin_popcountll
#define sz(x) ((int)(x).size())
#define all(x) (x).begin(), (x).end()
 
using namespace std;
using namespace __gnu_pbds;
 
typedef long long ll;
typedef unsigned long long ull;
typedef long double lld;
//typedef tree<int, null_type, less<int>, rb_tree_tag, tree_order_statistics_node_update > pbds; // find_by_order, order_of_key
 
#ifndef ONLINE_JUDGE
#define debug(x) cerr << #x <<" "; _print(x); cerr << endl;
#else
#define debug(x)
#endif
 
void _print(ll t) {cerr << t;}
void _print(int t) {cerr << t;}
void _print(string t) {cerr << t;}
void _print(char t) {cerr << t;}
void _print(lld t) {cerr << t;}
void _print(double t) {cerr << t;}
void _print(ull t) {cerr << t;}
 
template <class T, class V> void _print(pair <T, V> p);
template <class T> void _print(vector <T> v);
template <class T> void _print(set <T> v);
template <class T, class V> void _print(map <T, V> v);
template <class T> void _print(multiset <T> v);
template <class T, class V> void _print(pair <T, V> p) {cerr << "{"; _print(p.ff); cerr << ","; _print(p.ss); cerr << "}";}
template <class T> void _print(vector <T> v) {cerr << "[ "; for (T i : v) {_print(i); cerr << " ";} cerr << "]";}
template <class T> void _print(set <T> v) {cerr << "[ "; for (T i : v) {_print(i); cerr << " ";} cerr << "]";}
template <class T> void _print(multiset <T> v) {cerr << "[ "; for (T i : v) {_print(i); cerr << " ";} cerr << "]";}
template <class T, class V> void _print(map <T, V> v) {cerr << "[ "; for (auto i : v) {_print(i); cerr << " ";} cerr << "]";}
template <class T, class V> void _print(unordered_map <T, V> v) {cerr << "[ "; for (auto i : v) {_print(i); cerr << " ";} cerr << "]";}
 
template<class T> using pbds =tree<T, null_type, less<T>, rb_tree_tag, tree_order_statistics_node_update> ;

/*---------------------------------------------------------------------------------------------------------------------------*/
ll gcd(ll a, ll b) {if (b > a) {return gcd(b, a);} if (b == 0) {return a;} return gcd(b, a % b);}
ll expo(ll a, ll b, ll mod) {ll res = 1; while (b > 0) {if (b & 1)res = (res * a) % mod; a = (a * a) % mod; b = b >> 1;} return res;}
void extendgcd(ll a, ll b, ll*v) {if (b == 0) {v[0] = 1; v[1] = 0; v[2] = a; return ;} extendgcd(b, a % b, v); ll x = v[1]; v[1] = v[0] - v[1] * (a / b); v[0] = x; return;} //pass an arry of size1 3
ll mminv(ll a, ll b) {ll arr[3]; extendgcd(a, b, arr); return arr[0];} //for non prime b
ll mminvprime(ll a, ll b) {return expo(a, b - 2, b);}
bool revsort(ll a, ll b) {return a > b;}
ll combination(ll n, ll r, ll m, ll *fact, ll *ifact) {ll val1 = fact[n]; ll val2 = ifact[n - r]; ll val3 = ifact[r]; return (((val1 * val2) % m) * val3) % m;}
void google(int t) {cout << "Case #" << t << ": ";}
vector<ll> sieve(int n) {int*arr = new int[n + 1](); vector<ll> vect; for (int i = 2; i <= n; i++)if (arr[i] == 0) {vect.push_back(i); for (int j = 2 * i; j <= n; j += i)arr[j] = 1;} return vect;}
ll mod_add(ll a, ll b, ll m) {a = a % m; b = b % m; return (((a + b) % m) + m) % m;}
ll mod_mul(ll a, ll b, ll m) {a = a % m; b = b % m; return (((a * b) % m) + m) % m;}
ll mod_sub(ll a, ll b, ll m) {a = a % m; b = b % m; return (((a - b) % m) + m) % m;}
ll mod_div(ll a, ll b, ll m) {a = a % m; b = b % m; return (mod_mul(a, mminvprime(b, m), m) + m) % m;}  //only for prime m
ll phin(ll n) {ll number = n; if (n % 2 == 0) {number /= 2; while (n % 2 == 0) n /= 2;} for (ll i = 3; i <= sqrt(n); i += 2) {if (n % i == 0) {while (n % i == 0)n /= i; number = (number / i * (i - 1));}} if (n > 1)number = (number / n * (n - 1)) ; return number;} //O(sqrt(N))
//ll getRandomNumber(ll l, ll r) {return uniform_int_distribution<ll>(l, r)(rng);} 
/*--------------------------------------------------------------------------------------------------------------------------*/


//-----------computational geometry--------------------
//(x1,y1) (x2,y2)  vector representation of AB->(x2-x-y1)
//AB.BC=|AB||BC|cos@  AB(x1,y1) BC(x2,y2) AB.BC=x1*x2+y1*y2
//ABxBC=|AB||BC|sin@    vecA(x1,y1) vecB(x2,y2)  vecA x vecB=x1*y2-y1*x2
//area of parallelogram=AxB=|B||Asin@| (magnitude)
//area of triangle = AxB/2 
//dist btw 2 pts- sqrt((x2-x1)^2 + (y2-y1)^2)
//perpendicular distance of point(C) from line(AB) (ABxAC)/|AB| Base->AB Height->|AC|sin@
//dist of point C to line segment AB, if(AB.AC)=-ve i.e. @>=90 so near A (AC is ans)
//else if(AB.BC)=+ve C is near pt B (BC is ans)
//else if both pts contradict point C is in btw AB so perpendicular dist is ans
 
using namespace std;
void init_code()
{
    fast_io;
    #ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    freopen("error.txt", "w", stderr);
    #endif 
}


void dfs(int root, vector<vector<int>>& edges, vector<bool>& visited,vector<vector<int>>& parent, vector<int>& depth)
{
    visited[root] = true;
    for (auto i : edges[root]) 
    {
        if (!visited[i]) 
        {
            depth[i]=depth[root]+1;
            parent[i][0] = root;
            dfs(i, edges, visited, parent, depth);
        }
    }
}

int lca(int a, int b, int n, vector<int>& depth, vector<vector<int>>& parent)
{
    if(depth[a]<depth[b])
        swap(a,b);
    
    int diff=depth[a]-depth[b];
    for(int i=log2(n)+4;i>=0;i--)
    {
        if((1<<i)&diff)
            a=parent[a][i];
    }
    
    if(a==b)
        return a;
    
    for(int i=log2(n)+4;i>=0;i--)
    {
        if(parent[a][i]!=parent[b][i])
        {
            a=parent[a][i];
            b=parent[b][i];
        }
    }
    return parent[a][0];
}

int main() 
{
    init_code();
    ll t=1;
    //cin>>t;
    while(t--)
    {
        int n,q;
        cin>>n>>q;
        vector<vector<int>> edges(n);
        vector<bool> visited(n,false);
        for(int i=1;i<n;i++)
        {
            int a,b;
            cin>>a>>b;
            a--,b--;
            edges[a].pb(b);
            edges[b].pb(a);
        }
        
        vector<vector<int>> parent(n,vector<int>(log2(n)+5,-1));
        vector<int> depth(n,0);
        dfs(0,edges,visited,parent,depth);

        for(int i=1;i<log2(n)+5;i++)
        {
            for(int j=0;j<n;j++)
            {
                int intermediate=parent[j][i-1];
                if(intermediate!=-1)
                    parent[j][i]=parent[intermediate][i - 1];
            }
        }
        debug(edges)
        
        while(q--)
        {
            int node1,node2;
            cin>>node1>>node2;
            node1--,node2--;
            int lca_node=lca(node1,node2,n,depth,parent);
            cout<<lca_node+1<<nline;
        }
    }   
    return 0;
}  
