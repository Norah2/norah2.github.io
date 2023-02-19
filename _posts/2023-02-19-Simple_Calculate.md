---
layout: post
title: 简单计算器C代码的实现思路以及代码
date: 2023-02-19
description: 简单计算器C代码的实现思路以及代码，同时介绍了中缀表达式转后缀表达式的算法，以及中缀表达式计算的算法。  
categories: C/C++
tags: C/C++ 算法
author: NMt
mathjax: true
---

* content
{:toc}

简单计算器C代码的实现思路以及代码，同时介绍了中缀表达式转后缀表达式的算法，以及中缀表达式计算的算法。  

<div style='display: none'>
@@@@
</div>





{% raw %}

![][pt_03]  

这个问题的关键在于，中缀表达式的计算顺序与运算符有关，并不是单纯的从左至右的计算。而后缀表达式的计算优先级正好是从左至右，可以将中缀转后缀的逻辑运用在中缀表达式的计算中。  

# 中缀转后缀表达式算法（机算）：  

1. 初始化一个栈：由于暂时还不能确定运算顺序的运算符，所以需要从左到右处理各个元素，直到末尾；  
2. 遇到操作数：直接加入后缀表达式；  
3. 遇到界限符：遇到"("直接入栈，遇到")"，则依次弹出栈内运算符并加入后缀表达式，直到弹出 "("为止，注意"("不加入后缀表达式；  
4. 遇到运算符；依次弹出栈中优先级高于或等于当前运算符的所有运算符，并加入后缀表达式，若碰到"("或栈空则停止。之后再把当前运算符入栈。  

![][pt_01]  

# 机器实现中缀表达式计算的算法如下：  

1. 初始化两个栈，操作数栈和运算符栈；  
2. 若扫描到操作数，则压入操作数栈；  
3. 若扫描到运算符或界限符，则按照“中缀转后缀”相同的逻辑压入运算符栈（每当弹出一个运算符时，就需要再弹出两个操作栈的栈顶元素并执行相应运算，运算结果再压回操作数栈）。  

![][pt_02]  

理解了上面的两个算法之后，就可以应用到本题中来。  

# 代码解析  
  
根据题目可知，输入的表达式中会出现的字符有这样三种：数字、空格、运算符。
当遇到空格的时候，说明前面要么是数字，要么是运算符；  
当遇到一个完整数据的时候，则压入栈中；  
当遇到一个运算符的时候，则需要比较当前运算符和运算符栈顶元素的优先级。  

分析完毕之后就可以开始写代码了，下面分段介绍代码：  

字符数组buf: 用于保存输入的表达式，C风格的字符串;  
字符串num: 用于保存数字，数字可能不止一位数，因此用字符串保存;  
字符串expr: 用于将字符串数组转换成C++风格的字符串;  
操作数栈NumStack: 保存操作数;  
运算符栈SignStack: 保存运算符;  
map类型priority: 给运算符设置优先级，后续在算法中会根据优先级来判断如何操作，在运算符中加入一个结束符，结束符的优先级设为最低，这样可以保证在遇到结束符之后将前面所有的式子都计算完成。  

```c++
char buf[300];
int i;
double add_num, sum = 0;
string num = "", expr;
stack<double> NumStack;
stack<char> SignStack;
map<char, int> priority = {{'+', 1}, {'-', 1}, {'*', 2}, {'/', 2}, {'$', 0}};
```

使用函数`fgets()`函数将输入的表达式全部存入char数组中，但是`fgets()`函数会将行尾的`\n`也存入数组中；  
将C风格的char数组转成C++风格的string类型，保存到expr变量中，然后将字符串尾的换行符删除；  
若是expr只剩下`"0"`，说明没有其他的表达式输入了，退出程序；  
最后在expr变量的末尾加上结束符`"$"`。  

```c++
while(fgets(buf, 300, stdin) != NULL){
	expr = buf;
	expr.pop_back();
	if(expr == "0"){
		break;
	}
	expr.push_back('$');  // add a terminator
```

对expr中的字符进行遍历，主要是处理可能为数字、空格、运算符等的符号，当：  
字符为数字：将当前数字压入num尾部，因为数字可能不止一位，所以只有遇到空格之后才能判断已经得到了一个完整的数；  
字符为空格：遇到空格的时候，刚刚得到的可能是一个操作数，也有可能是一个运算符，可以根据num是否为`""`来判断是否为操作数，若是为操作数，则压入NumStack，并且将num转换为double类型的数字，使用`stod()`函数可以直接实现这一功能；  
字符为其他类型时：可以为加减乘除和结束符，若为结束符说明前面得到了一个操作数，所以先将这个操作数压入NumStack栈中，然后处理符号。只有当运算符栈为空或者当前运算符的优先级高于运算符栈顶运算符的优先级时，将运算符压入运算符栈；若运算符栈不空并且当前运算符的优先级小于或等于运算符栈顶运算符的优先级时，则弹出两个操作栈的栈顶元素并执行相应运算，运算结果再压回操作数栈。  

