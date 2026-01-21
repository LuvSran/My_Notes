# 区间合并定义
将有交集的区间合并
*eg*.
    [1,3],[3,5] --> [1,5]
    [1,3],[3,5],[7,10],[9,12] --> [1,5],[7,12]
## 模板
```
typedef pair<int,int> Pii;
vector<pii> a;
vector<pii> merge(vector<pii> &vc){
    int st=-2e9,ed=-2e9;
    vector<pii> seg;
    sort(vc.begin(),vc.end());
    for(auto x:vc){
        if(x.first>ed){
            if(ed!=-2e9) seg.push_back({st,ed});
                st=x.first; ed=x.second;
            }
        else if(x.first<=ed){
            ed=max(ed,x.second);
        } 
    }
   if(st!=-2e9) seg.push_back({st,ed}); //更新最后个区间，if条件防止传入空区间，还更新st,ed;
    return seg;
}
int main(){
    ios::sync_with_stdio(false); cin.tie(nullptr); cout.tie(nullptr);
    int n;
    cin >> n;
    while(n--){
        int l,r;
        cin >> l >> r;
        a.push_back({l,r});
    }
    auto ans=merge(a);
    cout << ans.size() ; //输出区间数量
    return 0;
}
```