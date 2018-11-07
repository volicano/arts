# 计算买卖股票的最佳时机

给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。
如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。
注意你不能在买入股票前卖出股票。

**示例 1:**
```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
```
**示例 2:**
```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

## 方法一：暴力法
```
<?php
public function maxProfit($array){
    $maxprofit = 0;
    for ($i=0;$i<(count($array)-1);$i++){
        for ($j=$i+1;$j<count($array);$j++){
            $temp = $array[$j]-$array[$i];
            if($temp>$maxprofit){
                $maxprofit = $temp;
            }
        }
    }
    return $maxprofit;
}
?>
```
**解决思路**

我们需要找出给定数组中两个数字之间的最大差值（即，最大利润）。
此外，第二个数字（卖出价格）必须大于第一个数字（买入价格）。
形式上，对于每组 i和 j（其中 j>i）我们需要找出 max(array[j]−array[i])。

## 方法二：一次遍历

```
<?php

  public function maxProfit($array){
      $minPrice = max($array);
      $maxProfix = 0;
      foreach ($array as $k=>$v){
          if ($v < $minPrice)
              $minPrice = $v;
          else if ($v - $minPrice > $maxProfix)
              $maxProfix = $v - $minPrice;
      }
      return $maxProfix;
  }
?>
```
**解决思路**

**算法**

假设给定的数组为：
[7, 1, 5, 3, 6, 4]

如果我们在图表上绘制给定数组中的数字，我们将会得到：

![图片](https://images-cdn.shimo.im/bhru4UsZlTEuiPwf/121_profit_graph.png!thumbnail)

使我们感兴趣的点是上图中的峰和谷。我们需要找到最小的谷之后的最大的峰。 
我们可以维持两个变量——minprice 和 maxprofit，它们分别对应迄今为止所得到的最小的谷值和最大的利润（卖出价格与最低价格之间的最大差值）。

来自：[https://leetcode-cn.com/articles/best-time-to-buy-and-sell-stock/](https://leetcode-cn.com/articles/best-time-to-buy-and-sell-stock/)
