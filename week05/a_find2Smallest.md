## ARTS

### Algorithm

### 题目：
找到数组中的最小和第二小元素。

**示例**：
```
输入：arr [] = {12,13,1,10,34,1}
输出：最小元素是1和第二个最小的元素是10
```
### 实现：
```
<?php
    public function find2Smallest($arr){
        $size = count($arr);
        if($size<2){
            echo 'Invalid Input';
            return;
        }
        $first = $second = $max = max($arr);
        for ($i=0;$i<$size;$i++){
            if($arr[$i]<$first){
                $second = $first;
                $first = $arr[$i];
            }else if($arr[$i]<$second&&$arr[$i]!=$first){
                $second = $arr[$i];
            }
        }
        if ($second == $max)
            echo("There is no second smallest element\n");
        else
            echo "The smallest element is ",$first," and second Smallest element is ", $second;
    }
​?>
```
### 分析：
1）假设最小值和第二最小值为max（数组总最大值）

      first = second = max 

2）循环遍历所有元素。

   a）如果当前元素小于first，则更新第一个和第二个。

   b）否则，如果当前元素小于second并且当前元素不等于第一最小元素，则更新第二个。


