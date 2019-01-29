## ARTS

### Algorithm

### 题目：有效的括号

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
有效字符串需满足：
1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。

注意空字符串可被认为是有效字符串。
**示例 1:**

**输入:** "()"

**输出:** true

**示例 2:**

**输入:** "()[]{}"

**输出:** true

**示例 3:**

**输入:** "(]"

**输出:** false

**示例 4:**

**输入:** "([)]"

**输出:** false

**示例 5:**

**输入:** "{[]}"

**输出:** true


方法一：
```
public function isValid($str){
    if($str == null || strlen($str) == 1){
        return false;
    }
    if(strlen($str) == 0){
        return true;
    }
    $ptr = 1;
    $myStack = array();
    for ($i = 0; $i < strlen($str); $i++) {
        $val = 0;
        switch($str[$i]){
            case '(':$val = -1;break;
            case ')':$val = 1;break;
            case '[':$val = -2;break;
            case ']':$val = 2;break;
            case '{':$val = -3;break;
            case '}':$val = 3;break;
            default:break;
        }
        if($i == 0){
            $myStack[$ptr] = $val;
            continue;
        }
        if($myStack[$ptr] + $val == 0){
            $myStack[$ptr--] = 0;
        }else{
            $myStack[++$ptr] = $val;
        }
    }
    return $myStack[1] == 0;
}
```

