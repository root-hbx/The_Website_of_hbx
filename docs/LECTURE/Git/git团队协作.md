### 基本操作
1. 创建分支：`git branch NAME`
2. 切换到分支：`git checkout NAME`
```bash
➜  git-tutorial git:(master) ✗ ls
daily_1.cpp  daily_2.cpp  daily_3.cpp  daily.cpp  hbx3.cpp  test1  test2  test3
➜  git-tutorial git:(master) ✗ git branch hot-fix    
➜  git-tutorial git:(master) ✗ git checkout hot-fix                
切换到分支 'hot-fix'
➜  git-tutorial git:(hot-fix) ✗ 
```
3. 合并分支：`git merge NAME`(将NAME分支的内容主动合并到现所在的branch)
4. 查看全部现有分支：`git branch -v`
```bash
➜  git-tutorial git:(hot-fix) ✗ git branch -v       
* hot-fix 9fdf6a0 commit 3
  master  9fdf6a0 commit 3
(END)

➜  git-tutorial git:(hot-fix) ✗ 
```
5. 删除本地某分支：`git branch -d dev`
### 有/无冲突合并
具体定义我相信了解时间线的会比较清楚，这里就以例子说明：
1. 场景1
>[1] master主线中有一个文件为a（文件+版本）[已add+commit]
[2] master主线创建一个分支new
[3] new分支中对文件进行修改成b（文件+版本）[已add+commit]
[4] master再merge分支new
=> 无冲突合并
2. 场景2
>[1] master主线中有一个文件为a（文件+版本）[已add+commit]
[2] master主线创建一个分支new
[3] new分支中对文件进行修改成b（文件+版本）[已add+commit]
[4] master主线中再对文件进行修改成a+（文件版本）[已add+commit]
[5] master再merge分支new
=> 冲突合并（很显然上述3/4可以互换位置）
### 实例展示
#### 1）无冲突合并
1. 初始：在master_branch中“全部完善”test_branch.cpp
```bash
➜  git-tutorial git:(master) ✗ vim test_branch.cpp
➜  git-tutorial git:(master) ✗ cat test_branch.cpp 
/*************************************************************************
	> File Name: test_branch.cpp
	> Author: amoscykl
	> Mail: amoscykl@163.com 
	> Created Time: 2024年02月09日 星期五 11时02分23秒
 ************************************************************************/

#include<iostream>
using namespace std;
int main()
{
	cout<<"I belong to branch master"<<endl;
	return 0;
}
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
├── test3
│   └── hbx_new.cpp
└── test_branch.cpp

3 directories, 10 files
➜  git-tutorial git:(master) ✗ git add .             
➜  git-tutorial git:(master) ✗ git status
位于分支 master
要提交的变更：
  （使用 "git restore --staged <文件>..." 以取消暂存）
	新文件：   .gitignore
	删除：     branch.md
	新文件：   hbx3.cpp
	新文件：   test1/hbx1.cpp
	新文件：   test1/hbx2.cpp
	新文件：   test2/hbx4.cpp
	新文件：   test3/hbx_new.cpp
	新文件：   test_branch.cpp

➜  git-tutorial git:(master) ✗ git commit -m "branch test 1" test_branch.cpp
[master 32046fb] branch test 1
 1 file changed, 14 insertions(+)
 create mode 100644 test_branch.cpp

➜  git-tutorial git:(master) ✗ git reflog    

32046fb (HEAD -> master) HEAD@{0}: commit: branch test 1
44f135a HEAD@{1}: checkout: moving from hot-fix to master
9fdf6a0 (hot-fix) HEAD@{2}: checkout: moving from master to hot-fix
44f135a HEAD@{3}: commit: branch test 1
9fdf6a0 (hot-fix) HEAD@{4}: checkout: moving from hot-fix to master
9fdf6a0 (hot-fix) HEAD@{5}: checkout: moving from hot-fix to hot-fix
9fdf6a0 (hot-fix) HEAD@{6}: checkout: moving from master to hot-fix
9fdf6a0 (hot-fix) HEAD@{7}: checkout: moving from hot-fix to master
9fdf6a0 (hot-fix) HEAD@{8}: checkout: moving from master to hot-fix
9fdf6a0 (hot-fix) HEAD@{9}: commit: commit 3
61e9b3c HEAD@{10}: commit: commit 2
ea892d4 HEAD@{11}: commit: commit 1
23e5ede HEAD@{12}: commit (initial): first
(END)
```
2. 新建分支：建一个branch，叫做new
```bash
➜  git-tutorial git:(master) ✗ git branch new
➜  git-tutorial git:(master) ✗ git branch -v 
* master 32046fb branch test 1
  new    32046fb branch test 1
(END)

➜  git-tutorial git:(master) ✗ 
```
3. 进行“无冲突合并”演示
```bash
➜  git-tutorial git:(master) ✗ git checkout new                             
A	.gitignore
A	hbx3.cpp
A	test1/hbx1.cpp
A	test1/hbx2.cpp
A	test2/hbx4.cpp
A	test3/hbx_new.cpp
切换到分支 'new'
➜  git-tutorial git:(new) ✗ tree
.
├── branch.md
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
├── test3
│   └── hbx_new.cpp
└── test_branch.cpp

3 directories, 11 files
➜  git-tutorial git:(new) ✗ vim test_branch.cpp
➜  git-tutorial git:(new) ✗ git add .       
➜  git-tutorial git:(new) ✗ git status
位于分支 new
要提交的变更：
  （使用 "git restore --staged <文件>..." 以取消暂存）
	新文件：   .gitignore
	新文件：   hbx3.cpp
	新文件：   test1/hbx1.cpp
	新文件：   test1/hbx2.cpp
	新文件：   test2/hbx4.cpp
	新文件：   test3/hbx_new.cpp
	修改：     test_branch.cpp

➜  git-tutorial git:(new) ✗ git commit -m "branch test in branch_new" test_branch.cpp
[new 2b6ff90] branch test in branch_new
 1 file changed, 1 insertion(+)
➜  git-tutorial git:(new) ✗ git reflog            
2b6ff90 (HEAD -> new) HEAD@{0}: commit: branch test in branch_new
32046fb HEAD@{1}: checkout: moving from master to new
015ec90 (master) HEAD@{2}: commit: branch test 1
54ad6a5 HEAD@{3}: commit: branch test in branch_new
32046fb HEAD@{4}: checkout: moving from hot-fix to master
9fdf6a0 HEAD@{5}: checkout: moving from master to hot-fix
32046fb HEAD@{6}: commit: branch test 1
44f135a HEAD@{7}: checkout: moving from hot-fix to master
9fdf6a0 HEAD@{8}: checkout: moving from master to hot-fix
44f135a HEAD@{9}: commit: branch test 1
9fdf6a0 HEAD@{10}: checkout: moving from hot-fix to master
9fdf6a0 HEAD@{11}: checkout: moving from hot-fix to hot-fix
9fdf6a0 HEAD@{12}: checkout: moving from master to hot-fix
9fdf6a0 HEAD@{13}: checkout: moving from hot-fix to master
9fdf6a0 HEAD@{14}: checkout: moving from master to hot-fix
9fdf6a0 HEAD@{15}: commit: commit 3
61e9b3c HEAD@{16}: commit: commit 2
ea892d4 HEAD@{17}: commit: commit 1
23e5ede HEAD@{18}: commit (initial): first
(END)
➜  git-tutorial git:(new) ✗

---------------------------人为分割线----------------------------------

➜  git-tutorial git:(master) ✗ git merge new    
Merge made by the 'ort' strategy.
 test_branch.cpp | 1 +
 1 file changed, 1 insertion(+)
➜  git-tutorial git:(master) ✗ 
➜  git-tutorial git:(master) ✗ git merge new
已经是最新的。
➜  git-tutorial git:(master) ✗ cat test_branch.cpp
/*************************************************************************
	> File Name: test_branch.cpp
	> Author: amoscykl
	> Mail: amoscykl@163.com 
	> Created Time: 2024年02月09日 星期五 11时02分23秒
 ************************************************************************/

#include<iostream>
using namespace std;
int main()
{
	cout<<"I belong to branch master"<<endl;
    cout<<"in branch new"<<endl;
	return 0;
}

➜  git-tutorial git:(master) ✗ 
```
#### 2）冲突合并
1. 初始：在master_branch中“全部完善”test_branch1.cpp
```bash
➜  git-tutorial git:(master) ✗ vim conf-test.md
➜  git-tutorial git:(master) ✗ cat conf-test.md
1111111111111111111111111111
➜  git-tutorial git:(master) ✗ git add conf-test.md
➜  git-tutorial git:(master) ✗ git status          
位于分支 master
要提交的变更：
  （使用 "git restore --staged <文件>..." 以取消暂存）
	新文件：   conf-test.md

未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）
	.gitignore
	hbx3.cpp
	test1/
	test2/
	test3/

➜  git-tutorial git:(master) ✗ git commit -m "conf_test examples" conf-test.md
[master 2b640c0] conf_test examples
 1 file changed, 1 insertion(+)
 create mode 100644 conf-test.md
➜  git-tutorial git:(master) ✗ tree
.
├── branch.md
├── conf-test.md
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
├── test3
│   └── hbx_new.cpp
├── test_branch1.cpp
└── test_branch.cpp

3 directories, 13 files
➜  git-tutorial git:(master) ✗ 
```
2. 新建分支conf
```bash
➜  git-tutorial git:(master) ✗ git branch conf                        
➜  git-tutorial git:(master) ✗ git checkout conf                      切换到分支 'conf'
```
3. conf进行修改
```bash
➜  git-tutorial git:(master) ✗ git branch conf                                
➜  git-tutorial git:(master) ✗ git checkout conf                              
切换到分支 'conf'
➜  git-tutorial git:(conf) ✗ cat conf-test.md
1111111111111111111111111111
➜  git-tutorial git:(conf) ✗ vim conf-test.md
➜  git-tutorial git:(conf) ✗ cat conf-test.md
1111111111111111111111111111
2222222222222222222222222222
3333333333333333333333333333
➜  git-tutorial git:(conf) ✗ git add conf-test.md
➜  git-tutorial git:(conf) ✗ git commit -m "conf_test in branch_conf" conf-test.md 
[conf 65cb04b] conf_test in branch_conf
 1 file changed, 2 insertions(+)
➜  git-tutorial git:(conf) ✗ 
```
4. master同时进行修改
```bash
➜  git-tutorial git:(master) ✗ cat conf-test.md
1111111111111111111111111111
➜  git-tutorial git:(master) ✗ vim conf-test.md
➜  git-tutorial git:(master) ✗ cat conf-test.md
1111111111111111111111111111
5555555555555555555555555555
6666666666666666666666666666
yyyyyyyyyyyyyyyyyyyyyyyyyyyy
➜  git-tutorial git:(master) ✗ git add conf-test.md
➜  git-tutorial git:(master) ✗ git commit -m "conf_test in branch_mast" conf-test.md
[master 43db437] conf_test in branch_mast
 1 file changed, 3 insertions(+)
➜  git-tutorial git:(master) ✗ 
```
5. 冲突产生
```bash
➜  git-tutorial git:(master) ✗ git merge conf                                       
自动合并 conf-test.md
冲突（内容）：合并冲突于 conf-test.md
自动合并失败，修正冲突然后提交修正的结果。

➜  git-tutorial git:(master) ✗ git status    
位于分支 master
您有尚未合并的路径。
  （解决冲突并运行 "git commit"）
  （使用 "git merge --abort" 终止合并）
未合并的路径：
  （使用 "git add <文件>..." 标记解决方案）
	双方修改：   conf-test.md
未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）
	.gitignore
	hbx3.cpp
	test1/
	test2/
	test3/

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```
6. 冲突解决方法：进入冲突文件，人为修改
```bash
➜  git-tutorial git:(master) ✗ cat conf-test.md
1111111111111111111111111111
<<<<<<< HEAD
5555555555555555555555555555
6666666666666666666666666666
yyyyyyyyyyyyyyyyyyyyyyyyyyyy
=======
2222222222222222222222222222
3333333333333333333333333333
>>>>>>> conf
➜  git-tutorial git:(master) ✗ vim conf-test.md
➜  git-tutorial git:(master) ✗ ......(修改ing)
➜  git-tutorial git:(master) ✗ cat conf-test.md
1111111111111111111111111111
5555555555555555555555555555
2222222222222222222222222222
3333333333333333333333333333
➜  git-tutorial git:(master) ✗ 
```
7. 现在提交到暂存区
```bash
➜  git-tutorial git:(master) ✗ git add conf-test.md
➜  git-tutorial git:(master) ✗ git status          
位于分支 master
所有冲突已解决但您仍处于合并中。
  （使用 "git commit" 结束合并）

要提交的变更：
	修改：     conf-test.md

未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）
	.gitignore
	hbx3.cpp
	test1/
	test2/
	test3/

➜  git-tutorial git:(master) ✗ 
```
8. 现在推送到本地库[此时git commit 不能带文件名！]
```bash
ERROR:
➜  git-tutorial git:(master) ✗ git commit -m "conf_test merge" conf-test.md 
fatal: 在合并过程中不能做部分提交。

TRUE:
➜  git-tutorial git:(master) ✗ git commit -m "conf_test merge"        
[master 6ad2082] conf_test merge
➜  git-tutorial git:(master) ✗ 
```
9. 看看LOG
```bash
6ad2082 (HEAD -> master) HEAD@{0}: commit (merge): conf_test merge
43db437 HEAD@{1}: commit: conf_test in branch_mast
2b640c0 HEAD@{2}: checkout: moving from conf to master
65cb04b (conf) HEAD@{3}: checkout: moving from conf to conf
65cb04b (conf) HEAD@{4}: commit: conf_test in branch_conf
2b640c0 HEAD@{5}: checkout: moving from master to conf
2b640c0 HEAD@{6}: commit: conf_test examples
a75a396 HEAD@{7}: reset: moving to HEAD
a75a396 HEAD@{8}: checkout: moving from will-conflict to master
0dd78db HEAD@{9}: commit: branch test in will-conflict
26ca918 HEAD@{10}: checkout: moving from master to will-conflict
a75a396 HEAD@{11}: commit (merge): branch test in master
d9e9049 HEAD@{12}: commit: branch test in master
2d4bcaa HEAD@{13}: checkout: moving from will-conflict to master
26ca918 HEAD@{14}: commit: branch test in will-conflict
2d4bcaa HEAD@{15}: checkout: moving from master to will-conflict
2d4bcaa HEAD@{16}: commit: branch test 2
6cff0bd HEAD@{17}: merge new: Merge made by the 'ort' strategy.
015ec90 HEAD@{18}: reset: moving to HEAD
015ec90 HEAD@{19}: checkout: moving from new to master
2b6ff90 (new) HEAD@{20}: checkout: moving from new to new
2b6ff90 (new) HEAD@{21}: checkout: moving from master to new
015ec90 HEAD@{22}: checkout: moving from new to master
2b6ff90 (new) HEAD@{23}: commit: branch test in branch_new
32046fb HEAD@{24}: checkout: moving from master to new
015ec90 HEAD@{25}: commit: branch test 1
54ad6a5 HEAD@{26}: commit: branch test in branch_new
32046fb HEAD@{27}: checkout: moving from hot-fix to master
9fdf6a0 HEAD@{28}: checkout: moving from master to hot-fix
32046fb HEAD@{29}: commit: branch test 1
44f135a HEAD@{30}: checkout: moving from hot-fix to master
9fdf6a0 HEAD@{31}: checkout: moving from master to hot-fix
44f135a HEAD@{32}: commit: branch test 1
9fdf6a0 HEAD@{33}: checkout: moving from hot-fix to master
9fdf6a0 HEAD@{34}: checkout: moving from hot-fix to hot-fix
9fdf6a0 HEAD@{35}: checkout: moving from master to hot-fix
9fdf6a0 HEAD@{36}: checkout: moving from hot-fix to master
9fdf6a0 HEAD@{37}: checkout: moving from master to hot-fix
9fdf6a0 HEAD@{38}: commit: commit 3
61e9b3c HEAD@{39}: commit: commit 2
ea892d4 HEAD@{40}: commit: commit 1
23e5ede HEAD@{41}: commit (initial): first
(END)
```