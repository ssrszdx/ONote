# CRM布局的学习

1.php构造函数

php最终都是输出HTML

public function _construct(){}

2.TGetRequest() 获取post/get变量名的值（Tpagecache类 页面缓存类）

preg_match() 正则表达式匹配

tRegisterID, tGetRegID 序列化和反序列化字符串

3.Zend studio 无法识别的文件在General -Content type里面设置。

4.在php manual中，[] 比如你的方法有几个参数，当某个参数是用中括号包装的，则说明这个参数可有可无

5.php重写基类的方法

public functionlayout()//与父类中的方法同名

     {  //code

       parent::layout();

     }

6.通过写事件时间日志去发现问题。

TUIPage:页面输出类，输出页面的Header,Body以及一些其它公共函数，在生成Body时会调用TLayout类的方法生成主页面内容。代码如下：

$page = new TUIPage();

echo $page->GetHeader("/js/tlist.js;/js/tquery.js");

echo $page-&gt;GetBody($layout);

echo $page->GetTail();

7.webgrind使用要重启apache服务

TLayout:布局基类，定义了一个布局所需要的元素及其相关方法：如Title,Button、Toolbar、窗口左侧、右侧、主窗口内容等。子类需要实现其layout()方法完成布局，同时也可以重写本类方法修改元素显示。

TObjectLayout:标准对象布局基类：定义了一个标准对象在LIST、EDIT、VIEW布局时所用到的公共方法，主要是对象权限的相关方法（是否可读、可写、可导出、可导入等）。其子类包括：TListLayout、TViewLayout、TEditLayout，这三种对象布局将在”标准页面布局”中详细说明。

TConfigLayout:非对象的布局基类：重写了TLayout的layout()方法对布局元素进行了组织，子类需要实现其layoutMainInfo()方法完成主页面布局，并提供了一些其它公共方法(如按扭、标题等)。其中的某些子类对象将在”其它布局”中详细说明。

TLayoutInfo、TLayoutTable、TLayoutCell：页面的布局基础元素，系统的标准对象VIEW、EDIT页面布局可以在KEY中进行配置，同时也可以手动组织这三个类完成VIEW和EDIT页面布局。页面显示同TEditLayout和TViewLayout。

标准对象布局包括：TListLayout、TEditLayou、TViewLayout三种,其页面数据的显示分别在tlist.js、tedit.js、tview.js进行了详细的定义，在使用这三种布局时，只需要对显示数据进行组织，其中：

     TListLayout: 其数据依赖于对象的$model的listAttr和getDatalist()方法。

     TEditLayout: 其数据依赖于对象的”KEY-查看页面布局”和$model的getDataObject ()方法。

     TViewLayout:其数据依赖于对象的”KEY-编辑页面布局”和$model的getDataObject ()方法。

* 数据访问模型定义文件, 下面以常规对象模型说明模型定义需要完成的功能
* ‍
* 相关概念:
* 1) 主对象:　指查询模型的核心对象
* 2) 关联对象: 指直接关联的对象, 该对象的属性可能会用于列表,或者做为查询条件
* 4) 反向关联对象:　指与主对象为一对多关系的关联对象,其属性只能做为查询条件
* 5) 自动关联对象: 指没有在模型定义里出现, 但由主对象或者关联对象的属性引用的对象(USE不自动关联)
* 7) 多态关联: 指一个关联属性关联到多种对象,即关联的对象类型不确定
* 8) 对象主表: 指该对象的主要信息表
* 9) 对象主属性:　指代表该对象的属性，如Account.Name
* 10) 组合字段: 底层不再考虑组合字段的情况,组合字段做为一种常规字段保存
* 模型提供如下功能:
* 1. 模型定义
* 1) 主对象定义:　 通过模型的对象类型声明主对象
* 2) 关联对象的定义:
* 属性前缀: 表示该关联对象相对于主对象的属性前缀
* 关联条件: 关联条件允许三种方式的组合
* a) 本对象的属性, 关联到对象的属性
* b) 本对象的属性, 值常量
* c) 关联到对象的属性, 值常量
* 属性作用: 显示/条件/显示和条件
* 3) 反向关联对象的定义:
* 反向关联通常以一棵反向关联树的形式出现, 即返向关联本身是一个完整的关联关系,
* 也分为主对象和关联对象, 因些该反向关联树的定义与1)和2)类似, 只不过主对象定义时,
* 需要指明反赂关联主对象与模型主对象之间的关联字段,即:
* 反向关联主对象定义需要说明:
* a) 反向关联主对象类型
* b) 反向关联主对象的关联属性
* c) 模型主对象或关联对象的关联属性
* 4) 自动关联关系:

  1) 通过属性的引用, 直接加入模型中 (USER对象不加入)

  2) 单选枚举值排序时, 自动关联到枚举值表

  3) 自定义引用, 自动关联
