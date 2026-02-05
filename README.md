# -ssh-
配置实验室电脑ssh跳板机转发
参考教程：
'https://www.cnblogs.com/XiiX/p/15095135.html'

学校虚拟机网址：
`https://vlab.ustc.edu.cn`


现在有三台机器：实验室电脑A,跳板服务器B,移动笔记本C
## Step1:
登陆网址，创建虚拟机

## Step2:
A上输入：
```
ssh -fCNR port:localhost:22 ubuntu@vlab.ustc.edu.cn
```
port 可以是1332，1331，1321

## Step3:
测试是否成功，在B上输入：
```
ssh -p 1332 name@localhost
```
这里我的name是assac(A的name)

## Step4:
在B上输入：
```
sudo sh -c "echo 'GatewayPorts yes' >> /etc/ssh/sshd_config"
sudo service ssh restart
```
#GatewayPorts yes确保外网也能访问这个1321这个监听端口，而不是只能被localhost访问。

## Step5:
现在可以在C上测试一下：
先输入：
```
ssh ubuntu@vlab.ustc.edu.cn
```
然后输入：
```
ssh -p 1332 assac@localhost
```


## vscode config配置：
先在C上打开powershell
输入：
```
ssh -L 8888:127.0.0.1:1332 ubuntu@vlab.ustc.edu.cn
```
然后输入学号登陆
#把C的 8888 端口，通过网关，直接连到网关内部的 1332 端口

然后在C上打开vscode，连接这个config对应的ssh即可：
```
Host my-final-target
    HostName 127.0.0.1
    Port 8888
    User assac
```
#电脑的 8888 端口，其实就是目标机器的 SSH 端口了。我们让 VS Code 连自己

请开始使用吧！











