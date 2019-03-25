## ARTS

### Algorithm

### 题目：[移除元素](https://leetcode-cn.com/problems/remove-element/)

给定一个数组 *nums *和一个值 *val*，你需要[原地](https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95)移除所有数值等于 *val *的元素，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在[原地](https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95)**修改输入数组**并在使用 O(1) 额外空间的条件下完成。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

**示例 1:**

给定 *nums* = **[3,2,2,3]**, *val* = **3**,

函数应该返回新的长度 **2**, 并且 *nums *中的前两个元素均为 **2**。

你不需要考虑数组中超出新长度后面的元素。

**示例 2:**

给定 *nums* = **[0,1,2,2,3,0,4,2]**, *val* = **2**,

函数应该返回新的长度 **5**, 并且 *nums *中的前五个元素为 **0**, **1**, **3**, **0**, **4**。

注意这五个元素可为任意顺序。

你不需要考虑数组中超出新长度后面的元素。


方法一：

```
function removeElement(&$nums, $val) {
        $j = 0;
        for($i=0;$i<count($nums);$i++){
            if($nums[$i]!=$val){
                $nums[$j]=$nums[$i];
                $j++;
            }
        }
        return $j;
}
```

方法二：

```
function removeElement(&$nums, $val) {        
         $nums = array_filter($nums,function ($v) use ($val){
            if($v === $val) return false; else return true;
         });
}
```

