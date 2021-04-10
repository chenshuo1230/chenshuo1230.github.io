---
title: leetcode每日一题
date: 2021-04-06 21:28:26
tags: 
---
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 的那 两个 整数，并返回它们的数组下标。
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。
你可以按任意顺序返回答案。

示例 1：
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
示例 2：

输入：nums = [3,2,4], target = 6
输出：[1,2]
示例 3：

输入：nums = [3,3], target = 6
输出：[0,1]
 

提示：

2 <= nums.length <= 103
-109 <= nums[i] <= 109
-109 <= target <= 109
只会存在一个有效答案
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/two-sum
突然想努力一下了，发现自己第一题都不会写，只会最简单的穷举法还超时（理所应当）
评论区大佬的运行时间0ms？
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> existMap= new HashMap<>();

        for(int i=0;i<nums.length;i++){
            if(existMap.containsKey(target-nums[i])){
                return new int[]{existMap.get(target-nums[i]),i};
            }
            existMap.put(nums[i],i);
        }
        return new int[]{-1,-1};
    }
}
使用了hashmap，未曾了解的技术，我还有很大的提升空间啊（啥也不会）看挺很久才看懂
class Solution { 
public int[] twoSum(int[] nums, int target){ 
//创建集合 HashMap<Integer,Integer> map = new HashMap<>(); 
for(int i = 0; i < nums.length; i++){ 
//如果包含上次存储的target - nums[i] 。
if(map.containsKey(nums[i])){
//map.get(nums[i]) 取出来的是上次存储的能和这次配对的下标，
//比如{2 , 7 , 11, 15}，target是9 ； 这次进来的就是 nums[i] = 7 ，
// map.get(nums[i])对应的是2的下标 // "i" 对应的是7的下标。 
//这是2个能相加等于9的下标 
return new int[]{map.get(nums[i]), i};
} 
//target - nums[i] 代表的是下一个元素的值，“i” 代表能和target - nums[i]凑成一对的值的下标。
//事先存好，上面在根据，这个target - nums[i]这个值，去找元素。比如数组是{2 , 7 , 11, 15}，target是9 ；存的就是{7 , 0} -->key ,value ；7对应下一次if里面的判断值，0代表的值能和7相加等于9 map.put(target - nums[i], i); } return null; } }
资料：boolean containsKey(Object key)如果此映射包含指定键的映射关系，则返回 true。当且仅当此映射包含针对满足 (key==null ? k==null : key.equals(k)) 的键 k 的映射关系时，返回 true。（最多只能有一个这样的映射关系）。 

解题思路 #
这道题最优的做法时间复杂度是 O(n)。

顺序扫描数组，对每一个元素，在 map 中找能组合给定值的另一半数字，如果找到了，直接返回 2 个数字的下标即可。
如果找不到，就把这个数字存入 map 中，等待扫到“另一半”数字的时候，再取出来返回结果。
