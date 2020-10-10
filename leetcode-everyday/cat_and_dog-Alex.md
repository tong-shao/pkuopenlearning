#### 猫狗收容所问题：

有家动物收容所只收留猫和狗，但有特殊的收养规则，收养人有两种收养方式，第一种为直接收养所有动物中最早进入收容所的，第二种为选择收养的动物类型（猫或狗），并收养该种动物中最早进入收容所的。

​    给定一个操作序列代表所有事件。若第一个元素为1，则代表有动物进入收容所，第二个元素为动物的编号，正数代表狗，负数代表猫；若第一个元素为2，则代表有人收养动物，第二个元素若为0，则采取第一种收养方式，若为1，则指定收养狗，若为-1则指定收养猫。请按顺序返回收养的序列。若出现不合法的操作，即没有可以符合领养要求的动物，则将这次领养操作忽略。

测试样例：

```
4 [[1,1],[1,-1],[2,0],[2,-1]]
返回：[1,-1]
```

利用队列实现，维护两个队列，一个用于猫，一个用于狗，动物进入收容所是入队，有人收养按条件出队。

```
#include<iostream>
#include<cstdio>
#include<cstring>
#include<queue>
#include<vector>

using namespace std;
struct animal {//定义结构体animal，包含代表动物的序号，与代表顺序的序号
	int number;
	int order;
	animal();
	animal(int n, int o) :number(n), order(o) {}
};
queue<animal>cat;//代表猫的队列
queue<animal>dog;//代表狗的队列

int main() {
	int n;//代表操作序列的次数
	int order = 0;
	scanf_s("%d", &n);
	for (int i = 0; i < n; i++) {
		int method, type;
		scanf_s("%d%d", &method, &type);
		if (method == 1) {
			if (type > 0) {
				dog.push(animal(type, order++));
			}
			if (type < 0) {
				cat.push(animal(type, order++));
			}
		}
		if (method == 2){
			if (type == 0 && !dog.empty() && !cat.empty()) {//第一种收养方式
				if (dog.front().order < cat.front().order) {
					printf("%d", dog.front());
					dog.pop();
				}
				else {
					printf("%d", cat.front());
					cat.pop();
				}
			}
				if (type == 0 && dog.empty() && !cat.empty()) {
					printf("%d", cat.front());
					cat.pop();
				}
				if (type == 0 && !dog.empty() && cat.empty()) {
					printf("%d", dog.front());
					dog.pop();
				}
				if (type == 1 && !dog.empty()) {//第二种收养方式
					printf("%d", dog.front());
					dog.pop();
				}
				if (type == -1 && !cat.empty()) {
					printf("%d", cat.front());
					cat.pop();
				}
		}
	}
	return 0;
}
```



#### 