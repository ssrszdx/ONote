# Regsvr32和Regasm注册DLL COM组件

普通DLL是不用注册嘀. 只有com组件才需要注册,注册时他把clsid和progid(可选)及DLL的路径写到注册表中. 于是用这些组件的客户端在创建该组件时就不用知道他的路径. 只需调用cocreateinstance并传入clsid,系统就能创建该组件的实例了.

由于本人今天在使用C#创建COM组件的时候使用regsvr32来注册自己创建的组件报错![注册.NET的COM组件](0.5792359100839981-20220830214046-q5wkmpy.png)  
但是使用VS自带的工具使用命令regasm path却可以成功的注册自己创建的.NET组件，感到非常奇怪。于是上网搜索。得到下面的解释：  
使用regasm.exe来注册.NET 组件，regsvr32并不适用于.NET组件。 （或者您也可以将regasm.exe理解为.NET的regsvr32） 希望能对您有所帮助！ ====================== - 微软全球技术中心 本贴子仅供CSDN的用户作为参考信息使用。其内容不具备任何法律保障。您需要考虑到并承担使用此信息可能带来的风险。具体事项可参见使用条款([http://support.microsoft.com/directory/worldwide/zh-cn/community/terms_chs.asp](http://support.microsoft.com/directory/worldwide/zh-cn/community/terms_chs.asp))。

总结：根据上面的回答可以知道regsvr32不适用于.NET的组件的注册，感觉意思是对于非托管的代码使用regsvr32来注册，对于托管的代码使用regasm来注册。
