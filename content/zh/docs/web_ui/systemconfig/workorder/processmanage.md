---
title: "工单配置"
weight: 1
description: 用于配置工单的流程。
---

工单主要分为审批工单和技术支持工单。

- 审批工单：审批工单主要用于控制资源分配，包括主机申请、主机调整配置、主机删除、加入项目申请、项目配额申请、域配额申请。
    - 主机申请：支持多级审批，该类型工单启用后，用户创建虚拟机时将会发起主机申请工单，待每个流程中的审批用户审批通过后，自动部署虚拟机。
    - 主机调整配置：仅支持一级审批，该类型工单启用后，用户调整虚拟机配置时将会发起主机调整配置工单，待流程中的审批用户审批通过后，自动调整配置。
    - 主机删除：支持多级审批，该类型工单启用后，用户删除虚拟机时将会发起主机删除工单，待每个流程中的审批用户审批通过后，自动删除虚拟机，存放在回收站。
    - 加入项目申请：仅支持一级审批，该类型工单启用后，用户登录{{<oem_name>}}平台后，如用户还没有项目，可直接申请加入项目；或用户在项目视图下，单击右侧![](../../../images/system/gd.png)图标，选择 **_"申请加入项目"_** 发起加入项目工单申请，待流程中的审批用户审批通过后，用户自动加入项目。
    - 项目配额申请：仅支持一级审批，该类型工单启用后，用户在项目视图下，单击右侧![](../../../images/system/gd.png)图标，选择 **_"项目配额申请"_** ，配置申请的配额数，发起项目配额工单申请，待流程中的审批用户审批通过后，自动调整项目配额。
    - 域配额申请：仅支持一级审批，该类型工单启用后，用户在域管理后台视图下，单击右侧![](../../../images/system/gd.png)图标，选择 **_"域配额申请"_** ，配置申请的配额数，发起域配额工单申请，待流程中的审批用户审批通过后，自动调整域配额。 
- 技术支持工单：主要用于协助用户解决平台使用问题等。
    - 技术支持工单：仅支持一级审批，该类型工单启用后，用户在项目视图或域管理后台视图下，单击右侧![](../../../images/system/gd.png)图标，选择 **_"申请技术支持"_** ，描述所遇到的问题，并设置优先级为“次要、中等、危急、致命”等，发起技术支持工单，审批流程中的用户将会按照优先级优先处理级别高的问题，直至问题解决，由用户关闭该工单。

{{% alert title="说明" %}}
工单启用后，用户提交工单或有待处理工单时，将会在用户支持的通知渠道收到工单的消息提醒。
{{% /alert %}}

**入口**：在云管平台单击左上角![](../../../images/intro/nav.png)导航菜单，在弹出的左侧菜单栏中单击 **_"系统配置/工单/工单配置"_** 菜单项，进入工单配置页面。

![](../../../images/system/process.png)

## 配置工单

该功能用于配置工单审批流程。

1. 在工单配置页面，单击列表上方 **_"配置流程"_** 按钮，弹出配置流程对话框。
2. 配置以下参数：
    - 工单类型：选择具体的工单类型，包括主机申请、主机调整配置、主机删除、加入项目申请、项目配额申请、域配额申请、技术支持工单等。
    - 审批环节：设置具体的审批流程。其中主机申请和主机删除最多支持5个审批环节。审批环节支持同时指定角色审批和指定人审批。
        - 指定角色审批：选择指定审批角色、审批人范围。其中域配额申请、加入项目工单，审批人范围为域和全局；其他工单的审批人范围为项目、域和全局。
            - 当审批人范围为项目时，将根据申请资源的所属项目，在对应项目中查找匹配角色的用户审批工单。
            - 当审批人范围为域时，将根据申请资源的所属域，在对应域中查找匹配角色的用户审批工单。
            - 当审批人范围为全局时，将在系统中查找匹配角色的用户审批工单。
{{% alert title="说明" %}}
- 主机申请、主机调整、主机删除等类型工单需要根据虚拟机的归属项目、归属域，在对应项目、对应域中查找匹配角色的用户。
- 项目配额申请工单将根据申请配额的项目，以及项目所在域，在对应项目、对应域中查找匹配角色的用户。
- 域配额申请工单将根据申请配额的域，在对应域中查看匹配角色的用户。
- 加入项目工单将根据提交加入项目工单的用户所在域，在对应域中查看匹配角色的用户。
- 技术支持工单将根据提交技术支持工单的用户当前所在的项目，以及项目所在的域，在对应项目、对应域中查找匹配角色的用户。
{{% /alert %}}
        - 指定人审批：即指定具体的用户审批工单，即该类型的所有工单都将提交给指定的用户进行审批。
