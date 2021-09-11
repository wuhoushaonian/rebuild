# 自用萌咖Windows VPS一键重装为Linux系统，Windows DD Linux
**本方法适用于主机商只提供 Windows 系统，有或者无 VNC，有或者无 DHCP ，有或者无可视化桌面的 Windows VPS 需要重装为 Linux 的场景**<br />
在这种方法中 VNC 存不存在并不重要，因为不需要在 VNC 中操作什么，VNC 可以用来观看 DD 进度，并无其它作用<br />
## 下面介绍有 DHCP 的 DD 步骤：
### Windows 有桌面
有桌面的情况下首先下载 3 个文件到 Windows 上：[win32loader.bat](https://raw.githubusercontent.com/wuhoushaonian/rebuild/main/win32loader.bat)、[initrd.img](https://raw.githubusercontent.com/wuhoushaonian/rebuild/main/initrd.img)、[vmlinuz](https://raw.githubusercontent.com/wuhoushaonian/rebuild/main/vmlinuz)<br />
右键管理员打开 win32loader.bat，按 2 选择 Local file ，然后把刚才下载的 initrd.img 和 vmlinuz 放在 C:win32-loader 目录下，然后可以回车确认开始 dd 了<br />
![b7e477d765cbc97-160](https://user-images.githubusercontent.com/74554257/132939319-ca7e5fb8-e325-49c7-8f14-2b87401edfb2.png)<br />
### Windows 无桌面
**无桌面就需要在命令提示符中使用命令行下载文件，打开文件（后文附有PowerShell 的基本使用方法）**<br />
在PowerShell中输入以下命令<br />
```text
#下载程序
Invoke-WebRequest -Uri ‘https://raw.githubusercontent.com/wuhoushaonian/rebuild/main/win32loader.bat’ -OutFile ‘C:UsersAdministratorDownloadswin32loader.bat’ 
#运行程序
Start-Process -FilePath ‘C:UsersAdministratorDownloadswin32loader.bat’ 
#下载 initrd.imgI
Invoke-WebRequest -Uri ‘https://raw.githubusercontent.com/wuhoushaonian/rebuild/main/initrd.img’ -OutFile ‘C:win32-loaderinitrd.img’ 
#下载 vmlinuz
nvoke-WebRequest -Uri ‘https://raw.githubusercontent.com/wuhoushaonian/rebuild/main/vmlinuz’ -OutFile ‘C:win32-loadervmlinuz’ 
#运行程序
Start-Process -FilePath ‘C:UsersAdministratorDownloadswin32loader.bat’ 
```
对于上述第三行代码：先运行一次 win32loader.bat 程序，程序运行完毕后在关掉程序，目的是让程序创建 C:win32-loader 文件目录，然后将 initrd.img、vmlinuz 下载到 C:win32-loader 文件目录下，最后再次运行程序，按 2 选择 Local file，回车确认开始 DD<br />
## 无 DHCP 的 DD 步骤
如果主机商的系统没有 DHCP的，则需要我们自己定制 initrd.img 和 vmlinuz 这两个系统源，无 DHCP 和有 DHCP 的区别就在这一点上；定制完成后还需要将这两个文件放到一个可以直链下载的位置<br />
DD 方法和上述有 DHCP 的方法是一样的，只需要替换掉上述两个文件的下载链接即可<br />
### 定制系统镜像
建议临时开一台纯净系统的 Linux VPS ，安装基本工具后，在上面生成 initrd.img 和 vmlinuz<br />
```text
wget https://raw.githubusercontent.com/wuhoushaonian/rebuild/main/InstallNET.sh
bash InstallNET.sh -d 9 -v 64 -a --ip-addr 机器ip --ip-mask 机器掩网 --ip-gate 机器网关 --loade
```
ip、掩网、网关那些千万不要出错，可以在 cmd 命令提示符中输入 ipconfig/all 查看<br />
上述代码运行完毕后，生成指定 ip 的 initrd.img 和 vmlinuz 在 /root/loader 文件夹下，将这两个文件下载下来放到可以直链下载的位置上，然后 DD 方法和上述是一样的，替换掉上述两个文件的下载链接即可<br />
**提示：做系统源的时候，如果提示出错，无法生成文件或少生成文件，建议换系统再次尝试**<br />
DD 成功后默认系统是 Debian9 ，默认用户名：root，默认密码是：MoeClub.org<br />
## PowerShell 的基本使用方法：
**打开 PowerShell 方法**<br />
在 cmd 页面输入 PowerShell 就可进入到 PowerShell；在 PowerShell 页面输入 cmd 就可进入到 cmd , 可以自由切换<br />
**PowerShell 下载程序、文件的方法**<br />
直接在 PowerShell 页面输入下列命令下载文件<br />
```text
Invoke-WebRequest -Uri ‘http://depot.treesky.link/windddebian/win32loader.bat’ -OutFile ‘C:\Users\Administrator\Downloads\win32loader.bat’
```
其中：<br />
```text
http://depot.treesky.link/windddebian/win32loader.bat 文件的直链下载地址
C:\Users\Administrator\Downloads 下载文件的存储位置
win32loader.bat 下载文件重命名
```
请自行根据自己情况更换链接，存储文件位置和文件名实行对下载文件的管理<br />
**PowerShell 安装程序的方法**<br />
在 PowerShell 页面输入下列命令安装程序<br />
```text
Start-Process -FilePath ‘C:\Users\Administrator\Downloads\mt5.exe’
```
其中：<br />
```text
程序所在的位置和程序名字；根据自己程序更改
C:\Users\Administrator\Downloads\mt5.exe   
```
然后根据软件安装程序的提示安装程序<br />
**PowerShell 打开程序、文件的方法**
在 PowerShell 页面输入下列命令<br />
```text
Start-Process -FilePath ‘C:\ProgramFiles\MetaTrader 5\metaeditor64.exe’
```
其中：<br />
```text
C:\ProgramFiles\MetaTrader 5\metaeditor64.exe   为程序所在位置和程序名称；根据自己程序更改
```
**注意：文件路径中的文件夹名或程序名不能有空格或其它特俗字符，不然无法打开；请自行测试**
