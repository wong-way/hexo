---
title: leetcode-638
date: 2018-06-02 11:43:22
categories: leetcode
tags:
- oj
- leetcode
- dynamic programming
description: leetcode-638题Shopping Offers
---
# leetcode-638 Shopping Offers

## 问题描述
>In LeetCode Store, there are some kinds of items to sell. Each item has a price.
However, there are some special offers, and a special offer consists of one or more different kinds of items with a sale price.
You are given the each item's price, a set of special offers, and the number we need to buy for each item. The job is to output the lowest price you have to pay for exactly certain items as given, where you could make optimal use of the special offers.
Each special offer is represented in the form of an array, the last number represents the price you need to pay for this special offer, other numbers represents how many specific items you could get if you buy this offer.
You could use any of special offers as many times as you want.

```text
Example 1:
Input: [2,5], [[3,0,5],[1,2,10]], [3,2]
Output: 14
Explanation: 
There are two kinds of items, A and B. Their prices are $2 and $5 respectively. 
In special offer 1, you can pay $5 for 3A and 0B
In special offer 2, you can pay $10 for 1A and 2B. 
You need to buy 3A and 2B, so you may pay $10 for 1A and 2B (special offer #2), and $4 for 2A.
Example 2:
Input: [2,3,4], [[1,1,0,4],[2,2,1,9]], [1,2,1]
Output: 11
Explanation: 
The price of A is $2, and $3 for B, $4 for C. 
You may pay $4 for 1A and 1B, and $9 for 2A ,2B and 1C. 
You need to buy 1A ,2B and 1C, so you may pay $4 for 1A and 1B (special offer #1), and $3 for 1B, $4 for 1C. 
You cannot add more items, though only $9 for 2A ,2B and 1C.
```

## 解题思路
这道题总得可以分成两个方向，一个就是购买组合特价商品，一个就是单独购买

* 购买组合商品必须要满足组合的条件，才能以特价购买
* 单独购买的灵活性更好，可以随机组合
我觉得采用递归方式解决这道题，如果能购买组合商品，递归计算剩余的需求需要花费多少金额。

## 解题代码
```java
public int shoppingOffers(List<Integer> price, List<List<Integer>> special, List<Integer> needs) {
    int count = Integer.MAX_VALUE;
    if (isAllZero(needs))
        return 0;
    for (List<Integer> list :special) {
        int temp=0;
        if(!isAllGreater(list,needs)){
            for (int i = 0; i < needs.size(); i++) {
                temp+=needs.get(i)*price.get(i);
            }
        }else{
            List<Integer> newNeeds = new ArrayList<>();
            newNeeds.addAll(needs);
            for (int i = 0; i < newNeeds.size(); i++) {
                newNeeds.set(i, newNeeds.get(i) - list.get(i));
            }
            temp = list.get(list.size()-1)+shoppingOffers(price,special,newNeeds);
        }
        if(temp<count)
            count = temp;

    }

    return count;
}

public boolean isAllZero(List<Integer> list) {
    for (int i = 0; i < list.size(); i++) {
        if (list.get(i) != 0)
            return false;
    }
    return true;
}

public boolean isAllGreater(List<Integer> list1, List<Integer> list2) {
    for (int i = 0; i < list2.size(); i++) {
        if(list2.get(i)<list1.get(i))
            return false;
    }
    return true;
}
```

## 问题总结

上面这种方式按生活常理来说能够解决问题，在这中间有一种假设，即购买组合特价商品比单独购买划算。但是我的思路只通过了52/53个测试用例，最后一个测试用例未通过，原因是测试用例是这样

```text
[0,0,0]
[[1,1,0,4],[2,2,1,9]]
[2,2,1]
```

它代表的含义是，ABC三个商品价格均为0，但是如果1个A,1个B组合起来是4元，这种方式似乎不符合常理，同样没有人会选择这种方式。

## 代码改进

```java
public int shoppingOffers(List<Integer> price, List<List<Integer>> special, List<Integer> needs) {
    int count = Integer.MAX_VALUE;
    if (isAllZero(needs))
        return 0;
    for (List<Integer> list : special) {
        int temp = 0;
        int alone = 0;
        int combination = 0;

        for (int i = 0; i < needs.size(); i++) {
            alone += needs.get(i) * price.get(i);
        }

        if (isAllGreater(list, needs)) {
            List<Integer> newNeeds = new ArrayList<>();
            newNeeds.addAll(needs);
            for (int i = 0; i < newNeeds.size(); i++) {
                newNeeds.set(i, newNeeds.get(i) - list.get(i));
            }
            combination = list.get(list.size() - 1) + shoppingOffers(price, special, newNeeds);
        }
        temp = Math.min(alone, combination);
        if (temp < count)
            count = temp;
        System.out.println(alone+"，"+combination+","+temp);
    }

    return count;
}

public boolean isAllZero(List<Integer> list) {
    for (int i = 0; i < list.size(); i++) {
        if (list.get(i) != 0)
            return false;
    }
    return true;
}

public boolean isAllGreater(List<Integer> list1, List<Integer> list2) {
    for (int i = 0; i < list2.size(); i++) {
        if (list2.get(i) < list1.get(i))
            return false;
    }
    return true;
}

```

## 评论区代码

```java
public int shoppingOffers(List<Integer> price, List<List<Integer>> special, List<Integer> needs) {
    return helper(price, special, needs, 0);
}

private int helper(List<Integer> price, List<List<Integer>> special, List<Integer> needs, int pos) {
    int local_min = directPurchase(price, needs);
    for (int i = pos; i < special.size(); i++) {
        List<Integer> offer = special.get(i);
        List<Integer> temp = new ArrayList<Integer>();
        for (int j= 0; j < needs.size(); j++) {
            if (needs.get(j) < offer.get(j)) { // check if the current offer is valid
                temp =  null;
                break;
            }
            temp.add(needs.get(j) - offer.get(j));
        }
        
        if (temp != null) { // use the current offer and try next
            local_min = Math.min(local_min, offer.get(offer.size() - 1) + helper(price, special, temp, i)); 
        }
    }

    return  local_min;
}

private int directPurchase(List<Integer> price, List<Integer> needs) {
    int total = 0;
    for (int i = 0; i < needs.size(); i++) {
        total += price.get(i) * needs.get(i);
    }
    
    return total;
}
```