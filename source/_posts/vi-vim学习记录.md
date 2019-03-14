---
title: vi/vim学习记录
date: 2018-12-13 15:12:01
thumbnail: https://i.imgur.com/ZtvyyvX.jpg #/images/Linux/键盘飞牙图.jpg
categories: [Linux,数据操作]
tags: vi/vim编辑器
---

## 随便说几句 #
&emsp;&emsp;在Linux上，有着各式各样的文本编辑器，为什么非要学习vi/vim呢？原因无外乎有以下几点：
&emsp;&emsp;&emsp;&emsp;- 所有的UNIK-like系统都会内置vi文本编辑器，其它的不一定存在；
&emsp;&emsp;&emsp;&emsp;- 很多软件的编辑借口都会主动调用vi（crontab、visudo、edquota等）；
&emsp;&emsp;&emsp;&emsp;- vim具有程序编辑的能力，可以主动地通过自体颜色来辨别语法的正确性；
&emsp;&emsp;&emsp;&emsp;- 程序简单，编辑速度很快。
<!-- more -->
&emsp;&emsp;也就是说，不论如何你都能在Linux上使用到vi编辑器，而不会因为更换了一个系统，又要重新学习编辑器的使用。不会使用vi，那么很多命令根本无法操作（crontab、visudo、edquota），所以不管怎样，都需要学习vi编辑器。

## vi/vim按键说明 #
以下是大部分vi/vim的按键说明表

