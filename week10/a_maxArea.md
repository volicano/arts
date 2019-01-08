## ARTS

### Algorithm

### 题目：盛最多水的容器

给定 *n* 个非负整数 *a*1，*a*2，...，*a*n，每个数代表坐标中的一个点 (*i*, *a**i*) 。在坐标内画 *n* 条垂直线，垂直线 *i* 的两个端点分别为 (*i*, *a**i*) 和 (*i*, 0)。找出其中的两条线，使得它们与 *x* 轴共同构成的容器可以容纳最多的水。

**说明：**你不能倾斜容器，且 *n* 的值至少为 2。

![图片](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
 
**示例:**

**输入:** [1,8,6,2,5,4,8,3,7]

**输出:** 49

### 方法一：求矩形最大面积
```
<?php
public function maxArea($array) {
    $maxarea = 0;
    for ($i=0;$i<count($array);$i++){
        for ($j=$i+1;$j<count($array);$j++){
            $maxarea = max($maxarea,min($array[$i],$array[$j])*($j-$i));
        }
    }
    return $maxarea;
}
?>
```

### 方法二：双指针法
```
<?php

public function maxArea($array) {
    $maxarea = $l = 0;
    $r = count($array) - 1;
    while ($l < $r) {
        $maxarea = max($maxarea, min($array[$l], $array[$r]) * ($r - $l));
        if ($array[$l] < $array[$r])
            $l++;
        else
            $r--;
    }
    return $maxarea;
}
?>
```

分析：第一种方法好理解就是通过循环计算出所求的矩形最大面积。第二种方法感觉稍微难一些

它是通过动态位移的方法来计算最大面积。




