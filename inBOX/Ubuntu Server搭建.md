## 1. 基础操作

sudo apt install net-tools
sudo apt install openssh-server

sudo apt install openssh-client

sudo apt update
sudo apt upgrade

sudo apt remove [卸载某软件名称]

sudo apt autoclean && sudo apt autoremove

## 2. 硬盘扩容

 df -h #看一下挂载的信息
 用指令显示存在的卷组，vgdisplay
 Free PE / Size 6144/ <24.00 GiB 这是还可以扩充的大小，然后输入指令扩容

sudo fdisk -l  //fdisk 分区工具 sudo fdisk /dev/sda

`sudo parted -l`，可查看所有磁盘的分区情况。

用于查看磁盘和分区的使用情况，如 `lsblk`，可以列出磁盘设备、分区、挂载点等信息

sudo lvextend -L +20G /dev/mapper/ubuntu--vg-ubuntu--lv     //增加20G 

sudo lvextend -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv   //增加所有

resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv            //执行调整

## 3. 更换下载源

### 方法一：

https://linuxmirrors.cn/

sudo -i  在root用户下执行命令

### 方法二

从 Ubuntu 24.04 开始，Ubuntu 的软件源配置文件变更为 DEB822 格式，路径为 /etc/apt/sources.list.d/ubuntu.sources。

```bash
sudo vim /etc/apt/sources.list.d/ubuntu.sources
```

将原有的第一段配置修改为如下：

```text
Types: deb
URIs: https://mirrors.tuna.tsinghua.edu.cn/ubuntu
Suites: noble noble-updates noble-backports
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

## 4. 安装docker

1. **更新软件包索引**：
   `sudo apt update`
2. **安装所需的依赖项**：
   `sudo apt install lsb_release apt-transport-https ca-certificates curl software-properties-common`
3. **添加Docker的官方GPG密钥**：
   `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`
4. **添加Docker的APT存储库** 修改为清华来源
   `echo \ "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu \ $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`                                                                                           echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
5. 再次更新索引，安装docker
   `sudo apt update`

   sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

   验证Docker安装
   Docker 可能需要重启后才能正确运行。重启后，运行以下命令来验证 Docker 是否正确安装：
   `sudo docker run hello-world`

   **管理Docker服务**： 使用以下命令来管理 Docker 服务：

   sudo systemctl start docker

   sudo systemctl stop docker

   sudo systemctl status docker
   sudo systemctl enable docker   //使Docker服务开机自启

   **非root用户运行Docker**： 将用户添加到 `docker` 组，以便无需 `sudo` 运行 Docker 命令：

   `sudo usermod -aG docker your-username`

   然后，用户需要注销并重新登录，或者重启系统以使这些更改生效。
6. **配置Docker镜像加速和代理**     编辑或创建 `/etc/docker/daemon.json` 文件，并添加以下内容：
   `{   "registry-mirrors": [     "https://mirrors.tuna.tsinghua.edu.cn/docker-ce"   ] }`

   之后，重启Docker服务：

   `sudo systemctl daemon-reload  sudo systemctl restart docker`

   此外，`systemd`也会从 `/etc/systemd/system/docker.service.d`和 `/lib/systemd/system/docker.service.d`文件夹下读取配置，所以可以再其中一个文件夹中创建一个名为 `http-proxy.conf`的文件用来保存代理信息。内容如下：

   ```bash
   [Service]
   Environment="HTTP_PROXY=http://192.168.31.166:7890"
   Environment="HTTPS_PROXY=http://192.168.31.166:7890"
   Environmeng="NO_PROXY=<registry.domain>"
   ```

## 5. 配置git ssh

ssh-keygen

## 6.安装node

curl -fsSL https://deb.nodesource.com/setup_20.x | sudo bash -
sudo apt-get install -y nodejs

## 7.安装gcc

sudo apt install build-essential

## 8.安装nginx

sudo apt install nginx

## 9.安装mysql

sudo apt install mysql-server

## 10. 创建project 文件夹

sudo mkdir project

sudo chmod 777 project

sudo apt install tree 

## 11. 安装python

sudo apt install python3-pip

sudo apt install python3


## 配置系统代理

编辑 `~/.bashrc`或 `~/.profile`文件，添加上述 `export`行，然后保存并重新加载配置文件或重新登录，以使更改生效：

`echo 'export http_proxy="http://192.168.31.166:7890"' >> ~/.bashrc `

`echo 'export https_proxy="http://192.168.31.166:7890"' >> ~/.bashrc `

echo 'export noproxy="unset http_proxy; unset https_proxy"'`>> ~/.bashrc`

`source ~/.bashrc`

alias proxy='export http_proxy="http://192.168.31.166:7890"; export https_proxy="http://192.168.31.166:7890"' alias noproxy='unset http_proxy; unset https_proxy'

source ~/.bashrc  更改生效

## 安装brew

1. **安装依赖**：安装Homebrew需要一些依赖，运行以下命令来安装它们：

   bash

   `sudo apt install build-essential curl file git`
2. **安装Homebrew**：使用curl命令从Homebrew的官方网站下载并执行安装脚本：

   bash复制

   `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
3. **添加Homebrew到你的PATH**：安装完成后，你可能需要将Homebrew的安装路径添加到你的PATH环境变量中。你可以通过在你的 `~/.bashrc`或 `~/.zshrc`文件中添加以下行来做到这一点：

   bash

   `export PATH="/home/linuxbrew/.linuxbrew/bin:/home/linuxbrew/.linuxbrew/sbin:$PATH"`

   然后，运行以下命令来使更改生效：

   bash

   `source ~/.bashrc`

   API URL: http://127.0.0.1:54321
   GraphQL URL: http://127.0.0.1:54321/graphql/v1
   S3 Storage URL: http://127.0.0.1:54321/storage/v1/s3
   DB URL: postgresql://postgres:postgres@127.0.0.1:54322/postgres
   Studio URL: http://127.0.0.1:54323
   Inbucket URL: http://127.0.0.1:54324
   JWT secret: super-secret-jwt-token-with-at-least-32-characters-long
   anon key: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZS1kZW1vIiwicm9sZSI6ImFub24iLCJleHAiOjE5ODM4MTI5OTZ9.CRXP1A7WOeoJeXxjNni43kdQwgnWNReilDMblYTn_I0
   service_role key: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZS1kZW1vIiwicm9sZSI6InNlcnZpY2Vfcm9sZSIsImV4cCI6MTk4MzgxMjk5Nn0.EGIM96RAZx35lJzdJsyH-qQwv8Hdp7fsn3W0YpN81IU
   S3 Access Key: 625729a08b95bf1b7ff351a663f3a23c
   S3 Secret Key: 850181e4652dd023b7a98c58ae0d2d34bd487ee0cc3254aed6eda37307425907
   S3 Region: local

## docker token

https://app.docker.com/settings/personal-access-tokens
docker login -u dxpkuer
dckr_pat_wyLNkSdwlO4arv-r_IJZ1FWhkV4
