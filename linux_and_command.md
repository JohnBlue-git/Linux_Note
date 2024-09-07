
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
chmod u=rwx <file>

# change the ownership
chown <linuxize>:<users> <file1>
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



# Common command

## Reveal

<details>
<summary>top htop ps</summary>

```console
ps -ef | grep -i <...>

kill -9 <pid>
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

# 可以一直印出來
$tail -f xxx.log

#
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
```console
scp <-r> <account>@<ip | name>:<source> <destination>
```

### wget (handles FTP downloads)
```console
# -r for recursive download, a default maximum recursion depth of 5
wget -r --user="user@login" --password="..." ftp://server.com/
```



## Others

### soft link
```console
# relative path is acceptable
# but sometimes have to use absolute path
ln -s <source_directory> <link_name>
```

### compress
```console
1. **Creating the Archive**:
tar -cvpf archive.tar /path/to/directory`
    - **`c`**: Create a new archive.
    - **`v`**: Verbose mode, shows the progress in the terminal.
    - **`p`**: Preserve permissions.
    - **`f`**: Specify the filename of the archive.

2. **Extracting the Archive**:
tar -xvpf archive.tar`    
    - **`x`**: Extract the archive.
    - **`v`**: Verbose mode.
    - **`p`**: Preserve permissions.
    - **`f`**: Specify the filename of the archive.
    - z: 存取 .tar.gz 格式的套件為「gzip」，此格式會進行打包並且壓縮。

3. To view the contents of a tar file in Linux without extracting it,
tar -tf filename.tar.gz
```

### crontab
```console
# common
crontab -l列
crontab -e編輯
crontab -r刪除

# setting
<time> <binary>
...
```



# Special

## BMC console related
```console
# ssh
ssh <yujen.lan@192.168.11.1>
"crtl + D"

# minicom
minicom -wD /dev/ast2600evb.1021
"ctrl a + x"
```

## Virtual USB Drive (not yet try)
- **Create**: `dd if=/dev/zero of=virtual_usb.img bs=1M count=100`
- **Format**: `mkfs.ext4 virtual_usb.img`
- **Mount**: `sudo mount -o loop virtual_usb.img /mnt/virtual_usb`
- **Use**: Copy files to/from `/mnt/virtual_usb`
- **Unmount**: `sudo umount /mnt/virtual_usb`
- **Delete**: `rm virtual_usb.img`



# some reference (not yet orgrnize)
```console
https://blog.niclin.tw/2017/03/19/linux-%E5%9F%BA%E6%9C%AC%E6%8C%87%E4%BB%A4/
https://s40723210.github.io/cd2020/content/Linux%20%E6%8C%87%E4%BB%A4.html
```
