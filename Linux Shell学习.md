##Linux Shell学习##

一、数据成分：数据类型、变量申明    
二、控制成分：分支(if)、选择(case)、循环(for、while)、函数(function) 
三、文件系统操作   
四、其它一些重要Shell命令        
五、Shell中一些默认规则

### 一、数据成分
**数组**  
在Shell中，用括号来示数组，数组元素用“空格”符号分割。定义数组的一般形式为array_name=(value1 ... valuen)

**变量**  
如果需要定义自己变量，可以在函数中定义：local 变量=值，这时变量就是内部变量，修改其值不会影响函数外部相同变量的值

### 二、控制成分

  **1、分支**  
   if语句，用法如下：  
> if [ 条件判断式 ]; then  
> 当条件判断式成立时，相关工作内容  
> elif[ 条件判断式 ]；then  
> 当条件判断式成立时，相关工作内容   
> else  
> 以上所有判断式均不成立时，相关工作内容  
> fi #反写if表示结束if  

  注意if与“[”之间有空格，而且“[]”的内部两边也有空格，如果将then另起一行则不需要';'

-------------------------------
 **Shell编程中if语句内判断参数**

1.文件或目录相关判断  
用法： -b/c/d/e/f/g/h/k/p/r/s/u/w/o pathname 主要是检查文件或目录及相关属性的存在性并返回true或false  
针对文件的参数：b/c/f/h/p/s 分别对应file存在且为块文件则返回true,file存在且为字符文件则返回true,file存在且为正规文件则返回true,file存在且为符号链接文件则返回true(在一些旧系统上无效)，file存在且为命令管道则返回true,当file存在且大于0返回true

针对文件和目录的参数：d/e/g/k/r/w/u/o 分别对应路径是否为目录、存在性、文件或目录存在且设置了SGID位则为true、文件或目录存在且设置了粘滞位则为true、r、w、x表示读写及可执行，文件或目录存在且设置了SUID位则返回true,文件或目录存在且被当前过程的有效用户ID所指定的用户拥有则返回true  

判断一个变量是否为空：[ -n $var ]结果出现错误：too many agruments.原因在于：环境变量没放在双引号中，所以 bash 认为方括号中的自变量过多。可以用双引号将字符串自变量括起来消除该问题。

2.Shell中比较字符写法  
  e:equal g:greater l:less n:not t:than
  -eq/ne/gt/lt/le/ge分别对应等于、不等于、大于、小于等于、大于等于    
 -z 空串 ==两个字符相等 ！=两个字符不相等 -n非空串  



  **2、选择**  
根据变量内容进行相应的动作，使用case语句
>case $变量名称 in  
>"第一个变量内容")  
>程序段  
>;;       <==每个类别结尾使用两个连续的分号来处理！  
>"第n个变量内容")  
>程序段  
>;;  
>*)  
>程序段  
>;;  
>esac   <==最终的case结尾  
>  **3、循环**  
>循环共有两种：一种是不定循环；还有一种是固定循环。不定循环有：while do done和until do done。固定循环主要是指for do done。
>while[condition]  
>do              <==do 是循环的开始  
>程序段  
>done    <==done是循环结束  


> until [condition]  
> do  
> 程序段落    
> done  


> for var in conl con2 con3 ...  
> do  
> 程序段  
> done  

  **4、函数**  
函数可以在shell script当中做出一个类似自定义执行指令的东西，最大的功能是，可以简化很多程序代码。
>function funcation_name() {  
>程序段  
>}

因为shell script的执行方式是自上而下，由左而右，因此在shell script当中的function的设定一定要在程序的最前面，这样才能够在执行时被找到可用的程序段。另外函数的声明，可以不加关键词function。调用时直接一个函数名即可不需要括号。

### 三、文件系统操作  
**1.磁盘与目录容量**  
（1）df-列出文件系统的整体磁盘使用量  
	使用方法：df [-ahikHTm] [目录或文件名]  

 (2) du-评估文件系统的磁盘使用量  
	使用方法：du [-ahskm] 档案或目录名称    
