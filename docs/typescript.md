#### interface 和 type 的区别
相同点: 都可以描述一个对象或者函数；都允许拓展。<br/>
不同点: `type` 可以声明基本类型别名，联合类型，元组等类型；`type` 语句中还可以使用 `typeof` 获取实例的类型进行赋值；`interface` 能够声明合并。
一般来说，如果不清楚什么时候用 `interface/type`，能用 `interface` 实现，就用 `interface` , 如果不能就用 `type`。