```c++
// 如果为数字
if(expr[i] >= '0' && expr[i] <= '9'){
	num.push_back(expr[i]);
}else if(expr[i] == ' '){
	if(num != ""){
		NumStack.push(stod(num));
		num = "";
	}else{
		continue;
	}
}else{
	// add_num = NumStack.top();
	if(expr[i] == '$'){
		NumStack.push(stod(num));
		num = "";
	}
	while(!SignStack.empty()){
		if(priority[expr[i]] <= priority[SignStack.top()]){
			add_num = NumStack.top();
			NumStack.pop();
			switch (SignStack.top())
			{
			case '+':
				sum = NumStack.top() + add_num;
				break;
			case '-':
				sum = NumStack.top() - add_num;
				break;
			case '*':
				sum = NumStack.top() * add_num;
				break;
			case '/':
				sum = NumStack.top() / add_num;
				break;
			default:
				;
				break;
			}
			NumStack.pop();
			NumStack.push(sum);
			SignStack.pop();
		}else{
			break;
		}
	}
	SignStack.push(expr[i]);
}
```

循环一轮之后，操作数栈的栈顶元素就是表达式的计算值，进行输出即可，最后需要将NumStack的栈顶元素（表达式计算结果）和SignStack的栈顶元素（结束符）分别弹出，目的是清空操作数栈和运算符栈，为了方便下一行表达式的遍历计算。  

```c++
printf("%.02f\n", NumStack.top());
NumStack.pop();
SignStack.pop();
```

完整代码：  

```c++
#include <cstdio>
#include <string>
#include <stack>
#include <map>
using namespace std;
int main(){
    char buf[300];
    int i;
    double add_num, sum = 0;
    string num = "", expr;
    stack<double> NumStack;
    stack<char> SignStack;
    map<char, int> priority = {{'+', 1}, {'-', 1}, {'*', 2}, {'/', 2}, {'$', 0}};
    while(fgets(buf, 300, stdin) != NULL){
        expr = buf;
        expr.pop_back();
        if(expr == "0"){
            break;
        }
        expr.push_back('$');  // add a terminator
        // 遍历整行   处理数字，处理空格，处理符号
        for(i=0; i<expr.size(); i++){
            // 如果为数字
            if(expr[i] >= '0' && expr[i] <= '9'){
                num.push_back(expr[i]);
            }else if(expr[i] == ' '){
                if(num != ""){
                    NumStack.push(stod(num));
                    num = "";
                }else{
                    continue;
                }
            }else{
                // add_num = NumStack.top();
                if(expr[i] == '$'){
                    NumStack.push(stod(num));
                    num = "";
                }
                while(!SignStack.empty()){
                    if(priority[expr[i]] <= priority[SignStack.top()]){
                        add_num = NumStack.top();
                        NumStack.pop();
                        switch (SignStack.top())
                        {
                        case '+':
                            sum = NumStack.top() + add_num;
                            break;
                        case '-':
                            sum = NumStack.top() - add_num;
                            break;
                        case '*':
                            sum = NumStack.top() * add_num;
                            break;
                        case '/':
                            sum = NumStack.top() / add_num;
                            break;
                        default:
                            ;
                            break;
                        }
                        NumStack.pop();
                        NumStack.push(sum);
                        SignStack.pop();
                    }else{
                        break;
                    }
                }
                SignStack.push(expr[i]);
            }
        }
        printf("%.02f\n", NumStack.top());
        NumStack.pop();
        SignStack.pop();
    }
    return 0;
}
```

**注意：在输入的时候要一次性读入一行，我在第一次写的时候，按字符读入，最后在提交结果的时候显示运行超时。但是用fgets()函数读入一行存入char数组，再转存到string类型的变量中后遍历会节省很多时间。**  

第一次运行超时的代码记录（效率低，算法机制和上面一样）：  

```c++
#include <cstdio>
#include <string>
#include <stack>
#include <map>
using namespace std;
 
double Calculate(char symbol, double x, double y);
 
int main(){
    double sum=0, add_num, tmp;
    char symbol;
    stack<double> num;
    stack<char> signal;
    map<char, int> priority = {{'+', 1}, {'-', 1}, {'*', 2}, {'/', 2}, {'\n', 0}};
    do{
        if(!num.empty()){
            num.pop();
        }
        if(!signal.empty()){
            signal.pop();
        }
        scanf("%lf", &add_num);
        fgets(&symbol, 2, stdin);
        if(symbol == '\n'){
            continue;
        }else{
            fgets(&symbol, 2, stdin);
            signal.push(symbol);
            num.push(add_num);
            /////
            do{
                scanf("%lf", &add_num);
                fgets(&symbol, 2, stdin);
                if(symbol != '\n'){
                    fgets(&symbol, 2, stdin);
                }
                num.push(add_num);
                // 循环
                do{
                    if(priority[symbol] <= priority[signal.top()]){
                        tmp = num.top();
                        num.pop();
                        sum = Calculate(signal.top(), num.top(), tmp);
                        num.pop();
                        signal.pop();
                        num.push(sum);
                    }else{
                        break;
                    }
                }while ((!signal.empty()));
                signal.push(symbol);
                 
                 
            }while(symbol != '\n');
            /////
            printf("%.02f\n", num.top());
        }
    }while(add_num != 0 || symbol != '\n');
    return 0;
}
 
double Calculate(char symbol, double x, double y){
    switch (symbol)
    {
    case '+':
        return x + y;
        break;
    case '-':
        return x - y;
        break;
    case '*':
        return x * y;
        break;
    default:
        return x / y;
        break;
    }
}
```


转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2023/02/19/Simple_Calculate/) 

<!--本文用到的链接-->

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/76_Simple_Calculate/01.png  
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/76_Simple_Calculate/02.png  
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/76_Simple_Calculate/03.png  

{% endraw %}