**2.文件的压缩与打包**  
（1）tar [OPTION...] [FILE]...  
选项与参数：  
  -c:建立打包档案，可搭配-v来察看过程中被打包的文档名  
  -x:从归档文件中抽取文件即解压缩  
  -j: filter the archive through bzip2.透过bzip2的支持进行压缩/解压缩，此时文档名最好为\*.tar.bz2   
  -t:列举出归档文件中的内容  
  -v:列出压缩/解压缩过程中处理的文件名  
  -z:filter the archive through gzip.透过gzip的支持进行压缩/解压缩，此时文档名最好为*.tar.gz  
  -f:filename，-f 后面要立刻接要被处理的文档名  
  -C 目录：解压缩至特定目录  

压缩： tar -jcv -f filename.tar.bz2 要被压缩的档案或目录名称  
查询： tar -jtv -f filename.tar.bz2  
解压缩：tar -jxv filename.tar.bz2 -C 将要解压到的目录

**3.文件查找find**  
 语法：find pathname -options [-print -exec -ok]  
参数：  
   pathname:find命令所查找的目录路径。    
 	-print:find命令将匹配的文件输出到标准输出    
    -exec: find命令对匹配的文件执行该参数所给出的shell命令。  
    -ok: 和-exec的作用相同，不过以安全的模式来执行该参数所给出的shell命令，执行询问是否执行。  
 find命令选项：  
   -name:按照文件名查找  
   -perm:按照文件权限查找  
   -prune: 使用find命令不在当前指定的目录中查找，同时使用-depth该命令会被忽略    
   -user或-group:按照文件用户或用户组查找    
   -mtime -n/+n:按照文件的更改时间来查找文件，-n表示文件更改时间距现在n天以内，+n表示文件更改时间距现在n天以前。    
   -nogroup/-nouser:查找无效用户组/用户的文件。
   -newer file1 ! file2:查找更改时间比文件1新但比文件2旧的文件  
   -type:查找某一类型的文件  
   -size/depth/fstype/mount/follw/cpio:  

### 四、其它一些重要的Shell命令

**1.awk**    
awk '{pattern + action}' filenames 
调用方式：  
1.命令行方式  
awk [-F field-separator] 'commands' input-file(s)
其中，commands是真正awk命令，[-F域分隔符]是可选的，input-file(s)是待处理的文件。
在awk中文件的每一行中，由域分隔符分开的每一项称为一个域。默认情况下域分隔符是空格。  
2.shell脚本方式  
将所有的awk命令插入一个文件，并使awk程序可执行，然后awk命令解释器作为脚本的首行，一般通过键入脚本名称来调用。相当于shell脚本首行的：#！/bin/sh 可以换成：#！/bin/awk   
3.将所有的awk命令插入一个单独文件，然后调用:
awk -f awk-script-file input-file(s)
其中，-f选项加载awk-script-file中的awk脚本，input-file(s)表示输入文件

gsub函数：    
**2.sed**   
sed是一种在线编辑器，它一次处理一行内容。处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”(pattern space)，接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下一行，这样不断重复，直至文件末尾。文件内容并没有改变，除非使用重定向存储输出。Sed主要用来自动编辑一个或多个文件；简化对文件的反复操作；编写转换程序等。
sed命令行格式为： sed [-nefri] 'command' file(s) 或 
sed [options] -f scriptfile file(s)  
常用选项：
  >-n:使用安静（silent）模式。在一般sed的用法中，所有来自STDIN的资料一般都会被列出到屏幕上。加上-n参数后，则只有经过sed特殊处理的那一行（或者动作）才会被列出来      
  >-e∶直接在指令列模式上进行 sed 的动作编辑；  
  >-f∶直接将 sed 的动作写在一个档案内， -f filename 则可以执行 filename 内的sed 动作；  
  >-r∶sed 的动作支援的是延伸型正规表示法的语法。(预设是基础正规表示法语法)  
  >-i∶直接修改读取的档案内容，而不是由萤幕输出。         

