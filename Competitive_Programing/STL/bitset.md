```
bitset<8> bst; //创建一个长度为 8 的 bitset，所有位初始值为 0
bitset<8> b("10101000"); // 初始化为 10101000,下标从右往左0开始```
```

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