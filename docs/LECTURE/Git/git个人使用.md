### 建立工作区
#### 1. 设立项目文件夹
```bash
mkdir git-tutorial
cd git-tutorial          
```
#### 2. 注册账户[非必须]
（名字+邮箱）
```bash
git-tutorial git config --global user.email huboxuan2004@gmail.com               

git-tutorial git config --global user.name root-hbx   
```

tips：查看git本地仓库状态的命令
```bash
git status
```

#### 3. 初始化仓库
```bash
➜  git-tutorial git init         
提示：使用 'master' 作为初始分支的名称。这个默认分支名称可能会更改。要在新仓库中
提示：配置使用初始分支名，并消除这条警告，请执行：
提示：
提示：	git config --global init.defaultBranch <名称>
提示：
提示：除了 'master' 之外，通常选定的名字有 'main'、'trunk' 和 'development'。
提示：可以通过以下命令重命名刚创建的分支：
提示：
提示：	git branch -m <name>
已初始化空的 Git 仓库于 /home/root-hbx/git-tutorial/.git/
➜  git-tutorial git:(master) ✗ 
```
### 一般工作流
#### 1. 创建文件
```bash
vim daily.cpp
......

➜  git-tutorial git:(master) ✗ cat daily.cpp
/*************************************************************************
	> File Name: daily.cpp
	> Author: amoscykl
	> Mail: amoscykl@163.com 
	> Created Time: 2024年02月09日 星期五 01时55分15秒
 ************************************************************************/

#include<iostream>
using namespace std;
int main()
{
    int a;
	cin >> a;
    cout<<a<<endl;
    return 0;
}
➜  git-tutorial git:(master) ✗ 
```
#### 2. git add ( + project) => 提取到“暂存区”
```bash
➜  git-tutorial git:(master) ✗ git add daily.cpp
➜  git-tutorial git:(master) ✗ 
```
#### 3. git commit -m "your note" + project => 推送到“本地库”
```bash
➜  git-tutorial git:(master) ✗ git commit -m "first" daily.cpp       
[master （根提交） 23e5ede] first
 1 file changed, 16 insertions(+)
 create mode 100644 daily.cpp
➜  git-tutorial git:(master) 
```
### 日志查询
#### 1. 简要LOG查询
```bash
➜  git-tutorial git:(master) git reflog      
23e5ede (HEAD -> master) HEAD@{0}: commit (initial): first
(END)
➜  git-tutorial git:(master) 
```
#### 2. 复杂LOG查看
```bash
➜  git-tutorial git:(master) git log   

commit 23e5ede535aeb1d63bf8edf07b19d980fab272e6 (HEAD -> master)
Author: root-hbx <huboxuan2004@gmail.com>
Date:   Fri Feb 9 02:01:55 2024 +0800

    first
(END)

➜  git-tutorial git:(master) 
```
### 实例分析
#### 1. 准备工作
```bash
cls
vim daily_1 / daily_2.cpp（此处只是为了示意，自行填充）
......

➜  git-tutorial git:(master) ✗ ll
总计 12K
-rw-rw-r-- 1 root-hbx root-hbx 381  2月  9 02:09 daily_1.cpp
-rw-rw-r-- 1 root-hbx root-hbx 383  2月  9 02:10 daily_2.cpp
-rw-rw-r-- 1 root-hbx root-hbx 393  2月  9 01:56 daily.cpp
➜  git-tutorial git:(master) ✗ cat daily_1.cpp
/*************************************************************************
	> File Name: daily_1.cpp
	> Author: amoscykl
	> Mail: amoscykl@163.com 
	> Created Time: 2024年02月09日 星期五 02时09分00秒
 ************************************************************************/

#include<iostream>
using namespace std;
int main()
{
    cout<<"hbx nb!"<<endl;
    return 0;
}
➜  git-tutorial git:(master) ✗ cat daily_2.cpp
/*************************************************************************
	> File Name: daily_2.cpp
	> Author: amoscykl
	> Mail: amoscykl@163.com 
	> Created Time: 2024年02月09日 星期五 02时09分44秒
 ************************************************************************/

#include<iostream>
using namespace std;
int main()
{
    cout<<"hbx shit!"<<endl;
    return 0;
}
➜  git-tutorial git:(master) ✗ 
```
#### 2. 全部提交到暂存区
#approach1：挨个处理
```bash
➜  git-tutorial git:(master) ✗ git add daily_1.cpp
➜  git-tutorial git:(master) ✗ git add daily_2.cpp
```

