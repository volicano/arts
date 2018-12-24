## ARTS

### Algorithm

### 题目：
首先，维基百科了下定义：
>在[计算机科学](https://zh.wikipedia.org/wiki/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)中，**二分搜索**（英语：binary search），也称**折半搜索**（英语：half-interval search）[[1]](https://zh.wikipedia.org/wiki/%E4%BA%8C%E5%88%86%E6%90%9C%E7%B4%A2%E7%AE%97%E6%B3%95#cite_note-1)、**对数搜索**（英语：logarithmic search）[[2]](https://zh.wikipedia.org/wiki/%E4%BA%8C%E5%88%86%E6%90%9C%E7%B4%A2%E7%AE%97%E6%B3%95#cite_note-FOOTNOTEKnuth1998%C2%A76.2.1_(%22Searching_an_ordered_table%22),_subsection_%22Binary_search%22-2)，是一种在[有序数组](https://zh.wikipedia.org/wiki/%E6%9C%89%E5%BA%8F%E6%95%B0%E5%AF%B9)中查找某一特定元素的搜索[算法](https://zh.wikipedia.org/wiki/%E6%BC%94%E7%AE%97%E6%B3%95)。搜索过程从数组的中间元素开始，如果中间元素正好是要查找的元素，则搜索过程结束；如果某一特定元素大于或者小于中间元素，则在数组大于或小于中间元素的那一半中查找，而且跟开始一样从中间元素开始比较。如果在某一步骤数组为空，则代表找不到。这种搜索算法每一次比较都使搜索范围缩小一半。  

梳理重点，找解题思路：
>首先，二分查找法需要数组是一个有序的数组。

>一、要知道中间位置就需要知道起始位置和结束位置，然后取出中间位置的值来和我们的值做对比。

>二、如果中间位置的值等于我们的给定值，那恭喜你人品蛮好的。

>三、如果中间值大于我们的给定值，说明我们的值在中间位置之前，此时需要再次二分，因为在中间之前，所以我们需要变的值是结束位置的值。

>四、反之，如果中间值小于我们给定的值，那么说明给定值在中间位置之后，此时需要再次将后一部分的值进行二分，因为在中间值之后，所以我们需要改变的值是开始位置的值。


代码实现：
```
<?php 
public function binary_Search($array,$num){
    $length = count($array);
    $start = 0;
    $end = $length-1;
    if($length<1){
        echo 'the array is too short';die;
    }
    while ($start<=$end){
        /*
         * 在名著《编程珠玑》中的实现是
         * M = (U+L)/2   代码中参数B=L表示begin,也就是查找的起始位置，E=U表示end,也就是查找的结束位置。
         * 这种做法会导致数值计算溢出，假设代码运行在32位机器上，如果U 和 L 的值足够大，两者相加之后的值超过32位的话，
         * U+L 会导致计算溢出，超出32位以上的比特位可能会被丢弃，于是U+L会得到比预期结果要小的多的值
         * 新解：int M = L + (U - L) / 2;  来自https://www.jianshu.com/p/9b98708fde26的思考
         */
        $middle = ceil(($start+$end)/2);
        if($array[$middle]==$num){
            return $middle;
        }
        if($array[$middle]<$num){
            $start = $middle+1;
        }else{
            $end = $middle-1;
        }
    }
    return -1;
}
?>
```