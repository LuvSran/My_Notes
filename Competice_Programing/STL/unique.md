unique函数接受指针或迭代器a,b；对 a~b（前闭后开）范围内相邻的相同元素进行去重，把重复的元素放到末尾，并返回去重后末尾元素迭代器的下一个迭代器(指针)，一般配合sort和erase用
```cpp
int main() {
    vector<int> vec = {1, 2, 2, 3, 4, 4, 4, 5};
    sort(vec.begin(), vec.end()); // 使相同的元素相邻
    vec.erase(unique(vec.begin(), vec.end()), vec.end()); //去重
     
     for (int num : vec) {
        cout << num << " ";
    }  //输出1 2 3 4 5
    return 0;
}
```