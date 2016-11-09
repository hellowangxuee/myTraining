----------------------
### 命令行结构  
shell 是对用户提出的运行程序的请求进行解释的程序。  
shell 分析命令时，认为 | 的优先级高于 ；  
tee 命令将通过管道的数据流截获并放进某个文件，他不是shell的一部分，用途之一是将中间结果保存到一个文件中去  
另一个命令结束符 & ，他与换行符或者分号的功能基本一致，但他可以告诉 shell 不必等待命令结束  
$ long-running-command &    //长时间运行的进程 id  
$ sleep 5       //5秒之后出现提示符  
$ & 表示命令的终止  
$ echo hello >junk  
$ >junk echo hello       //两条命令是一样的效果，先运行带一个参数的echo ， 再把输出放入文件junk 中。
### 元字符
|元字符|说明|
|----|-----
|>|prog>file 将标准输出定向到file
|>>|prog>>file 将标准输出附加到file
|<|prog< file 从文件中获取标准输出
|||p1 | p2 将p1的标准输出作为p2的标准输入
|<< str|here document:标准输入从here document读入，知道出现下一个str
| * |匹配文件名中的零个或多个字符
|?|匹配文件名中任意单个字符
|{CCC}|匹配文件名中CCC范围内的任意字符，如0~9或a~z都是合法的
|;|命令结束符：p1;p2运行p1，再运行p2
|&|与;类似，但不等p1结束
|'...'|运行...中的命令，输出结果代替'...'
|(...)|在子shell 里运行...的命令
|{...}|在当前shell 运行...中的命令（很少使用）
|$1、$2等|$0......$9可代表shell 文件的参数
|$var|shell 变量var 的值
|${var}|变量var的值
|\\|\c将字符c作为文字，但\后面的加换行符无效
|'...'|...表示文字
|"..."|在...中的$、'...'和\得到解释后，将...作为文本文字
|#|表示注释的开始
|Var=值|为变量var赋值
|p1&&p2|运行p1;若成功，再运行p2
|p1\|\|p2|运行p1；若不成功，再运行p2
如果不想使用元字符的特殊功能，可以有几种方法来表示他们。防止他们被shell 解释  
$ echo '\*\*\*'  
\*\*\*  
也可以使用双引号"..."，但是 shell 会在双引号中寻找 $，"..."以及\，因此如果用户不想处理双引号中的字符串，就不要使用双引号。  
$ echo \\\*\\\*\\\*  
一种引号可以保护另一种引号：  
$ echo "Don't do that!"  
Don't do that!  
引号不必包含整个参数：  
$ echo x'\*'y  
x\*y  
$ echo '\*'A'?'  
\*A?  
$ echo abc\\  
\> def\\  
\> ghi  
abcdefghi  
行末尾加入\可以表示该行未完，避免 shell 中单行输入过长  
### 创建新命令
如果一个命令序列要被反复执行多次，那么最好把他组织成一个新命令，这样就可以像使用常规命令那样执行它。  
例：  
用管道线命令统计用户数量：  
$ who | wc -l  
要实现它，可以创建一个名为 nu 的新程序来完成这件工作。  
$ echo 'who | wc -l' >nu  
shell 和编辑器、who 或 wc 一样，也是一个程序，名字为 sh 。 
*******
终于可以直接复制粘贴了，再也不用自己用文字编辑命令了，好开心！！！   
#### 切换为root用户： 
1. sudo -i  

