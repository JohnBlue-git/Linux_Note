
# Linux environment related

## Machine is x64 (x86_64) or arm architecture ?
```console
# output: x86_64 or arm / aarch64
uname -m

# provides detailed information about your CPU architecture
lscpu
```

## profile bash_profile
- login shell will access `~/.profile` or `~/.bash_profile` first, then access `source ~/.bashrc`; interactive shell will reach `~/.bashrc` directly.
- `~/.profile` is more common than `~/.bash_profile`, because it will be read and executed by all the shell, but the later can only be read and executed in Bash.
- `~/.bashrc` often store name or function for specific user; `~/.profile` and `~/.bash_profile` often stoe enviroment settings for specific user.
- The `~/.bashrc` and `~/.profile` and `~/.bash_profile` located in `/etc/` will applied to all users; files located in user root will overwrite the settings.
```console
# example of enivoment variable
export PATH= $PATH :/place/with/the/file;$PATH :/place/with/the/file
```

## kernel version and header
```console
# check version
cat /etc/os-release
uname -r
# install header
sudo apt-get install linux-headers-$(uname -r)
```

## Linux directory structure
Most Linux distribution will follow "Filesystem Hierarchy Standard, FHS": \
/bin, /sbin  /bin 主要放置一般使用者可以操作的指令，/sbin 放置系統管理員可以操作的指令。連結到 /usr/bin，/usr/sbin \
/boot  主要放置開機相關檔案 \
/dev  放置 device 裝置檔案，包話滑鼠鍵盤等 \
/etc  主要放置系統檔案 \
/home, /root  for normal user and system user
/lib, /lib64  主要為系統函式庫和核心函式庫，若是 64 位元則放在 /lib64。連結到 /usr/lib, /usr/lib64 \
/proc  將記憶體內的資料做成檔案類型 \
/sys 與 /proc 類似，但主要針對硬體相關參數 \
/usr  /usr 全名為 unix software resource 縮寫，放置系統相關軟體、服務（注意不是 user 的縮寫喔！） \
/var  store some variables or some logs \
/tmp  store temporary files \
/media, /mnt  放置隨插即用的裝置慣用目錄， /mnt 為管理員/使用者手動掛上（mount）的目錄 \
/opt  全名為 optional，通常為第三方廠商放置軟體處 \
/run  系統進行服務軟體運作管理處 \
/srv  通常是放置開發的服務（service），如：網站服務 www 等 \

## Linux user
sudo deluser <account>
sudo adduser <account>
sudo adduser <account> sudo
https://weirenxue.github.io/2021/06/17/user_linux_sudo/
sudo usermod -aG sudo <username>
https://askubuntu.com/questions/2214/how-do-i-add-a-user-to-the-sudo-group

## Linux authority
```console
# to change user
su <user>

# to access higher authorty
sudo <command>

sudo feature:
- restrict command for specific user
- provise sudo log
- /etc/sudoers
- time stamp (only get sudo for specific time)
- Ubuntu don't have root password
- ReHat 's sudoer don't need sudo
- http://note.drx.tw/2008/01/linuxsudo.html?m=1
# 可以透過設定 sudoers 檔案：
# 編輯 /etc/sudoers 檔案，新增一筆允許一般使用者執行 ipmitool 的規則，而不需要密碼驗證。
$ sudo vim /etc/sudoers
username ALL=(ALL) NOPASSWD:/usr/bin/ipmitool

# 修改裝置檔案的權限。
chmod 666 /dev/ipmi0
      777
chmod ugo=rwx <file>
chmod u+=rwx <file>

# change the ownership
chown <goup>:<user> <file>
```

