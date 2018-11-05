#ARTS

### Algorithm

[1. 两数之和](https://leetcode-cn.com/problems/two-sum/)
### 题目：
给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。
你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。

示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]


### 实现：
```
<?php
    public function twoSum($array, $sum){
    ​    //判断数组是否合法
        if(empty($array)||count($array)<2){
            return null;
        }
        //进行循环查找操作​
        $arr = array();
        foreach ($array as $k=>$v){
            $temp = $sum-$v; 
            if(in_array($temp,$array)&&array_search($temp,$array)!=$k){
                $arr[] = $k;
                $arr[] = array_search($temp,$array);
                break;
            }
        }
        return $arr;
    }
​?>

```
### 分析：
此题目主要考察数组下标和值本身的关系特性。