**第一部份：一般模式可用的光标移动、复制粘贴、搜索替换等**<table> <tr><th colspan="2" bgcolor=#555><font color="#FFFFFF"> 移动光标的方法 </font></th></tr> <tr><td bgcolor=#F5F5DC width=25%> h 或 向左箭头键(←)</td><td bgcolor=#F8F8FF> 光标向左移动一个字符 </td></tr> <tr><td bgcolor=#F5F5DC width=25%> j 或 向下箭头键(↓)</td><td bgcolor=#F8F8FF> 光标向下移动一个字符 </td></tr> <tr><td bgcolor=#F5F5DC width=25%> k 或 向上箭头键(↑)</td><td bgcolor=#F8F8FF> 光标向上移动一个字符 </td></tr> <tr><td bgcolor=#F5F5DC width=25%> l 或 向右箭头键(→)</td><td bgcolor=#F8F8FF> 光标向右移动一个字符 </td></tr> <tr><td colspan="2"> 如果你将右手放在键盘上的话，你会发现 hjkl 是排列在一起的，因此可以使用这四个按钮来移动光标。如果想要进行多次移动的话，例如向下移动 30 行，可以使用 "30j" 或 "30↓" 的组合按键，亦即加上想要进行的次数(数字)后，按下动作即可！</td></tr> <tr><td bgcolor=#F5F5DC width=25%> [Ctrl] + [f]</td><td bgcolor=#F8F8FF> 屏幕『向下』移动一页，相当于 [Page Down]按键 (常用)</td></tr> <tr><td bgcolor=#F5F5DC width=25%> [Ctrl] + [b]</td><td bgcolor=#F8F8FF> 屏幕『向上』移动一页，相当于 [Page Up] 按键 (常用) </td></tr> <tr><td bgcolor=#F5F5DC width=25%> [Ctrl] + [d]</td><td bgcolor=#F8F8FF> 屏幕『向下』移动半页 </td></tr> <tr><td bgcolor=#F5F5DC width=25%> [Ctrl] + [u]</td><td bgcolor=#F8F8FF> 屏幕『向上』移动半页 </td></tr> <tr><td bgcolor=#F5F5DC width=25%> + </td><td bgcolor=#F8F8FF> 光标移动到非空格符的下一行 </td></tr> <tr><td bgcolor=#F5F5DC width=25%> - </td><td bgcolor=#F8F8FF> 光标移动到非空格符的上一行 </td></tr> <tr><td bgcolor=#F5F5DC width=25%> n<space>	 </td><td bgcolor=#F8F8FF> 那个 n 表示『数字』，例如 20 。按下数字后再按空格键，光标会向右移动这一行的 n 个字符。例如 20<space> 则光标会向后面移动 20 个字符距离。</td></tr> <tr><td bgcolor=#F5F5DC width=25%> 0 </td><td bgcolor=#F8F8FF> 或功能键[Home]	这是数字『 0 』：移动到这一行的最前面字符处 (常用)</td></tr> <tr><td bgcolor=#F5F5DC width=25%> $ </td><td bgcolor=#F8F8FF> 或功能键[End]	移动到这一行的最后面字符处(常用)</td></tr> <tr><td bgcolor=#F5F5DC width=25%> H </td><td bgcolor=#F8F8FF> 光标移动到这个屏幕的最上方那一行的第一个字符 </td></tr> <tr><td bgcolor=#F5F5DC width=25%> M </td><td bgcolor=#F8F8FF> 光标移动到这个屏幕的中央那一行的第一个字符 </td></tr> <tr><td bgcolor=#F5F5DC width=25%> L </td><td bgcolor=#F8F8FF> 光标移动到这个屏幕的最下方那一行的第一个字符 </td></tr> <tr><td bgcolor=#F5F5DC width=25%> G </td><td bgcolor=#F8F8FF> 移动到这个档案的最后一行(常用) </td></tr> <tr><td bgcolor=#F5F5DC width=25%> nG </td><td bgcolor=#F8F8FF> n 为数字。移动到这个档案的第 n 行。例如 20G 则会移动到这个档案的第 20 行(可配合 :set nu) </td></tr> <tr><td bgcolor=#F5F5DC width=25%> gg </td><td bgcolor=#F8F8FF> 移动到这个档案的第一行，相当于 1G 啊！ (常用)</td></tr> <tr><td bgcolor=#F5F5DC width=25%> n<Enter> </td><td bgcolor=#F8F8FF> n 为数字。光标向下移动 n 行(常用) </td></tr> <tr><th colspan="2" bgcolor=#555><font color="#FFFFFF"> 搜索替换 </font></th></tr> <tr><td bgcolor=#F5F5DC width=25%> /word </td><td bgcolor=#F8F8FF> 向光标之下寻找一个名称为 word 的字符串。例如要在档案内搜寻 kun 这个字符串，就输入 /kun 即可！ (常用) </td></tr> <tr><td bgcolor=#F5F5DC width=25%> ?word </td><td bgcolor=#F8F8FF> 向光标之上寻找一个字符串名称为 word 的字符串。 </td></tr> <tr><td bgcolor=#F5F5DC width=25%> n </td><td bgcolor=#F8F8FF> 这个 n 是英文按键。代表重复前一个搜寻的动作。举例来说， 如果刚刚我们执行 /kun 去向下搜寻 kun 这个字符串，则按下 n 后，会向下继续搜寻下一个名称为 kun 的字符串。如果是执行 ?kun 的话，那么按下 n 则会向上继续搜寻名称为 kun 的字符串！ </td></tr> <tr><td bgcolor=#F5F5DC width=25%> N </td><td bgcolor=#F8F8FF> 这个 N 是英文按键。与 n 刚好相反，为『反向』进行前一个搜寻动作。 例如 /kun 后，按下 N 则表示『向上』搜寻 kun 。 </td></tr> <tr><td colspan="2"> 使用 /word 配合 n 及 N 是非常有帮助的！可以让你重复的找到一些你搜寻的关键词！ </td></tr> <tr><td bgcolor=#F5F5DC width=25%> :n1,n2s/word1/word2/g </td><td bgcolor=#F8F8FF> n1 与 n2 为数字。在第 n1 与 n2 行之间寻找 word1 这个字符串，并将该字符串取代为 word2 ！举例来说，在 100 到 200 行之间搜寻 kun 并取代为 KUN 则：『:100,200s/kun/KUN/g』。(常用) </td></tr> <tr><td bgcolor=#F5F5DC width=25%> :1,$s/word1/word2/g </td><td bgcolor=#F8F8FF> 从第一行到最后一行寻找 word1 字符串，并将该字符串取代为 word2 ！(常用) </td></tr> <tr><td bgcolor=#F5F5DC width=25%> :1,$s/word1/word2/gc	</td><td bgcolor=#F8F8FF> 从第一行到最后一行寻找 word1 字符串，并将该字符串取代为 word2 ！且在取代前显示提示字符给用户确认 (confirm) 是否需要取代！(常用) </td></tr> <tr><th colspan="2" bgcolor=#555><font color="#FFFFFF"> 删除、复制与贴上 </font></th></tr> <tr><td bgcolor=#F5F5DC width=25%> x, X </td><td bgcolor=#F8F8FF> 在一行字当中，x 为向后删除一个字符 (相当于 [del] 按键)， X 为向前删除一个字符(相当于 [backspace] 亦即是退格键) (常用)	</td></tr> <tr><td bgcolor=#F5F5DC width=25%> nx </td><td bgcolor=#F8F8FF> n 为数字，连续向后删除 n 个字符。举例来说，我要连续删除 10 个字符， 『10x』。 </td></tr> <tr><td bgcolor=#F5F5DC width=25%> dd </td><td bgcolor=#F8F8FF> 删除游标所在的那一整行(常用)</td></tr> <tr><td bgcolor=#F5F5DC width=25%> ndd </td><td bgcolor=#F8F8FF> n 为数字。删除光标所在的向下 n 行，例如 20dd 则是删除 20 行 (常用)</td></tr> <tr><td bgcolor=#F5F5DC width=25%> d1G </td><td bgcolor=#F8F8FF> 删除光标所在到第一行的所有数据</td></tr> <tr><td bgcolor=#F5F5DC width=25%> dG </td><td bgcolor=#F8F8FF> 删除光标所在到最后一行的所有数据</td></tr> <tr><td bgcolor=#F5F5DC width=25%> d$ </td><td bgcolor=#F8F8FF> 删除游标所在处，到该行的最后一个字符</td></tr> <tr><td bgcolor=#F5F5DC width=25%> d0 </td><td bgcolor=#F8F8FF> 那个是数字的 0 ，删除游标所在处，到该行的最前面一个字符 </td></tr> <tr><td bgcolor=#F5F5DC width=25%> yy </td><td bgcolor=#F8F8FF> 复制游标所在的那一行(常用)</td></tr> <tr><td bgcolor=#F5F5DC width=25%> nyy </td><td bgcolor=#F8F8FF> n 为数字。复制光标所在的向下 n 行，例如 20yy 则是复制 20 行(常用) </td></tr> <tr><td bgcolor=#F5F5DC width=25%> y1G </td><td bgcolor=#F8F8FF> 复制游标所在行到第一行的所有数据</td></tr> <tr><td bgcolor=#F5F5DC width=25%> yG </td><td bgcolor=#F8F8FF> 复制游标所在行到最后一行的所有数据</td></tr> <tr><td bgcolor=#F5F5DC width=25%> y0 </td><td bgcolor=#F8F8FF> 复制光标所在的那个字符到该行行首的所有数据</td></tr> <tr><td bgcolor=#F5F5DC width=25%> y$ </td><td bgcolor=#F8F8FF> 复制光标所在的那个字符到该行行尾的所有数据</td></tr> <tr><td bgcolor=#F5F5DC width=25%> p, P </td><td bgcolor=#F8F8FF> p 为将已复制的数据在光标下一行贴上，P 则为贴在游标上一行！ 举例来说，我目前光标在第 20 行，且已经复制了 10 行数据。则按下 p 后， 那 10 行数据会贴在原本的 20 行之后，亦即由 21 行开始贴。但如果是按下 P 呢？ 那么原本的第 20 行会被推到变成 30 行。 (常用)</td></tr> <tr><td bgcolor=#F5F5DC width=25%> J </td><td bgcolor=#F8F8FF> 将光标所在行与下一行的数据结合成同一行</td></tr> <tr><td bgcolor=#F5F5DC width=25%> c </td><td bgcolor=#F8F8FF> 重复删除多个数据，例如向下删除 10 行，[ 10cj ]</td></tr> <tr><td bgcolor=#F5F5DC width=25%> u </td><td bgcolor=#F8F8FF> 复原前一个动作。(常用)</td></tr> <tr><td bgcolor=#F5F5DC width=25%> [Ctrl]+r </td><td bgcolor=#F8F8FF> 重做上一个动作。(常用)</td></tr> <tr><td colspan="2"> 这个 u 与 [Ctrl]+r 是很常用的指令！一个是复原，另一个则是重做一次～ 利用这两个功能按键，你的编辑，嘿嘿！很快乐的啦！ </td></tr> <tr><td bgcolor=#F5F5DC width=25%> . </td><td bgcolor=#F8F8FF> 不要怀疑！这就是小数点！意思是重复前一个动作的意思。 如果你想要重复删除、重复贴上等等动作，按下小数点『.』就好了！ (常用) </td></tr> </table>

