---
title: leetcode_58
date: 2017-07-09 22:22:07
tags: leetcode
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第58道 Length of Last Word
---

### leetcode\_58\_Length of Last Word

> Given a string *s* consists of upper/lower-case alphabets and empty space characters `' '`, return the length of last word in the string.
>
> If the last word does not exist, return 0.
>
> **Note:** A word is defined as a character sequence consists of non-space characters only.
>
> For example, 
>
> Given *s* = `"Hello World"`,
>
> return `5`.

**题意分析**：

这道题要求我们找到一个英文句子中，最后一个单词的长度。

**解题思路**

我的方法是通过找到最后一个空格所在位置，然后直接用下标进行运算，方法比较简单。

**解题代码**

```java
public class Solution58 {
    public static int lengthOfLastWord(String s) {
        if(s.replaceAll(" ","").isEmpty())
            return 0;
        s= s.trim();
        int index =0;
            index = s.lastIndexOf(' ');
        return s.length()-1-index;

    }

    public static void main(String[] args) {
        String test = "a";
        int result = lengthOfLastWord(test);
        System.out.println(result);
    }
}
```

这道题其实用java来解决提供了`string.trim()`方法可以去除首尾的空格，同时可以运用`string.lastIndexOf(char)`方法可以直接周到最后一个空格的位置，代码比较简单，但是对于c或者c++来说则没有那么容易，下面是不用这些方法的解决方式

```java
public static int lengthOfLastWort1(String s){
        if(s.length()==0)
            return 0;
        int end = s.length();
        int begin = -1;
        for(int i = s.length()-1; i>= 0 ;i--){
            if(s.charAt(i)==' '&&end-i-1<1)//判断是不是末尾的空格
                end = i;
            if (s.charAt(i)==' '&&end -i>1){//不是末尾的空格，则赋给begin
                begin = i;
                break;
            }
        }
        System.out.println(end-begin-1);
        return end-begin-1;
}
```

这种方法就是通过一个一个搜索，找出最后一个单词的起始位置,确定end指针和begin指针的位置。

| 0     | 1    | 2    | 3    | 4    | 5    |
| ----- | ---- | ---- | ---- | ---- | ---- |
| begin | b    | a    | b    | y    | end  |