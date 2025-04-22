# 请求被中止: 未能创建 SSLTLS 安全通道

# .NET HttpWebRequest（请求被中止: 未能创建 SSL/TLS 安全通道）和（基础连接已经关闭: 发送时发生错误）问题查找解决

## 前言：

　　前段时间在对接第三方接口的时候发生了一个非常奇葩的问题，就是使用 .NET Framework 4.6 HttpWebRequest进行网络请求的相关问题。背景，关于调用第三方的接口都是使用使用自己封装的一个HttpWebRequestHepler帮助类，在本地开发时调用第三方接口都是正常的。然而当我部署到运维给我一个服务器（阿里云服务器）时刚开始提示是**请求被中止: 未能创建 SSL/TLS 安全通道，**之后经过一番修改以后就是提示**基础连接已经关闭: 发送时发生错误。**之后尝试了各种方法，还是没有办法解决**基础连接已经关闭: 发送时发生错误**这个问题。最后真的是无能为力，光这个问题找了一下午的解决方案，最后换到了我自己的阿里云服务器是可以正常调通第三方接口的。然后让运维看了下服务器结果是这个服务器都没有​**开通外网**​，所以导致了这个问题的出现。下面记录下问题排除的过程，希望能够帮助到遇到这种坑的小伙伴。

## 一、自己封装的一个通用的HttpWebRequestHepler Http Web网络请求帮助类：

```text
 /// <summary>
    /// Http Web网络请求帮助类
    /// </summary>
    public class HttpWebRequestHepler
    {
        private static HttpWebRequestHepler _httpWebRequestHepler;
        private string _resContent;//响应内容
        private string _errInfo;//错误信息
        private int _responseCode;//响应状态码

        public static HttpWebRequestHepler _
        {
            get => _httpWebRequestHepler ?? (_httpWebRequestHepler = new HttpWebRequestHepler());
            set => _httpWebRequestHepler = value;
        }

        /// <summary>
        /// 数据请求
        /// </summary>
        /// <param name="requestUrl">请求地址</param>
        /// <param name="postData">请求参数</param>
        /// <param name="accessToken">授权token</param>
        /// <param name="contentType">请求标头值类型</param>
        /// <param name="method">请求方式</param>
        /// <returns></returns>
        public string HttpWebResponseData(string requestUrl, string postData, string accessToken = "", string contentType = "application/json", string method = "POST")
        {
            HttpWebResponse wr = null;

            try
            {
                var hp = (HttpWebRequest)WebRequest.Create(requestUrl);
                hp.Timeout = 60 * 1000 * 10;//以毫秒为单位，设置等待超时10分钟
                hp.Method = method;
                hp.ContentType = contentType;
                if (!string.IsNullOrWhiteSpace(accessToken))
                {
                    hp.Headers.Add("Authorization", "Bearer " + accessToken);//增加headers请求头信息
                }


                if (postData != "")//带参数请求
                {
                    byte[] data = Encoding.UTF8.GetBytes(postData);
                    hp.ContentLength = data.Length;
                    Stream ws = hp.GetRequestStream();

                    // 发送数据
                    ws.Write(data, 0, data.Length);
                    ws.Close();
                }

                wr = (HttpWebResponse)hp.GetResponse();
                var sr = new StreamReader(wr.GetResponseStream() ?? throw new InvalidOperationException(), Encoding.UTF8);

                this._resContent = sr.ReadToEnd();
                sr.Close();
                wr.Close();
            }
            catch (Exception exp)
            {
                this._errInfo += exp.Message;
                if (wr != null)
                {
                    this._responseCode = Convert.ToInt32(wr.StatusCode);
                }

                return this._resContent;
            }

            this._responseCode = Convert.ToInt32(wr.StatusCode);

            return this._resContent;
        }

    }
```

## 二、请求被中止: 未能创建 SSL/TLS 安全通道问题解决：