此时查看状态：
```bash
➜  git-tutorial git:(master) ✗ git status         
位于分支 master
要提交的变更：
  （使用 "git restore --staged <文件>..." 以取消暂存）
	新文件：   daily_1.cpp
	新文件：   daily_2.cpp

➜  git-tutorial git:(master) ✗ 
```

#approach2：git_add_dot添加该目录下所有子文件/子文件夹 
```bash
➜  git-tutorial git:(master) ✗ cat daily_3.cpp
/*************************************************************************
	> File Name: daily_3.cpp
	> Author: amoscykl
	> Mail: amoscykl@163.com 
	> Created Time: 2024年02月09日 星期五 02时24分20秒
 ************************************************************************/

#include<iostream>
using namespace std;
int main()
{
    int hbx;
	cin >> hbx;
	cout<<hbx<<endl;
    return 0;
}

➜  git-tutorial git:(master) ✗ git add .        
```

此时查看状态：
```bash   
➜  git-tutorial git:(master) ✗ git status 
位于分支 master
要提交的变更：
  （使用 "git restore --staged <文件>..." 以取消暂存）
	新文件：   daily_1.cpp
	新文件：   daily_2.cpp
	新文件：   daily_3.cpp

➜  git-tutorial git:(master) ✗ 
```
#### 3. 全部提交到暂存区
```bash
➜  git-tutorial git:(master) ✗ git commit -m "commit 1" daily_1.cpp
[master ea892d4] commit 1
 1 file changed, 14 insertions(+)
 create mode 100644 daily_1.cpp
➜  git-tutorial git:(master) ✗ git status                          
位于分支 master
要提交的变更：
  （使用 "git restore --staged <文件>..." 以取消暂存）
	新文件：   daily_2.cpp
	新文件：   daily_3.cpp


➜  git-tutorial git:(master) ✗ git commit -m "commit 2" daily_2.cpp
[master 61e9b3c] commit 2
 1 file changed, 14 insertions(+)
 create mode 100644 daily_2.cpp
➜  git-tutorial git:(master) ✗ git status                          
位于分支 master
要提交的变更：
  （使用 "git restore --staged <文件>..." 以取消暂存）
	新文件：   daily_3.cpp


➜  git-tutorial git:(master) ✗ git commit -m "commit 3" daily_3.cpp
[master 9fdf6a0] commit 3
 1 file changed, 16 insertions(+)
 create mode 100644 daily_3.cpp
➜  git-tutorial git:(master) git status                          
位于分支 master
无文件要提交，干净的工作区
➜  git-tutorial git:(master) 

```
#### 4. 查看LOG
##### git reflog
```bash
9fdf6a0 (HEAD -> master) HEAD@{0}: commit: commit 3
61e9b3c HEAD@{1}: commit: commit 2
ea892d4 HEAD@{2}: commit: commit 1
23e5ede HEAD@{3}: commit (initial): first
(END)
```
##### git log
```bash
commit 9fdf6a073dfc395c0365b1ff4d3877633918c670 (HEAD -> master)
Author: root-hbx <huboxuan2004@gmail.com>
Date:   Fri Feb 9 02:28:52 2024 +0800

    commit 3

commit 61e9b3ca30464dc4a63b73c0ce3f64a396db4895
Author: root-hbx <huboxuan2004@gmail.com>
Date:   Fri Feb 9 02:28:35 2024 +0800

    commit 2

commit ea892d4b39a84ec7635e7213f952921788a82a9f
Author: root-hbx <huboxuan2004@gmail.com>
Date:   Fri Feb 9 02:28:08 2024 +0800

    commit 1

commit 23e5ede535aeb1d63bf8edf07b19d980fab272e6
Author: root-hbx <huboxuan2004@gmail.com>
Date:   Fri Feb 9 02:01:55 2024 +0800

    first
(END)
```