**第二部份：一般模式切换到编辑模式的可用的按钮说明**<table> <tr><th colspan="2" bgcolor=#555><font color="#FFFFFF"> 进入输入或取代的编辑模式 </font></th></tr> <tr><td bgcolor=#F5F5DC width=20%> i, I	</td><td bgcolor=#F8F8FF> 进入输入模式(Insert mode)：<br> i 为『从目前光标所在处输入』， I 为『在目前所在行的第一个非空格符处开始输入』。 (常用) </td></tr> <tr><td bgcolor=#F5F5DC width=20%> a, A	</td><td bgcolor=#F8F8FF> 进入输入模式(Insert mode)：<br> a 为『从目前光标所在的下一个字符处开始输入』， A 为『从光标所在行的最后一个字符处开始输入』。(常用)</td></tr> <tr><td bgcolor=#F5F5DC width=20%> o, O	</td><td bgcolor=#F8F8FF> 进入输入模式(Insert mode)：<br> 这是英文字母 o 的大小写。o 为『在目前光标所在的下一行处输入新的一行』； O 为在目前光标所在处的上一行输入新的一行！(常用) </td></tr> <tr><td bgcolor=#F5F5DC width=20%> r, R	</td><td bgcolor=#F8F8FF> 进入取代模式(Replace mode)：<br> r 只会取代光标所在的那一个字符一次；R会一直取代光标所在的文字，直到按下 ESC 为止；(常用)</td></tr> <tr><td colspan="2"> 上面这些按键中，在 vi 画面的左下角处会出现『--INSERT--』或『--REPLACE--』的字样。 由名称就知道该动作了吧！！特别注意的是，我们上面也提过了，你想要在档案里面输入字符时， 一定要在左下角处看到 INSERT 或 REPLACE 才能输入喔！ </td></tr> <tr><td bgcolor=#F5F5DC width=20%> [Esc] </td><td bgcolor=#F8F8FF> 退出编辑模式，回到一般模式中(常用) </td></tr> </table>

