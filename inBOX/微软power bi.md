Power BI Desktop的4个插件：

1、power query 你可以理解为数据仓库，ETL技术，抽取（extract）、转换（transform）、加载（load）至目的端的过程，简单的说就是清洗数据的

2、power pivot：数据分析的工具，你可以理解为内置透视表的加强版

3、power map：很强大的地图工具，可以将分析数据以图的形式按区域、时间、数值等多个维度的数据以图表（包括3d动画）的形式展现出来，so nice，excel开始烧显卡了。

4、power view：可以快速创建各种数据可视化对象，从表和矩阵到条形图、柱形图和气泡图以及多个图表的图表，快速形成商业报告

1-2-3或者1-2-4几个组合在一起，一条龙作业，商业智能分析一人完成


可从各种数据源获得数据，进行清洗。包括各种数据库、网络服务、各种格式的[本地文件](https://www.zhihu.com/search?q=%E6%9C%AC%E5%9C%B0%E6%96%87%E4%BB%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1764517943%7D)等。

支持的数量总容量要比工作表大，07版工作表上限是1048576行，[powerquery](https://www.zhihu.com/search?q=powerquery&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1764517943%7D)的上限可能是上亿行。

具有[网络爬虫](https://www.zhihu.com/search?q=%E7%BD%91%E7%BB%9C%E7%88%AC%E8%99%AB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1764517943%7D)功能，能抓捕一些网页上的格式化信息，使用函数指定页码等。

合并多[工作簿](https://www.zhihu.com/search?q=%E5%B7%A5%E4%BD%9C%E7%B0%BF&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1764517943%7D)或多工作表。之前需要使用vba代码合并，pq合并这些这些簿、表十分方便，而且支持父子文件夹。在工作表中可使用函数获得动态路径，这样转移文件夹后也可自动合并。

支持一维表、二维表的相互转换。之前使用vba转换这类表格代码量较大，中国式报表中的特殊格式比较多，使用pq转换会方便一些。

常用步骤全部图形化，手动操作后自动记录，每个步骤可查看效果，可删除、添加步骤，完成后只需要添加数据，刷新即可获得最新数据。如图形化功能不足以满足效果，可在高级编辑器中直接修改函数。也可自行制作自定义函数，多次调用。

常用功能为合并表格、填充、去重、筛选、自定义列、分类汇总、分列、[合并列](https://www.zhihu.com/search?q=%E5%90%88%E5%B9%B6%E5%88%97&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1764517943%7D)、转置列、合并行、反转行、查找替换（不支持[正则表达式](https://www.zhihu.com/search?q=%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1764517943%7D)）、条件列、透视、逆透视、[合并查询](https://www.zhihu.com/search?q=%E5%90%88%E5%B9%B6%E6%9F%A5%E8%AF%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1764517943%7D)、各种格式转换。

函数必须严格大小写，数值格式要求非常严格。

主要数据表现方式包括list、record、table，list是列，record是行，两者构成table，单元格是field。list、record、table可相互转换，在函数中可截取、合并、替换、[增删列行](https://www.zhihu.com/search?q=%E5%A2%9E%E5%88%A0%E5%88%97%E8%A1%8C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1764517943%7D)等。

格式包括text、number、datetime、date、duration、time、datetimezone等。

常用函数800多个。常用功能已图形化，高级功能比较难，需要理解其基本逻辑。

合并查询类似sql中的各种join，支持模糊匹配。

可横向、纵向合并表格。

支持[递归](https://www.zhihu.com/search?q=%E9%80%92%E5%BD%92&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1764517943%7D)、迭代、循环等。

对于容量较大的数据，pq主要用于[数据清洗](https://www.zhihu.com/search?q=%E6%95%B0%E6%8D%AE%E6%B8%85%E6%B4%97&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1764517943%7D)，加载到[数据模型](https://www.zhihu.com/search?q=%E6%95%B0%E6%8D%AE%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1764517943%7D)后，使用powerpivot的dax功能汇总。有些汇总在pq中也可以实现，一般需要自行调整函数。