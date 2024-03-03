### 引入概念
在任何当前工作的 Git 仓库中，每个文件都是这样的：
- **追踪的（tracked）**- 这些是 Git 所知道的所有文件或目录。这些是新添加（用 `git add` 添加）和提交（用 `git commit` 提交）到主仓库的文件和目录。
- **未被追踪的（untracked）** - 这些是在工作目录中创建的，但还没有被暂存（或用 `git add` 命令添加）的任何新文件或目录。
- **被忽略的（ignored）** - 这些是 Git 知道的要全部排除、忽略或在 Git 仓库中不需要注意的所有文件或目录。本质上，这是一种告诉 Git 哪些未被追踪的文件应该保持不被追踪并且永远不会被提交的方法。

说明：
- 所有被忽略的文件都会被保存在一个 `.gitignore` 文件中
- `.gitignore` 文件是一个纯文本文件，包含了项目中所有指定的文件和文件夹的列表，这些文件和文件夹是 Git 应该忽略和不追踪的
- 在 `.gitignore` 中，你可以通过提及特定文件或文件夹的名称或模式来告诉 Git 只忽略一个文件或一个文件夹。你也可以用同样的方法告诉 Git 忽略多个文件或文件夹
一般情况：
- 一个 `.gitignore` 文件会被放在仓库的根目录下。根目录也被称为当前工作目录，它包含了组成项目的所有文件和其他文件夹
- 也就是说，你可以把它放在版本库的任何文件夹中。你甚至可以有多个 `.gitignore` 文件
### 使用场景
显而易见：添加到 `.gitignore` 文件中的文件类型是任何不需要被提交的文件

其中一些可能包括：
- 操作系统文件。每个操作系统（如 macOS、Windows 和 Linux）都会生成系统特定的隐藏文件，其他开发者不需要使用这些文件，因为他们的系统也会生成这些文件。例如，在 macOS 上，Finder 会生成一个 `.DS_Store` 文件，其中包括用户对文件夹的外观和显示的偏好，如图标的大小和位置
- 由代码编辑器和 IDE（IDE 代表集成开发环境）等应用程序生成的配置文件。这些文件是为你、你的配置和你的偏好设置定制的
- 从你的项目中使用的编程语言或框架自动生成的文件，以及编译后的代码特定文件，如 `.o` 文件
- 由软件包管理器生成的文件夹，如 npm 的 `node_modules` 文件夹。这是一个用于保存和跟踪你在本地安装的每个软件包的依赖关系的文件夹
- 包含敏感数据和个人信息的文件。这类文件的一些例子是含有你的凭证（用户名和密码）的文件和含有环境变量的文件，如 `.env` 文件（`.env` 文件含有需要保持安全和隐私的 API 密钥）
- 运行时文件，如 `.log` 文件。它们提供关于操作系统的使用活动和错误的信息，以及在操作系统中发生的事件的历史
### 开启使用
```bash
touch .gitignore
```
- 名字前面有点（`.`）的文件默认是隐藏的
- 当单独使用 `ls` 命令时，隐藏的文件是不可见的
- 要从命令行查看所有的文件--包括隐藏的文件--要在 `ls` 命令中使用 `-a` 标志
```bash
ls -a
```

具体演示：
```bash
➜  git-tutorial git:(master) ls -a
.  ..  daily_1.cpp  daily_2.cpp  daily_3.cpp  daily.cpp  .git
➜  git-tutorial git:(master) touch .gitignore
➜  git-tutorial git:(master) ✗ ls -a           
.  ..  daily_1.cpp  daily_2.cpp  daily_3.cpp  daily.cpp  .git  .gitignore
➜  git-tutorial git:(master) ✗ 
```

```bash
➜  git-tutorial git:(master) ls -a
.  ..  daily_1.cpp  daily_2.cpp  daily_3.cpp  daily.cpp  .git
➜  git-tutorial git:(master) touch .gitignore
➜  git-tutorial git:(master) ✗ ls -a           
.  ..  daily_1.cpp  daily_2.cpp  daily_3.cpp  daily.cpp  .git  .gitignore
```
### 使用指令
##### 1. 如果你想只忽略一个特定的文件，你需要提供该文件在项目根目录下的完整路径：
```bash
/text.txt（它本身就在工作目录下）
/test/text.txt（它在test文件夹下）<=> test/text.txt
```
##### 2. 如果你想忽略所有具有特定名称的所有文件，你需要写出该文件的字面名称：

eg：如果你想忽略任何 `text.txt` 文件，你可以在 `.gitignore` 中添加以下内容：
```bash
text.txt
```
在这种情况下，你不需要提供特定文件的完整路径。这种模式将忽略位于项目中==任何地方==的具有该特定名称的所有文件。
##### 3. 要忽略整个目录及其所有内容，你需要包括目录的名称，并在最后加上/：
```bash
test/
```

这个命令将忽略位于你的项目中任何地方的名为 `test` 的目录（包括目录中的其他文件和其他子目录）。

==需要注意的是==，如果你只写一个文件的名字或者只写目录的名字而不写斜线 `/`，那么这个模式将同时匹配任何带有这个名字的文件或目录：
```bash
# 匹配任何名字带有 test 的文件和目录
test
```
##### 4. 删除具体内容：
#忽略所有名称以特定单词开头的文件和目录
要做到这一点，你需要指定你想忽略的名称，后面跟着 `*` 通配符选择器
```bash
# 忽略所有名字以 `hbx` 开头的文件和目录
hbx*
```

