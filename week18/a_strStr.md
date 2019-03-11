## ARTS

### Algorithm

### 题目：实现strStr()

题目：实现 [strStr()](https://baike.baidu.com/item/strstr/811469) 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  **-1**。

**示例 1:**

**输入:** haystack = "hello", needle = "ll"

**输出:** 2

**示例 2:**

**输入:** haystack = "aaaaa", needle = "bba"

**输出:** -1


### 方法一：暴力破解

```
function strStr($haystack, $needle) {
        $m = strlen($haystack);
        $n = strlen($needle);
        while ($i < $m && $j < $n)
        {
            if ($haystack[$i] == $needle[$j])
            {
                //①如果当前字符匹配成功（即$haystack[i] == $needle[j]），则i++，j++    
                $i++;
                $j++;
            }
            else
            {
                //②如果失配（即$haystack[i]! =$needle[j]），令i = i - (j - 1)，j = 0    
                $i = $i - $j + 1;
                $j = 0;
            }
        }
        //匹配成功，返回模式串$needle在文本串$haystack中的位置，否则返回-1
        if ($j == $n)
            return $i - $j;
        else
            return -1;            
        
}
```

虽然是个简单题目但是复杂度感觉很高。

思路：假设现在文本串haystack匹配到 i 位置，模式串needle匹配到 j 位置，则有：

如果当前字符匹配成功（即haystack[i] == needle[j]），则i++，j++，继续匹配下一个字符；

如果失配（即haystack[i]! = needle[j]），令i = i - (j - 1)，j = 0。相当于每次匹配失败时，i 回溯，j 被置为0。

