`mp.count(key)`查找一个名为mp的map中是否存在键key，存在则返回1，否则返回0
*注意*`if(mp["abc"] > 0)` 与 `if(mp.count("abc"))` 在大多数情况下不是等效的，因为执行`if(mp["abc"] > 0)`时，如果没有"abc"这个键，会自动创建一个"abc",对应值为0.而.count只是检查是否存在键"abc"
***
