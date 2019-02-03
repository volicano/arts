## ARTS

### Tip

## 什么是好的api设计

什么是好的api接口

对于前端用户来说：

1、易于学习

2、易使用：没有复杂的程序、复杂的细节，易于学习；灵活的API允许按字段排序、可自定义分页、 排序和筛选等。一
个完整的API意味着被期望的功能包含在内。

对于开发人员来说：

1、易阅读：

2、易开发、易扩展：个最小化的接口是使用尽可能少的类以及尽可能少的类成员。这样使得理解、记忆、调试以及改变
API更容易。

设计原则

1. 深入了解需求

2. 了解数据库结构

3. 了解客户端原型

良好的API设计的思考点？

1、尽可能的小

接口尽量只做一件事情。

2、良好的响应速度

你有没有看到哪个APP访问速度很慢，大家还愿意用他的？做到秒开才是王道，由于各种验证，各种日志记录，已经消耗

了不少时间，所以更要注意效率问题。

3、尽量少的外部依赖

4、客户端与服务端的平衡

服务端尽量去控制更多的东西，比如说时间显示，逻辑控制。客户端只负责显示；考虑到客户端升级打包时间问题！

5、安全问题

权限

为什么会有这个问题呢？如果是自己的网站，那么，你所访问的地址，就是你自己提供的，根本不需要什么访问权限控

制！但是，如果对外提供服务，那就得考虑了。来访的人不是内部的怎么办？他登录了没有？现在有多少人在访问这个

服务？这些东西，都应该被一目了然的呈现出来。那么，怎样控制来访人员呢？

方法1、在程序里写死几个密码一类的东西，让客户端访问时，带上此变量以验证；

方法2、为每个客户端（我说的是安卓或ios等一套源代码）提供一个appId,appKey，访问时携带；

方法3、使用Oauth等授权方式。

加密：

所有的访问，几乎都是采用明码传输，那么就必须有一定的假设信息被截获的防范措施（实际上，这个假设也很容易成

立）！对于一般的信息，加一个普通的普通的签名即可，如：appId+appKey+访问参数+timestamp+随机字符n个 再 md5

得到签名，服务器端首先进行该签名验证，确认后，再进行后续操作！

6、完善的文档 （接口、参数的命名）

对以后的升级和修改能有据可循。

7、版本的升级问题（兼容）

因为，如果整个网站都是你的，你想怎么改都可以，反正别人访问也就只有你网址这一个入口。但是移动APP不一样了，

每个人都是独立的，他们各自的版本都是不一样的。如果共用一套接口的话，小的改动还好，向后兼容就行了，但是对

于一些大方向的改动，这将是致命的，要么强行使用户不能使用以促使其更新，要么你继续写一长篇无用的不可维护的

冗余代码！所以，做好版本控制是必须的!

从代码层方面

举例：
```
public function listOp()
{
$parts_inquiry_model = model('parts_inquiry');
$parts_quotedmodel=Model('parts_quoted');
$parts_inquiry_product_model = model('parts_inquiry_product');
$param_userid = getParamFastVeto('userid');
$state = getParameter("state");
$curpage = getParameter('curpage');
if((!isset($state)&&empty($state))){
$state = 1;
}
if(!in_array($state,$parts_inquiry_model->getStatus())){
$this->echoError("订单状态码错误");
}
$condition['user_id'] = $param_userid;
//查询状态
switch ($state){
case "2":
// 不处理
break;
default :
$condition["status"] = $state;
break;
}
$filed = "inquiry_id,car_code,car_type_name,add_time,status,quoted_num,order_num,remark";
$inquirylist = $parts_inquiry_model->listsData($condition, $filed, 0, 10);
if($inquirylist){
$parts_quoted_model = Model('parts_quoted');
foreach($inquirylist as $k=>$v){
$where['inquiry_id'] = $v['inquiry_id'];
$parts = $parts_inquiry_product_model->listsData($where,'*',false,false);
$inquirylist[$k]['currenttime'] = TIMESTAMP;
$procurement_number = 0;
foreach ($parts as $value){
$procurement_number += $value['procurement_number'];
}
$inquirylist[$k]['car_logo'] = $this->getCarLogo($v['car_code']);
$inquirylist[$k]['count'] = $procurement_number;
$data = $parts_quotedmodel->table('parts_quoted')->field('user_id,MAX(quoted_id) AS quoted_id')-
>where(array('inquiry_id'=>$v['inquiry_id']))->limit(false)->order(false)->group('user_id')->key('quoted_id')->page(false)->select();
$inquirylist[$k]['quoted_num'] = empty($data)?0:count($data); //计算报价的商家数量
$inquirylist[$k]['jxsnum'] = $inquirylist[$k]['quoted_num'];
if(TIMESTAMP>($v['add_time']+3600*24)){
$arr["status"] = 0;
$flag = $parts_inquiry_model->updateData($where,$arr);
}
$inquirylist[$k]['partlist'] = $parts;
}
$result_arr['result'] = true;
$result_arr['resultMsg'] = '获取成功';
$result_arr['resultData'] = $inquirylist;
$result_arr['load'] = false;
$totalnum = $parts_inquiry_model->countData($condition);
if(intval($curpage)*10 < intval($totalnum)){
$result_arr['load'] = true;
}
$this->echoJson($result_arr);
}else{
$this->echoError("询价订单暂不存在");
}
}
```
改造后：
```
public function listOp(){
$order_state = getParameter("order_state");
if(empty($order_state)){
$order_state= 0;
}
$orientation = getParamFastVeto("orientation"); //视角 enum("provider,"consumer");
$search_key = getParameter("search_key");
if(!in_array($orientation,array(
"consumer","provider"
))){
$this->echoError("不支持的方式");
}
if(!in_array($order_state,$this->repairModel->getOrderState())){
$this->echoError("订单状态码错误");
}
$result = $this->repairModel->getOrders($orientation,$this->company_id,$order_state,$search_key);
$this->echoSuccess($this->carryOrderDefaultInList($result));
}
private function carryOrderDefaultInList($result){
if(empty($result)){
return null;
}
$rule=array(
"return_key"=>array(
"order_sn"=>"",
"company_name"=>"service_provider",
"contact_number"=>"service_provider_number",
"contact_im"=>"service_provider_im",
"order_time"=>"add_time",
"order_state"=>"",
"server_name"=>"service_name",
"server_person"=>"service_person",
"payment"=>"payment_code",
"initial_amount"=>"",
"finished_amount"=>"",
"reservation_time"=>"appointment",
),
"callback"=>array(
"order_time"=>"formattime",
"order_state"=>"getOrderState",
"reservation_time"=>"weekday",
"payment"=>"getPayment",
"initial_amount"=>"formatMoney",
"finished_amount"=>"formatMoney",
"actions"=>"consumerActions",
"go_pay"=>"getPayState"
),
"default"=>array(
"server_person"=>"待分配",
"payment"=>"待支付"
)
);
return $this->getNeedItems($result,$rule);
}
public function getPayState($data,$targetItem,$sourceItem){
if($sourceItem["order_state"]=="20"||$sourceItem["order_state"]=="30"){
return true;
}else{
return false;
}
}
```



