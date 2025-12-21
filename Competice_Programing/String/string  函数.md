# substr
string的substr()函数，`str.substr(pos,len)`在字符串str中，从str\[pos]开始，提取长度为len的子串，不写len则默认提取到 str末尾
# replace
string的replace()函数，用于将字符串某一部分替换为指定内容
str.replace(pos,len,rstr) 开始替换的str下标,要替换的长度(原 str从pos开始删除的长度),用于替换的rstr字符串
.replace会修改原字符串，并返回修改后的内容
# find
s1.find(s,pos)在字符串变量S1中从下标pos开始找是否有子串s,省略pos则默认pos为0，rfind默认为string::npos
```
S1.find(s); //正着找
S1.rfind(s);//倒着找
```
找到，则返回s第一次出现的位置(字串首字母所在下标)，没找到，则返回`string::npos`;
*eg*
```
int main(){
    string s1="abc";
    cout << s1.find("bc"); //输出1
	return 0;
} 
