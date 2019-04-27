## ARTS

### Algorithm

### 题目：选择排序

选择排序（Selection sort）是一种简单直观的排序算法。首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。

方法一：
```
public function selectSort($array){
    $count = count($array);
    if($count<=1){
        return $array;
    }
    //下标从0开始，所以进行n-1趟
    for ($i=0;$i<$count-1;$i++){
        //假设未排序的当前趟次位置为最小值，并记录下标
        $min = $i;
        //从当前趟往后选择出最小值，当前趟次往后应为i+1
        for ($j=$i+1;$j<$count;$j++){

            if($array[$j]<$array[$min]){
                $min = $j;
            }
        }
        //选择出最小值后，如果元素没在正确位置，则交换
        if($min!=$i){
            $temp = $array[$i];
            $array[$i] = $array[$min];
            $array[$min] = $temp;
        }
    }
    return $array;
}
```

解法思路：
1. 首先，在序列中找到最小元素，存放到排序序列的起始位置；
2. 接着，从剩余未排序元素中继续寻找最小元素，放到已排序序列的末尾。
3. 重复第二步，直到所有元素均排序完毕。


