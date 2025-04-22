# cnpm报错  无法加载文件 .ps1，因为在此系统上禁止运行脚本

下载使用cnpm时出错，
cnpm : 无法加载文件 C:\Users\Administrator\AppData\Roaming\npm\cnpm.ps1，因为在此系统上禁止运行脚本

主要原因时没有执行可用脚本
解决：

以管理员身份运行power shell
输入set-ExecutionPolicy RemoteSigned

输入A 回车
再次输入cnpm -v就可以运行了