常用命令：  
   >a \∶ 新增， a 的后面可以接字串，而这些字串会在新的一行出现(目前的下一行)eg:sed '1a drink tea' 
   >c \∶ 取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！  
   >d ∶ 删除, eg:sed '2,$d' file-----删除file文件的第二行到末尾所有行    
   >i ∶ 插入， i 的后面可以接字串，而这些字串会在新的一行出现(目前的上一行)；  
   >p ∶ 列印，亦即将某个选择的资料印出。通常 p 会与参数 sed -n 一起运作～  
   >s ∶ 取代，可以直接进行取代的工作哩！通常这个 s 的动作可以搭配正规表示法！eg:sed 's/\(love\)able/\1rs/p' file---love被标记为1，所有loveable会被替换成lovers且替换的行会被打印出来。    
   >  eg: sed 's/要替换的字符串/新的字符串/g'  
   >
   >sed 's/要替换的字符串/新的字符串/g'    #替换一行中的某部分，要替换字符串可用正则
   >
   >sed -i '/匹配字符串/d' filename  #删除匹配行
   >
   >sed -i '/匹配字符串/s/替换源字符串/替换目标字符串/g' filename   #替换匹配行中的某个字符串
   >
   >sed -i '/匹配字符串/s/^#//' filename 去掉匹配行的注释
   >
   >sed -i '/匹配字符串/s/^/#&/' filename 加匹配行的注释
   >
   >sed -i '/匹配字符串/s/hbase$/hyperbase1/' filename 将匹配行行尾的hbase换成hyperbase1

**3.合并与分割（sort,uniq,join,cut,paste,split）**

**cut [-d,-c,-f]**
这个指令可以将一段以行为单位的讯息的某一段切出来
-d:后面接分隔符，与-f一起使用
-f:根据-d的分隔符将相应段的信息取出
-c:以字符(characters)的单位取出固定字符区间
eg: cat /etc/passwd | cut -d ':' -f 1  #取出passwd文件中的所有用户名
eg: cut -c 12-  #以行为单位，取出字符的第12个字符以后的字符，包括第12个字符。




xargs
前面的输出转换为后方指令的参数输入.
eg: 查找系统中的每一个普通文件，然后使用xargs命令测试它们分别属于哪些文件。  
find . type f -print|xargs file  
统计当前目录下所有文件的大小含目录，精确到字节
find . -type f -print|xargs ls -l|awk 'BEGIN{size=0}{size+=$5};END{print size}'  

Shell中的加减乘除
shell包含三个命令：bc、expr、let可用于数值计算
eg：echo 300 + 5 |bc
   expr 300 + 5; expr 300 \* 5;
   let i=300+5|echo $i

日期显示：date  
 > $ date "+%Y-%m-%d"  ==> 显示为：2016-05-18  
 > $ date "+%Y/%m/%d"  ==>显示为：2016/05/18  
 > $ date "+%Y-%m-%d %H:%M:%S"  ==>2016-05-18 10:14:20  

**4  用于判断的参数**
[ -e  filename ] 如果filename存在，则为真
[ -a filename ]  如果filename存在则为真
[ -d  filename ] 如果filename为目录，则为真
[ -f   filename ] 如果filename为常规文件，则为真
[ -r/w/x   filename ]分别对应文件是否可读可写可执行
[ filename1 -nt filename2 ]  如果filename1比filename2新，则为真。or 如果 FILE1 exists and FILE2 does not则为真
[ filename1 -ot filename2 ]  如果filename1比filename2旧，或者 FILE2 存在且 FILE1 不存在则为真
[ filename1 -ef filename2 ]  如果filenam1和filename2指向相同的设备名和节点号则为真
字符串比较
[ -z string ]  "string"的长度为零则为真
[ -n string ] or [ string ] "string"的长度为非零non-zero则为真
[ string1==string2 ] 如果2个字符串相同。 “=” may be used instead of “==” for strict POSIX compliance则为真。
[ string1 != string2 ] 如果字符串不相等则为真

[ -s "$FILE" ] 文件大小非0时为真

### 五、Shell中的一些默认规则  
**1.$?、$#、$@、$!**  
$#表示传给脚本的参数个数，不包括当前可执行文件的在内
$?表示上一个命令执行的返回值  
$@表示所有参数列表。
$!表示最后运行的后台Process的PID     
**2.>、>>、<、<<、<<<**

