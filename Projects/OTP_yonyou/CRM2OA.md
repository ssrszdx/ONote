# CRM2OA

接口说明文档：

CTP-》Rest接口-》组织模型管理

按照登录名获取人员信息。

http://128.96.100.119:8090/seeyon/rest/orgMember?loginName=bdgly

‍

‍

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

http://localhost:8090/seeyon/u8UserInfoController.do?method=checkU8Key&u8serverName=u8data&u8Key=1

http://open.seeyon.com/seeyon/

麦瑞思。

VPN  ： 用户名：fsccteg   密码：fsccri12#$

218.61.106.194:6443

OA 帐号 cs 密码qx123456;  admin1 密码my123456

OA正式服务器：IP：128.96.100.113  
数据库 SQL server 2014  
OA数据库:oaxin  
sa密码：MeiYan@OA.com  
windows 登陆密码：MeiYan@OA.com

OA开发测试服务器：IP：128.96.100.119  
数据库 SQL server 2014  
OA数据库:oaxin  
sa密码：MeiYan@OA.com  
windows 登陆密码：MeiYan@OA.com

128.96.100.247  
administrator  
tizi@123  
这是U8服务器   999账套  
数据库密码 sa Fs57480510

OA bdgly:my123456

获取token，访问http://128.96.100.119:8090/seeyon/rest/token/{restusername}/{password}?loginName={loginName}  
Restusername：rest帐号  
password：rest帐号的密码  
loginName：配置文档中 推送底表数据使用的OA账号

尹进:  
登陆名：CRM

尹进:  
密码：735963fd-e965-410b-9213-e13312cedc11

Get http://128.96.100.119:8090/seeyon/rest/message/all/8942153232870558675?pageNo=1&pageSize=20

http://ip:port/seeyon/rest/message/loginName

‍

[表单触发 · 致远开放平台 (seeyon.com)](http://open.seeyon.com/book/ctp/formTrigger.html)

---

sslvpn  
账号：u8erp  
密码：lyg@U8erp

‍

U8server:172.22.10.230  
U8MSSQL:172.22.10.231  
账号：lyg\u8admin  
密码：lyg@U8erp0122

0132123808

用户：demo1<br />密码：无  
999测试账套

LY0068  
cps333193

‍

开发需求计划

**1**.**1	CRM费用申请单送审到OA审批**

**CRM中填报的费用申请单，业务人员在提交后**，**送审到OA中，使用OA的审批流程进行审批，审批结束后回写CRM单据，改变状态为已审核**。

**1**.**2	 CRM服务请求单送审到OA审批**

**CRM中服务请求单，业务人员在提交后，送审到OA中，使用OA的审批流程进行审批，审批结束后回写CRM单据，改变状态为已审核。**

**1.3	CRM穿透查看OA的表单流程详情**

**费用申请单、服务请求单在提交送审到**OA后，可在CRM系统中查审OA审批流程，以审批流程图或审批进程表方式体现，让提交者能直观看到当前审批状态。

1.4 CRM填报的单据可提交、撤销

**已提交到**OA系统的单据，可以在未终审前撤销回来；

**OA系统中已终审的单据，CRM不可撤销；**

**2**、**销售机会行动触发OA投标申请表**

**1.1 CRM销售机会数据推送到OA**

**CRM中建立销售机会，在销售机会建立保存后，推送到OA底表中，OA表单可以引用到销售机会单据号、客户等内容，并在OA表单中关联销售机会数据ID号。**

**推送字段包含**U8销售机会ID、U8销售机会单据号、客户编码、客户名称、部门、业务员等信息。

**1**.**2 销售机会行动触发OA投标申请表**

**在销售机会阶段升迁到某个阶段，建立一般行动时，当动作选定投标时，在**CRM中点击链接或按钮调用OA投标申请表单，填写OA投标申请表并使用OA表单审批流程，OA表单审批结束后，CRM一般行动完成。

**如果**OA投标申请表没有审批完成，CRM该阶段的一般行动不能完成，阶段不能升迁。

**1）CRM点击链接或按钮单点登录OA的投标申请表表单新建页面；**

**2）投标申请表审批结束后回写CRM一般行动完成动作；**

**3手机端、PC端OA与CRM集成单点登录**

**1)实现在OA系统PC端可以单点登录到CRM系统PC端；**

**2)实现在OA系统M3端可以调出运行CRM系统APP端；**

**3）**合同确认修订下关于M3内嵌，另**明确下**因OA接口问题和需求变更而导致的功能调整乙方免责**。**
