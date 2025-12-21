```
struct KMP{ // 1_Index
    vector<int> fail;
    string P; 
    int n_p;
    void Run_fail(string& _p){ //处理模式串P的fail数组(所有前缀子串的最长公共真前后缀)
        P=_p;
        n_p=(int)P.size()-1;
        fail.resize(n_p+3);
        fail[1]=0;
        for(int i=2,j=0;i<=n_p;++i){
            while(j!=0 && P[j+1]!=P[i]) j=fail[j];
            if(P[j+1]==P[i]) ++j;
            fail[i]=j;
        }
    }
    vector<int> Match_pos(string& S){ //获取P在S中所有匹配位置的起点
        vector<int> res;
        int n=(int)S.size()-1;
        for(int i=1,j=0;i<=n;++i){
           while(j!=0 && P[j+1]!=S[i]) j=fail[j];
           if(P[j+1]==S[i]) ++j;
           if(j==n_p) res.push_back(i-n_p+1),j=fail[j];
        }
        return res;
    }
};
KMP T;
```