**3.()、(())、[]、[[]]、{}**  
()可以用于命令组、命令替换及初始化数组；(())用于整数扩展，这种扩展计算是整数型的计算，不支持浮点型。常用于算术运算比较；[]在bash的内部命令中和test是等同的；[[]]，[[是bash程序语言的关键字，并不是一个命令比单中括号更加通用；｛｝大括号拓展，代码块  
**4.正则表达式**  

**5.atime、mtime、ctime**
atime: access time
mtime: modify time
ctime: change time
mtime与ctime的区别在于，只有修改了文件内容，才会更新文件的mtime，而对于文件更名，修改文件的属主等操作，只会更新ctime。atime每次查看文件内容的时候会更新，如cat，而ls操作是不会更新的。
ls -lc filename 列出文件的ctime
ls -lu filename 列出文件的atime
ls -l filename 列出文件的mtime



**6.shell语句中经常用到的**  
**(1).>/dev/null 2>&1**
>/dev/null 代表空设备文件 
>代表重定向到哪里，例如：echo "123" > /home/123.txt 
>1 表示stdout标准输出，系统默认值是1，所以">/dev/null"等同于"1>/dev/null" 
>2 表示stderr标准错误 
>& 表示等同于的意思，2>&1，表示2的输出重定向等同于1

**(2).${varname:-word}**  
如果varname存在且非null，则返回其值；否则，返回word。用途：如果变量未定义，则返回默认值。

**(3).rsync**  
Rsync的命令格式可以为以下六种：   
　　rsync [OPTION]... SRC DEST   
　　rsync [OPTION]... SRC [USER@]HOST:DEST   
　　rsync [OPTION]... [USER@]HOST:SRC DEST   
　　rsync [OPTION]... [USER@]HOST::SRC DEST   
　　rsync [OPTION]... SRC [USER@]HOST::DEST   
　　rsync [OPTION]... rsync://[USER@]HOST[:PORT]/SRC [DEST]   
　　对应于以上六种命令格式，rsync有六种不同的工作模式：   
　　1)拷贝本地文件。当SRC和DES路径信息都不包含有单个冒号":"分隔符时就启动这种工作模式。如：rsync -a /data /backup   
　　2)使用一个远程shell程序(如rsh、ssh)来实现将本地机器的内容拷贝到远程机器。当DST路径地址包含单个冒号":"分隔符时启动该模式。如：rsync -avz *.c foo:src   
　　3)使用一个远程shell程序(如rsh、ssh)来实现将远程机器的内容拷贝到本地机器。当SRC地址路径包含单个冒号":"分隔符时启动该模式。如：rsync -avz foo:src/bar /data   
　　4)从远程rsync服务器中拷贝文件到本地机。当SRC路径信息包含"::"分隔符时启动该模式。如：rsync -av root@172.16.78.192::www /databack   
　　5)从本地机器拷贝文件到远程rsync服务器中。当DST路径信息包含"::"分隔符时启动该模式。如：rsync -av /databack root@172.16.78.192::www   
　　6)列远程机的文件列表。这类似于rsync传输，不过只要在命令中省略掉本地机信息即可。如：rsync -v rsync://172.16.78.192/www     
**rsync经常用的参数**  
-a,  --archive 归档模式，表示以递归方式传输文件，并保持所有文件属性，等于-rlptgoD
-v,  --verbose 详细模式输出
-x,  --one-file-system 不要跨越文件系统边界
-P,  等同于 --partial 保留那些因故没有完全传输的文件，以是加快随后的再次传输

