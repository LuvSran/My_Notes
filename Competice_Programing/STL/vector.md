 删除元素，erase()，o(n)
`erase()`函数，删除指定迭代器的元素或指定迭代器范围左闭右开的元素
```
a.erase(a.begin()+pos); //删除下标为pos的元素
a.erase(a.begin()+st,a.begin()+ed); //删除下标为[st,ed)范围的元素
```
查询末尾元素，.back()。删除末尾元素.pop_back()