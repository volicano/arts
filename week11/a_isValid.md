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
public function checkMatch($str){
    if(!$str)return false;
    $arr = str_split($str);
    $left = array('{','[','(');
    $right = array('}',']',')');
    $stack = array();
    reset($arr);  //使用while遍历数组需要先reset()，防止遍历不完整
    while(list($key, $val) = each($arr)){
        if(in_array($val,$left,true)){
            //入栈
            array_push($stack,$val); //把出现的全部左括号压入栈中
        }else if(in_array($val,$right,true)){
            $topStack = end($stack); //如果出现右括号，则栈顶的元素肯定是与其匹配的左括号(因为括号是对应的)，先取出栈顶元素。
            if(isset($topStack) && !empty($topStack)){
                if(array_search($val,$right,true) === array_search($topStack,$left,true)){ //判断当前右括号是不是与左括号匹配
                    //出栈
                    array_pop($stack); //匹配的话就pop出栈
                }else{
                    return false; //左右不匹配
                }
            }else{
                return false; //右括号多，因为没取出对应的左括号
            }
        }
    }
    return empty($stack) ? true : false;  //循环完成后判断$stack中是否还有值，有的话证明左括号多
}
```
解答思路

栈，体现的是后进先出，即LIFO。队列，体现的是先进先出，即FIFO。

栈：

array_pop() //尾出

array_push() //尾进

或

array_shift()//头进

array_unshift()//头出
