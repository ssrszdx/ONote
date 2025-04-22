# 解决golang超时

# VS Code解决golang编译提示dial tcp 172.217.160.113:443: connectex: A connection attempt failed

在编译go的过程中是不是经常会遇到这样的报错
`module ***: Get "https://proxy.golang.org/***": dial tcp 172.217.160.113:443: connectex: A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond.`

**被墙了，直接在命令行执行走代理。**

```
go env -w GOPROXY=https://goproxy.cn
```