* 5) 多态关联对象:
* 只能使用该关联对象的主属性作为显示字段或者查询条件，这时要求数据字典中声明对象类型属性
* 然后由模型统一实现, 多态引用要求有三个属性, 名字分别统一为: Xxx, XxxID, XxxType(如:ReferTo, ReferToID, ReterToType)
* 同时要求多态引用属性的 ReferObjName 值为 ReferObject (通用引用对象)
* ‍
* 2. 根据模型定义生成QueryMaker(查询构造器)
* 1)主对象及其关联对象构成一个主QueryMaker
* 2)每一反向关联对象树构造一个反向QueryMaker
* 3. 根据模型定义得到可用属性
* 1) 可列表属性=由主对象可以列表的属性 + 可以显示的关联对象的可以列表并能在其它对象上显示的属性
* 2) 可查看属性=由主对象可以查看的属性 + 可以显示的关联对象的可以查看并能在其它对象上显示的属性
* 3) 可编辑属性=由主对象可以编辑的属性
* 4) 可做为直接条件的属性=由主对象可以做为条件的属性 + 可以作为条件的关联对象上的可以做为条件的属性
* 5) 可做为反向条件的属性=反向关联树中可以做为条件的属性
* 4. 建立属性与QueryMaker之间的映射关系
* 模型的属性由下列属性构成:
* 直接属性:　主对象及关联对象的所有属性
* 反向关联属性: 反向关联树中的属性
* 属性映射关系:
* 属性全名=> 表别名, 字段的别名, QueryMake的引用
* 5. 根据开关决定SQL语句的生成, 有下列开关信息:
* 1) 是否排除删除记录, 即是否在所有的对象主表中加入is_deleted=0的条件
* 2) 是否只查询删除记录, 即要在主对象的主表中加入is_deleted=1条件
* ‍
* 6. 为查询加入数据权限
* 7. 生成查询SQL
* 8. 将查询结果转化为对象输出
* 9. 支持非对象的表查询 TODO

除标准对象外，系统也定义了几种常用的非标准对象布局，在使用这几种布局时，需要继承相对应的Object对象，对显示列和显示数据按照一定的格式进行组织。

 BaseListLayout

基本列表布局类，数据对象基类为：BaseListObject，用于非对象的数据列表界面显示；Ext控件为：Ext.grid.GridPanel()

 BaseSelectListLayout

双选择框布局，数据对象基类为：BaseListObject，Ext控件为：Ext.form.FormPanel({xtype:itemselector})

 BaseColumnTreeLayout

树状列表布局，数据对象基类为：BaseListObject，Ext控件为：Ext.tree.ColumnTree()；

BaseEditListLayout

可编辑列表布局，数据对象基类为：BaseListObject，Ext控件为：Ext.grid.EditorGridPanel()；

BaseTabColumnTreeLayout

多页签树状列表布局，数据对象基类：BaseTabListObject；Ext控件：Ext.TabPanel、Ext.tree.ColumnTree；

BaseTabListLayout

多页签列表布局, 数据对象基类：BaseTabListObject；Ext控件：Ext.TabPanel、Ext.grid.GridPanel；

BaseEditLayout

基本编辑对象，数据对象基类：BaseEditObject，EXT控件：Ext.form.FormPanel,

**公用页面**

CRM系统中的一个主对象, 在查看时 , 可以显示关联的其它对象的列表 , 这些关联的其它对象列表, 称为相关对象

相关对象定义涉及两个表 :

       tc_user_rel_profile: 记录了一个对象具有相关对象有哪些

       tc_user_rel_condition: 记录了主对象与相关对象之间采取什么样的过滤条件

![graphic](08de5b380f7ded9c6108abaee498b415-20220206111702-tq0kwhg.png)