**第三部份：一般模式切换到指令行模式的可用的按钮说明**<table>  <tr><th colspan="2" bgcolor=#555><font color="#FFFFFF"> 指令行的储存、离开等指令 </font></th></tr> <tr><td bgcolor=#F5F5DC width=20%> :w </td><td bgcolor=#F8F8FF> 将编辑的数据写入硬盘档案中(常用) </td></tr> <tr><td bgcolor=#F5F5DC width=20%> :w! </td><td bgcolor=#F8F8FF> 若文件属性为『只读』时，强制写入该档案。不过，到底能不能写入， 还是跟你对该档案的档案权限有关啊！ </td></tr> <tr><td bgcolor=#F5F5DC width=20%> :q </td><td bgcolor=#F8F8FF> 离开 vi (常用) </td></tr> <tr><td bgcolor=#F5F5DC width=20%> :q! </td><td bgcolor=#F8F8FF> 若曾修改过档案，又不想储存，使用 ! 为强制离开不储存档案。 </td></tr> <tr><td colspan="2"> 注意一下啊，那个惊叹号 (!) 在 vi 当中，常常具有『强制』的意思～ </td></tr> <tr><td bgcolor=#F5F5DC width=20%> :wq </td><td bgcolor=#F8F8FF> 储存后离开，若为 :wq! 则为强制储存后离开 (常用) </td></tr> <tr><td bgcolor=#F5F5DC width=20%> ZZ </td><td bgcolor=#F8F8FF> 这是大写的 Z 喔！若档案没有更动，则不储存离开，若档案已经被更动过，则储存后离开！ </td></tr> <tr><td bgcolor=#F5F5DC width=20%> :w [filename] </td><td bgcolor=#F8F8FF> 将编辑的数据储存成另一个档案（类似另存新档） </td></tr> <tr><td bgcolor=#F5F5DC width=20%> :r [filename] </td><td bgcolor=#F8F8FF> 在编辑的数据中，读入另一个档案的数据。亦即将 『filename』 这个档案内容加到游标所在行后面 </td></tr> <tr><td bgcolor=#F5F5DC width=20%> :n1,n2 w [filename] </td><td bgcolor=#F8F8FF> 将 n1 到 n2 的内容储存成 filename 这个档案。 </td></tr> <tr><td bgcolor=#F5F5DC width=20%> :! command </td><td bgcolor=#F8F8FF> 暂时离开 vi 到指令行模式下执行 command 的显示结果！例如：<br>『:! ls /home』即可在 vi 当中察看 /home 底下以 ls 输出的档案信息！ </td></tr> <tr><th colspan="2" bgcolor=#555><font color="#FFFFFF"> vim 环境的变更 </font></th></tr> <tr><td bgcolor=#F5F5DC width=20%> :set nu </td><td bgcolor=#F8F8FF> 显示行号，设定之后，会在每一行的前缀显示该行的行号 </td></tr> <tr><td bgcolor=#F5F5DC width=20%> :set nonu </td><td bgcolor=#F8F8FF> 与 set nu 相反，为取消行号！ </td></tr> </table> &emsp;&emsp;特别注意，在 vi/vim 中，数字是很有意义的！数字通常代表重复做几次的意思！ 也有可能是代表去到第几个什么什么的意思。举例来说，要删除 50 行，则是用 『50dd』 对吧！ 数字加在动作之前，如我要向下移动 20 行呢？那就是『20j』或者是『20↓』即可。