**rsync参数的具体解释如下：**   
-v, --verbose 详细模式输出   
-q, --quiet 精简输出模式   
-c, --checksum 打开校验开关，强制对文件传输进行校验   
-a, --archive 归档模式，表示以递归方式传输文件，并保持所有文件属性，等于-rlptgoD   
-r, --recursive 对子目录以递归模式处理   
-R, --relative 使用相对路径信息   
-b, --backup 创建备份，也就是对于目的已经存在有同样的文件名时，将老的文件重新命名为~filename。可以使用--suffix选项来指定不同的备份文件前缀。   
--backup-dir 将备份文件(如~filename)存放在在目录下。   
-suffix=SUFFIX 定义备份文件前缀   
-u, --update 仅仅进行更新，也就是跳过所有已经存在于DST，并且文件时间晚于要备份的文件。(不覆盖更新的文件)   
-l, --links 保留软链结    
-L, --copy-links 想对待常规文件一样处理软链结   
--copy-unsafe-links 仅仅拷贝指向SRC路径目录树以外的链结   
--safe-links 忽略指向SRC路径目录树以外的链结   
-H, --hard-links 保留硬链结   
-p, --perms 保持文件权限   
-o, --owner 保持文件属主信息   
-g, --group 保持文件属组信息   
-D, --devices 保持设备文件信息   
-t, --times 保持文件时间信息   
-S, --sparse 对稀疏文件进行特殊处理以节省DST的空间   
-n, --dry-run现实哪些文件将被传输   
-W, --whole-file 拷贝文件，不进行增量检测   
-x, --one-file-system 不要跨越文件系统边界   
-B, --block-size=SIZE 检验算法使用的块尺寸，默认是700字节   
-e, --rsh=COMMAND 指定使用rsh、ssh方式进行数据同步   
--rsync-path=PATH 指定远程服务器上的rsync命令所在路径信息   
-C, --cvs-exclude 使用和CVS一样的方法自动忽略文件，用来排除那些不希望传输的文件   
--existing 仅仅更新那些已经存在于DST的文件，而不备份那些新创建的文件   
--delete 删除那些DST中SRC没有的文件   
--delete-excluded 同样删除接收端那些被该选项指定排除的文件   
--delete-after 传输结束以后再删除   
--ignore-errors 及时出现IO错误也进行删除   
--max-delete=NUM 最多删除NUM个文件   
--partial 保留那些因故没有完全传输的文件，以是加快随后的再次传输   
--force 强制删除目录，即使不为空   
--numeric-ids 不将数字的用户和组ID匹配为用户名和组名   
--timeout=TIME IP超时时间，单位为秒   
-I, --ignore-times 不跳过那些有同样的时间和长度的文件   
--size-only 当决定是否要备份文件时，仅仅察看文件大小而不考虑文件时间   
--modify-window=NUM 决定文件是否时间相同时使用的时间戳窗口，默认为0   
-T --temp-dir=DIR 在DIR中创建临时文件   
--compare-dest=DIR 同样比较DIR中的文件来决定是否需要备份   
-P 等同于 --partial   
--progress 显示备份过程   
-z, --compress 对备份的文件在传输时进行压缩处理   
--exclude=PATTERN 指定排除不需要传输的文件模式   
--include=PATTERN 指定不排除而需要传输的文件模式   
--exclude-from=FILE 排除FILE中指定模式的文件   
--include-from=FILE 不排除FILE指定模式匹配的文件   
--version 打印版本信息   
--address 绑定到特定的地址   
--config=FILE 指定其他的配置文件，不使用默认的rsyncd.conf文件   
--port=PORT 指定其他的rsync服务端口   
--blocking-io 对远程shell使用阻塞IO   
-stats 给出某些文件的传输状态   
--progress 在传输时现实传输过程   
--log-format=formAT 指定日志文件格式   
--password-file=FILE 从FILE中得到密码   
--bwlimit=KBPS 限制I/O带宽，KBytes per second   
-h, --help 显示帮助信息  

**(4).declare**  
**用途说明：**declare命令是bash的一个内建命令，它可以用来声明shell变量，设置变量的属性（Declare variables and/or give them attributes）。该命令也可以写作typeset。    
语法：declare [+/-][rxi][变量名称=设置值]或declare -f
参数：
>**+/-:** "-"可用来指定变量的属性，"+"则是取消变量所设的属性  
>-f: 仅显示函数  
>r  : 将变量设置为只读  
>x  : 指定的变量会成为环境变量，可供shell以外的程序来使用  
>i  : [设置值]可以是数值，字符串或运算式。
>a  : 将变量声明为数组变量。

**(5).Shell文件描述符**  
在Shell执行命令时，每个进程都和三个打开的文件相联系，并使用文件描述符来引用这些文件。这三个文件分别是标准输入，标准输出，错误输出分别对应着0，1，2三个文件描述符。
> exec 3</tmp/1.txt 以只读方式打开/tmp/1.txt，文件描述符对应为3
> exec 3>/tmp/1.txt 以只写方式打开/tmp/1.txt, 文件描述符对应为3
> exec 3<>/tmp/1.txt 以读写方式打开/tmp/1.txt，文件描述符对应为3
> exec 3<&- 关闭文件描述符3

