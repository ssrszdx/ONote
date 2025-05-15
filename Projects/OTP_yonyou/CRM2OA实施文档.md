# CRM2OA实施文档

### 目录

1. 功能介绍
2. 代码部署
3. 业务实施
4. 注意事项

### 内容

1. **功能介绍**

    1.1 服务请求单，费用申请单，投标申请单审批消息推送到致远OA系统里面完成审批。

    1.1.1 送审OA :

    ![cea411b9a3ba728dabdd62fc0f3aa0b.png](cea411b9a3ba728dabdd62fc0f3aa0b-20220318154256-1khhz7f.png)

    ![image.png](image-20220319094029-jndgbpq.png)

    ![image.png](image-20220319093914-mm0y2po.png)

    1.1.2 撤销送审： 撤销后OA系统里面的待办会撤回。审核过后送审和撤审按钮都会消失，送审完可以撤审。

    ![image.png](image-20220319094240-2pk2cxz.png)

    1.1.3 OA审批历史

    ![image.png](image-20220319095438-t2jdayl.png)

    1.1.4 审核通过后消息提醒，打开消息可以查看单据内容

    ![image.png](image-20220319095845-c7o99kn.png)

    ‍

    ![image.png](image-20220319095942-02a14mk.png)

    ‍

    1.2 销售机会根据投标申请单的审批状况进行的阶段升迁控制

    ![image.png](image-20220319100205-4nptr1t.png)

    ![image.png](image-20220319100342-nb56eot.png)

    ![image.png](image-20220319100530-1hbazgr.png)

    1.3 OA系统可以单点登录到CRM系统

    ![dc010f5c2ea7642cd5429e1731a7c2b.jpg](dc010f5c2ea7642cd5429e1731a7c2b-20220318155743-iake08b.jpg)

    ![7c8268c0e37dadedfe6080d77bee8e3.jpg](7c8268c0e37dadedfe6080d77bee8e3-20220318155812-xozaxnv.jpg)

    1.4 移动M3端单点登录

    ![9ee3a4237714e551b9eb54aadbcfd22.jpg](9ee3a4237714e551b9eb54aadbcfd22-20220321200820-m5cyif0.jpg)![20e2496f1fd740556dfb7ecc14b737e.jpg](20e2496f1fd740556dfb7ecc14b737e-20220321200849-xrm191y.jpg)
2. **代码部署**

    2.1 备份 \U8SOFT\turbocrm70\code 文件夹。

    2.2 用**实施包**里面的code文件夹覆盖替换。

    2.3 执行**实施包**里面的scripts 脚本，在对应的账套上执行。

    2.4. 右键管理员执行\U8SOFT\turbocrm70\tsvr\clearcache.cmd 命令。
3. **业务实施**

    **3.1** 审批消息首先要配置一下哪些字段推送到OA系统里面。由于OA系统只是审批查看的功能，都设置成文本类型即可。个别比如投标申请单，需要在CRM系统里面填写的，在OA系统里面设置成文本即可；需要在OA系统里面填写的且CRM不推送该字段，那么需要什么字段类型在OA里面设置什么字段类型即可。

    **3.1.1** 在crm系统里面，找到系统管理模块->对象管理->类型设置

    ![21b0be37f008ac3913300c0875fa840.png](21b0be37f008ac3913300c0875fa840-20220318154425-lyyg5bc.png)

	**3.1.2** 在crm系统里面，找到系统管理模块->对象管理->属性设置

	在简体中文标签的描述里面配置上对应的OA系统里面的字段名即可

	**3.2**  PC.crm 单点登录设置

**	3.2.1** 个人设置 

        主要三个参数：登录CRM系统的用户名 loginname 密码password 账套名 orgcode

        登录日期operatedate 跨会计区间的时候可能用到，改成当前会计区间内的登录时间即可。

        界面语言： langTag， 这个参数不用修改，默认zh-CN 简体中文

![46dcb1a67e280f8f1cac33b269ce372.jpg](46dcb1a67e280f8f1cac33b269ce372-20220318160828-ltljqik.jpg)

![61f48cb8b0cf997b926941b0736f24d.jpg](61f48cb8b0cf997b926941b0736f24d-20220318160152-fs9t823.jpg)

    **3.2.2**  系统设置

 system账户登录OA系统->信息集成配置->关联系统管理->外部系统->U8CRM

URL : http://128.96.100.247:8072/login/loginpost.php

![image.png](image-20220319100830-2dhgyc4.png)

参数配置: loginname password  注意这些参数名称要保持一致。

![image.png](image-20220319101041-ypgylm6.png)

3.3  M3单点登录设置

admin1 账户登录OA系统，M3移动管理平台->移动应用注册

![16478633011.png](16478633011-20220321194830-shiu140.png)

‍

接入设置：

![16478642401.png](16478642401-20220321200407-k7v6rg0.png)

‍

4. **注意事项**

    4.1  U8系统升级或者打补丁的时候请注意以下文件是否更新，若更新需要合并文件内容。

    **costapplyview.php，defineobjectview.php，serviceview.php， tview.lib, home.lib**

    4.2  具体实施的时候要确保CRM的登录名和OA系统的登录名一致。

    4.3  代码实施的时候清理CRM缓冲一定要放到最后一步。

‍
