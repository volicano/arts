## ARTS

### Algorithm

### 题目：最长公共前缀

题目：编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

**示例 1:**

**输入: **["flower","flow","flight"]

**输出:** "fl"

**示例 2:**

**输入: **["dog","racecar","car"]

**输出:** ""

**解释:** 输入不存在公共前缀。

**说明:**

所有输入只包含小写字母 a-z 。


### 解法一：
```
public function longestCommonPrefix($arr) {
    if(count($arr)<1){
        return "";
    }
    $common_str= $arr[0];
    foreach ($arr as $key=>$value)
    {
        $arr[$key] = str_split($value);
    }
    $length = count($arr);
    $temp = $arr[0];
    $len = count($temp);
    //这个for循环中的$i 为矩阵的列，这样就能取出来每个子字符串数组的第i位
    for ($i=0;$i<$len;$i++)
    {
        //这里for循环中的$n为矩阵的行，就是传入字符串数组的下标
        for ($n=1; $n<$length; $n++)
        {
            echo $arr[$n][$i]."<br>";
            //如果基值的第i位与后面字符串的第i位不相等的话，截取基数字符串到第i位前
            if($temp[$i]!=$arr[$n][$i])
            {
                //当然这儿有一个特殊情况，就是比较第0位还没比完时候，就bi掉了，那么就返回无相同前缀
                if($i == 0)
                {
                    return "无相同前缀";
                }
                return substr($common_str,0,$i);
            }
        }
        //此处判断是 加入基数字符串遍历完成，全部都相同，那么我们返回基数字符串
        if ($i==$len-1)
        {
            return $common_str;
        }
    }
    return $common_str;
}
```

解答思路：

虽然是一道难度为简单的题，感觉不难但是真正写程序的时候还是难住了。
想了好久没想出来，后来从网上看到其他网友的思路，搬过来了。虽然看懂了但是，这道题应该是加星题。
大致思路是先拿传进来的数组的第一个字符串为参照基数，然后把传进来的字符串数组转成矩阵形式，
通过循环遍历矩阵的形式进行比较。
