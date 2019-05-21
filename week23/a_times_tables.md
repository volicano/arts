## ARTS

### Algorithm

### 题目：PHP实现九九乘法表

使用PHP实现九九乘法表
方法一：
```
public function cal(){
    for ($i=1;$i<=9;$i++){
        for ($j=1;$j<=$i;$j++){
            echo "$i*$j=".$i*$i;echo "&nbsp;&nbsp;&nbsp;";
        }
        echo '<br>';
    }
    die;
}
```

