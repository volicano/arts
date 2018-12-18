## ARTS

### Algorithm

### 题目：
合并两个排序的数组。

Given two sorted arrays, the task is to merge them in a sorted manner.
**Examples:**
```
Input :  arr1[] = { 1, 3, 4, 5}  
         arr2[] = {2, 4, 6, 8}
Output : arr3[] = {1, 2, 3, 4, 4, 5, 6, 8}

Input  : arr1[] = { 5, 8, 9}  
         arr2[] = {4, 7, 8}
Output : arr3[] = {4, 5, 7, 8, 8, 9}
```
给定两个排序的数组，任务是以排序的方式合并它们。
**例子：**
```
输入：arr1[] = {1,3,4,5 }
     arr2[] = {2,4,6,8} 

输入：arr1[] = {5,8，9}   
     arr2[] = {4,7，8} 
输出：arr3[] = {4，5，7，8，8，9}
```

### 方法一：
```
<?php 
public function mergeArrays($arr1,$arr2){
    $arr3 = array_merge($arr1,$arr2);
    sort($arr3);
    return $arr3;
}
?>
```

### 方法二：
```
<?php 
public function mergeArrays2($arr1,$arr2){
    $i = $j = $k =0;
    while ($i<count($arr1)&&$j<count($arr2)){

        if($arr1[$i]<$arr2[$j]){
            $arr3[$k++]=$arr1[$i++];
        }else{
            $arr3[$k++]=$arr2[$j++];
        }
    }
    while ($i<count($arr1)){
        $arr3[$k++]=$arr1[$i++];
    }
    while ($j<count($arr2)){
        $arr3[$k++]=$arr2[$j++];
    }
   return $arr3;
}
?>
```