3. 单击 **_"确定"_** 按钮，完成工单配置。

### 第三方工单流程对接

目前仅技术支持工单支持对接第三方工单系统的流程，在配置工单之前请先在[第三方](../thirdparty)中对接第三方工单系统。

1. 在工单配置页面，单击列表上方 **_"配置流程"_** 按钮，弹出配置流程对话框。
2. 选择类型为“第三方工单系统”，并选择技术支持工单对应的第三方工单系统的工单流程。
3. 单击 **_"确定"_** 按钮，完成工单配置。

## 启用工单

该功能用于启用“禁用”状态的工单，禁用的工单将不再起作用。

1. 在工单配置页面，单击工单右侧操作列 **_"启用"_** 按钮，弹出操作确认对话框。
2. 单击 **_"确定"_** 按钮，启用工单审批流程。

## 禁用工单

该功能用于禁用“启用”状态的工单，禁用的工单将不再起作用。

1. 在工单配置页面，单击工单右侧操作列 **_"禁用"_** 按钮，弹出操作确认对话框。
2. 单击 **_"确定"_** 按钮，禁用工单审批流程。

## 设置流程

该功能用于修改工单审批流程。

1. 在工单配置页面，单击工单右侧操作列 **_"修改流程"_** 按钮，弹出修改流程对话框。
2. 修改审批环节的步骤名称、指定角色审批或指定人审批等，支持添加或减少审批环节等。
3. 修改完成后，单击 **_"确定"_** 按钮。

## 查看工单详情

该功能用于查看工单详情信息。

1. 在工单配置页面，单击工单名称项，进入工单详情页面。
2. 详情页面顶部菜单项支持对工单进行管理操作。
3. 查看工单的云上ID、ID、名称、状态、域、项目、启用状态、更新时间、创建时间、备注。


### 查看操作日志

该功能用于查看工单相关操作的日志信息。

1. 在工单详情页面，单击“操作日志”页签，进入操作日志页面。
    - 加载更多日志：列表默认显示20条操作日志信息，如需查看更多操作日志，请单击 **_"加载更多"_** 按钮，获取更多日志信息。
    - 查看日志详情：单击操作日志右侧操作列 **_"查看"_** 按钮，查看日志的详情信息。支持复制详情内容。
    - 查看指定时间段的日志：如需查看某个时间段的操作日志，在列表右上方的开始日期和结束日期中设置具体的日期，查询指定时间段的日志信息。
    - 导出日志：目前仅支持导出本页显示的日志。单击右上角![](../../../images/system/download.png)图标，在弹出的导出数据对话框中，设置导出数据列，单击 **_"确定"_** 按钮，导出日志。

## 工单中心管理

默认右上角将不会显示![](../../../images/system/gd.png)图标。只有存在启用状态的任意类型的工单时，才会显示![](../../../images/system/gd.png)图标。

### 技术支持工单

只有存在启用状态的技术支持工单后，单击右上角![](../../../images/system/gd.png)图标，展开的下拉菜单才会存在技术支持工单。

- 未完成的工单n条，即用户提交的技术支持工单还未完成的数量，单击跳转工单-我发起的页签，；
- 待审批n条，即待技术支持处理的技术支持工单的数量，单击跳转工单-待我处理页签；

#### 我发起的

技术支持-我发起的页面用于查看用户发起的技术支持工单。

##### 申请技术支持

仅在项目视图和域管理后台视图支持申请技术支持。

1. 单击右上角![](../../../images/system/gd.png)图标，选择下拉菜单 **_"申请技术支持"_** 菜单项，弹出申请技术支持对话框。

    ![](../../../images/system/applytsgd.png)

2. 或在技术支持-我发起的页面，单击列表上方 **_"申请技术支持"_** 按钮，弹出申请技术支持对话框。
3. 输入问题描述、设置问题级别，单击 **_"确定"_** 按钮，提交技术支持工单，此时工单状态为已提交。
4. 当不在技术支持-我发起的页面提交技术支持工单时，提交技术支持工单后，将会跳转到技术支持-我发起的页面。

##### 查看技术支持工单

当用户需要追加问题描述信息，或者技术支持已处理需要用户反馈时，可通过查看功能补充问题描述新。

1. 在技术支持-我发起的页面，单击工单右侧操作列 **_"查看"_** 按钮，进入查看工单页面。
2. 单击 **_"回复"_** 按钮，追加问题描述等，单击 **_"提交"_** 按钮，追加信息。

