---
title: leetcode_20
date: 2017-07-04 22:07:41
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第20道 Valid Parentheses    
---

>Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.
>
>The brackets must close in the correct order, `"()"` and `"()[]{}"` are all valid but `"(]"` and `"([)]"` are not.

### leetcode\_20\_括号匹配

括号匹配是用栈解决的一个经典问题，我对这个问题的理解如下

**左右括号匹配，在数量上必须满足偶数，这是一个必要条件，而不是充分条件。**

整体的解决思路如下：

1. 将不同的括号定义为不同的数字

   ```java
   private static final int PARENTHESIS = 1;
   private static final int BEACKET = 2 ;
   private static final int BRACE = 3;
   ```

2. 遇到左括号就把它放在栈中

3. 遇到右括号就把栈顶元素取出，并判断是否匹配（注意栈为空情况的检查）

4. 循环结束判断栈是否为空

完整代码如下：

```java
import java.util.Stack;
public class Solution20 {
    private static final int PARENTHESIS = 1;
    private static final int BEACKET = 2 ;
    private static final int BRACE = 3;
    public static boolean isValid(String s) {
        Stack<Integer> mStack = new Stack<Integer>();
        boolean isValid = true;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                mStack.push(PARENTHESIS);
            } else if (s.charAt(i) == '[') {
                mStack.push(BEACKET);
            } else if (s.charAt(i) == '{') {
                mStack.push(BRACE);
            } else if (s.charAt(i) == ')') {
                if(mStack.isEmpty())
                {
                    isValid = false;
                    break;
                }
                if (mStack.pop().equals(PARENTHESIS))
                    continue;
                else {
                    isValid = false;
                    break;
                }
            } else if (s.charAt(i) == ']') {
                if(mStack.isEmpty())
                {
                    isValid = false;
                    break;
                }
                if (mStack.pop().equals(BEACKET))
                    continue;
                else {
                    isValid = false;
                    break;
                }
            } else if (s.charAt(i) == '}') {
                if(mStack.isEmpty())
                {
                    isValid = false;
                    break;
                }
                if (mStack.pop().equals(BRACE))
                    continue;
                else {
                    isValid = false;
                    break;
                }
            }
        }
        if (!mStack.isEmpty())
            isValid = false;
        return isValid;
    }
    public static void main(String[] args) {
        String testStr = "()(){[]()}";
        if (isValid(testStr)) {
            System.out.println("valid");
        } else {
            System.out.println("invalid");
        }
    }
}

```

