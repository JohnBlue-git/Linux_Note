# note
temporary warehouse for remembering command


<details>
<summary>linux reference</summary>

```console
https://blog.niclin.tw/2017/03/19/linux-%E5%9F%BA%E6%9C%AC%E6%8C%87%E4%BB%A4/
    
https://s40723210.github.io/cd2020/content/Linux%20%E6%8C%87%E4%BB%A4.html
```
</details>

<details>
<summary>linux directory</summary>

```console
理論上所有的 Linux 發佈版本應該都要遵守檔案系統的標準（Filesystem Hierarchy Standard, FHS），但根據發佈版本不同或有差異，不過大致上檔案系統架構如下：
/bin, /sbin
/bin 主要放置一般使用者可以操作的指令，/sbin 放置系統管理員可以操作的指令。連結到 /usr/bin，/usr/sbin
/boot
主要放置開機相關檔案
/dev
放置 device 裝置檔案，包話滑鼠鍵盤等
/etc
主要放置系統檔案
/home, /root
/home 主要是一般帳戶的家目錄，/root 為系統管理者的家目錄
/lib, /lib64
主要為系統函式庫和核心函式庫，若是 64 位元則放在 /lib64。連結到 /usr/lib, /usr/lib64
/proc
將記憶體內的資料做成檔案類型
/sys
與 /proc 類似，但主要針對硬體相關參數
/usr
/usr 全名為 unix software resource 縮寫，放置系統相關軟體、服務（注意不是 user 的縮寫喔！）
/var
全名為 variable，放置一些變數或記錄檔
/tmp
全名為 temporary，放置暫存檔案
/media, /mnt
放置隨插即用的裝置慣用目錄， /mnt 為管理員/使用者手動掛上（mount）的目錄
/opt
全名為 optional，通常為第三方廠商放置軟體處
/run
系統進行服務軟體運作管理處
/srv
通常是放置開發的服務（service），如：網站服務 www 等
```
</details>

<details>
<summary>linux authority</summary>

```console

sudo 有以下特點：
1能夠限制指定用戶在指定主機上運行某些命令。
2可以提供日誌，忠實地記錄每個用戶使用 sudo 做了些什麼，並且能將日誌傳到中心主機或者日誌伺服器。
3為系統管理員提供配置文件，允許系統管理員集中地管理用戶的使用權限和使用的主機。
它預設的存放位置是 /etc/sudoers。
4使用時間戳文件來完成類似「售票」的系統。
當用戶執行 sudo 並且輸入密碼後，用戶獲得了一張預設為期 5 分鐘的「入場券」
(預設值可以在編譯的時更改)。超過時間以後，用戶必須重新輸入密碼。

安裝過程中, Ubuntu 並沒有問 root 的密碼, 
所以普通的使用者根本就不知root 帳號的密碼是什麼。不過登入系統後,
可以用sudo passwd root 先輸入自己的密碼再修改root的密碼。
不過強烈『不推薦』設定 root 密碼，如果有需要 root 權限的環境，
請用 sudo 或 sudo -s 就好。如果已經設了 root 密碼，
可以用 vi /etc/passwd  將第一行的 root 的第二個欄位 "x" 改成 "!" 驚嘆號
即可讓入侵者無法直接以 root 身份登入。

rehat系列只要是sudoer就不用sudo

reference
http://note.drx.tw/2008/01/linuxsudo.html?m=1
```
</details>


<details>
<summary>linux apt-get/dpkg/deb   yum/rpm/rpm</summary>

```console
https://linux.vbird.org/linux_basic/centos7/0520rpm_and_srpm.php

目前在 Linux 界軟體安裝方式最常見的有兩種，分別是：

- **dpkg**：這個機制最早是由 Debian Linux 社群所開發出來的，透過 dpkg 的機制， Debian 提供的軟體就能夠簡單的安裝起來，同時還能提供安裝後的軟體資訊，實在非常不錯。 只要是衍生於 Debian 的其他 Linux distributions 大多使用 dpkg 這個機制來管理軟體的， 包括 B2D, Ubuntu 等等。
- **RPM**：這個機制最早是由 Red Hat 這家公司開發出來的，後來實在很好用，因此很多 distributions 就使用這個機制來作為軟體安裝的管理方式。包括 Fedora, CentOS, SuSE 等等知名的開發商都是用這咚咚。
```
<img width="1017" alt="%E6%88%AA%E5%9C%96%202024-01-07%20%E4%B8%8B%E5%8D%8812 16 23" src="https://github.com/user-attachments/assets/61608c1a-dd08-405e-9e2a-765d794c1aa1">
<img width="1009" alt="%E6%88%AA%E5%9C%96%202024-01-07%20%E4%B8%8B%E5%8D%8812 15 03" src="https://github.com/user-attachments/assets/a92d5c74-6248-41bd-b21d-fa0ea9f8d356">
</details>

