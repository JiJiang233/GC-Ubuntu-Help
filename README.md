Ubuntu下Grasscutter的使用
---

![](https://img1.imgtp.com/2022/07/24/4kmAUzMP.png)

选用ubuntu 21.10 live server 环境为虚拟机 2核2线 内存4G 硬盘30G

我选用的软件：  
虚拟机：VMware® Workstation 16 Pro  
SSH：MobaXterm v22  
Java：18  
python：3.9
-
第一步：安装Java 18
-
```
sudo apt install openjdk-18-jdk  
#通过apt安装jdk

java -version
#验证操作的有效性
```
第二步：安装mongodb
-
```
wget https://repo.mongodb.org/apt/ubuntu/dists/focal/mongodb-org/6.0/multiverse/binary-amd64/mongodb-org-server_6.0.0_amd64.deb
# 下载

sudo dpkg -i mongodb-org-server_6.0.0_amd64.deb
#安装
```
第三步：clone仓库并编译
-
```
git clone https://github.com/Grasscutters/Grasscutter.git
#clone仓库

cd Grasscutter
#切换目录

chmod +x gradlew
#改变权限

./gradlew jar 
# 编译

chmod 777 编译生成好的文件名
#改变权限
```
第四步：clone GC 所需要的资源  
-
```
git clone https://github.com/Koko-boya/Grasscutter_Resources ./resources
#clone仓库
```
第五步：使用mitmdump进行代理
-
```
sudo apt install python3-pip
#安装包

pip install mitmdump -i https://pypi.tuna.tsinghua.edu.cn/simple
#安装pip
```
第六步：清理垃圾（适用于强迫症用户）
-
```
sudo vim delete.sh
```
- 直接写一个小脚本，直接复制下面这些行命令并粘贴，删除完还能自毁，多方便
```
rm -rf ../jdk-18_linux-x64_bin.tar.gz
rm -rf ../mongodb-linux-x86_64-ubuntu2004-6.0.0.tgz
rm -rf build
rm -rf gradle
rm -rf install.sh
rm -rf README_es-ES.md
rm -rf README_zh-CN.md
rm -rf start_config.cmd
rm -rf build.gradle
rm -rf gradlew
rm -rf README_fr-FR.md
rm -rf README_zh-TW.md
rm -rf CODE_OF_CONDUCT.md
rm -rf gradlew.bat
rm -rf README_HE.md
rm -rf settings.gradle
rm -rf CONTRIBUTING.md
rm -rf LICENSE       
rm -rf README.md
rm -rf src
rm -rf format_whitespace.py
rm -rf Grasscutter_Resources
rm -rf manage_languages.py
rm -rf __pycache__
rm -rf README_ru-RU.md
rm -rf start.cmd
rm -rf delete.sh
```
```
粘贴后按“Esc”键进入命令模式并输入“:wq”保存退出

./delete.sh
#执行
```
第七步：运行
-
```
mongod --dbpath /var/lib/mongo --logpath /var/log/mongodb/mongod.log --fork
#运行MongoDB

mitmdump -s proxy.py -k
#启动代理，使用"Ctrl Z"键停止程序

bg 1
#后台入驻代理

java -jar 编译好的Jar文件
```
第八步：持续运行
-
```
sudo apt install screen
#安装包

screen -S GC
#创建并运行第七行相同命令

如需恢复
screen -r GC
```
