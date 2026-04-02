```cpp
struct Manacher{
    vector<int> r;  // r[i]:以 t[i] 为中心的最长回文半径（包含自身）
    string t;       // 预处理后的字符串，下标从 1 开始
    void build(string& _s) { 
        int _n = _s.size() - 1;  // _s[1..n] 是有效部分
        t = "##";               // t[0] 和 t[1] 是占位符
        for (int i=1;i<=_n;++i) {
            t+=_s[i];
            t+='#';
        }
        int _m=t.size()-1;   // t[1..m] 是有效部分
        r.assign(_m+5,0);        // r[1..m] 存储回文半径
        for (int i=1,j=0;i<=_m;++i){
            if(2*j-i>=1 && j+r[j]>i)  r[i]=min(r[2*j-i], j+r[j]-i);
            while(i-r[i]>=1 && i+r[i]<=_m && t[i-r[i]]==t[i+r[i]]) r[i]+=1;
            if (i+r[i]>j+r[j]) j = i;
        }
    }
    int Len1(int idx){  // 以 s[idx] 为中心的最长奇回文串长度
        return r[2*idx]-1;
    }
    int Len2(int i1, int i2) {  //以s[i1]和s[i2]之间的间隙为中心的最长偶回文串长度
        return r[2*i1+1]-1;
    }
};
```