##### 结束技术支持工单

当技术支持工单处理完毕或无需处理时，可以结束技术支持工单。当工单被技术支持处理后，处于“待用户反馈”状态时，用户超过3天未反馈工单时，工单将会自动结束。如还有问题，请重新提交技术支持工单。

1. 在技术支持-我发起的页面，单击工单右侧操作列 **_"结束"_** 按钮，弹出结束对话框。
2. 选择是否解决，输入评价等，单击 **_"确定"_** 按钮，结束技术支持工单。

#### 待我处理

技术支持-待我处理页面用于技术支持处理用户提交的工单。

1. 在技术支持-待我处理页面，单击工单右侧操作列 **_"查看"_** 按钮，进入查看工单页面。此时工单流程状态从“已提交”或“等待处理”变为“处理中”。
2. 单击 **_"回复"_** 按钮，向用户解答问题或请用户补充更多信息，单击 **_"提交"_** 按钮，回复用户，此时工单流程状态为“待用户反馈”，且工单将出现在“我已处理”列表。

#### 我已处理

技术支持-我已处理页面用于显示技术支持已经处理，但是用户未结束的技术支持工单。

#### 第三方工单

只有配置了第三方工单系统后，才会出现该页面。用于查看对接第三方工单系统的工单审批信息。

#### 所有工单

该功能用于查看所有技术支持工单。仅管理后台和域管理后台支持查看，其中在域管理后台下仅支持查看该域下的所有工单。

### 审批工单

只有存在启用状态的任意一种类型的审批工单后，单击右上角![](../../../images/system/gd.png)图标，展开的下拉菜单才会存在审批工单。

- 未完成的工单n条，即用户提交的审批工单还未完成的数量，单击跳转工单-我发起的页签，；
- 待我审批n条，即待用户审批的审批工单的数量，单击跳转工单-待我审批页签；

#### 我发起的

审批工单-我发起的页面用于查看用户提交的审批工单。

##### 发起审批工单

1. 当用户申请主机、删除主机、调整配置、申请加入项目、申请项目配额、申请域配额时，将会发起审批工单，跳转到审批工单-我发起的页面。
2. 后续用户可以单击右上角![](../../../images/system/gd.png)图标，在展开的下拉菜单中选择“未完成”，跳转到审批工单-我发起的页面。

##### 撤销审批工单

只有未完成的工单才支持撤销。

1. 在审批工单-我发起的页面，单击“未完成”状态的工单右侧操作列 **_"撤销"_** 按钮，弹出撤销工单对话框。
2. 输入原因后，单击 **_"确定"_** 按钮，撤销审批工单。

#### 待我审批

审批工单-待我审批页面显示当前登录用户需要审批的工单信息。

##### 通过审批工单

**处理加入项目工单**

1. 在审批工单-待我审批页面，单击“加入项目申请”类型的工单右侧操作列 **_"通过"_** 按钮，弹出通过对话框。
2. 设置以下信息：
    - 项目：设置用户需要加入的项目。三级权限开启后，默认只能加入用户所在域的项目，三级权限关闭后，只能加入到Default域下的项目。
    - 角色：设置用户加入项目时的角色，不同角色具有不同权限。
    - 备注：备注信息。
3. 单击 **_"确定"_** 按钮，通过审批工单。

**其他类型工单**

1. 在审批工单-待我审批页面，单击“加入项目申请”类型之外的工单右侧操作列 **_"通过"_** 按钮，弹出通过对话框。
2. 输入备注信息，单击 **_"确定"_** 按钮，通过审批工单。

{{% alert title="说明" %}}

当用户发起的主机调整配置工单领导审批通过后，但是由于资源不足等问题导致主机配置调整失败，工单流程将不会结束。等待资源充足时，用户在同步待调整配置的主机状态后可自行审批工单，继续调整主机配置。

{{% /alert %}}

##### 驳回审批工单

该功能用于驳回审批工单。

1. 在审批工单-待我审批页面，单击工单右侧操作列 **_"驳回"_** 按钮，弹出驳回对话框。
2. 输入备注信息，单击 **_"确定"_** 按钮，驳回审批工单。

#### 我已审批

审批工单-我已审批列表显示当前登录用户已审批的工单信息。

#### 我参与的

审批工单-我参与的显示的工单申请审批人包含当前登录用户，但是由其他用户审批处理的工单信息

#### 所有工单

该功能用于查看所有审批工单。仅系统管理员支持查看。