## ARTS

### Algorithm

[4. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

### 题目：

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.
**Example 1:**
```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```
**Example 2:**
```
Input: "cbbd"
Output: "bb"
```
给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。
**示例 1：**
```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```
**示例 2：**
```
输入: "cbbd"
输出: "bb"
```
### 第一版代码
求回文子串，毫无疑问需要每个字符遍历一遍，分别求出来各个字符的回文长度，然后选出最长的那一个。
```
function palindrome($str)
{   
    $n = strlen($str);
    $pos = 0;
    $max = 0;

    for ($i = 0; $i < $n; $i++) { 
        for ($j = 0; ($i - $j >= 0) && ($i + $j < $n); $j++) { 
            if ($str[$i - $j] != $str[$i + $j]) {
                break;
            }
            if ($j > $max) {
                $max = $j;
                $pos = $i;
            }
        }
    }
    return (substr($str, $pos - $max, $max * 2 + 1));
}
```
可以看到，这个代码是有bug的，因为回文子串可能是奇数长度，也可能是偶数长度，
因为我们是以一个字符为中心来求的，所以用这种方法只能求出奇数长度的回文子串，
接下来我们来改进一下。

### 第二版改进

因为我们只能求出奇数长度的回文子串，因此我们需要把字符串改进一下首先通过在每个字符的两边都插入
一个特殊的符号，将所有可能的奇数或偶数长度的回文子串都转换成了奇数长度。比如 abba 变成 #a#b#b#a#，
 aba变成 #a#b#a#，这样我们再来看一下代码：
 
```
function longestPalindrome($str)
{
    $pos = 0;
    $max = 0;
    $newStr = "#" . implode(str_split($str), "#") . "#";
    $n = strlen($newStr);
    for ($i = 0; $i < $n; $i++) {
        for ($j = 0; ($i - $j >= 0) && ($i + $j < $n); $j++) {
            if ($newStr[$i - $j] != $newStr[$i + $j]) {
                break;
            }
            if ($j > $max) {
                $max = $j;
                $pos = $i;
            }
        }
    }
    $r = substr($newStr, $pos - $max, $max * 2);
    $res = str_replace("#", "", $r);
    return ($res);
}
```
看解答不是最优解，还有更复杂度更低些解法，后续补充。
