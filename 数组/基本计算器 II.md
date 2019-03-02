# 基本计算器 II
实现一个基本的计算器来计算一个简单的字符串表达式的值。

字符串表达式仅包含非负整数，+， - ，*，/ 四种运算符和空格  。 整数除法仅保留整数部分。

**示例 1:**

> 输入: "3+2*2"<br>
输出: 7

**示例 2:**

> 输入: " 3/2 "<br>
输出: 1

**示例 3:**

> 输入: " 3+5 / 2 "<br>
输出: 5

**说明：**

你可以假设所给定的表达式都是有效的。
请不要使用内置的库函数 eval。
# 分析
利用栈储存除了'+'之外的运算后的数，最后将栈中所有的数加起来即可。遇见'-'储存负数，遇见'*'栈中弹出一个数，两数相乘再储存进去，遇见'/'弹出一个数，两数相除再储存进去。
注意到字符串结尾对最后一个数字进行运算的情况。

java代码如下：
```java
public int calculate(String s) {
        char sign = '+';
        Stack<Integer> stack = new Stack<>();
        int num = 0;
        for(int i=0;i<s.length();i++){
            if(Character.isDigit(s.charAt(i))){
                num = num * 10 + s.charAt(i) - '0';
            }
            if(!Character.isDigit(s.charAt(i)) && s.charAt(i)!=' ' || i==s.length()-1){
                switch(sign){
                    case '+':
                        stack.push(num);
                        break;
                    case '-':
                        stack.push(-num);
                        break;
                    case '*':
                        stack.push(stack.pop() * num);
                        break;
                    case '/':
                        stack.push(stack.pop() / num);
                        break;
                }
                num = 0;
                sign = s.charAt(i);
            }
        }
        int res = 0;
        for(int i:stack){
            res += i;
        }
        return res;
    }
```
