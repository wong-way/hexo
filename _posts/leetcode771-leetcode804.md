---
title: leetcode771&leetcode804
date: 2018-06-02 10:56:17
categories: leetcode
tags:
- oj
- leetcode 
description: leetcode-771 Jewels and Stones , leetcode-804 Unique Morse Code Words
---
# leetcode

## leetcode-771 Jewels and Stones

### 问题描述

>You're given strings J representing the types of stones that are jewels, and S representing the stones you have.  Each character in S is a type of stone you have.  You want to know how many of the stones you have are also jewels.
The letters in J are guaranteed distinct, and all characters in J and S are letters. Letters are case sensitive, so "a" is considered a different type of stone from "A".
```text
Example 1:

Input: J = "aA", S = "aAAbbbb"
Output: 3
Example 2:

Input: J = "z", S = "ZZ"
Output: 0
Note:
```

> * S and J will consist of letters and have length at most 50.
> * The characters in J are distinct.

## 解题代码

```java
public int numJewelsInStones(String J, String S) {
    int count=0;
    for(int i = 0;i<S.length();i++){
        if(J.indexOf(S.charAt(i))>=0)
        {
            count++;
            // System.out.println(i);
        } 
    }
    return count;
}
```

## leetcode-804
>International Morse Code defines a standard encoding where each letter is mapped to a series of dots and dashes, as follows: "a" maps to ".-", "b" maps to "-...", "c" maps to "-.-.", and so on.
For convenience, the full table for the 26 letters of the English alphabet is given below:

```text
[".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",".--","-..-","-.--","--.."]
```

>Now, given a list of words, each word can be written as a concatenation of the Morse code of each letter. For example, "cab" can be written as "-.-.-....-", (which is the concatenation "-.-." + "-..." + ".-"). We'll call such a concatenation, the transformation of a word.
Return the number of different transformations among all words we have.

```text
Example:
Input: words = ["gin", "zen", "gig", "msg"]
Output: 2
Explanation: 
The transformation of each word is:
"gin" -> "--...-."
"zen" -> "--...-."
"gig" -> "--...--."
"msg" -> "--...--."

There are 2 different transformations, "--...-." and "--...--.".
```

>Note:
The length of words will be at most 100.
Each words[i] will have length in range [1, 12].
words[i] will only consist of lowercase letters.

### 解题代码

```java
 public int uniqueMorseRepresentations(String[] words) {
    String[] dict = {".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..", ".---", "-.-", ".-..", "--", "-.", "---", ".--.", "--.-", ".-.", "...", "-", "..-", "...-", ".--", "-..-", "-.--", "--.."};
    Set set = new HashSet();
    for (String str : words) {
        String morseCode = "";
        char[] arr = str.toCharArray();
        for (int i = 0; i < arr.length; i++) {
            morseCode += dict[arr[i] - 'a'];
        }
        set.add(morseCode);
    }
    return set.size();
}
```