<details>
<summary>linux internet</summary>

```console
	ifconfig
網路設定

	ping IP
https://linux.vbird.org/linux_server/centos6/0140networkcommand.php

	nc -vz IP Port
-v set verbosity level
-z report connection status only
-l bind and listen 
https://zh.wikipedia.org/zh-tw/Netcat

	在windows上
test-netconnection IP -p Port

	proxy
查詢env | grep proxy
查詢echo  http_proxy
暫時關閉(關機會重啟)unset http_proxy
暫時關閉(關機會重啟)unset https_proxy

	DNS
#
DNS 的全名是『 Domain name system 』， 中文譯名為『領域名稱系統』， 
這個咚咚的用途是什麼哇！為什麼我們的電腦或者是 Internet 一定需要他 
( 尤其是以 WWW 的方式來上網時 ) ？呵呵！ 
#
他最大的用途就是『造福懶惰與記憶性薄弱的人類～』 哈哈！沒錯！
DNS 系統是以所謂的階層式的管理，所以，請注意喔！那個 .tw 只記錄底下那一層的這六個主要的 domain 的主機而已！
至於例如 edu.tw 底下還有個 ncku.edu.tw 這部機器，那就直接授權交給 edu.tw 那部機器去管理了！
也就是說『 每個上一層的 DNS 主機，所記錄的資訊，其實只有其下一層的主機名稱而已！ 』
至於再下一層，則直接『授權』給下層的某部主機來管理囉！呵呵！
所以您就應該會知道 DNS 到底是如何管理的吧！ ^_^
#
reference:
http://old.linux.vbird.org/linux_server/0350dns/0350dns.php
http://dns-learning.twnic.net.tw/dns/03opDNS.html
https://emmielin.medium.com/dns-查詢流程概念筆記-3a420460d396
```
![Untitled](https://github.com/user-attachments/assets/23181e67-5306-4ad8-9f52-67585cb0e20d)
</details>


<details>
<summary>linux command</summary>

```console


	(cd … && …)
(cd … && mv …)
(cd … && cp …)
有些binary需要進到bin/去執行
也可以搭配crontab使用

	crontab
crontab -l列
crontab -e編輯
crontab -r刪除
時間 binary
#…

	ls tail
ls -la
# 僅輸出最後 5 筆檔案資訊
ls -l | tail -n 5
# 擷取第 2 個字元至第 10 個字元
ls -l | tail -n 5 | cut -c 2-10 --complement
https://blog.gtwang.org/linux/linux-cut-command-tutorial-and-examples/

	mv
$mv source/A destination/B
$mv source/A destination/
改名稱$mv source/A source/B
如果一次很多 ＊pattern並且$mv source/*.txt destination/
因為mv很多限制 就使用迴圈for I in $(ls *_20231130.txt); do mv $i ${I%_20231130.txt}_20231201.txt; done

	cp
$cp source/A destination/B
$cp source/A destination/
如果一次很多 ＊pattern並且$cp source/*.txt destination/

	cat
$cat xxx.txt
$cat xxx.txt | grep Pattern
$cat xxx.txt | grep -a Pattern
查os版本cat /etc/os-release

	tail
$tail xxx.log
可以一直印出來$tail -f xxx.log

	mkdir
$mkdir …

	rm
$rm …
$rm -rf …

	scp
$scp source/A apuser@IP:destination/B
$scp -r source apuser@IP:destination

	du df
查容量du -h Directory

	vim
編輯i
退出編輯esc
退q
不存退q！
存退wq

	*
使用萬用字元時務必加上單引號( ' )
但是我實驗起來又好像未必
find $HOME -name '*.mp3' -o -name '*.ogg'
ls -la '*20240101*'

	date
TODAY=$(date +Y%m%d)

	touch
可以產生檔案
可以更新時間戳記time stamp
touch -d "2 days ago" <file>

	echo
user=$(echo $USER)

	tar 尚未整理完整
壓.tar.gz
tar -zcvf $FILE $DIR
解
tar -zxvf $FILE

	pwd
cur_dir=$(pwd)

	ipc
查看共享內存ipcs -m
查看隊列ipcs -q
查看信號量ipcs -s

	install 舉例
ubuntu
...
redhat/centos
sudo yum install https://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-1.noarch.rpm
sudo yum install git

	diff
diff monitor.ini monitor.ini

	tail
# 僅輸出最後 5 筆檔案資訊
ls -l | tail -n 5
# 輸出最新
tail -f <file>

	awk 處理文本
https://www.runoob.com/linux/linux-comm-awk.html
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

	cut
https://blog.gtwang.org/linux/linux-cut-command-tutorial-and-examples/
# 擷取第 2-3 個、第 5-6 個與第 8-9 個字元
ls -l | cut -c 2-3,5-6,9
# 擷取 CSV 檔的第 1-3 個與第 5 個欄位
cut -d , -f 1-3,5 data.csv
# 排除 CSV 檔的第二個欄位
cut -d , -f 2 --complement data.csv

	sed
可以行作為單位編輯
https://www.runoob.com/linux/linux-comm-sed.html
sed -e 4a\newLine testfile
找pattern
/hm/
判斷第幾行
sed -n '/hm/=' getPath.sh
顯示/p 刪除/d ...
sed -n '/hm/p' getPath.sh

	histoy
history
HISTTIMEFORMAT="%d/%m/%y %T " history

	find
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
<summary>linux script</summary>

```console

	注意事項
一些前置詞不要用 像是PATH 會干擾到cat等程式
如果scrip在/ap執行 那pwd就是/ap

	在windows容易有問題
/bin/bash^M: bad interpreter
sed -i -e 's/\r$//' script.sh

	起script
./xxx.sh

	查詢script
＄ps -ef | grep xxx.sh
＄pgrep xxx.sh
找到ID:17802殺script
kill -9 -17802

	起手
#/bin/bash

	$	${} $()
BASEDIR=/Speedy/rmdata
TODAY=$(date +Y%m%d)
cat_result=$(cat $file_name | grep -a 000001)
使用
$BASEDIR
${TODAY}
${${string}:${start_pos}:${length}}
用$(( ))去 + - ...
result=$((1 + 2))

	List of files
# method (1)
#LIST FILES=$(1s -p /Speedy/rmdata/ I grep -a $TODAY I grep -v /)
# Using Is -p tells Is to append a slash to entries which are a directory, and using grep -v / tells grep to return only lines not containing a slash.
# method (2)
LIST_FILES=(${BASEDIR}/es_balance_${TODAY}.txt ${BASEDIR}/es_balance_extend_${ODAY}.txt)

	$1 $2 ...
file_name=$1
$0: shell script 的名稱

	function
#
function appendFile() {
	file_name=$1
	cat_result=$(cat $file_name I grep -a 000001)
	str_start=6
	str_len=$(expr length $cat_result - $str_start)
	append_line=000002${cat_result:$str_start:$str_len}
	echo $append line » $file name
}
for var_file in ${LIST_FILE[@]}
do
	appendFile $var_file
done
#
function mkDir() {
	DIR=$1
	if [ -d $DIR ]; then 
		#rm $1/*
		rm -rf $DIR
		mkdir -p $DIR
	else
		mkdir -p $DIR
	fi
}
mkDir <directory>
#
function add_numbers() {
    local sum=$(( $1 + $2 ))
    return $sum
}
add_numbers 5 10
echo $?

	Whether exist?
Program
which <program_name>
Directory & file:
if ! [ - d <path> ]; then echo “Directory does not exist." fi
if ! [ - f <file> ]; then echo "File does not exist." fi

	if
#
if [...]; then ...
elif [...]; then ...
else ...
fi
#
if [ $HO_RUNNING -eq 1 ]; then
#
if ! [...]; then

	loop
s_web_id...is a file
WEBID_LIST=$(cat s_web_id | awk '{print $1}')
for WEBID in ${WEBID_LIST[@]}
do
    $BATCH_PATH/UserStart.sh $WEBID
done
```
</details>

<details>
<summary>mkDir</summary>

```console
function mkDir() {
	DIR=$1
	if [ -d $DIR ]; then 
		#rm $1/*
		rm -rf $DIR
		mkdir -p $DIR
	else
		mkdir -p $DIR
	fi
}
```
</details>

<details>
<summary>ftp</summary>

```console
ftp -n $host <<END_SCRIPT
quote USER $user
quote PASS $pass
mkdir $MONTHDIR
cd $MONTHDIR
mkdir $HOSTNAME
cd $HOSTNAME
put $element
quit
END_SCRIP
```
</details>


<details>
<summary>read ini</summary>

```console
raw.ini
[session1]
key1=a
key2=b
key3=c
[session2]
key1=...
...

readIni.sh
file=raw.ini
ln=$(sed -n '/\[session1\]/=' $file)
lnd=$(($ln + 3))
sed -n $ln','$lnd'p' $file | sed -n '/key3/p' | awk -F= '{print $2}'
```
</details>

<details>
<summary>read json</summary>

```console
raw.json
{
        "message": {
                "id": 4095,
                "temperature": 409.5
        },
        "origin": "receiver",
        "protocol": "alecto_wsd17",
        "uuid": "0000-b8-27-eb-0f3db7",
        "repeats": 3
}

readJson.sh
jq .message.temperature raw.json
```
</details>

<details>
<summary>execute program / script</summary>

```console
program_name=hello.sh
$(pwd)/$program_name
./$program_name
```
</details>

<details>
<summary>get path</summary>

```console
function get_parent_path() {
  path=$(dirname $(dirname $(realpath $0)))
  echo $path
	return $path
}
get_parent_path
```
</details>

<details>
<summary>start stop</summary>

```console

使用 1
#! /bin/bash

PROG_PATH="/ap"
PROG_NAME=gitlab-runner
USER="speedy"
WORKING_DIR="/ap/apuser-runner"
CONFIG="/ap/.gitlab-runner/config.toml"

PROG_COMMAND="run --working-directory $WORKING_DIR --config $CONFIG -user $USER &"

startProg.sh $BINARY_PATH $PROC_NAME $PROG_COMMAND
注意$PROG_COMMAND字串前面一定不能是空格...似乎會判斷錯誤

使用 2
#! /bin/bash

第一種
PROC_NAME_LIST="qs_data_adapter qs_tse_stock qs_tse_other qs_otc_stock qs_otc_other qs_taifex_future qs_taifex_option qs_emg_stock"
for PROC_NAME in $PROC_NAME_LIST
do
	...
done

第二種
WEBID_LIST=$(cat $DATA_PATH/s_web_id | awk '{print $1}')
for WEBID in ${WEBID_LIST[@]}
do
    ./startProg_WEBID.sh $WEBID
done
這個就會需要多一點參數了




startProg.sh
#! /bin/bash

function GET_PID() {
	PROG_NAME=$1
	HT_PID=$(ps -efa | grep -w $PROG_NAME | grep -v grep | grep -v tail | grep -v vi | grep -v 'sh -c'| awk '{print $2}')
	return $HT_PID
}

PROG_PATH=$1
PROG_NAME=$2
PROG_COMMAND=$3

GET_PID $PROG_NAME
HT_PID=$?
if [ "x$HT_PID" != "x" ]; then
        echo ""
        echo " ++++++++++++++++++++++++++++++++++++++++"
        echo " $PROG_NAME process is already running..."
        echo " ++++++++++++++++++++++++++++++++++++++++"
        exit 0
fi

$BINARY_PATH/$PROC_NAME $PROG_COMMAND
#sleep 1

GET_PID PROG_NAME
HT_PID=$?
if [ "x$HT_PID" != "x" ]; then
        echo ""
        echo "$ ++++++++++++++++++++++++++++++++++++++++"
        echo "$ Execute $PROC_NAME success  "
        echo "$ ++++++++++++++++++++++++++++++++++++++++"
else
        echo ""
        echo " ++++++++++++++++++++++++++++++++++++++++"
        echo " Execute $PROC_NAME failed   "
        echo " ++++++++++++++++++++++++++++++++++++++++"
fi



使用stop
#! /bin/bash

PROG_NAME=gitlab-runner

stopProg.sh $PROG_NAME



stopProg.sh
#! /bin/bash

function GET_PID() {
	PROG_NAME=$1
	HT_PID=$(ps -efa | grep -w $PROG_NAME run | grep -v grep | grep -v tail | grep -v vi | grep -v 'sh -c'| awk '{print $2}')
	return $HT_PID
}

PROG_NAME=$1

GET_PID $PROG_NAME
HT_PID=$?
if [ "x$HT_PID" != "x" ]; then
	if kill -SIGQUIT $HT_PID ; then
		echo ""
    echo " ++++++++++++++++++++++++++++++++++++++++"
    echo " $PROC_NAME (pid $HT_PID) stopped  "
    echo " ++++++++++++++++++++++++++++++++++++++++"
	else
    echo ""
    echo " ++++++++++++++++++++++++++++++++++++++++"
    echo " $PROC_NAME could not be stopped   "
    echo " ++++++++++++++++++++++++++++++++++++++++"
    fi
else
    echo ""
    echo " ++++++++++++++++++++++++++++++++++++++++"
    echo " $PROC_NAME process is not running "
    echo " ++++++++++++++++++++++++++++++++++++++++"
fi

```

future ...
restart ...
https://superuser.com/questions/1362291/auto-restart-an-executable-after-the-application-crashes
</details>
