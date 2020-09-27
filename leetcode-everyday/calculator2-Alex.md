# 计算器：包括括号与+-*

计算器2

```
#include<iostream>
#include<string>
#include<stack>
#include<cstdio>
#include<algorithm>
#include<vector>
using namespace std;

//从符号栈和数字栈中取出符号和数字进行运算  并将结果存入数字栈中
void Op(stack<string>& NumStack, stack<char>& OpStack) {
	int num1 = stoi(NumStack.top());

	NumStack.pop();
	int num2 = stoi(NumStack.top());

	NumStack.pop();
	char op = OpStack.top();
	OpStack.pop();
	int ans;
	if (op == '+')
		ans = num1 + num2;
	else if (op == '-')
		ans = num2 - num1;
	else if (op == '*')
		ans = num1 * num2;
	NumStack.push(to_string(ans));
}
int main(void) {
	string s;
	cin >> s;
	int n = s.size();
	stack<string> NumStack;//数字栈
	stack<char> OpStack;// 符号栈
	//从左到右依次读入数据
	for (int i = 0; i < n;) {
		string NumStr;
		if (s[i] >= '0' && s[i] <= '9') {  //多位数字
			while (i < n && s[i] >= '0' && s[i] <= '9')
				NumStr += s[i++];
			NumStack.push(NumStr);
		}
		else if (s[i] == '-' && (NumStack.empty() || OpStack.top() == '(')) { //第一个数字为负数数所以需要单独判断  将第一个数字为负数添加到数字栈中
			NumStr += s[i++];
			while (i < n && s[i] >= '0' && s[i] <= '9')
				NumStr += s[i++];
			NumStack.push(NumStr);
		}
		else if (s[i] == '(')  //左括号为'('直接push进符号栈
			OpStack.push(s[i++]);
		else if (s[i] == ')') {  //右括号'(',两种情况。符号栈顶为'('直接出栈。
		//若不是，则说明有（）内有数字可以运算。进行运算，并将i保持不变。以便下一次判断。
				if (OpStack.top() == '(') {
					OpStack.pop();
					i++;
				}
				else {
					Op(NumStack, OpStack);
				}
		}
		else if (s[i] == '+' || s[i] == '-') {  //+和-号一样的优先级，当栈为空或
		//栈顶为'(',则直接入栈。若优先级小于栈顶符号，则先运算，i不变。
				if (OpStack.empty() || OpStack.top() == '(') {
					OpStack.push(s[i++]);
				}
				else {
					Op(NumStack, OpStack);
				}
		}
		else if (s[i] == '*') { //当*优先级大于栈顶，则直接入栈，
			//若 <= 优先级则取数运算。i不变。
				if (!OpStack.empty() && OpStack.top() == '*') {
					Op(NumStack, OpStack);
				}
				else
					OpStack.push(s[i++]);
		}
	}
	//最后进行运算，最后数字栈中栈顶元素即为最后结果。
	Op(NumStack, OpStack);
	cout << NumStack.top() << endl;
	//system("pause");
	return 0;
}
```

