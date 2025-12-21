`lower_bound(first,last,value)`，在指针/迭代器`first~last`闭区间范围内二分查找`>=value`的元素并返回其指针/迭代器
`upper_bound`，查找`>value`的
*注意*
`set`直接用是线性查找，要二分应用自带的`setName.lower_bound(value)`函数