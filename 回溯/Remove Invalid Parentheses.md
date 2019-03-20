# Remove Invalid Parentheses
删除最小数量的无效括号，使得输入的字符串有效，返回所有可能的结果。

**说明:** 输入可能包含了除 ( 和 ) 以外的字符。

**示例 1:**

> **输入:** "()())()"<br>
**输出:** ["()()()", "(())()"]

**示例 2:**

> **输入:** "(a)())()"<br>
**输出:** ["(a)()()", "(a())()"]

**示例 3:****

> **输入**: ")("<br>
**输出:** [""]

# 分析

首先统计非法的左括号与右括号数量，给定的字符串可能非法的左括号多，也可能右括号多，也可能一样多，比如‘)(’,然后就是在递归中依次减去多余的括号，在非法的左括号与右括号数量都为零之后，还需要验证字符串的合法性。要注意如果有两个连续的一样的括号，跳过就行了。比如‘((’，去掉哪一个结果都是一样的。

java代码如下：
```java
public List<String> removeInvalidParentheses(String s) {
        List<String> res = new ArrayList<>();
        int left = 0, right = 0;
        for(int i=0;i<s.length();i++){
            if(s.charAt(i) == '(') left++;
            else if(s.charAt(i) == ')' && left == 0) right++;
            else if(s.charAt(i) == ')') left--;
        }
        delete(res, s, 0, left, right);
        return res;
    }
    
    public void delete(List<String> res, String s, int index, int left, int right){
        if(left == 0 && right == 0) {
            if(isLegal(s))
               res.add(s);
            return; 
        }
        
        for(int i=index;i<s.length();i++){
            if(i != index && s.charAt(i) == s.charAt(i-1)) continue;
            if(left > 0 && s.charAt(i) == '(')
                delete(res, s.substring(0, i) + s.substring(i+1), i, left-1, right);
            if(right > 0 && s.charAt(i) == ')')
                delete(res, s.substring(0, i) + s.substring(i+1), i, left, right-1);
        }
    }
    
    boolean isLegal(String s){
        int count = 0;
        for(int i=0;i<s.length();i++){
            if(s.charAt(i) == '(')
                count++;
            else if(s.charAt(i) == ')' && --count < 0)
                return false;
        }
        return count == 0;
    }
```
