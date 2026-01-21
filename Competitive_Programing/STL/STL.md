Set
```
struct Z{
    int x,y;
    bool operator<(const Z& t)const{
        return x<t.x; //this排在t之前，即x小的接近begin()
    }
};
```
mt19937
```
std::mt19937_64 rng(std::chrono::steady_clock::now().time_since_epoch().count());
int rand(int l, int r){ //返回l,r范围内的随机数
    std::uniform_int_distribution<int> distribution(l, r);
    return distribution(rng);
}
void xipai(vector<int>& vc){ //随机打乱元素顺序
     std::shuffle(vc.begin(),vc.end(),rng);
}
```
bitset
```
 bst.set(); //将所有位设为1
 bst.set(2); //将bst[2]设为1
 bst.set(2,0); //将bst[2]设为0

 bst.reset(); //将所有位设为0
 bst.reset(2);  //将bst[2]设为0

 bst.flip(); //反转所有位
 bst.flip(2); //反转bst[2]

 bst.test(2); //bst[2]为1返回true

 bst.count(); //返回1的数量，o(n)
 bst.size(); //返回大小(位数量)
 bst.any();  //至少有一位是1返回true

 bst.to_ullong(); //return转换成的unsigned long long
```

vector
删除元素，erase()，o(n)
`erase()`函数，删除指定迭代器的元素或指定迭代器范围左闭右开的元素
```
a.erase(a.begin()+pos); //删除下标为pos的元素
a.erase(a.begin()+st,a.begin()+ed); //删除下标为[st,ed)范围的元素
```
priority_queue
cmp比较函数
```cpp
struct cmp{
	bool operator()(const Type& a,const Type& b) const{
		return a>b; //比较逻辑，true时a在b下
	 }
};
```