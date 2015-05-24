# Git 
###git 提交版本
    git status               //查看当前状态
	... ...
	git add .	
	... ...
	git commit -m "版本名称"
	git push origin master  //或者push到你的开发分支上

###git 分支开发
    git branch develop     //创建develop分支
	git checkout develop   //切换到develop分支上
	
###git 回滚版本
	git branch Reserve     //备份一下当前版本
	git reflog
	... ...
	git reset --hard XXXX  //回滚到之前的版本

###git 协同开发时更新本地版本
	git status
	git pull origin        //本地的当前分支自动与对应的origin主机”追踪分支”(remote-tracking branch)进行合并
	/*如果当前分支只有一个追踪分支，连远程主机名都可以省略*/
	git pull               //命令表示当前分支自动与唯一一个追踪分支进行合并

###git Fork从主分支上更新
	$git remote -v         //查看远程branch状态
	
	/*添加remote name 是upstream 的主分支*/
	git remote add upstream https://github.com/TommyHili/Angular-Blog.git      	
	git fetch upstream     //从远程拉取最新的branch
	git merge upstream/master 
    /* rebase也可以 ×/
	git rebase upstream/master
###git 其他命令使用

- git revert 撤销 某次操作，此次操作之前和之后的commit和history都会保留，并且把这次撤销作为一次最新的提交 