## 基础用法小练习 #
&emsp;&emsp;为了能够良好的应用所学，于是我将鸟叔的在该章节的小练习记录在这里，有空的时候来进行练习。由于鸟叔官网上的繁中不便于阅读，所以我打一份简中的在这里。接下来的操作都是用CentOS7.4的man_db.conf来做练习，可以在鸟哥的官网[下载](http://linux.vbird.org/linux_basic/0310vi/man_db.conf)
```
请在/tmp这个目录下建立一个名为test的目录；
进入test目录；
将/etc/man_db.conf（如果你是通过上述链接下载那么就复制下载后的文件)复制到当前目录；
使用vi打开本目录下的man_db.conf文件；
在vi中设置一下行号；
移动到第43行，向右移动59个字符，请问你看到的小括号内是哪几个文字？
移动到第一行，并且向下查找【gzip】这个字符串，请问它在第几行？
接着下来，将29到41行之间的【小写man】修改为【大写MAN】，并且询问是否修改，如何操作？结果出现改变了几个man？
修改完之后，突然反悔了，恢复有几种方法？
复制66到71这6行内的内容（含有MANDB_MAP），并且粘贴到最后一行后；
删除113到128行之间开头为#符号的注释数据；
将当前修改完的文件保存为man.test.config文件；
在第25行，删除15个字符，结果出现的第一个字符是什么？
在第一行新增一行，该内容输入【I am a student...】；
保存退出。
```
&emsp;&emsp;如果你编辑的是上面所说的文件，那么下面的就是正确答案及步骤：
```
【mkdir /tmp/test】
【cd /tmp/test】
【cp /etc/man_db.conf .】(或者是复制你自己的下载的文件到当前目录)
【/bin/vi man_db.conf】
【:set nu，就可以看见左侧显示了行号
【43G】到43行，【59空格】按下59+空格即可向右移动59个字符，显示as这个字符在小括号内。
【1G】或【gg】到第一行，输入【/gzip】，会去到93行才对。
直接执行【:29,41s/man/MAN/gc】将会在29和41行间替换man为MAN，加上c让用户确认替换，一直按【y】最终会完成【13次替换，共13行】。
一直按【u】恢复，或者【:q!】不保存且退出，再重新打开该文件。
【66G】然后再【6yy】之后最下面的提示栏会出现【复制了6行】(提示内容视你系统的语言而定)，按下【G】到最后一行再按【p】换行粘贴。
因为113-128共16行，因此【113G】然后【16dd】就能删除113-128行的内容，此时光标所在的行变成了【Flags】开头。
【:w man.test.config】，你会发现最下方的状态栏显示“man.test.config" [NEW]..的字样。
【25G】之后，再按下【15x】即可删除15个字符，出现【tree】字样。
先【1G】到第一行，然后按下大写的【O】会在上面新增一行且进入编辑模式；接着你要输入的字符串，按下【Esc】退出编辑模式。
【:wq】。
```
&emsp;&emsp;如果结果和上面的一样，那么基本上的操作都没有什么问题了，剩下的只需要多多打字练习。

## vi/vim键位图 #
![vi/vim键位图（转自菜鸟教程）](https://i.imgur.com/8eefgQy.gif) 
<!-- /images/Linux/vi-vim键位图.gif-->

## 参考 #
&emsp;&emsp;《鸟哥的Linux私房菜》、菜鸟教程
