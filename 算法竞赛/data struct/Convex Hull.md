# 维护凸壳
有若干个 线性函数$y=k_i*x+b_i$，对于不同的 $x$ ，查找可以得到的最小/大的 $y$
$o(1)插入，o(logn)查询$
下凸壳，求最小
```cpp
using ll =long long;
#define pb push_back
struct Line{
    ll k, b;
};
struct CHTMin{
    vector<Line> lines;
    bool bad(const Line& a, const Line& b, const Line& c){
        return (__int128)(b.b - a.b) * (c.k - b.k) <= (__int128)(c.b - b.b) * (b.k - a.k);
    }
    void Add(ll k, ll b){ //添加一组k,b
        Line newLine = {k, b};
        while (lines.size() >= 2 && bad(lines[lines.size() - 2], lines.back(), newLine)) lines.pop_back();
        lines.pb(newLine);
    }
    ll Ask(ll x){ //查询x对应的最小的y
        int l = 0, r = lines.size() - 1;
        while (l < r){
            int mid = (l + r) / 2;
            ll y1 = lines[mid].k * x + lines[mid].b;
            ll y2 = lines[mid + 1].k * x + lines[mid + 1].b;
            if (y1 >= y2) l = mid + 1;
            else r = mid;
        }
        return lines[l].k * x + lines[l].b;
    }
	void Run(vector<pair<ll,ll>>& v){ //传(ki,bi)数组，不需要预处理，下标1开始
        lines.clear();
        map<ll,ll> mp;
        int _n=(int)v.size()-1;
        for(int i=1;i<=_n;++i){
            auto& [k,b]=v[i];
            if(mp.count(k)==0) mp[k]=b;
            else mp[k]=min(mp[k],b);
        }
        for(auto it=mp.rbegin();it!=mp.rend();++it){
            Add(it->first,it->second);
        }
    };
};
```
上凸壳，求最大
```cpp
using ll =long long;
#define pb push_back
struct Line {
    ll k, b;  // 直线: y = k*x + b

};
struct CHTMax {
    vector<Line> lines;
    bool bad(const Line& a, const Line& b, const Line& c) {
        return (__int128)(b.b - a.b) * (c.k - b.k) >= (__int128)(c.b - b.b) * (b.k - a.k);
    }
    // 添加新的一组k,b
    void Add(ll k, ll b) {
        Line newLine = {k, b};
        while (lines.size() >= 2 && bad(lines[lines.size() - 2], lines.back(), newLine)) {
            lines.pop_back();
        }
        lines.push_back(newLine);
    }
    // 查询x对应的y最大值
    ll Ask(ll x) {
        int l = 0, r = lines.size() - 1;
        while (l < r) {
            int mid = (l + r) / 2;
            ll y1 = lines[mid].k * x + lines[mid].b;
            ll y2 = lines[mid + 1].k * x + lines[mid + 1].b;
            if (y1 <= y2) l = mid + 1;  
            else r = mid;
        }
        return lines[l].k * x + lines[l].b;
    }
	void Run(vector<pair<ll,ll>>& v){  //传(ki,bi)数组，不需要排序预处理，下标1开始
        lines.clear();
        map<ll,ll> mp;
        int _n=(int)v.size()-1;
        for(int i=1;i<=_n;++i){
            auto& [k,b]=v[i];
            if(mp.count(k)==0) mp[k]=b;
            else mp[k]=min(mp[k],b);
        }
        for(auto it=mp.begin();it!=mp.end();++it){
            Add(it->first,it->second);
        }
    };
};
```