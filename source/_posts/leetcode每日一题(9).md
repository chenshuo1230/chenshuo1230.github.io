---
title: leetcode每日一题
date: 2021-04-06 21:28:26
tags: 
---
编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

 

示例 1：

输入：strs = ["flower","flow","flight"]
输出："fl"
示例 2：

输入：strs = ["dog","racecar","car"]
输出：""
解释：输入不存在公共前缀。
 

提示：

0 <= strs.length <= 200
0 <= strs[i].length <= 200
strs[i] 仅由小写英文字母组成

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-common-prefix
python的玄学解法:
def longestCommonPrefix(self, strs):
        if not strs: return ""
        s1 = min(strs)
        s2 = max(strs)
        for i,x in enumerate(s1):
            if x != s2[i]:
                return s2[:i]
        return s1
Java的快速解法:
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if(strs.length==0)return "";
        //公共前缀比所有字符串都短，随便选一个
        String s=strs[0];
        for (String string : strs) {
            while(!string.startsWith(s)){
                if(s.length()==0)return "";
                //公共前缀不匹配就让它变短
                s=s.substring(0,s.length()-1);
            }
        }
        return s;
    }
}
