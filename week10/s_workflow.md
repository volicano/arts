## ARTS

### Share

### 工作流核心模型图

![图片](https://uploader.shimo.im/f/I3xr38SofsQcfPBn.png!thumbnail)

discount_budget                业务信息表（要审批的业务流程单）

wf_workflow                    流程单

wf_node                        流程节点

wf_workflow_user               流程和审批人员关联表

wf_workflow_process            流程审批记录表（审核记录）

wf_workflow_rule                审批流程定义表



##

例如有一个审批单，现在要有三级人员审批。

我们可以把定义规则写在rules表中（可以是序列化数组或者json字符串）；

当我们提交一个申请单的时候会在wf_workflow、wf_node、wf_workflow_users这三张工作流相关表中生成数据，wf_workflow生成工作流id，wf_node生成要审核的等级，wf_workflow_users生成每一个等级需对应的人；

当出现审核记录时会在wf_workflow_process表生成记录。