**(6).${}总结**  
${}并不是专门为提取文件名或目录名的，它的使用是变量的提取和替换等等操作，它可以提取非常多的内容。
>\#:表示从左边算起第一个  
>%：表示从右边算起第一个  eg: var=135.txt.backup  echo ${var%.*}  显示的是135.txt
>\##:表示从左边算起最后一个  
>%%：表示从右边算起最后一个  
>\#总是表示从左边算起，%总是表示从右边算起


**(7).tcpdump命令**

**(8).netstat命令**
常见参数
-a(all)显示所有选项，默认不显示LISTEN相关
-t(tcp)仅显示tcp相关选项
-u(udp)仅显示udp相关选项
-n 拒绝显示别名，能显示数字的全部转化成数字
-l 仅列出有在Listen(监听)的服务状态
-p 显示建立相关链接的程序名
-r 显示路由信息，路由表
-e 显示扩展信息，例如uid等
-s 按各个协议进行统计
-c 每隔一个固定时间，执行该netstat命令
note:LISTENLISTENING的状态只有用-a或者-l才能看到

**(9). ps命令**
a 显示所有进程
-a 显示同一终端下的所有程序
-A 显示所有进程
c 显示进程的真实名称
-N 反向选择
-e 等于“-A”
e 显示环境变量
f 显示程序间的关系
-H 显示树状结构
r 显示当前终端的进程
T 显示当前终端的所有程序
u 指定用户的所有进程

p pidlist 通过进程ID进行选择，标识为-p与 --pid

-au 显示较详细的资讯
-aux 显示所有包含其他使用者的行程
-C   列出指定命令的状况
--lines  每页显示的行数
--width  每页显示的字符数
--version 显示版本信息

chattr命令
linux系统下的档案会有很多attributes,这些attributes的使用对于保证系统的安全有很大的帮助。档案的属性有很多，其中用的比较多的有两个：
- i属性，一旦档案具有这个属性，则该文件既不能被删除也不能被重命名，也不能创建链接文件，而且也不能向该档案中写入数据

- a属性，如果档案具有此属性，则只能向该档案中append数据，通常这两个属性都是用来保护系统中非常重要的档案。




**(10).如何根据端口号找到相应的进程**
- 使用lsof 命令

isof -i:port_number|grep "(LISTEN)"

lsof命令参数解释 
1) -P :这个选项约束着网络文件的端口号到端口名称的转换。约束转换可以使lsof运行得更快一些。在端口名称的查找不能奏效时，这是很有用的。 
2) -n : 这个选项约束着网络文件的端口号到主机名称的转换。约束转换可以使lsof的运行更快一些。在主机名称的查找不能奏效时，它非常有用。 
3) -l :这个选项约束着用户ID号到登录名的转换。在登录名的查找不正确或很慢时，这个选项就很有用。 
4) +M :此选项支持本地TCP和UDP端口映射程序的注册报告。 
5) -i4 :仅列示IPv4协议下的端口。 
6) -i6 : 仅列示IPv6协议下的端口。
- 使用netstat命令

>netstat -nltp|awk 'BEGIN{prt=":PORT"}{if($4 ~ prt) print $0}'

硬件RAID，通过已安装的RAID厂商或服务器厂商的管理工具来查看，如HP服务器的Smart Array用hpacucli命令；IBM服务器ServerRAID可以用MegaCli工具。
软RAID, cat /proc/mdstat 可以查看RAID级别，状态等信息

Crontab是一个很方便的在unix/linux系统上定时(循环)执行某个任务的程序
使用cron服务，用 service crond status 查看 cron服务状态，如果没有启动则 service crond start启动它，
cron服务是一个定时执行的服务，可以通过crontab 命令添加或者编辑需要定时执行的任务：

