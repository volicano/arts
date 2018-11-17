## ARTS

### Algorithm

[3. 无重复字符的最长子串的长度](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)
### 题目：
Given a string, find the length of the **longest substring** without repeating characters.

**Examples:**

Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. 

Note that the answer must be a **substring**, "pwke" is a *subsequence* and not a substring.


给定一个字符串，找出不含有重复字符的**最长子串**的长度。

**示例 1:**
```
输入: "abcabcbb"
输出: 3 
解释: 无重复字符的最长子串是 "abc"，其长度为 3。
```
**示例 2:**
```
输入: "bbbbb"
输出: 1
解释: 无重复字符的最长子串是 "b"，其长度为 1。
```
**示例 3:**
```
输入: "pwwkew"
输出: 3
解释: 无重复字符的最长子串是 "wke"，其长度为 3。
     请注意，答案必须是一个子串，"pwke" 是一个子序列 而不是子串。
```

### 方法一：暴力法
```
public function lengthOfLongestSubstring($str) {
    $length = strlen($str);
    $ans = 0;
    for ($i = 0; $i < $length; $i++){
        for ($j = $i + 1; $j <= $length; $j++){
            if (allUnique($str, $i, $j)){
                $ans = max($ans, $j - $i);
            }
        }
    }
    return $ans;
}

public function allUnique($str, $start, $end) {
    $set = "";
    for ($i = $start; $i < $end; $i++) {
        $ch = $str[$i];
        if (strstr($set,$ch)){
            return false;
        }
        $set.=$ch;
    }
    return true;
}
```
思路：
逐个检查所有的子字符串，看它是否不含有重复的字符。


步骤：
1、为了枚举给定的字符串，我们假设字符开始和结束的索引分别为 i 和 j ，那么我们有 0 <= i < j <= n （这里的结束索引 j 是按惯例排除的）,因此我们使用i从0到n-1以及j从i+1到n这两个嵌套的循环。

2、检测字符串中是否含有重复。


### 方法二：滑动窗口

```
public function lengthOfLongestSubstring($str) {
    $n = strlen($str);
    $ans = $i = $j = 0;
    $set = "";
    while ($i < $n && $j < $n) {
        if (!strstr($set,$str[$j])){
            $set.=($str[$j++]);
            $ans = max($ans, $j - $i);
        }else {
            $set[$i++]="";
        }
    }
    return $ans;
}
```
思路：

如果从索引 i 到 j - 1之间的子字符串 s{}ij}已经被检查为没有重复字符。我们只需要检查 s[j] 对应的字符是否已经存在于子字符串 s{ij}中。

