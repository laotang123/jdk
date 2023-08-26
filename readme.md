# centos7编译jdk12虚拟机


* 下载源码
git clone git@github.com:laotang123/jdk.git -b jdk-12+33
* 安装相关依赖
```shell
sudo yum -y groupinstall "Development Tools"

yum install -y java-11-openjdk* autoconf unzip zip alsa-lib-devel libXtst-devel libXt-devel libXrender-devel libXrandr-devel libXi-devel cups-devel freetype-devel fontconfig-devel

```
* 生成makefile
chmod +x configure
./configure --with-debug-level=fastdebug
make all JOBS=4
//JOBS=N可选多线程

* 安装vscode
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
yum check-update
sudo yum install code # or code-insiders

命令行输入 code启动
    * 配置launch.json

```json

{
    "version": "0.2.0",
    "configurations": [        
        {
            "name": "HotSpot Linux Debug",
            "type": "cppdbg",
            "request": "launch",
            "program": "/home/liujf/myopenjdk/jdk12-33/build/linux-x86_64-server-fastdebug/jdk/bin/java",
            "args": ["Hello"],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [
                {"name":"JAVA_HOME","value":"/home/liujf/myopenjdk/jdk12-33/build/linux-x86_64-server-fastdebug/jdk/"},
                {"name":"CLASSPATH","value":".:/home/liujf/myopenjdk/jdk12-33/src/hotspot"}
            ],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "为 gdb 启用整齐打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }
    ]
}
```
program 是jdk编译后的java命令
args是运行参数，Hello是当前工作路径下的Hello.class，注意该文件是编译后的jdk javac产生的
CLASSPATH 是java文件的home路径

参考：https://www.cnblogs.com/mazhimazhi/p/13992240.html
