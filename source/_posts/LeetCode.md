---
title: LeetCode
date: 2018-05-31 09:49:47
tags:
---

## 两数之和

给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。

你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。

示例:


    给定 nums = [2, 7, 11, 15], target = 9
    
    因为 nums[0] + nums[1] = 2 + 7 = 9
    所以返回 [0, 1]
    
分析:

    思路一：暴力解法，两次for循环，遍历所有可能，这也是容易想到的方法，时间复杂度O(n^2),空间复杂度O(1); 
    思路二：利用哈希表，每次存储target减去当前数的差值(key)，当前值的下标(value)，当再碰到这个值时，即找到了符合要求的值。时间复杂度O(n),空间复杂度O(n);
        
代码:

1:
    
    //思路一暴力解法
    public int[] twoSum(int[] nums, int target) {
            // write your code here
            int[] a = new int[2];
            for (int i = 0; i < nums.length - 1; i++){
    
                // 注意j等于i + 1;若j = 1则循环顺序不对
                for (int j = i + 1; j < nums.length; j++ ){
                    if (nums[i] + nums[j] == target){
    
                        a[0] = i;
                        a[1] = j;
                        break;
                    }
                }
            }
            return a;
        }
         
2:         
        
    //思路二利用哈希表
    public int[] twoSum(int[] nums, int target) {
    
            int[] a = new int[2];
            HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
    
            for (int i = 0; i < nums.length; i++){
    
                if (map.containsKey(nums[i])){
    
                    a[0] = map.get(nums[i]);
                    a[1] = i;
                    return a;
                }
                map.put(target - nums[i], i);
            }
    
            return a;
    }
    

    