## install / package / dependency / ...
```console
# redhat/centos
sudo yum install https://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-1.noarch.rpm
sudo yum install git

# apt-get/dpkg/deb   yum/rpm/rpm

目前在 Linux 界軟體安裝方式最常見的有兩種，分別是：

**dpkg**：這個機制最早是由 Debian Linux 社群所開發出來的，透過 dpkg 的機制， Debian 提供的軟體就能夠簡單的安裝起來，同時還能提供安裝後的軟體資訊，實在非常不錯。 只要是衍生於 Debian 的其他 Linux distributions 大多使用 dpkg 這個機制來管理軟體的， 包括 B2D, Ubuntu 等等。

**RPM**：這個機制最早是由 Red Hat 這家公司開發出來的，後來實在很好用，因此很多 distributions 就使用這個機制來作為軟體安裝的管理方式。包括 Fedora, CentOS, SuSE 等等知名的開發商都是用這咚咚。

https://linux.vbird.org/linux_basic/centos7/0520rpm_and_srpm.php

# apt
列出所有可更新的软件清单命令：sudo apt update
升级软件包：sudo apt upgrade
列出可更新的软件包及版本信息：apt list --upgradable
升级软件包，升级前先删除需要更新软件包：sudo apt full-upgrade
安装指定的软件命令：sudo apt install <package_name>
安装多个软件包：sudo apt install <package_1> <package_2> <package_3>
更新指定的软件命令：sudo apt update <package_name>
显示软件包具体信息,例如：版本号，安装大小，依赖关系等等：sudo apt show <package_name>
删除软件包命令：sudo apt remove <package_name>
清理不再使用的依赖和库文件: sudo apt autoremove
移除软件包及配置文件: sudo apt purge <package_name>
查找软件包命令： sudo apt search <keyword>
列出所有已安装的包：apt list --installed
列出所有已安装的包的版本信息：apt list --all-versions

# apt VS apt-get

apt:
- User-friendly: Provides a more user-friendly interface.
- Unified Commands: Combines the most commonly used commands from apt-get and apt-cache.
- Progress Bar: Includes a progress bar for operations.
- Recommended Packages: Automatically installs recommended packages by default.

apt-get (recommended to use in scripts):
- Script-Friendly: More suited for scripting and automation.
- More Commands: Includes a wider range of commands and options.
- Stable Behavior: Maintains behavior and outputs that are expected in scripts over releases.

# check library installed? or version
# take libboost for example
dpkg -l libboost-all-dev

# ldd, List Dynamic Dependencies
# 用於列出一個執行檔的動態連結庫依賴關係的工具:
ldd /usr/bin/ipmitool

# file type checker 用於分析檔案的內容和結構。
file /usr/bin/ipmitool

```
<img width="1017" alt="%E6%88%AA%E5%9C%96%202024-01-07%20%E4%B8%8B%E5%8D%8812 16 23" src="https://github.com/user-attachments/assets/61608c1a-dd08-405e-9e2a-765d794c1aa1">
<img width="1009" alt="%E6%88%AA%E5%9C%96%202024-01-07%20%E4%B8%8B%E5%8D%8812 15 03" src="https://github.com/user-attachments/assets/a92d5c74-6248-41bd-b21d-fa0ea9f8d356">

