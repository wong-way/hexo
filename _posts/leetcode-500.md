---
title: leetcode-500
date: 2018-06-12 12:12:33
categories: leetcode
tags:
- leetcode
- oj
description: leetcode No.500 Keyboard Row
---
# leetcode 三道Easy题（No.500）

## leetcode No.500

### 问题描述

>Given a List of words, return the words that can be typed using letters of alphabet on only one row's of American keyboard like the image below.
>```text
>Example 1:
>Input: ["Hello", "Alaska", "Dad", "Peace"]
>Output: ["Alaska", "Dad"]
>```

### 解题代码
```java
public String[] findWords(String[] words) {
    String firstRow = "qwertyuiop";
    String secondRow = "asdfghjkl";
    String thirdRow = "zxcvbnm";
    List<String> result = new LinkedList<>();
    for (String word : words) {
        int index = 0;
        int temp = 0;

        for (int i = 0; i < word.length(); i++) {
            if (firstRow.indexOf( word.toLowerCase().charAt(i)) != -1) {
                temp = 1;
            } else if (secondRow.indexOf( word.toLowerCase().charAt(i)) != -1) {
                temp = 2;
            } else {
                temp = 3;
            }

            if (index == 0)
                index = temp;
            if (index != 0 && index != temp) {
                break;
            }

        }
        if (index == temp) {
            result.add(word);
        }
    }
    return  result.toArray(new String [0]);
}
private int getRowNum(char ch) {
    String row1 = "qwertyuiopQWERTYUIOP";
    String row2 = "asdfghjklASDFGHJKL";
    String row3 = "zxcvbnmZXCVBNM";

    if (row1.indexOf(ch) >= 0) {
        return 0;
    }

    if (row2.indexOf(ch) >= 0) {
        return 1;
    }

    if (row3.indexOf(ch) >= 0) {
        return 2;
    }

    return -1;
}

public String[] findWords1(String[] words) {


    List<String> list = new ArrayList<String>();
    for (int i = 0; i < words.length; i++) {
        char[] tempStr = words[i].toCharArray();
        if (tempStr.length <= 0) {
            continue;
        }
        boolean isResult = true;
        int old = getRowNum(tempStr[0]);
        for (int j = 1; j < tempStr.length; j++) {
            if (old != getRowNum(tempStr[j])) {
                isResult = false;
                break;
            }
        }

        if (isResult) {
            list.add(words[i]);
        }
    }

    return list.toArray(new String[list.size()]);
}
```

## leetcode No.557 Reverse Words in a String III

### 问题描述

>Given a string, you need to reverse the order of characters in each word within a sentence while still preserving whitespace and initial word order.
>```text
>Example 1:
>Input: "Let's take LeetCode contest"
>Output: "s'teL ekat edoCteeL tsetnoc"

### 解题代码

```java
public String reverseWords(String s) {

    String[] words = s.split(" ");
    String res = "";
    for (String word:
            words) {
        res+=reverseWord(word);

        res+=" ";
    }
    return res.substring(0,res.length()-1);
}
public String reverseWord(String s){
    String result = "";
    for(int i = s.length()-1;i>=0;i--){
        result += s.charAt(i);
    }

    return result;

}
public String reverseWords1(String s) {
    char[] arr = s.toCharArray();
    int i=0,j=0;
    while(j < arr.length) {
        if(arr[j] == ' ') {
            reverse1(arr, i, j-1);
            j++;
            i=j;
        } else
            j++;
    }
    reverse1(arr, i , j-1);
    s = new String(arr);
    return s;
}

void reverse1(char[] arr, int i, int j) {
    while(i<j) {
        char tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
        i++;
        j--;
    }
}
```

## leetcode No.476 Number Complement

### 问题描述

>Given a positive integer, output its complement number. The complement strategy is to flip the bits of its binary representation.

### 解题思路

* 我首先想到的是，如果两个数字满足上诉条件，那么他们相加的和转化为二进制一定为全1。针对这个特点，那么他的结果一定是2^n-1-num

* 第二个思路则是，按照规则对每一位进行处理，将每一位对位取反

### 解题代码

```java
public int findComplement(int num) {
    String str ="";
    while(num!=0){
        str=((num&1)^1)+str;
        num=num>>1;

    }
    return Integer.valueOf(str,2);
}
public int findComplement1(int num) {
   return ((Integer.highestOneBit(num) << 1) - 1)-num
}
```