　　把项目部署到阿里云服务器中，请求第三方提示请求被中止: 未能创建 SSL/TLS 安全通道。首先字面上可以看出来这个https请求安全协议的问题。微软官方说明是,NET 4.6需要添加

[ServicePointManager.SecurityProtocol属性](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/dotnet/api/system.net.servicepointmanager.securityprotocol%3Fview%3Dnet-5.0)，指定[schnanel安全包支持的安全协议](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/dotnet/api/system.net.securityprotocoltype%3Fview%3Dnet-5.0%23System_Net_SecurityProtocolType_SystemDefault)。

微软官方解释：

> 此属性选择要用于新连接的安全套接字层 (SSL) 或传输层安全性 (TLS) 协议的版本;不会更改现有连接。  
> 从 .NET Framework 4.7 开始，此属性的默认值为 [SecurityProtocolType.SystemDefault](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/dotnet/api/system.net.securityprotocoltype%3Fview%3Dnet-5.0%23System_Net_SecurityProtocolType_SystemDefault) 。 这允许基于 [SslStream](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/dotnet/api/system.net.security.sslstream%3Fview%3Dnet-5.0) (（如 FTP、HTTP 和 SMTP) ）的 .NET Framework 网络 api 从操作系统或系统管理员执行的任何自定义配置继承默认安全协议。 有关默认情况下在每个版本的 Windows 操作系统上启用了哪些 SSL/TLS 协议的信息，请参阅 [TLS/SSL (SCHANNEL SSP) 中的协议 ](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/windows/win32/secauthn/protocols-in-tls-ssl--schannel-ssp-)。  
> 对于通过 .NET Framework 4.6.2 的 .NET Framework 版本，不会列出此属性的默认值。 安全环境不断变化，默认的协议和保护级别会随着时间的推移而更改，以避免已知的漏洞。 默认值因单独的计算机配置、已安装的软件和应用的修补程序而异。

解决方案：


```csharp
//指定请求包的安全协议，因为不知道你当前项目到底是哪个版本所以为了安全保障都加上 
ServicePointManager.SecurityProtocol = SecurityProtocolType.Ssl3 | SecurityProtocolType.SystemDefault | SecurityProtocolType.Tls | SecurityProtocolType.Tls11 | SecurityProtocolType.Tls12 | SecurityProtocolType.Tls13;
```

​![](https://pic3.zhimg.com/80/v2-180156671760cf566dd9aab96245c28e_1440w.webp)​

## 三、基础连接已经关闭: 发送时发生错误

这个问题查阅了网上几个比较典型的博客试了下，结果都没有办法解决我的问题，一下记录下这几个博客的解决方案，希望可以帮助到遇到这样问题的小伙伴。

1、一般来说添加了上面的[ServicePointManager.SecurityProtocol属性](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/dotnet/api/system.net.servicepointmanager.securityprotocol%3Fview%3Dnet-5.0)就可以解决这个基础连接关闭的问题。

2、C# HttpRequest基础连接已经关闭: 接收时发生意外错误（[原文地址](https://link.zhihu.com/?target=https%3A//blog.csdn.net/liehuo123/article/details/7071636%3Futm_medium%3Ddistribute.pc_relevant_t0.none-task-blog-2%257Edefault%257EBlogCommendFromMachineLearnPai2%257Edefault-1.control%26dist_request_id%3D%26depth_1-utm_source%3Ddistribute.pc_relevant_t0.none-task-blog-2%257Edefault%257EBlogCommendFromMachineLearnPai2%257Edefault-1.control)）：

```text
//增加下面两个属性即可 
hp.KeepAlive = false; 
hp.ProtocolVersion = HttpVersion.Version10; 
```

## 四、开启阿里云服务器外网（我的解决方案）

　　查看一下你的服务器是否开通了外网，假如没有开通服务器外网在进行尝试。[阿里云服务器配置外网访问参考](https://link.zhihu.com/?target=https%3A//blog.csdn.net/weixin_43122090/article/details/103548956)。因为这个奇葩问题花费了一天宝贵的时间，考虑问题还是得多方面考虑。