#忽略任何以特定单词结尾的文件或目录
如果你想，则需使用 `*` 通配符选择器，后面跟你想忽略的文件扩展名
```bash
# 忽略所有以 `特定文件扩展名(.img / .md / .cpp)` 结尾的文件和目录
*.md
*.cpp
*.py
```

#总体处理，但有特例
假设：你在你的 `.gitignore` 文件中添加了以下内容
```bash
.md
```
这个模式会忽略所有以 `.md` 结尾的文件，但你不希望 Git 忽略一个 `README.md` 文件。

要做到这一点，你需要使用带有感叹号的否定模式，即 `!`，来排除一个本来会被忽略的文件：
```bash
# 忽略所有 .md 文件:       .md  
# 不忽略 README.md 文件:  !README.md
```

在 `.gitignore` 文件中使用这两种模式，所有以 `.md` 结尾的文件都会被忽略，除了 `README.md` 文件。

==需要记住的是==，如果你忽略了整个目录，这个模式就不起作用。
例如，你忽略了所有的 `test` 目录：
```bash
test/
```

假设在一个 `test` 文件夹内，你有一个文件，`example.md`，你不想忽略它。
你不能像这样在一个被忽略的目录内排除一个文件：
```bash
test/
!test/example.md
```
应该如何做呢？正解：
```bash
test/*
!test/example.md
```
### 实例演示
ps：下列所有组别的测试之间，均需要抹除“上一轮提交到暂存区的记录”
```bash
➜  git-tutorial git:(master) ✗ git restore --staged .
```

1. 初始
```bash
➜  git-tutorial git:(master) ✗ tree
.
├── daily_1.cpp
├── daily_2.cpp
├── daily_3.cpp
├── daily.cpp
├── hbx3.cpp
├── test1
│   ├── hbx1.cpp
│   └── hbx2.cpp
├── test2
│   └── hbx4.cpp
└── test3
    └── hbx_new.cpp

3 directories, 9 files
➜  git-tutorial git:(master) ✗ 
```
2. 忽略test2文件夹下的所有内容（如此不包含test2文件夹本身）
```bash
➜  git-tutorial git:(master) ✗ cat .gitignore
/test2/*
➜  git-tutorial git:(master) ✗ git add . 
➜  git-tutorial git:(master) ✗ git status
位于分支 master
要提交的变更：
  （使用 "git restore --staged <文件>..." 以取消暂存）
	新文件：   .gitignore
	新文件：   hbx3.cpp
	新文件：   test1/hbx1.cpp
	新文件：   test1/hbx2.cpp
	新文件：   test3/hbx_new.cpp

➜  git-tutorial git:(master) ✗ 
```
3. 忽略test3文件夹(如此包含test3文件夹+文件夹下所有文件)
```bash
➜  git-tutorial git:(master) ✗ cat .gitignore
/test3
➜  git-tutorial git:(master) ✗ git add . 
➜  git-tutorial git:(master) ✗ git status
位于分支 master
要提交的变更：
  （使用 "git restore --staged <文件>..." 以取消暂存）
	新文件：   .gitignore
	新文件：   hbx3.cpp
	新文件：   test1/hbx1.cpp
	新文件：   test1/hbx2.cpp
	新文件：   test2/hbx4.cpp

➜  git-tutorial git:(master) ✗ 
```
4. 忽略所有c++代码（即：后缀是.cpp）
```bash
➜  git-tutorial git:(master) ✗ vim .gitignore
➜  git-tutorial git:(master) ✗ cat .gitignore
*.cpp
➜  git-tutorial git:(master) ✗ git add .             
➜  git-tutorial git:(master) ✗ git status
位于分支 master
要提交的变更：
  （使用 "git restore --staged <文件>..." 以取消暂存）
	新文件：   .gitignore

➜  git-tutorial git:(master) ✗ 
```
5. test1全清空，只留下hbx1.cpp
```bash
➜  git-tutorial git:(master) ✗ cat .gitignore
/test1/*
!/test1/hbx1.cpp
➜  git-tutorial git:(master) ✗ git add .     
➜  git-tutorial git:(master) ✗ git status
位于分支 master
要提交的变更：
  （使用 "git restore --staged <文件>..." 以取消暂存）
	新文件：   .gitignore
	新文件：   hbx3.cpp
	新文件：   test1/hbx1.cpp
	新文件：   test2/hbx4.cpp
	新文件：   test3/hbx_new.cpp

➜  git-tutorial git:(master) ✗ 
```
6. 所有.cpp文件全部清空，只留下hbx1.cpp
```bash
➜  git-tutorial git:(master) ✗ vim .gitignore        
➜  git-tutorial git:(master) ✗ cat .gitignore
*.cpp
!/test1/hbx1.cpp
➜  git-tutorial git:(master) ✗ git add .             
➜  git-tutorial git:(master) ✗ git status               
位于分支 master
要提交的变更：
  （使用 "git restore --staged <文件>..." 以取消暂存）
	新文件：   .gitignore
	新文件：   test1/hbx1.cpp

➜  git-tutorial git:(master) ✗ 
```