## ARTS

### Algorithm

### 题目：求一个函数的实现

求一个数组$res, 将其拆分成$n个"随机"数字, 使其相加之和等于$sum, 并且满足每个数字最大值不能大于$max, 最小值不能小于$min, 请完成下面函数.

例如:  

split_num(20, 4, 8, 1) 

返回:

array(0 => 6, 1 => 5, 2 => 3, 3 => 6)

split_num(20, 4, 8, 1)

这里面 

20是要求出来的四个数字的和

4 是四个随机数

8 是数字里的$max

1 是数字里的$min

#### 求这个函数的实现

function split_num($sum, $n, $max, $min) {
    

}

(考点: 逻辑, 循环, 随机函数应用, 任务拆分)

### 方案一：循环法
```
function split_num($sum, $num, $max, $min) {
        srand(time());
        $res = array(); 
        if ($min * $num > $sum) {
                return null;
        } else if ($min * $num == $sum) {
                for ($i = 0; $i < $num; ++ $i) {
                        $res[] = $min;
                }
                return $res;
        } 
        $left = $sum; 
        for ($i = 0; $i != $num; ++ $i) {
                $cur_max = $left - ($num - $i - 1) * $min;
                $cur_max = $cur_max < $max ? $cur_max : $max;
                $cur_min = $left - ($num - $i - 1) * $max;
                $cur_min = $cur_min < $min ? $min : $cur_min;
                echo("[" . $cur_min . "," . $cur_max ."]");
                $rd_num = rand($cur_min, $cur_max);
                $res[] = $rd_num;
                $left -= $rd_num;
        } 
        $res[$num - 1] += $left;
        return $res;
}
```
### 方案二：借助内置函数
```
function split_num($sum, $num, $max, $min) {
  $res = array_fill(0, $num-1, $min);
  while(array_sum($res) < $sum) {
    $k = rand(0, $num-1);
    if($res[$k] < $max) $res[$k]++;
  }
  return $res;
}
```

