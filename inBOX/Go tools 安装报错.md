# [golang环境问题——vscode中安装go插件报错、打开go文件总弹出install提示](https://www.cnblogs.com/wsablog/p/16143098.html)

## 先设置系统环境变量：

**GOPATH  工作目录    放自己项目的地方  比如 D:\goproject，然后自己创建三个文件夹bin、pkg、src**

## GOROOT  安装目录   比如 D:\go

**然后加Path   %GOROOT%\bin**

## go env -w GO111MODULE=on

## go env -w GOPROXY=https://goproxy.cn,direct

## 重新启动 VSCode

重装 go 插件，或者点击 `Install All`，提示`SUCCESS`即可。在无弹窗