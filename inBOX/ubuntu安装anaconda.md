本文演示在Ubuntu系统下安装Anaconda3并做相关配置。

步骤总结如下：

1、下载安装包

2、安装

3、配置（环境变量、镜像源）

## 一、下载安装包

1.1、在清华镜像下载Linux版本的anaconda，[清华镜像官网anaconda下载](https://link.zhihu.com/?target=https%3A//mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)，

本人选择的是Anaconda3-2021.11-Linux-x86_64.sh，具体版本可以自行选择，

![](https://pic4.zhimg.com/80/v2-6487291c449917b41271047005924da3_1440w.webp)

下载后，将文件放在Ubuntu系统的某个位置，如下：

![](https://pic3.zhimg.com/80/v2-1d4a710dfc6c02159974590e7dcc5be6_1440w.webp)

## 二、安装

2.1打开终端，并进入到Anaconda3安装包的路径位置，并输入以下命令进行安装：

```text
bash Anaconda3-2021.11-Linux-x86_64.sh
```

如图：

![](https://pic4.zhimg.com/80/v2-7317700afffafb088e2af59327cab25b_1440w.webp)

过程中不断回车，有yes的输入yes，

![](https://pic2.zhimg.com/80/v2-517fb657ba635aca8560fde007bcece5_1440w.webp)

安装成功：

![](https://pic1.zhimg.com/80/v2-e33ae4324fe5e55ef09bc01f2ccb9ca8_1440w.webp)

## 三、配置（环境变量、镜像源）

3.1、配置环境变量

上述安装Anaconda3后，在终端输入，**conda list**的命令，如果成功，说明前面安装过程中已经配置环境变量了，这步骤可以跳过。

但是，如果出现conda命令找不到，说明可能是因为安装过程中没有自动配置环境变量（可能默认设置是no，并不是yes），这是需要我们手动设置环境变量。

![](https://pic3.zhimg.com/80/v2-2c56038d063d84d623edb0e64c7992be_1440w.webp)

在终端输入：**sudo gedit ~/.bashrc**

在文件末尾输入export PATH="**/home/xy/anaconda3**/bin:$PATH"

其中，黑色加粗部分要根据自己的安装路径位置进行更改，如图所示：

![](https://pic2.zhimg.com/80/v2-152dffd07cf9126fc7885727865e880d_1440w.webp)

保存并退出。

然后，在终端输入**source ~/.bashrc**

再次输入conda list或者python命令，进行测试，成功

![](https://pic1.zhimg.com/80/v2-3feab47f676ea6fdb89b2f8c6d6e0aa0_1440w.webp)

![](https://pic1.zhimg.com/80/v2-15ce973f0d1b622aeec9f9554a48824c_1440w.webp)

3.2 pip镜像源设置

Anaconda3包含了pip和conda工具，但是默认下载依赖包是使用的官网的网址进行下载安装，往往网速很慢，这是需要我们分别对pip和conda工具进行设置国内的镜像源。

如图：未设置镜像源是，pip安装第三方包的下载速度很慢。

![](https://pic3.zhimg.com/80/v2-bb28f0e82ac4045f1b1c960264f8e64e_1440w.webp)

这时，我们只需要，在终端执行以下命名，就可以更改镜像源，

```text
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

这里设置的镜像源为清华源，你也可以改成国内其他的镜像源。

再次安装第三方包进行测试，网上杠杠的。

![](https://pic2.zhimg.com/80/v2-621fa24471d08d7b6a780c05d1e346fd_1440w.webp)

3.3conda镜像源设置

在终端执行以下命令：

```text
conda clean -i
sudo gedit ~/.condarc
```

并将以下内容，复制到.condarc文件中

```text
channels:
 - defaults
show_channel_urls: true
default_channels:
 - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
 - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
 - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
 conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
 msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
 bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
 menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
 pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
 pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
 simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

conda 安装第三方包测试：

![](https://pic1.zhimg.com/80/v2-1673200693aa7c2b6b51d1b2d8edb038_1440w.webp)

![](https://pic1.zhimg.com/80/v2-a82b407e04e4b19661fd12cc99b68008_1440w.webp)

当然，conda还有其他的命令，做其他的管理，如虚拟环境的管理。

以上就是在Ubuntu系统下安装Anaconda3并相关配置的操作说明，觉得可以的点点赞。