\*　　\*　　\*　　\*　　\*　　command 
分　时　日　月　周　命令 
第1列表示分钟1～59 每分钟用*或者 */1表示 
第2列表示小时1～23（0表示0点） 
第3列表示日期1～31 
第4列表示月份1～12 
第5列标识号星期0～6（0表示星期天） 
第6列要运行的命令 
crontab -u //设定某个用户的cron服务，一般root用户在执行这个命令的时候需要此参数
crontab -l //列出某个用户cron服务的详细内容
crontab -r //删除没个用户的cron服务
crontab -e //编辑某个用户的cron服务
比如说root查看自己的cron设置：crontab -u root -l
再例如，root想删除fred的cron设置：crontab -u fred -r
在编辑cron服务时，编辑的内容有一些格式和约定，输入：crontab -u root -e
进入vi编辑模式，编辑的内容一定要符合下面的格式：\*/1 \* \* \* \* ls >> /tmp/ls.txt
编辑/etc/crontab文件，在末尾加上一行： 30 5 * * * root init 6 这样就将系统配置为了每天早上5点30自动重新启动。
需要将crond设置为系统启动后自动启动的服务，可以在/etc/rc.d/rc.local 中，在末尾加上
service crond start
很多时候，我们计划任务需要精确到秒来执行，根据以下方法，可以很容易地以秒执行任务。
以下方法将每10秒执行一次
>\# crontab -e
>\* * * * * /bin/date >>/tmp/date.txt
>\* * * * * sleep 10; /bin/date >>/tmp/date.txt
>\* * * * * sleep 20; /bin/date >>/tmp/date.txt
>\* * * * * sleep 30; /bin/date >>/tmp/date.txt
>\* * * * * sleep 40; /bin/date >>/tmp/date.txt
>\* * * * * sleep 50; /bin/date >>/tmp/date.txt







yum命令详解

[yum](http://www.cnblogs.com/chuncn/archive/2010/10/17/1853915.html)（全称为 Yellow dog Updater Modified）是一个在Fedora和RedHat以及SUSE中的Shell前端软件包管理器。基於RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包，无须繁琐地一次次下载、安装。yum提供了查找、安装、删除某一个、一组甚至全部软件包的命令。

1 安装
yum install 全部安装
yum install package1 安装指定的安装包package1
yum groupinsall group1 安装程序组group1

2 更新和升级
yum update 全部更新
yum update package1 更新指定程序包package1
yum check-update 检查可更新的程序
yum upgrade package1 升级指定程序包package1
yum groupupdate group1 升级程序组group1

3 查找和显示
yum info package1 显示安装包信息package1
yum list 显示所有已经安装和可以安装的程序包
yum list package1 显示指定程序包安装情况package1
yum groupinfo group1 显示程序组group1信息yum search string 根据关键字string查找安装包

4 删除程序
yum remove &#124; erase package1 删除程序包package1
yum groupremove group1 删除程序组group1
yum deplist package1 查看程序package1依赖情况

5 清除缓存
yum clean packages 清除缓存目录下的软件包
yum clean headers 清除缓存目录下的 headers
yum clean oldheaders 清除缓存目录下旧的 headers
yum clean, yum clean all (= yum clean packages; yum clean oldheaders) 清除缓存目录下的软件包及旧的headers





set -e 这句语句告诉bash如果任何语句的执行结果不是true则应该退出。这样的好处是防止错误像滚雪球般变大导致一个致命的错误，而这些错误本应该在之前就被处理掉。

如果要增加可读性，可以使用set -o errexit，作用与set -e相同。

command || {echo "command failed"; exit 1; }

command

if [ "$?" -ne 0 ]; then 

​        echo  " command failed";

​         exit 1;

fi



umask 0022

权限有四部分：setUid/setGid/粘着位(sticky)、属主权限、组权限、其他用户的权限通常用到的是后3个对于umask来说，有效的设定值只有后三位数。在bash中，虽然可带入四位数字，但第一位数字必须为0。



强制卸载：

rpm -aq|grep PROGRAM|xargs rpm -e --nodeps 


hello world


任务：

统计单词数：

sed 's/[,.:;/!?]//g' file | awk '{for(i=1;i<=NF;i++) array[$i]++;}END{for(i in array) print i,array[i]}'

awk 'BEGIN{RS="[,.:;/!?]"}{for(i=1;i<=NF;i++)array[$i]++;}END{for(i in array) print i,array[i]}' file


sshpass
