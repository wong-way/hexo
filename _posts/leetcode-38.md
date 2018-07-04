---
title: leetcode_38
date: 2017-07-08 00:16:53
categories: leetcode
tags:
- leetcode
- oj
description: leetcode No.38 Count and Say
---

### leetcode\_38\_Count and Say

> The count-and-say sequence is the sequence of integers with the first five terms as following:
>
> `1` is read off as `"one 1"` or `11`.
>
> `11` is read off as `"two 1s"` or `21`.
>
> `21` is read off as `"one 2`, then `one 1"` or `1211`.
>
> Given an integer *n*, generate the *n*th term of the count-and-say sequence.
>
> Note: Each term of the sequence of integers will be represented as a string.

这道题是facebook的面试题，题意比较难理解。题意大概是说给一个数字n，求出数列第n个字符串应该是什么。而第n个字符串是将第n-1个字符串读一遍，例如，第三个字符串则是将第二个字符串给读一遍，读作：“一个2，一个1”，用字符串表示出来则是“1211”。

由于这道题，第n项总是与第n-1项有关，于是我采取**递归**的方式解决这个问题，我的解题思路如下：

* 如果n=1，则直接返回`"1"`
* 如果n>=2，则采取递归调用的方式，调用本身
* 对每一个已经初始化的字符串进行遍历，从`i=1`开始，如果它与前一位相同，则将计数器加一，如果不相同，则将`count`与`initString`加到`resultString`
* 注意，由于每次相加都是对前一位进行操作，因此不管字符串最后一位与之前的位是否相同，都没有进行相应的操作，因此在循环结束之后，再对字符串最后一位进行操作。

下面是我的代码

```java
public static String countAndSay(int n) {
        if(n == 1)
            return "1";
        String initString = countAndSay(n-1);
        String resultString = "";
        int count=1;
        for(int i = 1 ; i< initString.length();i++){
            if(initString.charAt(i)==initString.charAt(i-1)){
                count++;
                continue;
            }else{
                resultString= resultString+count+initString.charAt(i-1);
                count=1;
            }
        }
        //System.out.println("n :"+n+"count :"+count);
        resultString = resultString+count+initString.charAt(initString.length()-1);
       //System.out.println("result :"+resultString);
        return resultString;
    }
```