#### Ubuntu 下设置系统默认搜索路径的方法
1. 打开 /etc/profile
2. 在profile 文件最后加入脚本或可执行文件的路径，多个路径用：隔开  ，以 /usr/you/bin/nu 为例
3. 保存关闭 profile 文件，执行
4. 输出当前PATH值，发现已经有了想要的搜索路径。示例如下：
```Linux
sudo gedit /etc/profile

export PATH=$PATH:/usr/you/bin

source /etc/profile

echo $PATH
```
```Linux
baby-wx@babywx-B85M-HD3:~/wx$ echo 'who | wc -l' >nu
baby-wx@babywx-B85M-HD3:~/wx$ ls
nu
baby-wx@babywx-B85M-HD3:~/wx$ cat nu
who | wc -l
baby-wx@babywx-B85M-HD3:~/wx$ sh < nu
2
baby-wx@babywx-B85M-HD3:~/wx$ sh nu
2
```
如果一个文件是可执行的，且他包含文本文件，则 shell 认为这是一个 shell 命令组成的文件，成为 shell 文件。所以只需要让  nu 变成可执行的即可。  
```linux
baby-wx@babywx-B85M-HD3:~/wx$ chmod +x nu  
```
一般来说， nu 只有在当前目录中才能执行，为了使 nu 可以在任何工作目录下运行，可将它移到 bin 目录下，并将 /usr/you/bin 放入查找路径  
### 命令参数
shell 执行一个命令文件的时候，用第一个参数代替 $1 ,用第二个参数代替 $2 , 以此类推，直到 $9 。$* 表示任意串  
一个 shell 文件的参数不一定是文件名，例如，查询个人电话目录：  
在 /usr/you/lib/phone-book 的文件中包含：  
```Linux
dial-a-joke 212-976-3838
dial-a-prayer 212-246-4200
dow jones report 212-976-4141
```
可以使用 grep 命令进行查找，搜索任何你想要查找的信息。  
```linux
# echo 'grep $* /usr/you/lib/phone-book' >411
# cx 411
# mv 411 /usr/you/bin
# 411 joke
dial-a-joke 212-976-3838
# 411 dial
dial-a-joke 212-976-3838
dial-a-prayer 212-246-4200
# 411 'dow jones'
grep: jones: 没有那个文件或目录
/usr/you/lib/phone-book:dow jones report 212-976-4141
```
显然最后一个包含空格的出了问题，空格将它变成了两个参数，显然是不可以的。补救措施：使用shell 的双引号。尽管单引号内部的字符串不佳解释，但 shell 会在双引号中寻找 $、\、和'...' 等特殊字符。因此可以修改411：  
```Linux
grep "$*" /usr/you/lib/phone-book
```
这样 $* 将会被参数代替，但它将作为一个参数而传递给 grep ，即使它包含空格。  
```Linux
root@babywx-B85M-HD3:/usr/you/lib# 411 'dow jones'
dow jones report 212-976-4141
```
### 程序输出作为参数
任何程序的输出都可以放入命令行，只要使用（''）包住：  
### shell 变量
不是所有的 shell 变量都有特定的含义。你可以创建新的变量，给他们赋值。通常特殊意义的变量都是大写，因此普通变量名就用小写。  
set 可以显示你定义的所有变量的值。如果只想查看一两个变量使用 echo 就够了。  
一个变量的值与创建它的 shell 有关，而且该值并不会传递给子 shell。  
```Linux
baby-wx@babywx-B85M-HD3:/usr/you$ x=Hello
baby-wx@babywx-B85M-HD3:/usr/you$ sh
$ echo $x

$ ctl-d
baby-wx@babywx-B85M-HD3:/usr/you$ echo $x
Hello
```
有时候通过 shell 文件来改变 shell 变量的值是很有用的。
```Linux
root@babywx-B85M-HD3:~# echo 'x="Good Bye"
> echo $x' >setx
root@babywx-B85M-HD3:~# cat setx
x="Good Bye"
echo $x
root@babywx-B85M-HD3:~# echo $x               # x 在初始 shell 中是 Hello
Hello
root@babywx-B85M-HD3:~# sh setx               # x 在子 shell 中是 Good Bye，但在初始 shell 中还是 Hello
Good Bye
root@babywx-B85M-HD3:~# echo $x
Hello
```
符号 2&>1 让shell 将标准错误输出合并到标准输出流中。还可以 1>&2 把标准输出流加入到标准错误输出流中：  
 echo this 1>&2  
在错误输出上打印结果。在 shell 中可以将信息送往终端，而不会偶然被遗忘在一个管道或者文件中。  
here document:文件的输入就是该文件的一部分，而不是别的文件。 <<指示了这种结果，后面紧接着的单字用于终止输入， shell 要替换 here document 里的 $、'...'和\，除非单词前用了反斜杠。这种文件被看作文本文件。  
### shell 程序里的循环
for  变量  in 文件列表  
do  
    命令  
done  
例：  
```linux
$ for i in *
> do
>      echo $i
> done
```