## Linux internet related
```console
# ping
ping <IP>

# telnet ...

# net cat
nc -vz <IP> <Port>
-v set verbosity level
-z report connection status only
-l bind and listen 
https://zh.wikipedia.org/zh-tw/Netcat

# on windows
test-netconnection <IP> -p <Port>

# proxy
查詢env | grep proxy
查詢echo  http_proxy
暫時關閉(關機會重啟)unset http_proxy
暫時關閉(關機會重啟)unset https_proxy

# ifconfig
ifconfig -a

# DNS
# Domain name system
DNS 系統是以所謂的階層式的管理，所以，請注意喔！那個 .tw 只記錄底下那一層的這六個主要的 domain 的主機而已！
至於例如 edu.tw 底下還有個 ncku.edu.tw 這部機器，那就直接授權交給 edu.tw 那部機器去管理了！
也就是說『 每個上一層的 DNS 主機，所記錄的資訊，其實只有其下一層的主機名稱而已！ 』
至於再下一層，則直接『授權』給下層的某部主機來管理囉！呵呵！
所以您就應該會知道 DNS 到底是如何管理的吧！ ^_^
#
/etc/resolv.conf
#
http://old.linux.vbird.org/linux_server/0350dns/0350dns.php
http://dns-learning.twnic.net.tw/dns/03opDNS.html
https://emmielin.medium.com/dns-查詢流程概念筆記-3a420460d396
```
![Untitled](https://github.com/user-attachments/assets/23181e67-5306-4ad8-9f52-67585cb0e20d)


## Systemd vs Systemctl

Systemd and Systemctl are two essential components in the Linux operating system, often used interchangeably but serving different purposes.
- Systemd is a system and service manager responsible for initializing the system, managing services, and handling system events. It replaces the traditional SysV init system and provides a more efficient and reliable way to manage system services. Systemd uses unit files to define how services should be started, stopped, and managed. These unit files specify the behavior of each service, including its dependencies, startup order, and resource limits
- Systemctl, on the other hand, is a command-line utility used to manage system services in Linux. It is part of the Systemd system and service manager. With Systemctl, users can start, stop, restart, enable, disable, and check the status of services running on their system. It provides a simple and efficient way to manage system services without the need for complex commands

Key Differences
- Purpose: Systemd: Manages the system's boot process, handles daemons, and manages system services. Systemctl: Interacts with Systemd to control services.
- Functionality: Systemd: Responsible for initializing the system, starting services in parallel, and monitoring system resources. Systemctl: Used to start, stop, restart, enable, disable, and check the status of services.
- Unit Files: Systemd: Uses unit files to define how services should be managed. Systemctl: Reads these unit files to determine how to handle each service.
- Advanced Features: Systemd: Supports socket activation, which allows services to be started on-demand when a connection is made to a specific socket. It also provides tools for managing system resources, such as cgroups, which allow users to set resource limits for services and prevent resource contention^2^. Systemctl: Provides detailed information about each service, including its status, PID, memory usage, and more


# Common command

## Reveal

<details>
<summary>top htop ps</summary>

```console
ps -ef | grep -i <...>

kill -9 <pid>

# To kill all processes related to :
#   xargs kill -9 :
#    Passes the PIDs and forcefully terminating them ( sends the SIGKILL signal).
ps -ef | grep yujen | grep <related to> | awk '{print $2}' | xargs kill -9
```
</details>

<details>
<summary>echo pwd ls cat head tail</summary>

```console
#
echo $USER

#
pwd

# 
ls -la

# 僅輸出最後 5 筆檔案資訊
ls -l | tail -n 5

# 擷取第 2 個字元至第 10 個字元
ls -l | tail -n 5 | cut -c 2-10 --complement
https://blog.gtwang.org/linux/linux-cut-command-tutorial-and-examples/

# count how many
ls -1 /path/to/folder | wc -l

# keep printing
$tail -f xxx.log

# cat
cat xxx.txt
```
</details>

<details>
<summary>grep</summary>

```console
# 不管大小寫 non-sensitive
grep -i

# 往下
grep -A 5

# 往上
grep -B 5

# 上下
grep -C 5

# -r (or --recursive): This option tells grep to search recursively through all directories and subdirectories.
# -n (or --line-number): This option causes grep to display the line number where the pattern is found within each file.
grep -rn

# regex and extract
#
$ stat test.txt
  File: test.txt
  Size: 28954           Blocks: 64         IO Block: 4096   regular file
Device: 705h/1797d      Inode: 1310792     Links: 1
Access: (0666/-rw-rw-rw-)  Uid: ( 1000/codespace)   Gid: ( 1000/codespace)
Access: 2024-10-09 07:05:39.825899553 +0000
Modify: 2024-10-09 07:05:38.857899504 +0000
Change: 2024-10-09 07:05:38.857899504 +0000
 Birth: -
# then
$ stat test.txt | grep -Eo "Size: [0-9]+" | grep -Eo "[0-9]+"
28954

# To find strings that match the pattern "Machine ... found" using grep with a regular expression, you can use the following command:
grep -E 'Machine .* found' file.txt
```
</details>

<details>
<summary>history</summary>

```console
最近4個
`history | tail -4`

忽略重複的
export HISTCONTROL=ignoredups

#
HISTTIMEFORMAT="%d/%m/%y %T " history

https://blog.gtwang.org/linux/mastering-linux-command-line-history/
```
</details>

<details>
<summary>tree find</summary>

```console
#
tree -L <depth> <directory>

# 查找当前目录下名为 file.txt 的文件：
find . -name file.txt

# 将当前目录及其子目录下所有文件后缀为 **.c** 的文件列出来:
find . -name "*.c"

# 尋找/home 目录下
find /home -name file.txt

# maxdepth
find -maxdepth 1

# modified within 1 day
find . -mtime -1

# modified before 1 day ago
find . -mtime +1

# 将当前目录及其子目录中的所有文件列出：
find . -type f

# 查找 /home 目录下大于 1MB 的文件：
find /home -size +1M

find /etc -iname 'Network'
find /var/log -iname '*.log' -type f
find /etc -iname 'apache2' -type d
find . -maxdepth 1 -newermt $(date + '%Y-%m-%d') | cut -c 3-10
find /ap/sts -type d -name build_QuotationServer
     <path>                <dirname>
但是如果dir還有sub_dir...會有太多
還有很多 檔案大小 各式異動 各式時間 判斷
find /var -type f -size +50M
find $HOME -type f -atime +7

https://blog.miniasp.com/post/2010/08/27/Linux-find-command-tips-and-notice
```
</details>

<details>
<summary>du dh</summary>

```console
# to list diectory size
du -h <dir>
# to summary
du -sh <dir>

What this means:
- `s` to give only the total for each command line argument.
- `h` for human-readable suffixes like `M` for megabytes and `G` for gigabytes (optional).
- `/*` simply expands to all directories (and files) in `/`.

# to sum whole machine storage
dh -f
```
</details>

<details>
<summary>ipc</summary>

```console
#查看共享內存
ipcs -m

#查看隊列
ipcs -q

#查看信號量
ipcs -s
```
</details>

<details>
<summary>diff</summary>

```console
diff monitor.ini monitor.ini
```
</details>



## Navigate
<details>
<summary>cd mkdir</summary>

```console
#
cd <...>

#
mkdir <…>
```
</details>

<details>
<summary>mv cp</summary>

```console
#
mv source/A destination/B
mv source/A destination/
改名稱$mv source/A source/B
如果一次很多 ＊pattern並且$mv source/*.txt destination/
因為mv很多限制 就使用迴圈for I in $(ls *_20231130.txt); do mv $i ${I%_20231130.txt}_20231201.txt; done

#
cp source/A destination/B
cp source/A destination/
如果一次很多 ＊pattern並且$cp source/*.txt destination/
```
</details>

<details>
<summary>rm</summary>

```console
rm -rf *

# exclude
rm -rf !(one|two|three)
```
</details>

<details>
<summary>(cd … && …)</summary>

```console
(cd … && mv …)
(cd … && cp …)
有些binary需要進到bin/去執行
也可以搭配crontab使用
```
</details>

### watch
The watch command runs a specified command repeatedly at a set interval.
```console
# To run the ls command every 2 seconds:
watch -n 2 ls
```

### wc - Count how many keys in json
This method use grep to find keys and use wc to count them.
```console
grep -o '"[^"]*":' <json file> | wc -l
```

### Monitor the thread count of a proces
To get number of threads
```console
# 1.
# where nlwp stands for Number of Light Weight Processes (threads). 
ps -o nlwp <pid>
ps -eo nlwp | tail -n +2 | awk '{ num_threads += $1 } END { print num_threads }'

# 2.
ps -o thcount <pid>

# 3.
ps -T -p <pid>

# 4.
# Check via  /proc filesystem
# by pid
ls /proc/<pid>/task | wc -l
# by name
ls /proc/$(pgrep -n <your_process_name>)/task | wc -l
```
To monitor the thread count, simply use watch:
```console
# 1.
watch -n 1 ps -o nlwp <pid>

# 2.
watch -n 1 ps -o thcount <pid>

# 3
watch -n 1 ps -T -p <pid>

# 4 Check via  /proc filesystem
watch -n 1 'ls /proc/<pid>/task | wc -l'
watch -n 1 'ls /proc/$(pgrep -n <your_process_name>)/task | wc -l'
```

## Edit text

<details>
<summary>touch</summary>

```console
# 可以產生檔案
# 可以更新時間戳記time stamp
touch -d "2 days ago" <file>
```
</details>

<details>
<summary>vim</summary>

```console
# vim
vim <file>

# search /\< >\>
/\< word to search >\>
# "*" forward
# "#" backward
# "enter" to search next

# insert
i
"esc"

# quit
:q
:wq
:q!
```
</details>

<details>
<summary>sed</summary>

```console
可以行作為單位編輯
sed -e 4a\newLine testfile
找pattern
/hm/
判斷第幾行
sed -n '/hm/=' getPath.sh
顯示/p 刪除/d ...
sed -n '/hm/p' getPath.sh

https://www.runoob.com/linux/linux-comm-sed.html
```
</details>

<details>
<summary>cut</summary>

```console
# 擷取第 2-3 個、第 5-6 個與第 8-9 個字元
ls -l | cut -c 2-3,5-6,9
# 擷取 CSV 檔的第 1-3 個與第 5 個欄位
cut -d , -f 1-3,5 data.csv
# 排除 CSV 檔的第二個欄位
cut -d , -f 2 --complement data.csv

https://blog.gtwang.org/linux/linux-cut-command-tutorial-and-examples/
```
</details>

<details>
<summary>awk 處理文本</summary>

```console
log.txt
2 this is a test
3 Do you like awk
This's a test
10 There are orange,apple,mongo
# 取第1第4
awk '{print $1,$4}' log.txt
awk '{printf "%-8s %-10s\n",$1,$4}' log.txt
# 逗號分隔 取第1第2
awk -F, '{print $1,$2}' log.txt
# 输出第二列包含 "th"，并打印第二列与第四列
$ awk '$2~/th/ {print $2,$4}' log.txt

https://www.runoob.com/linux/linux-comm-awk.html
```
</details>



## Transmission

### scp
the drawback is trassmission will fail when folder structure is equal or more than 3 layers 
```console
scp <-r> <account>@<ip | name>:<source> <destination>
```

### unison (scp alternative)
how to install
```console
# please install unison on both sides
sudo apt install unison

# it is possible to have compatibility problem among different linux
# we can check dependency first
ldd unison
# if the dependency on both side are the same
# we can scp "unison" binary from older Linux to newer Linux
# such that the binary can be compatible
```
how to use (it is a bi-direction sync)
```console
# normal execute
unison ssh://<account>@<ip>//source/ ./target/

# batch execute (without user interaction)
unison -batch ssh://<account>@<ip>//source/ ./target/

# bactch execute withouut checking (force to sync)
unison -batch -confirmbigdel=false ssh://<account>@<ip>//source/ ./target/
```

### curl
basic usage
```console
# basic usage 
curl --help
# Usage: curl [options...] <url>
# -d, --data <data>          HTTP POST data
# -f, --fail                 Fail silently (no output at all) on HTTP errors
# -h, --help <category>      Get help for commands
# -i, --include              Include protocol response headers in the output
# -o, --output <file>        Write to file instead of stdout
# -O, --remote-name          Write output to a file named as the remote file
# -s, --silent               Silent mode
# -T, --upload-file <file>   Transfer local FILE to destination
# -u, --user <user:password> Server user and password
# -A, --user-agent <name>    Send User-Agent <name> to server
# -v, --verbose              Make the operation more talkative
# -V, --version              Show version number and quit

# get
curl -X GET "http://www.example.com/api/resources"
curl -X GET "http://www.example.com/api/resources/1"

# patch
curl -X POST \
-H "Content-Type: application/json" \
-d '{"status" : false, "name" : "Jack"}' \
"http://www.example.com/api/resources"

# put
curl -X PUT \
-H "Content-Type: application/json" \
-d '{"status" : false }' \
"http://www.example.com/api/resources"

# delete
curl -X DELETE "http://www.example.com/api/resources/1"

# with cookie
curl --cookie "name=John" "http://www.example.com"

# with user agent
curl --user-agent "Mozilla/5.0 (compatible; MSIE 5.01; Windows NT 5.0)" "http://www.example.com"

# with basic auth
curl -u <user>:<password> "http://www.example.com/api/resources/1"
```
request with attachments or download attachments
```console
# -d command-line option
#   will force Curl to submit data to the server using the application/x-www-form-urlencoded format. 
#   (the keys and values are encoded in key-value tuples separated by an ampersand (&), with an equals symbol (=) between the key and the value)
curl -X POST \
-H "Content-Type: application/x-www-form-urlencoded" \
-d "status=false&name=Jack" \
"http://www.example.com/api/resources"

# --data-binary command-line option
curl -X POST \
-H "Content-Type: application/octet-stream" \
--data-binary @<file name> \
"http://www.example.com/api/resources"

# -F command-line option
#  tells Curl to send data to the server in multipart/form-data format.
curl -X POST \
-H "Content-Type: multipart/form-data" \
-F "file=@<file name>" \
"http://www.example.com/api/resources"
```
curl command on windows
```console
$ & "C:\Program Files\Git\mingw64\bin\curl.exe" -k -X POST \
--data-binary @<file name> \
--header "Content-Type: application/octet-stream" \
"http://www.example.com/api/resources"
```

### wget
basic usage
```console
# Help
wget -h

# Download a File
wget http://example.com/file.zip

# Download a File with a Custom Name
wget -O custom_name.zip http://example.com/file.zip

# Download Multiple Files from a List
# Where `urls.txt` contains multiple URLs, one per line.
wget -i urls.txt

# Resume Interrupted Downloads
wget -c http://example.com/file.zip

# Download an Entire Website (Recursive Mode)
wget -r http://example.com/

# Set Download Speed Limits
# This restricts the download speed to `500 KB/s`.
wget --limit-rate=500k http://example.com/file.zip

# Download with Authentication
wget --user=username --password=password http://example.com/protected-file.zip
```

## Remote Mount

### sshfs

3rd party source code (a bit older with big issue, but it's somewhat fine to use) \
https://github.com/libfuse/sshfs

how to install
```console
# only need to installl on client side
sudo apt install sshfs
```
how to use
```console
# create folder on client side
mkdir /path/to/mount_point

# link remote source
sshfs <account>@<ip or name>:/path/to/source /path/to/mount_point

# link remote source (in robust manner)
sshfs -o reconnect,<account>,ServerAliveInterval=15,ServerAliveCountMax=3 <account>@<ip or name>:/path/to/source /path/to/mount_point
# reconnect: Automatically reconnects if the connection is lost.
# ServerAliveInterval=15: Sends a keepalive packet every 15 seconds.
# ServerAliveCountMax=3: Allows up to 3 missed keepalive packets before disconnecting.

# check
mount
# should see ...
# <my account>@your.host.com:/path/to/source on /home/account/target type fuse.sshfs (rw,nosuid,nodev,user=<my account>)

# unmount
fusermount -u /path/to/mount_point
```
re-mount when reboot
```bash
#!/bin/bash

# unmount
fusermount -u /home/yujen/sshfs-yujen
# sshfs
sshfs -o password_stdin,reconnect,ServerAliveInterval=15,ServerAliveCountMax=3 yujen@192.168.9.197:/media/disk4T/yujen /home/yujen/sshfs-yujen <<< "yujen"
```
re-mount when reboot (crontab)
```bash
# crontab -e
@reboot /path/to/script.sh
```

## Others

### soft link
```console
# relative path is acceptable
# but sometimes have to use absolute path
ln -s <source_directory> <link_name>
```

### export
```console
# list all environment variable
export -p
# declare -x HOME=“/root“
# declare -x PATH=“/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games“
# declare -x PWD=“/root“
# declare -x USER=“root“

# create environment variable
export NEWVAR

# assign value
export NEWVAR="value"

# append value
export NEWVAR+=":value"
export -p | grep NEWVAR

# delete
export -n NEWVAR
```

### related to current account enviroment
```console
# use ~
~/target/path

# for example
echo $USER # john
ls ~/bin # /usr/local/bin or /usr/john/bin
```

### compress (tar)
Creating the Archive
```console
tar -cvpf archive.tar <array of files>
    - `c`: Create a new archive.
    - `v`: Verbose mode, shows the progress in the terminal.
    - `p`: Preserve permissions.
    - `f`: Specify the filename of the archive.
```
Extracting the Archive
```console
tar -xvpf archive.tar -C /path/to/directory
    - `x`: Extract the archive.
    - `v`: Verbose mode.
    - `p`: Preserve permissions.
    - `f`: Specify the filename of the archive.
```
To view the contents of a tar file in Linux without extracting it
```console
tar -tf filename.tar.gz
```
Type of compression
```console
tar -cvzpf archive.tar.gz <array of files>
    ‘-z’: Uses gzip compression.
tar -cvjpf archive.tar.tbz <array of files>
    ‘-j’: Uses bzip2 compression.
```
Creates an archive by bundling files and directories together.
```console
# extarct to /path/to/directory
tar -xvpf archive.tar -C /path/to/directory
```
Flattern the folder ot files to be compressed (Only for GNU tar)
- GNU tar (common on Linux)
      - use '--transform'
      ```console
      tar --transform='s|.*/||' -czf flat.tar.gz </path/to/directory/*>
      ```
- BSD tar (used by default on macOS and some BSD systems)
      - on Mac, we can install GNU tar via Homebrew
      ```console
      brew install gnu-tar
      gtar --transform='s|.*/||' -czf flat.tar.gz </path/to/directory/*>
      ```
Concatenates multiple archive files into a single archive
```console
tar -Af main.tar additional1.tar additional2.tar
```

### compress (zip)
```console
# 將 data 目錄下所有檔案及副目錄壓儲到 file.zip:
zip -r file data/*

# 用 unzip 將 file.zip 壓縮檔內所有檔案及目錄解壓到當前目錄:
unzip file.zip
```

### compress (xz)
A relative newcomer to the compression scene, the `xz` command is recognized for its impressive compression capabilities. While it might take longer for large files, the compression results are noteworthy
```console
# The <bigfile>.xz`showcases the compressed version of the file. `xz`
xz [options] <bigfile>
```

### systemctl / service
```console
# systemctl
systemctl status <service>
systemctl start <service>
systemctl stop <service>
systemctl restart <service>

# service
service <service> status
service <service> start
service <service> stop
service <service> restart

# Example service configuraition file
# /etc/systemd/system/.../bmcweb.service
[Unit]
Description=Start bmcweb server

Wants=network.target
After=network.target
Wants=rcmd.service
After=rcmd.service
Wants=phosphor-ipmi-host.service
After=phosphor-ipmi-host.service
PartOf=rcmd.service

[Service]
ExecReload=kill -s HUP $MAINPID
ExecStart=/usr/bin/bmcweb
Type=simple
WorkingDirectory=/home/root

[Install]
WantedBy=network.target
```

### crontab
```console
# common
crontab -l列
crontab -e編輯
crontab -r刪除

# setting
*     *     *     *     *  command to be executed
-     -     -     -     -
|     |     |     |     |
|     |     |     |     +----- Day of the week (0 - 7) (Sunday=0 or 7)
|     |     |     +------- Month (1 - 12)
|     |     +--------- Day of month (1 - 31)
|     +----------- Hour (0 - 23)
+------------- Minute (0 - 59)

# Every Minute
* * * * * /path/to/command
# Every Hour:
0 * * * * /path/to/command
# Weekly on Sunday at 1 AM
0 1 * * 0 /path/to/command

# authencation
# user level
crontab -e
# root level
crontab -e

# notice that:
# it is still necessary to add sudo for priviledged command
* * * * * sudo /path/to/priviledged_command

# checking whether the contents in the table are executed?
sudo grep CRON /var/log/syslog | grep ... 
```

### Log
**journalctl** is a command line utility used to query and display logs from the systemd journal. It's incredibly useful for system administrators and anyone who needs to troubleshoot issues on Linux systems that use systemd.
```console
# Show All Logs
journalctl

# Show Logs Since Boot
journalctl -b
# for the previous boot
journalctl -b -1

# Follow Logs (Real-Time)
journalctl -f

# Show Logs for a Specific Unit
journalctl -u <service-name>

# Show Logs for a Specific Time Range
journalctl --since "2023-01-01" --until "2023-01-02"

# Show Kernel Logs (show kernel log only)
journalctl -k

# Limit Output to a Certain Number of Lines
journalctl -n 100

# Output in Reverse Order
journalctl -r

# Clear log
journalctl --vacuum-time=60s

# Display log usage status
journalctl --disk-usage
```

### Tee
Absolutely! The tee command in Unix and Linux systems is used to read from standard input and write to both standard output (displaying it on the terminal) and one or more files simultaneously. It's like a "T" junction that splits the input data stream, allowing you to see the output while also saving it to a file.
```console
# Basic Usage
<command> | tee filename
ls -l | tee output.txt

# Append Mode
# If you want to append the output to an existing file instead of overwriting it, you can use the -a (append) option:
<command> | tee -a filename

# Using tee to Discard Output
<command> | tee /dev/null
```

### Time (elaspsed)
```console
# Output Explanation
time ls
# real: The total elapsed time (wall clock time) it took to execute the command.
# user: The amount of CPU time spent in user mode.
# sys: The amount of CPU time spent in system (kernel) mode.

# Basic usage
# time <command>
time -v curl -k -u root:0penBmc -X GET https://localhost/redfish/v1
# with pipeline
time (find / -name "example.txt" | grep "example")
# with bash
time bash your_script.sh

# -p: Format the output in a POSIX-compliant way.
time -p <your_command>

# -o: Redirect the output to a file.
time -o output.txt <your_command>

# --verbose: Display detailed resource usage statistics.
time --verbose <your_command>

# advance time command
https://stackoverflow.com/questions/18215389/how-do-i-measure-request-and-response-times-at-once-using-curl
```

### usb access with minicom
```console
# minicom
minicom -wD /dev/ast2600evb.1021
"ctrl a + x"
```

### remote access with ssh
```console
# ssh
ssh <yujen.lan@192.168.11.1>

# To exit
exit
# or
crtl + D

# with pass (but this command is not always available)
sshpass -p <password> ssh <user>@<host>

# without asking permission (but still need password)
ssh -o StrictHostKeyChecking=no -l <user> <host>
```

### SSDP
The Simple Service Discovery Protocol (SSDP) is a network protocol based on the Internet protocol suite for advertisement and discovery of network services and presence information.
- It accomplishes this without assistance of server-based configuration mechanisms, such as Dynamic Host Configuration Protocol (DHCP) or Domain Name System (DNS), and without special static configuration of a network host.
- SSDP is the basis of the discovery protocol of Universal Plug and Play (UPnP) and is intended for use in residential or small office environments.

To check for available SSDP services without writing code via gssdp-discover:
```console
# Install gSSDP (if you haven't already):
sudo apt-get install gssdp

# Run gssdp-discover
#   This will send an SSDP M-SEARCH request
#   to the default multicast address 239.255.255.250 and port 1900
#   , and it will display information about any available devices or services that respond.
gssdp-discover

# You should see something like this if SSDP devices are found:
# Found service:
#  Location: http://192.168.1.100:8080/device
#  USN: uuid:12345678-1234-5678-1234-567812345678
#  Server: UPnP/1.0, Device/1.0
# Additional options: You can specify the search target (for example, a particular service type) by using the -s option. For example:

# This searches for all devices or services that are advertising themselves over SSDP.
gssdp-discover -s ssdp:all

# If you want to use the eth0 interface for sending the SSDP discovery request
gssdp-discover -i eth0
```


# Special

## minicom
Minicom is a text-based terminal emulator that is used for serial communication. It is particularly useful for debugging and configuring hardware devices like routers, switches, and embedded systems that communicate over serial ports.
<details>
<summary>basic usage</summary>

```console
# Open a Serial Port:
sudo minicom --device /dev/ttyUSB0

# Specify Baud Rate: (with a baud rate of 115200 bits per second)
sudo minicom --device /dev/ttyUSB0 --baudrate 115200

# Configuration
sudo minicom -s

# Run Minicom with a Configuration File:
sudo minicom -c /etc/minicom/minirc.dfl

# Advanced Usage
# Capture Output to a File:
sudo minicom --device /dev/ttyUSB0 --baudrate 115200 --capture /path/to/capture_file.log

# Advanced Usage
# Use Meta Key: (allows you to use the Meta or ALT key as the command key)
sudo minicom --metakey

# Additional Options
# -b <baudrate>: Sets the baud rate.
# -D <device_name>: Specifies the device name for the serial port.
# -h: Shows a list of arguments that minicom accepts.
# -o: Skips initialization.
# -m: Overrides the command key with the Meta or ALT key.
# -z: Uses terminal status line.
# -l: Translates IBM line characters to ASCII.
# -w: Turns on line-wrap.
# -H: Turns on output in hex mode.
# -a: Handles terminal attributes.
```
</details>
<details>
<summary>basic usage within session</summary>

```console
When you're inside a minicom session, the Ctrl+A key combination (also called the "command key") activates a set of commands that you can use to control the session. Here's a list of common Ctrl+A commands:

Ctrl+A, A: Toggles the linefeed.
Ctrl+A, B: Sends a break signal.
**Ctrl+A, C**: Clears the screen.
Ctrl+A, D: Turns local echo on or off.
Ctrl+A, E: Turns CRT echo on or off.
Ctrl+A, F: Enables or disables software flow control.
Ctrl+A, G: Generates a break condition.
Ctrl+A, H: Hangs up the connection.
Ctrl+A, I: Initializes the modem.
Ctrl+A, J: Jumps to a sub-menu or script.
Ctrl+A, K: Turns capture on or off (log output to a file).
**Ctrl+A, L**: Turns logging on or off.
Ctrl+A, M: Toggles the monitor line status.
Ctrl+A, N: Switches to the next screen.
Ctrl+A, O: Changes the linefeed mode.
Ctrl+A, P: Turns on or off printing of the screen.
Ctrl+A, Q: Resets the modem.
Ctrl+A, R: Receives a file using one of the transfer protocols (e.g., Xmodem, Ymodem, Zmodem).
**Ctrl+A, S**: Sends a file using one of the transfer protocols (e.g., Xmodem, Ymodem, Zmodem).
Ctrl+A, T: Initializes the terminal.
Ctrl+A, U: Flips between uppercase and lowercase.
Ctrl+A, V: Enters the Goto mode.
**Ctrl+A, X**: Exits the current session and returns to the main menu.
Ctrl+A, W: Toggles line wrap.
Ctrl+A, Y: Swaps the transmit speed.
**Ctrl+A, Z**: Shows help with a list of these commands.
```
</details>

## sf probe

sf probe is a command in **U-Boot** that detects and initializes SPI NOR flash. Before using the sf probe, you must ensure that the **SPI flash device** is connected and configured correctly.
ref: https://blog.csdn.net/u010632165/article/details/117756488
```console
sf probe [bus:]cs [hz] [mode]

sf probe 0:0

# bus: SPI bus number (optional).
# cs: SPI chip selection number.
# Hz: SPI bus speed (hertz, default is usually 100KHz).
# mode: SPI mode (0 to 3), which defines the clock polarity and phase.
```

## Virtual USB Drive (not yet try)
- **Create**: `dd if=/dev/zero of=virtual_usb.img bs=1M count=100`
- **Format**: `mkfs.ext4 virtual_usb.img`
- **Mount**: `sudo mount -o loop virtual_usb.img /mnt/virtual_usb`
- **Use**: Copy files to/from `/mnt/virtual_usb`
- **Unmount**: `sudo umount /mnt/virtual_usb`
- **Delete**: `rm virtual_usb.img`

## To delete directories or files created on or after a specific date and time
! -path . ! -path .. : to skip the current and parent directories.
. : Represents the current directory.
-type d : Specifies that you are looking for directories.
-newermt "2025-02-20 13:30" : Finds files or directories with a modification time on or after "2025-02-20 13:30".
-prune : Ensures that find doesn't recurse into directories that match the criteria.
-exec rm -rf {} + : Executes rm -rf on each directory found.
```console
find . ! -path . ! -path .. -type d -newermt "2025-02-20 13:30" -prune -exec rm -rf {} +
```



# some reference (not yet orgrnize)
```console
https://blog.niclin.tw/2017/03/19/linux-%E5%9F%BA%E6%9C%AC%E6%8C%87%E4%BB%A4/
https://s40723210.github.io/cd2020/content/Linux%20%E6%8C%87%E4%BB%A4.html
```
