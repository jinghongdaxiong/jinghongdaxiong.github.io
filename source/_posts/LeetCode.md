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
    
## 两数相加

给定两个非空链表来表示两个非负整数。位数按照逆序方式存储，它们的每个节点只存储单个数字。将两数相加返回一个新的链表。

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

示例：

    输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
    输出：7 -> 0 -> 8
    原因：342 + 465 = 807
    
### 结题思路

方法: 初等数学

我们是有变量来跟踪进位,并从包含最低有效位的表头开始模拟逐位相加的过程.

![示例图片](https://leetcode-cn.com/problems/add-two-numbers/Figures/2/2_add_two_numbers.svg)    

如图所示,对两数相加方法的可视化:342 + 465 = 807,每个节点都包含一个数字,并且数字按位逆序存储.

算法

就像你在纸上计算两个数字的和那样,我们首先从最低有效位也就是列表l1和l2的表头开始相加.由于每位数字都应当处于
0...9的范围内,我们计算两个数字的和时可能会出现"溢出".例如:5+7 = 12.在这种情况下,我们会将当前位的数值设
置为2,并将进位carry = 1带入下一次迭代.进位carry必定是0或者1,这是因为两个数字相加(考虑到进位)可能出现的
的最大和为9+ 9+1=19   
    
伪代码如下:
 * 将x设为节点p的值.如果p已经到达l1的末尾,则将其设置为0.
 * 将y设为节点q的值,如果q已经到达l2的末尾,则将其设置为0.
 * 设定sum = x + y + carry.
 * 更新进位的值,carry = sum/10.
 * 创建一个数值为(sum mod 10) 的新节点,并将其设置为当前节点的下一个节点,然后将当前节点
    前进到下一个节点.
 * 同时,将p和q前进到下一个节点.
 * 检查carry = 1是否成立,如果成立,则向返回列表追加一个含有数字1的新节点.
 * 返回哑节点的下一个节点.
 
请注意我们使用哑节点来简化代码.如果没有哑节点,则必须编写额外的条件语句来初始化表头的值.
         
请特别注意以下的情况:

| 测试用例 | 说明 |
| :-- | :-- | 
| l1 = [0,1] l2 = [0,1,2] | 当一个列表比另一个列表长时 | 
| l1 = [] l2 = [0,1] | 当一个列表为空时,即出现空列表 | 
| l1 = [9,9] l2 = [1] | 求和运算最后可能出现额外的进位,这一点很容易被遗忘  | 
         
复杂度分析

 * 时间复杂度: O(max(m,n)),假设m和n分别表示l1和l2的长度,上面的算法最多重复max(m,n)次.
  
 * 空间复杂度: O(max(m,n)),新列表的长度最多为max(m,n) + 1.
 
拓展

如果链表中的数字不是按逆序存储的呢?例如:
(3->4->2) + (4->6->5) = 8->0->7 
   
### 代码(java)    

    /**
     * Definition for singly-linked list.
     * public class ListNode {
     *      int val;
     *      LsitNode next;
     *      ListNode (int x) { val = x;}
     * }     
     */
     class Solution {
        public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
            ListNode dummyHead = new ListNode(0);
            ListNode p = l1, q = l2, curr = dummyHead;
            int carry =0; //进位
            while (p != null || q != null) {
                int x = (p != null) ? p.val : 0;
                int y = (q != null) ? q.val : 0;
                int sum = x + y + carry;
                carry = sum/10;
                curr.next = new listNode(sum%10);
                curr = curr.next;
                if(p != null) p=p.next;
                if(q != null) q=q.next;
            }
            if(carry>0) {
                curr.next = new ListNode(carry);
            }
            return dummyHead.next;
        }
         
     }
    
    
    