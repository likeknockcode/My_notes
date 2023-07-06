# Java开发环境

## 目录

-   [下载](#下载)
-   [安装](#安装)
-   [环境变量的配置](#环境变量的配置)
-   [测试是否安装成功](#测试是否安装成功)
-   [开发工具](#开发工具)

## 下载

-   进入[JDK官方地址](https://www.oracle.com/technetwork/java/javase/archive-139210.html "JDK官方地址")，根据自己所需版本下载JDK。

## 安装

-   windows下载完后的是.exe的安装包。双击以后根据安装向导安装即可。

## 环境变量的配置

-   win+x → 系统 → 高级系统设置 → 环境变量&#x20;

    ![环境变量.png](https://s1.ax1x.com/2023/06/08/pCkg3Wj.png "环境变量.png")
- 点击新建 → 输入**变量名**(约定俗称)和**变量值**(jdk安装路径)

  [![pCkIDQe.png](https://s1.ax1x.com/2023/06/08/pCkIDQe.png)](https://imgse.com/i/pCkIDQe)
- 打开Path

  [![pCkIsLd.png](https://s1.ax1x.com/2023/06/08/pCkIsLd.png)](https://imgse.com/i/pCkIsLd)
- 点击新建 → 添加 **%JAVA\_HOME%/bin**

  [![pCkIcdI.png](https://s1.ax1x.com/2023/06/08/pCkIcdI.png)](https://imgse.com/i/pCkIcdI)

  [![pCkIsLd.png](https://s1.ax1x.com/2023/06/08/pCkIsLd.png)](https://imgse.com/i/pCkIsLd)

## 测试是否安装成功

-   win+r 输入cmd打开命令窗口
-   输入**java -version**(注意中间有空格)

    [![pCkIrsH.png](https://s1.ax1x.com/2023/06/08/pCkIrsH.png)](https://imgse.com/i/pCkIrsH)
-   输出以上结果即jdk安装成功

## 开发工具

-   IDEA
-   下载方式（参考如下安装教程）

    <https://blog.csdn.net/qq_43554335/article/details/121928344>
-   创建项目新建测试类
    ```java
    public class hello {
        public static void main(String[] args)
        {
            System.out.println("hello world!");
        }
    }
    ```
-   控制台输出 hello world！运行成功。
