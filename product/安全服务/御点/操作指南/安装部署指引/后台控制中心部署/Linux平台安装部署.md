
## Linux 环境准备
1. 准备服务器，在服务器中安装其所支持的 Linux 系统（请参见 [1.2.1 环境要求](#1.2.1-环境要求)），本文以 CentOS 7.6 服务器为例进行说明。
2. 服务器接入企业网络、配置 IP、配置防火墙策略。
	-  **配置IP**：请为 CentOS 7.6 配置固定 IP。IP 的取值，根据企业网络 IP 范围，例如10.10.0.0/24，请分配一个未使用的 IP，例如10.12.90.236。
	-  **配置防火墙策略：**配置需要放行的端口，具体参考[1.4.2 基础网络配置](#1.4.2-基础网络配置)。
3. 将 [1.1 授权获取指引](#1.1-授权获取指引) 获取到的安装包，上传到 Linux 系统中。可通过 U 盘挂的方式拷贝，也可通过`rz`命令安装，通过主机安装 xshell 等 ssh 工具，ssh 远程`rz`命令上传。
4. 安装 unzip 命令工具：可通过`yum -y install unzip`命令进行安装。

## 安装过程
1. 把御点 Linux 安装包放到任意路径下，建议规范为：`/apps/svr`。
2. 解压安装包，参考命令如下：
 <img src="https://main.qcloudimg.com/raw/2c8483fe1ec3bf939276d71609a59fa4.png" />
3. 执行安装，解压后得到一个目录 PCMgrEnterprise_SAAS，该目录为源码安装目录，执行以下命令进行安装。
```
cd PCMgrEnterprise\_SAAS
 sh install.sh
```
<img src="https://main.qcloudimg.com/raw/9b5ab2fd0387c8fbda368f47dc29ef0c.png" />
4. 命令执行后会自动执行后台服务的安装：
![](https://main.qcloudimg.com/raw/b5c58d76dba2ca22e714623f3f0b5ce7.png)
5. 安装完成后，可以执行以下命令检测安装是否成功。
```
supervisorctl status
```
若进程列表中都显示为 “Running”状态，表示安装成功且运行正常，如下图所示：
 ![](https://main.qcloudimg.com/raw/9fd310acea6623b1e17f3da7b3944662.png)
 如果要重启服务，可以执行`supervisorctl service restart`，即可将御点服务全部重启。
6. 安装完成后，可通过浏览器输入产品安装所在服务器的地址，即可进入控制中心 Web 页面。
例如，安装服务器的 IP 地址是10.12.90.236，则在浏览器地址栏中输入`http://10.12.90.236`，单击【回车】，出现以下登录页面，使用默认出厂帐号：admin，密码：admin，进行登录。具体登录链接，参考 [2.2.1 登录链接](#2.2.1-登录链接)。
![](https://main.qcloudimg.com/raw/df50eb3cb7bf7a383b3e8da4e6f40f65.png)

## 授权导入
初次登录系统，会提示导入证书（证书申请，请参考[1.1 授权获取指引](#1.1-授权获取指引)）。
![](https://main.qcloudimg.com/raw/8d01ead486b3d0abd8a28868834132d6.png)

当证书导入成功后，在页面右上角【产品信息】中，将展示证书详情。
![](https://main.qcloudimg.com/raw/ac5d5772bfad2af141402cbbbc6ee2f1.png)
## 卸载过程

如果不需要本系统，可执行卸载命令。卸载分为部分删除（不清理配置）和全部删除。
  - 部分删除命令：
<img src="https://main.qcloudimg.com/raw/1160d3a54a416f8fa2c9607aa7a960f5.png"  />
  - 全部删除命令
  ![](https://main.qcloudimg.com/raw/4dde468b5f4de7e768496785bc42e42e.png)
