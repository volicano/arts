## ARTS

### Algorithm

### 题目：
查找第一个没有重复的数组元素。

**示例**：
```
输入：-1 2 -1 3 2 
输出：3 
说明：第一个不重复的数字是：3 

输入：9 4 9 6 7 4 
输出：6
```
### 方法一：
```
<?php
    public function firstNonRepeating($arr){
        $temp = array_count_values($arr);  //统计数组中所有值出现的次数
        $char = null;
        foreach ($temp as $k=>$value){
            if($value==1){
                $char = $k;
                break;
            }
        }
        if(is_null($char)){
            echo "the first norepeat element is not found!";
        }else{
            echo "the first norepeat element is $char";
        }
    }
​?>
```
### 方法二：
```
<?php
    public function firstNonRepeating2($arr){
       $count = count($arr);
       for($i=0;$i<$count;$i++){
           $j=0;
           for ($j=0;$j<$count;$j++){
               if($i!=$j&&$arr[$i]==$arr[$j]){
                    break;
               }
           }
           if($j==$count){
               return $arr[$i];
           }
       }
       return null;
    }
​?>
```


