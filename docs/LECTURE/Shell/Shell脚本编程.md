# 前言
1. 参考学习教程：[Shell-Script](https://www.shellscript.sh/)
2. 这是笔者的学习笔记，针对新手入门（20240223）
3. 建议学习时长：48h
# 使用方法
- 在工作目录中创建一个shell-script文件夹
- 所有文档均存储在shell-script下
- 下面会展示完整学习笔记，建议边看便做
# Chapter 1 - A First Script
>For our first shell script, we'll just write a script which says "Hello World". We will then try to get more out of a Hello World program than any other tutorial you've ever read
## 创建脚本文件 vim first.sh
```bash
➜  shell-script cat first.sh
#!/bin/sh                     [第一行告诉Unix该文件将由/bin/sh执行。这是几乎所有Unix系统上Bourne shell的标准位置]
# This is a comment!          [特殊符号`#`开头，这会将该行标记为注释，并且 shell 会完全忽略它]
[唯一的例外是: 当文件的第一行以 `#!`. 这是 Unix 特别处理的特殊指令]
[类似地，Perl 脚本应以 `#!/usr/bin/perl` 开头，告诉您的交互式 shell 后面的程序应该由 perl 执行]
[对于 Bourne shell 编程，我们将坚持 `#!/bin/sh.`]
echo "Hello      World"       # This is a comment, too! ["xx"内是一个整体,会直接打印] / [#后是注释，不执行且不输出!]
echo "Hello World"            ["xx"内是一个整体,会直接打印]
echo "Hello * World"          ["xx"内是一个整体,会直接打印]
echo Hello * World            [空格就是分隔，`*` 是通配符，它匹配任何字符（包括空格）零次或多次]
echo Hello      World         [空格就是分隔，不论中间有多少空格，分隔恒为1space]
echo "Hello" World            [简单，略]
echo Hello "     " World      [""内是一个整体字符串，等价于"Hello" "   " "World"]
echo "Hello "*" World"        [""内是一个整体字符串，内部的"*"使得*不作通配符解释，只是普通字符]
echo `hello` world            [学到后面就知道了，留个悬念]
echo 'hello' world            [''内是一个整体]
echo "hello" "*" "world"      [三个分别输出，简单]
➜  shell-script 
```
## 赋予文件权限 755
```bash
➜  shell-script chmod 755 first.sh
➜  shell-script 
```
- `chmod 755 first.sh` 是一个用于设置文件权限的命令，它在Unix/Linux系统中常用
- 命令由两部分组成：`chmod` 是改变文件权限的命令，而 `755` 是权限模式
- 权限数字组成：读（4）、写（2）和执行（1）

在 `755` 中，每一位都代表一类用户（所有者、组用户、其他用户），并且每一位都有不同的权限设置：
- ==最左边的数字== 是所有者（Owner）的权限。在 `755` 中是 7，它表示读（4）、写（2）和执行（1）的权限之和，因此所有者有读、写、执行的权限（4+2+1=7）
- ==第二位== 是组用户（Group）的权限。在 `755` 中是 5，表示读和执行的权限（4+1=5）
- ==第三位== 是其他用户（Others）的权限。在 `755` 中同样是 5，表示读和执行的权限（4+1=5）

因此，`chmod 755 first.sh` 的意思是赋予文件 `first.sh` 的所有者读、写、执行的权限，组用户和其他用户只有读和执行的权限
## 运行文件 ./first.sh
```bash
➜  shell-script ./first.sh
Hello      World
Hello World
Hello * World
Hello first.sh World
Hello World
Hello World
Hello       World
Hello * World
./first.sh: 1: hello: not found
world
hello world
hello * world
```
# Chapter 2 - Variables (Part1)
## '=' 实现变量赋值
**程序**
```bash
➜  shell-script cat var.sh  
#!/bin/sh
MY_MESSAGE="Hello World"
echo $MY_MESSAGE
```
**说明**
1. 赋值符号 `=` 不能有空格： `VAR=value` 是使用格式；而 `VAR = value` 不起作用
2. `$MY_MESSAGE`：变量MY_MESSAGE的值
3. shell 不关心变量的类型，事实上，shell将它们都存储为字符串
## read 实现交互设置
**程序**
```bash
➜  shell-script cat var2.sh
#!/bin/sh
echo What is your name?
read MY_NAME
echo "Hello $MY_NAME - hope you're well."
```
**说明**
1. 注意到：即使您为其提供全名并且不要在 `echo` 命令两边使用双引号，它仍会正确输出
2. 这是如何做到的？review：前面的 `MY_MESSAGE` 变量，当时我们必须在它周围加上双引号来设置它才行
3. `read` 命令会自动在其输入周围放置引号，以便正确处理空格（自动实现全字符串读入！）
**结果**
```bash
➜  shell-script ./var2.sh
What is your name?
root-hbx huboxuan@xjtu
Hello root-hbx huboxuan@xjtu - hope you're well.
➜  shell-script 
```
## 变量作用域
常识：
- bash中的变量不必像在 C 等语言中那样声明。但是，如果您尝试读取未声明的变量，则结果是空字符串，且您不会收到任何警告或错误！这可能会导致一些微妙的错误！
- `export` 它对变量的作用域有根本性的影响
### 未定义则默认为 empty_str
**程序**
```bash
#!/bin/sh  
echo "MYVAR is: $MYVAR"  
MYVAR="hi there"  
echo "MYVAR is: $MYVAR"
```
**结果**
```bash
$ ./myvar2.sh  
MYVAR is:  
MYVAR is: hi there
```
初始的MYVAR 尚未设置为任何值，因此为空。然后我们给它一个值，它就有了预期的结果
### 程序内部的变量不能跑到外面
**现象**
```bash
➜  shell-script MYVAR=hbx
➜  shell-script ./myvar2.sh
MYVAR is: 
MYVAR is: hi there
➜  shell-script 
```
It's still not been set (original MYVAR)!  What's going on?!
**分析**
- 从交互式 shell 调用 `myvar2.sh` 时，将生成一个新 shell 来运行脚本。这在一定程度上是因为脚本开头的 `#!/bin/sh` 那一行
- 我们需要 `export` 变量才能让它被另一个程序（ eg：一个 shell 脚本）继承
```bash
➜  shell-script MYVAR=cqy
➜  shell-script export MYVAR
➜  shell-script ./myvar2.sh 
MYVAR is: cqy
MYVAR is: hi there
➜  shell-script
```
**问题**
这样交互式shell，会根据最后命令行的输入改变variable_content
```bash
➜  shell-script MYVAR=cqy
➜  shell-script export MYVAR
➜  shell-script ./myvar2.sh 
MYVAR is: cqy
MYVAR is: hi there
➜  shell-script echo $MYVAR      
cqy
```
1. 事实上我更希望cqy只是一个交互尝试，并不该改变脚本中对于$MYVAR="hi there"的赋值
2. 为了从脚本中接收环境更改，我们必须获取脚本 - 这有效地在我们自己的交互式 shell 中运行脚本，而不是生成另一个 shell 来运行它

```bash
➜  shell-script MYVAR=cqy
➜  shell-script echo $MYVAR
cqy
➜  shell-script . ./myvar2.sh 
MYVAR is: cqy
MYVAR is: hi there
➜  shell-script echo $MYVAR
hi there
➜  shell-script 
```
- 在这种情况下，我们不需要 `export MYVAR`
- 一个容易犯的错误是说 `echo MYVAR` 而不是 `echo $MYVAR` 
- 与大多数语言不同，在获取变量的值时需要美元 （ `$` ） 符号，但在设置变量的值时不得使用
### 引用改写的规范
**抛砖引玉：**
```bash
#!/bin/sh  
echo "What is your name?"  
read USER_NAMEecho "Hello $USER_NAME"  
echo "I will create you a file called $USER_NAME_file"  
touch $USER_NAME_file
```
FALSE！
- 除非有一个名为 `USER_NAME_file` 的变量，否则shell 不知道变量在哪里结束，其余变量从哪里开始
- shell并不知$作用到USER_NAME_file的哪一成分截断

**正确写法：**
```bash
#!/bin/sh  
echo "What is your name?"  
read USER_NAME
echo "Hello $USER_NAME"  
echo "I will create you a file called ${USER_NAME}_file"     shell现知指代的是USER_NAME且我们希望它以_file为后缀
touch "${USER_NAME}_file"
```
results:
```bash
➜  shell-script vim user.sh  
➜  shell-script chmod 755 user.sh
➜  shell-script ./user.sh    
What is your name?
root hbx
Hello root hbx
I will create you a file called root hbx_file
➜  shell-script 
```
- 注意 `"${USER_NAME}_file"` 是用引号包围的
- 如果用户输入“Steve Parker”（注意空格），若无引号，传递给 `touch` 的参数将是 `Steve` 和 `Parker_file` - 也就是说，我们实际上是在说 `touch Steve Parker_file` ，这是两个完全不同的要 `touch` 编辑的文件
# Chapter 3 - Wildcards
如果你以前使用过Unix，通配符倒不是什么新鲜事
1. Think first how you would copy ==all the files from /tmp/a== into `/tmp/b`. All the .txt files? All the .html files?
```bash
(1) folder a->b : 
cp /tmp/a/* /tmp/b/

(2) All the .txt files:
cp /tmp/a/*.txt /tmp/b/

(3) All the .html files:
cp /tmp/a/*.html /tmp/b/
```
# Chapter 4 - Escape Characters
##  1. " -> \\"
某些字符对 shell 很重要;例如，我们已经看到，双引号 （ `"` ） 字符的使用会影响空格和 TAB 字符的处理方式，例如：
```bash
$ echo Hello       World
Hello World
$ echo "Hello       World"
Hello     World
```
那么我们如何显示： `Hello    "World"` ？

Answer: we use the code
```bash
echo "Hello   \"World\""
```
- 第一个和最后一个"字符(外围) 将整个语句整体包装到一体，传递给 `echo` 的参数中
-  \\" 是为了让 " 在此处被解释为“普通”语义

这里展示一种常见的错误写法：
```bash
echo "Hello   " World ""
```
这句话将被解释为三个参数：
1. "Hello   " 
2. World 世界
3. ""
（第一个和第二个 " " 标记了 Hello 和后面的空格；第二个参数是不带引号的`World`）
## 2. * and \' -> " \* " and " ' "
```bash
$ echo *                                     [`*` 展开为表示当前目录中的所有文件]
case.shtml escape.shtml first.shtml 
functions.shtml hints.shtml index.shtml 
ip-primer.txt raid1+0.txt
$ echo *txt                                  [`*txt` 表示以 `txt` 结尾的所有文件]
ip-primer.txt raid1+0.txt
$ echo "*"                                   [`*` 被放进双引号，从字面上解释（字面义）]
*
$ echo "*txt"                                [同上]
*txt
```
## 3. ", $,  \`, and \\ 使用\\进行转义
- `"`, `$`, `` ` ``, and `\` are still interpreted by the shell, even when they're in double quotes.
- The backslash (`\`) character is used to mark these special characters so that they are not interpreted by the shell, but passed on to the command being run
```bash
Aim:
1. A quote is ", backslash is \, backtick is `.
2. A few spaces are    and dollar is $. $X is 5.

Code:
1. 
➜  shell-script echo "A quote is \", backslash is \\ backtick is \`"
A quote is ", backslash is \ backtick is `
➜  shell-script 
2. 
➜  shell-script export X=5  
➜  shell-script echo "A few spaces are     and dollar is \$. \$X is ${X}."
A few spaces are     and dollar is $. $X is 5.
➜  shell-script 
```
# Chapter 5 - Loops
## for 循环
**程序 1**
```bash
➜  shell-script cat for.sh      
#!/bin/sh
for i in 1 2 3 4 5 6 7 8 9 10              #遍历从1到10的数字
do
	echo "This time is in Loop(${i})"
done
➜  shell-script 
```
**结果 1**
```bash
➜  shell-script vim for.sh
➜  shell-script chmod 755 for.sh 
➜  shell-script ./for.sh  
This time is in Loop(1)
This time is in Loop(2)
This time is in Loop(3)
This time is in Loop(4)
This time is in Loop(5)
This time is in Loop(6)
This time is in Loop(7)
This time is in Loop(8)
This time is in Loop(9)
This time is in Loop(10)
```

**程序 2**
```bash
#!/bin/sh
for i in hello 1 * 2 goodbye  # 按序遍历 hello、1、（当前文件夹下所有文件名）、2、goodbye
do
  echo "Looping ... i is set to $i"
done
```
**结果 2**
```bash
➜  shell-script vim for2.sh 
➜  shell-script chmod 755 for2.sh
➜  shell-script ./for2.sh
Looping ... i is set to hello
Looping ... i is set to 1
Looping ... i is set to first.sh
Looping ... i is set to for.sh
Looping ... i is set to for2.sh
Looping ... i is set to myvar2.sh
Looping ... i is set to root hbx_file
Looping ... i is set to user.sh
Looping ... i is set to var.sh
Looping ... i is set to var2.sh
Looping ... i is set to 2
Looping ... i is set to goodbye
```

**程序 3**
```bash
➜  shell-script cat for2.sh
#!/bin/sh
for i in hello 1 \* 2 goodbye         # 遍历hello 1 * 2 goodbye这五个字符
do
  echo "Looping ... i is set to $i"
done

➜  shell-script 
```
**结果 3**
```bash
➜  shell-script ./for2.sh
Looping ... i is set to hello
Looping ... i is set to 1
Looping ... i is set to *
Looping ... i is set to 2
Looping ... i is set to goodbye
➜  shell-script 
```
## while 循环
**程序 1**
```bash
➜  shell-script cat while.sh      
#!/bin/sh
INPUT_STRING=hello
while [ "$INPUT_STRING" != "bye" ]
do
  echo "Please type something in (bye to quit)"
  read INPUT_STRING
  echo "You typed: $INPUT_STRING"
done
# 这里发生的情况是，echo 和 read 语句将无限期运行，直到您在出现提示时键入“bye”
➜  shell-script 
```
**结果 1**
```bash
➜  shell-script vim while.sh
➜  shell-script chmod 755 while.sh
➜  shell-script ./while.sh
Please type something in (bye to quit)
hbx
You typed: hbx
Please type something in (bye to quit)
cqy
You typed: cqy
Please type something in (bye to quit)
bye
You typed: bye
➜  shell-script 
```

冒号 （ `:` ） 的计算结果始终为 true;虽然有时可能需要使用它，但通常最好使用真正的退出条件
**程序 2**
```bash
#!/bin/sh
while :
do
  echo "Please type something in (^C to quit)"
  read INPUT_STRING
  echo "You typed: $INPUT_STRING"
done
```
**结果 2**
```bash
➜  shell-script vim while2.sh
➜  shell-script chmod 755 while2.sh
➜  shell-script ./while2.sh
Please type something in (^C to quit)
shdla
You typed: shdla
Please type something in (^C to quit)
asfcsa
You typed: asfcsa
Please type something in (^C to quit)
^Z
[1]  + 204775 suspended  ./while2.sh
➜  shell-script 
```
# Chapter 6 - Test
- `test` 更常称为 `[` 
- `[` 是一个符号链接， `test` 只是为了让 shell 程序更具可读性
- 它通常也是一个内置的 shell（这意味着 shell 本身会 `[` 解释为 含义 `test` ，即使您的 Unix 环境设置不同）
## 1. `[` 实际上是一个程序，就像`ls`一样，所以它必须被空格包围
### 常见错误
```bash
if [$foo = "bar" ]

- This will not work; it is interpreted as `if test$foo = "bar" ]`, which is a ']' without a beginning '['.
- Put spaces around all your operators！
```
### 正确示范
```bash
if SPACE [ SPACE "$foo" SPACE = SPACE "bar" SPACE ]

- replace 'SPACE' with an actual space; if there isn't a space there, it won't work
```
### 要点说明
1. 有些 shell 也接受 “`==`” 进行字符串比较
2. 严格意义上：字符串应该使用单个 “`=`” 进行比较
3. 严格意义上：整数应该使用 “`-eq`”进行比较
## 2. if - elif - else 模板示范
### Model 1
```bash
if [ ... ]
then
  # if-code
else
  # else-code
fi

# fi 是 if 倒退的！稍后会再次用于 case 和 esac
```
### Model 2
```bash
if [ ... ]; then
  # do something
fi

# Commonly: "if [ ... ]" and "then" commands must be on different lines
# Alternatively: the semicolon ";" can separate them
```
### Model 3
```bash
if  [ something ]; then
 echo "Something"
 elif [ something_else ]; then
   echo "Something else"
 else
   echo "None of the above"
fi

# if-then => elif-then => else => fi
```
### 实例解析
Programming
```bash
#!/bin/sh
#test.sh

if [ "$X" -lt "0" ]
then
  echo "X is less than zero"
fi
if [ "$X" -gt "0" ]; then
  echo "X is more than zero"
fi
[ "$X" -le "0" ] && \
      echo "X is less than or equal to  zero"
[ "$X" -ge "0" ] && \
      echo "X is more than or equal to zero"
[ "$X" = "0" ] && \
      echo "X is the string or number \"0\""
[ "$X" = "hello" ] && \
      echo "X matches the string \"hello\""
[ "$X" != "hello" ] && \
      echo "X is not the string \"hello\""
[ -n "$X" ] && \
      echo "X is of nonzero length"
[ -f "$X" ] && \
      echo "X is the path of a real file" || \
      echo "No such file: $X"
[ -x "$X" ] && \
      echo "X is the path of an executable file"
[ "$X" -nt "/etc/passwd" ] && \
      echo "X is a file which is newer than /etc/passwd"
```
Result
```bash
➜  shell-script chmod 755 test.sh 
-------------------------------------------------------------------------------------
➜  shell-script X=5  
➜  shell-script export X                                                  
➜  shell-script ./test.sh              
X is more than zero
X is more than or equal to zero
X is not the string "hello"
X is of nonzero length
No such file: 5
```

```
-------------------------------------------------------------------------------------
➜  shell-script X=hello
➜  shell-script ./test.sh
./test.sh: 2: [: Illegal number: hello
./test.sh: 6: [: Illegal number: hello
./test.sh: 9: [: Illegal number: hello
./test.sh: 11: [: Illegal number: hello
X matches the string "hello"
X is of nonzero length
No such file: hello
```

```
-------------------------------------------------------------------------------------
➜  shell-script X=test.sh
➜  shell-script ./test.sh
./test.sh: 2: [: Illegal number: test.sh
./test.sh: 6: [: Illegal number: test.sh
./test.sh: 9: [: Illegal number: test.sh
./test.sh: 11: [: Illegal number: test.sh
X is not the string "hello"
X is of nonzero length
X is the path of a real file
X is the path of an executable file
X is a file which is newer than /etc/passwd
➜  shell-script 
```
**(1) 说明 :**
1. 可以使用分号 （ `;` ） 将两行连接在一起：这样做通常是为了在简单的 `if` 语句中节省一点空间
2. 反斜杠 （ `\` ） 具有类似但相反的目的：它告诉 shell 这不是行的末尾，但下一行应被视为当前行的一部分；这对于可读性很有用。习惯上在反斜杠 （ `\` ） 或分号 （ `;` ） 之后缩进下一行
```bash
if [ "$X" -nt "/etc/passwd" ]; then
      echo "X is a file which is newer than /etc/passwd"
fi
-------------------------------------------------------------------------------------
[ "$X" -nt "/etc/passwd" ] && \
      echo "X is a file which is newer than /etc/passwd"
```

**(2) test 可对数字、字符串和文件名执行许多测试 :** 
(1) There is a simpler way of writing `if` statements: The `&&` and `||` commands give code to run if the result is true, or false, respectively.
>[ A judgement B ] `&&` Action1 `||` Action2     
>if the result of [...] is TRUE => Action1
>else => Action2

(2) The categories of 'judgement' in daily usage [File]:
>-a \ -e : 文件存在
>-s  : 文件是套接字socket
>-nt : file is newer than
>-ot : file is older than
>-ef : paths refer to the same file
>-O : file is owned by the user running the test

(3) The categories of 'judgement' in daily usage [Num]:
>-lt、-gt、-le 和 -ge 比较仅适用于整数，不适用于字符串
>-lt : less than
>-gt : greater than
>-le : less than or equal to
>-ge : greater than or equal to

(4) The categories of 'judgement' in daily usage [String]:
>!= :  Not Equal To 
>=  :  Equal To

**(3) 输出优化实例：**
```bash
#!/bin/sh
#code_origin
X=0
while [ -n "$X" ]
do
  echo "Enter some text (RETURN to quit)"
  read X
  echo "You said: $X"
done
-------------------------------------------------------------------------------------
$ ./test2.sh
Enter some text (RETURN to quit)
fred
You said: fred
Enter some text (RETURN to quit)
wilma
You said: wilma
Enter some text (RETURN to quit)

You said:
$
-------------------------------------------------------------------------------------
注意，运行此脚本将以不整齐的方式结束！如何优化输出？
-------------------------------------------------------------------------------------
#!/bin/sh
#code_now
X=0
while [ -n "$X" ]
do
  echo "Enter some text (RETURN to quit)"
  read X
  if [ -n "$X" ]; then
    echo "You said: $X"
  fi
done
```
# Chapter 7 - Case
>该 `case` 语句省去了一整套 `if .. then .. else` 语句。它的语法非常简单，完全类比于C++中的“switch”语句
## Case模板
```bash
while :
do
	read STRING
	case $STRING in 
	  a)
		do_A  # 如果 STRING 匹配 a，则执行该代码段，直到双分号
		;;
	  b)
	    do_B
	    ;;
	    
	  ...
	  
	  *)
	    do_ending
	    ;;
	esac      # case statement is ended with `esac`
done          # we end the while loop with a `done`
# 如果我们想完全退出脚本，那么我们将使用命令 exit 而不是 break
```
## 实例说明
```bash
#!/bin/sh
echo "Please talk to me ..."

while :
do
  read INPUT_STRING
  echo "$INPUT_STRING"

  case $INPUT_STRING in
	hello)
		echo "Hello yourself!"
		;;
	bye)
		echo "See you again!"
		break
		;;
	*)
		echo "Sorry, I don't understand"
		;;
  esac
done
#echo 
echo "That's all folks!"
```

Results:
```bash
➜  shell-script chmod 755 talk.sh
➜  shell-script ./talk.sh 
Please talk to me ...
hbx
hbx
Sorry, I don't understand
root -hbx
root -hbx
Sorry, I don't understand
hello
hello
Hello yourself!
bye
bye
See you again!
That's all folks!
➜  shell-script 
```
# Chapter 8 - Variables (Part2)
## 常见的内置变量
>There are a set of variables which are set for you already, and most of these cannot have values assigned to them
- `$0` is the basename of the program as it was called (被调用程序的name)
- `$1 ... $9` (调用脚本的前 9 个附加参数[如果超过9这个数字，则使用下面的script])
- `$@` 是可指代所有参数 `$1 .. whatever` (“变色龙”参数)
- `$*`, is similar, but does not preserve any whitespace, and quoting (遇见空格自动截断的"变色龙"参数) => "File with spaces" becomes "File" "with" "spaces"
- As a general rule, use `$@` and avoid `$*`
- `$#` is the number of parameters the script was called with. (调用脚本时使用的参数数目)
```bash
#!/bin/sh
#code:
echo "There are $# parameters from the input shell" ($# 总参数数量)
echo "1. My name is $0" （$0 程序名）
echo "2. My first parameter is $1" （$! 展示第一个参数） 
echo "3. My second parameter is $2"（$2 展示第二个参数）
echo "All parameters are $@"       （$@ 展示全部参数）
```

```bash
1) 不传参数
➜  shell-script chmod 755 var3.sh
➜  shell-script ./var3.sh
There are 0 parameters from the input shell
1. My name is ./var3.sh
2. My first parameter is 
3. My second parameter is 
All parameters are 
```

```bash
2) 传参数
➜  shell-script ./var3.sh hello1 hello2 hello3 hello4 hello5
There are 5 parameters from the input shell
1. My name is ./var3.sh
2. My first parameter is hello1
3. My second parameter is hello2
All parameters are hello1 hello2 hello3 hello4 hello5
➜  shell-script 
```
## 超过额定参数数目的脚本
我们可以使用命令 `shift` 获取 9 个以上的参数;请看下面的脚本： var4.sh这个脚本一直使用 `shift` ，直到 `$#` 降到零，此时列表是空的
```bash
#!/bin/sh
while [ "$#" -gt "0" ]
do
  echo "\$1 is $1"
  shift
done
```
结果：
```bash
➜  shell-script ./var4.sh para1 2 3 4 5 6 7 8 9 10
$1 is para1
$1 is 2
$1 is 3
$1 is 4
$1 is 5
$1 is 6
$1 is 7
$1 is 8
$1 is 9
$1 is 10
```
- `shift` 是一个在Shell脚本中用于移动命令行参数的命令：用于在处理位置参数（positional parameters）时，将参数列表向左移动一定的位置
- 在脚本中，当你使用 `$1`、`$2`、`$3` 等来引用脚本的位置参数时，`shift` 可以将这些位置参数整体向左移动
- 每次执行 `shift` 命令，当前的 `$1` 将会被丢弃，而原来的 `$2` 就变成了新的 `$1`，以此类推
## 一些冷门的内置变量
1.  `$?` （上次运行命令的退出值）
2.  `$$` （当前正在运行的 shell 的 PID [进程标识符]）
3. `$!` （上次运行后台进程的 PID）
4. `IFS`（内部字段分隔符 || 默认值为 `SPACE TAB NEWLINE`[IFS=$' \t\n']，可自定义“分割符”）

**example 1**
```bash
#!/bin/sh
# A demo of $?
/usr/local/bin/my-command
if [ "$?" -ne "0" ]; then
  echo "Sorry, we had a problem there!"
fi
```
step1: 尝试运行 `/usr/local/bin/my-command` ，如一切顺利，则应以零值退出；如失败，应以非零值退出
(行为良好的应用程序在成功时应返回零)
step2: 然后，我们可以通过检查 `$?` 的值来处理这个问题

**example 2**
```bash
#!/bin/sh
old_IFS="$IFS"
IFS=:
echo "Please input some data separated by colons ..."
read x y z
IFS=$old_IFS
echo "x is $x y is $y z is $z"
```
1. `old_IFS="$IFS"`：保存当前 `IFS` 的值到变量 `old_IFS` 中。
2. `IFS=:`：将 `IFS` 设置为冒号（`:`），用于在 `read` 命令中作为字段分隔符。
3. `read x y z`：通过 `read` 命令从标准输入中读取用户输入，并将输入的数据以冒号为分隔符分配给变量 `x`、`y`、`z`。
4. `IFS=$old_IFS`：将 `IFS` 恢复为先前保存的值，即==将其还原为脚本执行之前的状态，以确保不会影响其他部分的脚本==。

通过保存和还原 `IFS` 的值，你可以在需要更改 `IFS` 时，确保在脚本的其他部分或其他脚本中不会受到这些更改的影响。这是一个==良好的编程实践==，以确保代码的可维护性和可重用性

Result1 (The paras are Num_right)
```bash
➜  shell-script ./ifs.sh                          
Please input some data separated by colons(:) ...
hbx:cqy:wlr
x is hbx y is cqy z is wlr
```
Result2 (The paras are Num_over)
```
➜  shell-script ./ifs.sh
Please input some data separated by colons(:) ...
hbx:cqy:wlr:wjby
x is hbx y is cqy z is wlr:wjby
```

>PS: 在处理 IFS（以及任何不完全由您控制的变量）时，重要的是要意识到它可能包含空格、换行符和其他“不可控”字符
>因此最好在它周围使用双引号，即： `old_IFS="$IFS"` 而不是 `old_IFS=$IFS` .
# Chapter 9 - Variables (Part3)
- As we mentioned in [Variables - Part I](https://www.shellscript.sh/variables1.html), curly brackets around a variable( { } ) avoid confusion:
- That's not all, though - these fancy brackets have a another, much more powerful use. We can deal with issues of variables being undefined or null (in the shell, there's not much difference between undefined and null).
=> {variable} 可以处理变量未定义或 null 的问题
## 程序引入
```bash
#!/bin/sh
echo -en "What is your name [ `whoami` ] "
read myname
if [ -z "$myname" ]; then
  myname=`whoami`
fi
echo "Your name is : $myname"
```
Result
```bash
#如果您通过按“RETURN”接受默认值，则此脚本将按如下方式运行:
➜  shell-script chmod 755 name.sh
➜  shell-script ./name.sh
-en What is your name [ root-hbx ] 
Your name is : root-hbx
```

```bash
#如果根据用户输入：
➜  shell-script ./name.sh
-en What is your name [ root-hbx ] 
cqy
Your name is : cqy
➜  shell-script 
```
Exp： 
1. `-en`  传递给 echo 告诉它不要添加换行符（对于 bash 和 csh）
2. whoami 命令会打印您的登录名(UID)
## 默认值设定
### This could be done better using a shell variable feature. By using curly braces and the special ":-" usage, you can specify a default value to use if the variable is unset.（大括号 和 “：-” => 可指定在变量未设置时使用默认值）

**Model:**
```bash
"...${variable:-(balabala)}"
```
**`:-` 操作符：**
- 如果变量已经被设置，那么它会使用该变量的值
- 如果变量未被设置（即为空或未定义），则它会==使用==指定的默认值

**Examples:**
```bash
#!/bin/sh
# name_new.sh

echo -en "1) What is your name [ `whoami` ] "
read myname
echo "Your name is : ${myname:-`whoami`}"     # mynameDEFAULT = 系统导入的个人ID

echo -en "2) What is your age?"
read MYAGE
echo "Your name is : ${MYAGE:-Guess hhh}"     # myageDEFAULT = Guess hhh
```

```bash
# Default + Default
➜  shell-script chmod 755 name_new.sh
➜  shell-script ./name_new.sh
-en 1) What is your name [ root-hbx ] 

Your name is : root-hbx
-en 2) What is your age?

Your name is : Guess hhh
```

```bash
# SelfInfo + Default
➜  shell-script ./name_new.sh
-en 1) What is your name [ root-hbx ] 
cqy
Your name is : cqy
-en 2) What is your age?

Your name is : Guess hhh
```

```bash
# Default + SelfInfo
➜  shell-script ./name_new.sh
-en 1) What is your name [ root-hbx ] 

Your name is : root-hbx
-en 2) What is your age?
66
Your name is : 66
➜  shell-script cat name_new.sh      
```
### There is another syntax, ":=", which sets the variable to the default if it is undefined.（大括号 和 “：=” => 可指定在变量未定义时使用默认值）

**Model:**
```bash
"...${variable:=(balabala)}"
```
**`=` 操作符：**
- 如果变量已经被设置，那么它会使用该变量的值
- 如果变量未被设置，则它会使用指定的默认值，并将这个默认值==赋给==该变量
*注意区分 :- 与 := 区别，一个是使用，一个是赋与*
# Chapter 10 - External Programs
External programs are often used within shell scripts; there are a few builtin commands (`echo`, `which`, and `test` are commonly builtin), but many useful commands are actually Unix utilities, such as `tr`, `grep`, `expr` and `cut`.
>- shell内置命令：`echo`, `which`, and `test`
>- Unix程序命令：`tr`, `grep`, `expr` and `cut`
## 这里主要介绍的是：反引号(\`)
### Func:
>用于指示将随附的文本作为命令执行
### 使用场景:
#### (1) 将 一个文本输出 抓取到 一个变量 中，以便轻松操作
```bash
➜  shell-script grep "^${USER}:" /etc/passwd | cut -d: -f5 
root-hbx,,,
➜  shell-script MYNAME=`grep "^${USER}:" /etc/passwd | cut -d: -f5` 
➜  shell-script echo $MYNAME                                              
root-hbx,,,
➜  shell-script 
# 反引号是从我们选择运行的任何命令或命令集中捕获标准输出
```
#### (2) 代码复用，提升程序运行效率
```bash
#!/bin/sh
# Order1
find / -name "*.html" -print | grep "/index.html$"
# Order2
find / -name "*.html" -print | grep "/contents.html$"
```
**解析Order1：**
- `find / -name "*.html" -print`：在根目录 `/` 下查找所有以 `.html` 结尾的文件，并将结果打印输出
- `grep "/index.html$"`：通过管道 `|` 将 `find` 的输出传递给 `grep`，使用 `grep` 过滤出以 `/index.html` 结尾的行
这个命令的目的是找到所有路径以 `/index.html` 结尾的 `.html` 文件

**解析Order2：**
同理，略之

**注意到上面`find / -name "*.html" -**print**`制作了两次，耗时多，下面是化简版：**
```bash
#!/bin/sh
HTML_FILES=`find / -name "*.html" -print`
echo "$HTML_FILES" | grep "/index.html$"
echo "$HTML_FILES" | grep "/contents.html$"
```
# Chapter 11 - Functions
Bash脚本编程的一个常被忽视的功能是：可以轻松地编写在脚本中使用的函数

A function may return a value in one of four different ways:  
- Change the state of a variable or variables  
    更改一个或多个变量的状态
- Use the `exit` command to end the shell script  
    使用命令 exit 结束 shell 脚本
- Use the `return` command to end the function, and return the supplied value to the calling section of the shell script  
    使用该 return 命令结束函数，并将提供的值返回到 shell 脚本的调用部分
- echo output to stdout, which will be caught by the caller just as c=`expr $a + $b` is caught  
    回显输出到 stdout，调用方将在 c='expr $a + $b' 被捕获时捕获
## 1. 程序引入
```bash
#!/bin/sh
# A simple script with a function...

add_a_user()   # 在调用函数之前，不会执行此代码; 函数被读入，但在实际调用它们之前基本上被忽略
{
  USER=$1         # $USER=bob
  PASSWORD=$2     # $PASSWORD=letmein
  shift; shift;   # 指向初始位置参数的指针向后移动了2位（ 1,2,3,4,5... => x,x,1,2,3,... ）
  # Having shifted twice, the rest is now comments ...
  COMMENTS=$@     # $COMMENTS=Bob Holness the presenter
  echo "Adding user $USER ..."
  echo useradd -c "$COMMENTS" $USER
  echo passwd $USER $PASSWORD
  echo "Added user $USER ($COMMENTS) with pass $PASSWORD"
}

###
# Main body of script starts here
###
echo "Start of script..."
add_a_user bob letmein Bob Holness the presenter           # 函数调用
add_a_user fred badpassword Fred Durst the singer          # 函数调用
add_a_user bilko worsepassword Sgt. Bilko the role model   # 函数调用
echo "End of script..."
```

```bash
➜  shell-script chmod 755 function.sh
➜  shell-script ./function.sh
Start of script...
Adding user bob ...
useradd -c Bob Holness the presenter bob
passwd bob letmein
Added user bob (Bob Holness the presenter) with pass letmein
Adding user fred ...
useradd -c Fred Durst the singer fred
passwd fred badpassword
Added user fred (Fred Durst the singer) with pass badpassword
Adding user bilko ...
useradd -c Sgt. Bilko the role model bilko
passwd bilko worsepassword
Added user bilko (Sgt. Bilko the role model) with pass worsepassword
End of script...
➜  shell-script 
```
1. Within that function, `$1` is set to `bob`, regardless of what `$1` may be set to outside of the function
2. So if we want to refer to the "original" $1 _inside_ the function, we have to assign a name to it - such as: `A=$1` before we call the function. Then, within the function, we can refer to `$A`
3. Use the `shift` command again to get the `$3` and onwards parameters into `$@`. The function then adds the user and sets their password.
## 2. 变量范围
Programmers used to other languages may be surprised at the scope rules for shell functions. Basically, there is no scoping, other than the parameters (`$1`, `$2`, `$@`, etc).
### 实例程序 1
```bash
#!/bin/sh
myfunc()
{
  echo "I was called as : $@"
  x=2
}

### Main script starts here 
echo "Script was called with $@" # 命令行传递给执行脚本的参数（a,b,c）
x=1                              # 变量x 实际上是一个全局变量
echo "x is $x"
myfunc 1 2 3                     # 传递给函数体的参数（1,2,3）
echo "x is $x"                   # 说明函数体内对应的x跟外面的一样：都是"全局变量"！
```

```bash
➜  shell-script chmod 755 scope.sh   
➜  shell-script ./scope.sh   
Script was called with 
x is 1
I was called as : 1 2 3
x is 2
```

```bash
➜  shell-script ./scope.sh a b c
Script was called with a b c
x is 1
I was called as : 1 2 3
x is 2
➜  shell-script 
```
### 实例程序 2
```bash
#!/bin/sh
myfunc()
{
  echo "\$1 is $1"
  echo "\$2 is $2"
  # cannot change $1 - we'd have to say:
  # 1="Goodbye Cruel"
  # which is not a valid syntax. However, we can
  # change $a:
  a="Goodbye Cruel"
}

### Main script starts here 
a=Hello
b=World
myfunc $a $b
echo "a is $a"
echo "b is $b"
```

```bash
➜  shell-script ./scope1.sh      
$1 is Hello
$2 is World
a is Goodbye Cruel
b is World
➜  shell-script 
```
Functions cannot change the values they have been called with, either - this must be done by changing the variables themselves, not the parameters as passed to the script.
## 3. 函数递归
```bash
#!/bin/sh                        指定了脚本要使用的解释器
factorial()                      函数定义，用于计算输入数字的阶乘
{
  if [ "$1" -gt "1" ]; then      如果传递给函数的第一个参数（$1）大于1
    i=`expr $1 - 1`              将参数 $1 减去1的结果赋给变量 i  [expr: 数学运算表达符]
    j=`factorial $i`             递归调用阶乘函数，计算 $i 的阶乘，并将结果赋给变量 j
    k=`expr $1 \* $j`            计算 $1 乘以 $j 的结果，并将结果赋给变量 k
    echo $k                      打印结果 k
  else
    echo 1
  fi
}

while :                          一个无限循环的开始，条件永远为真（: 表示 true）
do
  echo "Enter a number:"
  read x
  factorial $x
done
```

```bash
➜  shell-script ./factorial.sh
Enter a number:
5
120
Enter a number:
8
40320
Enter a number:
^Z
[2]  + 50114 suspended  ./factorial.sh
➜  shell-script 
```
`expr` 是一个用于进行表达式求值的命令。在这个脚本中，`expr` 被用于执行数学运算，例如加法、减法、乘法等：

- `expr $a + $b`: 计算变量 a 和 b 的和
- `expr $a - $b`: 计算变量 a 减去 b 的差
- `expr $a \* $b`: 计算变量 a 乘以 b 的积
- `expr $a / $b`: 计算变量 a 除以 b 的商
- `expr $a % $b`: 计算变量 a 除以 b 的余数

1. 在你的脚本中，`expr` 被用于进行乘法运算，如 k=expr $1 \* $j，计算阶乘的一部分。需要注意的是，在 Shell 中进行乘法运算时，使用 `*` 需要进行转义，因此写作 `\*`.
2. 需要指出的是，虽然 `expr` 在较早的 Shell 版本中是常见的数学计算工具，但在现代的 Shell 编程中，可以使用更先进的数学运算方式，比如 `$((expression))` 或者 `bc` 命令
## 4. 设计和调用函数库
As promised, we will now briefly discuss using libraries between shell scripts. These can also be used to define common variables, as we shall see.
**MainFunc:** 简洁包装，代码重用

```bash
# common.lib   表示这是一个名为 "common.lib" 的库文件
# Note no #!/bin/sh as this should not spawn # an extra shell. It's not the end of the world # to have one, but clearer not to.   这个脚本是库文件，不需要启动额外的 shell
#
STD_MSG="About to rename some files..."  用于存储标准消息的字符串变量

rename()
{
  # expects to be called as: rename .txt .bak 
  FROM=$1
  TO=$2

  for i in *$FROM          遍历 [当前目录下] 所有文件名，其中文件名以 `$FROM` 结尾
  do
    j=`basename $i $FROM`  使用basename命令获取文件名（去掉路径），并将其中的$FROM部分去除，结果赋给变量j
    mv $i ${j}$TO          使用 `mv` 命令将文件重命名，将 `$FROM` 替换为 `$TO`
  done
}
```

```bash
#!/bin/sh
# function2.sh
. ./common.lib
echo $STD_MSG
rename .cpp .c
```

```bash
#!/bin/sh
# function3.sh
. ./common.lib
echo $STD_MSG
rename .c .cpp
```
Here we see two user shell scripts, `function2.sh` and `function3.sh`, each sourceing the common library file `common.lib`, and using variables and functions declared in that file
```bash
➜  shell-script ls
 1.cpp          first.sh       function.sh  'root hbx_file'   user.sh   while2.sh
 2.cpp          for2.sh        ifs.sh        scope1.sh        var2.sh   while.sh
 common.lib     for.sh         myvar2.sh     scope.sh         var3.sh
 factorial.sh   function2.sh   name_new.sh   talk.sh          var4.sh
 find.sh        function3.sh   name.sh       test.sh          var.sh
```

```bash
➜  shell-script ./function2.sh
About to rename some files...
 1.c	        first.sh       function.sh  'root hbx_file'   user.sh   while2.sh
 2.c	        for2.sh        ifs.sh	     scope1.sh	      var2.sh   while.sh
 common.lib     for.sh	       myvar2.sh     scope.sh	      var3.sh
 factorial.sh   function2.sh   name_new.sh   talk.sh	      var4.sh
 find.sh        function3.sh   name.sh	     test.sh	      var.sh
```
```bash
➜  shell-script ./function3.sh
About to rename some files...
 1.cpp	        first.sh       function.sh  'root hbx_file'   user.sh   while2.sh
 2.cpp	        for2.sh        ifs.sh	     scope1.sh	      var2.sh   while.sh
 common.lib     for.sh	       myvar2.sh     scope.sh	      var3.sh
 factorial.sh   function2.sh   name_new.sh   talk.sh	      var4.sh
 find.sh        function3.sh   name.sh	     test.sh	      var.sh
➜  shell-script 
```
## 5. 返回代码
For now, though we shall briefly look at the `return` call.
```bash
#!/bin/sh

adduser()
{
	USER=$1
	PASSWORD=$2
	shift ; shift
	COMMENTS=$@
	useradd -c "${COMMENTS}" $USER

	if [ "$?" -ne "0" ]; then
		echo "Useradd Failed"
		return 1
	fi

	passwd $USER $PASSWORD
	if [ "$?" -ne "0" ]; then
		echo "Setting password failed"
		return 2
	fi

	echo "Added user $USER ($COMMENTS) with pass $PASSWORD"
}
```
```bash
## Main Script starts here:

adduser hbx 2223410945 ROOT HBX from XJTU
ADDUSER_RETURN_CODE=$?

if [ "$ADDUSER_RETURN_CODE" -eq "1" ]; then
	echo "Useradd went wrong!"
elif [ "$ADDUSER_RETURN_CODE" -eq "1" ]; then
	echo "Passwd went wrong!"
else
	echo "Con: You are added into this system!"
fi
➜  shell-script 
```
1. 定义一个返回代码 1 来指示 `useradd` 的任何问题，2 来指示 的任何问题 `passwd` 。这样，调用脚本就知道问题出在哪里
2. For some time, this tutorial wrongly checked "\$?" both times, rather than setting `ADDUSER_RETURN_CODE=$?`, and then looking at the value of `ADDUSER_RETURN_CODE` each time. ==This was a bug==. You have to save the value of `$?` immediately, because as soon as you run another command, such as `if`, its value will be replaced. That is why we save the `adduser` return value in the `$ADDUSER_RETURN_CODE` variable, before acting on its content. `$ADDUSER_RETURN_CODE` is certain to remain the same; ==`$?` will change with every command that is executed==.
# Chapter 12 - Hints and Tips
1. The content below is, to be honest, rather outdated. [The /examples part of this website has more, and more usable, tips and examples.](https://www.shellscript.sh/examples/)
2. Unix is full of **text manipulating utilities**, some of the more powerful of which we will now discuss in this section of this tutorial. The significance of this, is that virtually **everything under Unix is text**. Virtually anything you can think of is controlled by either a text file, or by a command-line-interface (CLI). The only thing you can't automate using a shell script is a GUI-only utility or feature. And under Unix, there aren't too many of them!

本节内容建议直接参考[该网页](https://www.shellscript.sh/hints.html)
# Appendix - Quick Reference
This is a quick reference guide to the meaning of some of the less easily guessed commands and codes of shell scripts.

|Command 命令|Description 描述|Example 例|
|---|---|---|
|&|Run the previous command in the background  <br>在后台运行上一个命令|`ls &`|
|&&|Logical AND 逻辑 AND|`if [ "$foo" -ge "0" ] && [ "$foo" -le "9"]`|
|\||Logical OR 逻辑手术室|`if [ "$foo" -lt "0" ] \| [ "$foo" -gt "9" ]`|
|^|Start of line 起始线|`grep "^foo"`|
|$|End of line 生产线结束|`grep "foo$"`|
|=|String equality (cf. -eq)  <br>==字符串相等== （cf. -eq） |`if [ "$foo" = "bar" ]`|
|!|Logical NOT 逻辑 NOT|`if [ "$foo" != "bar" ]`|
|$$|PID of current shell 当前外壳的PID|`echo "my PID = $$"`|
|$!|PID of last background command  <br>最后一个后台命令的 PID|`ls & echo "PID of ls = $!"`|
|$?|exit status of last command  <br>==上一个命令的退出状态== |`ls ; echo "ls returned code $?"`|
|$0|Name of current command (as called)  <br>==当前命令[程序]的名称==（如调用） |`echo "I am $0"`|
|$1|Name of current command's first parameter  <br>当前命令的第一个参数的名称|`echo "My first argument is $1"`|
|$9|Name of current command's ninth parameter  <br>当前命令的第九个参数的名称|`echo "My ninth argument is $9"`|
|$@|All of current command's parameters (preserving whitespace and quoting)  <br>当前命令的==所有参数（保留空格和引号）== |`echo "My arguments are $@"`|
|$*|All of current command's parameters (not preserving whitespace and quoting)  <br>当前命令的==所有参数（不保留空格和引号）== |`echo "My arguments are $*"`|
|-eq|Numeric Equality 数值相等|`if [ "$foo" -eq "9" ]`|
|-ne|Numeric Inquality 数值不合格|`if [ "$foo" -ne "9" ]`|
|-lt|Less Than 小于|`if [ "$foo" -lt "9" ]`|
|-le|Less Than or Equal 小于或等于|`if [ "$foo" -le "9" ]`|
|-gt|Greater Than 大于|`if [ "$foo" -gt "9" ]`|
|-ge|Greater Than or Equal 大于或等于|`if [ "$foo" -ge "9" ]`|
|-z|String is zero length==字符串长度为零== |`if [ -z "$foo" ]`|
|-n|String is not zero length  <br>==字符串长度不为零== |`if [ -n "$foo" ]`|
|-nt|Newer Than 高于|`if [ "$file1" -nt "$file2" ]`|
|-d|Is a Directory ==是一个目录== |`if [ -d /bin ]`|
|-f|Is a File ==是一个文件== |`if [ -f /bin/ls ]`|
|-r|Is a readable file 是==可读文件== |`if [ -r /bin/ls ]`|
|-w|Is a writable file 是==可写文件== |`if [ -w /bin/ls ]`|
|-x|Is an executable file 是==可执行文件== |`if [ -x /bin/ls ]`|
|( ... )|Function definition 功能定义|`function myfunc() { echo hello }`|
