# Suffix array

后缀数组是字串串处理领域很复杂的一个问题，不是一个可以简单掌握的编程技巧。

## 个人学习的目标

1) 学习核心概念
2) 理解后缀数组的创建算法 (sa[], rank[])
3) 理解后缀数组的应用原理 (height[])
4) 了解用后缀数组解决的热门编程问题 (求N个串的最长公共子串)

## 核心概念

1) 子串。
在字符串s中，取任意i<=j，那么在s中截取从i到j的这一段就叫做s的一个子串。
2) 后缀: suff(i)。
后缀就是从字符串的某个位置i到字符串末尾的子串，我们定义以s的第i个字符为第一个元素的后缀为suff(i)。
3) sa[]。
4) rank[]。
5) height[]。
定义height[i]=suffix(sa[i-1])和suffix(sa[i])的最长公共前缀，也就是排名相邻的两个后缀的最长公共前缀。
6) 后缀a和后缀b的最长公共前缀。
不妨设Rank[a]<Rank[b], 则有：
LCP(a，b)=min（height[ Rank[a] ]、height[ Rank[a]+1]、height[ Rank[a]+2]、……、height[ Rank[b] ]）
这也就转化成了求height数组在区间[Rank[a]，Rank[b] ]上的最小值，这就是经典的RMQ问题，可用树状数组或线段树解决

简单来说，SA数组表示的是“排第几的是谁”，RANK数组表示的是“你的排名是多少”。

## Computing the longest common substring of two strings using suffix arrays

···
Now, the greatest value is LCP[2]=3, but it is for SA[1] and SA[2], both of which start in the string A.
So, we ignore that. On the other hand, LCP[4]=2 is for SA[3] (corresponds to the suffix bc of B) and SA[4] (corresponding to suffix bcabc#bc of A).
So, this is the longest common substring between the two strings.
For getting the actual substring, you take a length 2 (value of the greatest feasible LCP) substring starting from either SA[3] or SA[4], which is bc.
···

## 罗穗骞 WC2009论文《后缀数组——处理字符串的有力工具》

## 最长公共子串问题的后缀数组解法

## SPOJ 694

Given a string, we need to find the total number of its distinct substrings.

## 个人学习

1) 一个长度为n的字符串，有n个后缀子字符串。
2) 这n个子字符串形成一个长度为n的数组。
第一个后缀字符串包含原字符串的第一个字母。
每个后缀子字符串在这个数组中的下标位置可以视作这个后缀字符串的唯一id，我们可以用这个下标位置标识它。
一个id=i的后缀字符串，就是从原字串的第i个字符开始的后缀字符串，其字符串长度为n-i。
3) 将这个字符串数组按字典序排序，这个排序好的数组，数组中的每个字符串在原数组中的下标位置，形成了一个新的正数数组。
我们称这个整数数组为后缀数组。记作sa[]。sa[0]就是排在最前面的后缀。
4) 通过排序操作求得sa[]，进一步，根据sa[]，可以转换求得rank[]。
rank[i]为以第i个字符开始的后缀在sa[]中的位置，即排序后的位置排名。
5) 根据sa[]和rank[]，可以设计算法线性时间求得height[]。
height数组是应用后缀数组解题是的核心，基本上使用后缀数组解决的题目都是依赖height数组完成的。

## SuffixArray.java

在SuffixArray.java中定义了Suffix类，同时包含了文本和下标位置。

Arrays.sort(suffixes); 所以这个实现是一种暴力解法。核心目标是描述直观展示后缀数组，帮助教学。
这个例子展示了sa[],height[]，目的是为了展示概念，计算方式用的都是最直观的暴力解法。
