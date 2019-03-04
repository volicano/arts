## ARTS

### Algorithm

### 题目：子集

题目：给定一组**不含重复元素**的整数数组 *nums*，返回该数组所有可能的子集（幂集）。

**说明：** 解集不能包含重复的子集。

**示例:**

**输入:** nums = [1,2,3]

**输出:**

[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

### 解法一：
```
class Solution {
    private $res = [];
    private $len,$nums;
 
    /**
     * @param Integer[] $nums
     * @return Integer[][]
     */
    function subsets($nums) {
        $this->len = count($nums);              //成员变量长度
        $this->nums = $nums;
        if($this->len == 0) return $this->res;  //初始化判断
        $this->findSub(0,[]);                   //递归回溯记录结果数组
        return $this->res;
    }
 
    /**
     * [递归回溯记录结果数组]
     * @param  [type] $start [开始下标]
     * @param  [type] $path  [暂存的结果数组]
     */
    private function findSub($start,$path){
        $this->res[] = $path;           //记录子集，每次递归都是一个新的子集
        for($i = $start;$i<$this->len;++$i){
            $path[] = $this->nums[$i];          //回溯递归过程
            $this->findSub($i+1,$path);
            array_pop($path);
        }
    }
}
```

