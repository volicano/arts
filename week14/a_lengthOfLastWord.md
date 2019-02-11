## ARTS

### Algorithm

### 题目：最后一个单词的长度

给定一个仅包含大小写字母和空格 ' ' 的字符串，返回其最后一个单词的长度。

如果不存在最后一个单词，请返回 0 。

**说明：** 一个单词是指由字母组成，但不包含任何空格的字符串。

**示例:**

**输入:**  "Hello World"

**输出:** 5

### 方法一：
```
class Solution {
    function lengthOfLastWord($s) {
        $s = trim($s);
        if($s==''){
            return 0;
        }
        $array = explode(' ',$s);
        if(count($array)==1){
            return strlen($array[0]);
        }
        foreach($array as $v){
            $max = strlen($v);
        }
        return $max;
    }
}
```

解答：简单粗暴的方法！
