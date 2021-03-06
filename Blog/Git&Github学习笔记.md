# Git&Github学习笔记

Git是一种分布式版本控制系统，而Github是一个远程代码库。

## 版本控制工具

### 协同写作

多人并行不悖的修改服务器端的同一个文件。

### 数据备份

不仅保存目录和文件的当前状态，还能够保存每一个提交过的历史状态

### 版本管理

在保存每一个版本的文件信息的时候要做到不保存重复数据，以节约存储空间，提高运行效率。这方面SVN采用的是增量式管理的方式，而Git采取了文件系统快照的方式。

### 权限控制

* 对团队中参与开发的人员进行权限控制
* 对团队外开发者贡献的代码进行审核--Git独有

### 历史记录

* 查看修改人、修改事件、修改内容、日志信息
* 将本地文件恢复到某一个历史状态

### 分支管理

允许开发团队在工作过程中多条生产线同时推进任务，进一步提高效率

## 版本控制简介

### 版本控制

工程设计领域中使用版本控制管理工程蓝图的设计过程，在IT开发过程中也可以使用版本控制思想管理代码的版本迭代。

### 版本控制工具

* 集中式版本控制工具：CVS、SVN、VSS...，存在单点故障问题
* 分布式版本控制工具：Git、Mercurial、Bazaar、Darcs..

## Git

### Git的优势

* 大部分操作在本地完成，不需要联网
* 完整性保证
* 尽可能添加数据而不是删除或修改数据
* 分支操作非常快捷流畅
* 与Linux命令全面兼容

### Git结构

### Git和代码托管中心

代码托管中心的任务：维护远程库
* 局域网环境下
	* GitLab服务器
* 外网环境下
	* GitHub
	* 码云
	- ### 本地库和远程库
		* 分区
			* 工作区：写代码的地方
			* 暂存区：临时存储，可以暂存、撤销修改等
			* 本地库：保存历史版本
		* 任务
			* 团队内部协作
			* 跨团队协作

1. ## Git命令行操作
	- ### 本地库初始化 
		- 命令： `git init`
		- .git目录中存放的是本地库相关的子目录和文件
	- ### 设置签名
		- 用户名、Email地址
		- 作用：区分不同开发人员的身份
		- 辨析：这里设置的签名和登录远程库的账号密码没有关系
		- 命令
			- 项目级别/仓库级别：仅在当前本地库范围内有效 
				- `git config user.name xxx`
				- `git config user.email xxx` 
			- 系统用户级别：登录当前操作系统的用户范围 `git config --global`
			- 级别优先级
				- 就近原则：项目级别优先于系统用户级别，二者都有时采用项目级别的签名
				- 如果只有系统用户级别的签名，就以系统用户级别的签名为准
				- 二者都没有是不允许的 
	- ### 基本操作
		- 状态查看操作 `git status`：查看工作区、暂存区状态
		- 添加操作 `git add [file name]`：将工作区的“新建/修改”添加到暂存区
		- 提交操作 `git commit -m "COMMIT MESSAGE" [file name]`：将暂存区的内容提交到本地库
		- 查看历史记录操作 `git log` `git log --pretty=oneline` `git --oneline` `git reflog`
		- 版本的前进后退
			- 本质是移动head指针
			- 基于索引值操作（推荐）：`git reset --hard [HASH INDEX]`
			- 使用^符号、使用~符号：只能后退：`git reset --hard HEAD^^^` `git reset --hard HEAD~3`
		- reset命令的三个参数对比
			- `git reset --soft`：仅在本地库移动HEAD指针
			- `git reset --mixed`：在本地库移动HEAD指针，也会充值暂存区
			- `git reset --hard` ：在本地库移动HEAD指针，重置暂存区和工作区
		- 删除文件并找回
			- 前提：删除前，文件存在时的状态提交到了本地库
			- 操作：`git reset -- hard [HEAD]`	
				- 删除操作已经提交到了本地库：指针位置指向历史记录
				- 删除操作尚未提交到本地库：指针位置使用HEAD 
		- 比较文件差异
			- `git diff [FILE NAME]`：将工作区中的历史和暂存区进行比较
			- `git diff [本地库中历史版本] [FILE NAME]`：将工作区中的文件和本地库历史记录比较
			- 不带文件名比较多个
	- ### Branch 分支管理
		- 什么是分支：在版本控制中
	 	- 分支的好处
	 		- 同时并行推进多个功能开发，提高开发效率
	 		- 各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响，失败的分支删除重新开始即可 
	 	- 分支操作
	 		- 创建分支 `git branch [BRANCH NAME]`
	 		- 查看分支 `git branch -v`
	 		- 切换分支 `git branch checkout [BRANNAME]`
	 		- 合并分支
	 			- 第一步：切换到接收修改的分支（被合并，增加新内容）上 `git checkout [BRANCH NAME OF GET MERGED]`
	 			- 第二步：执行merge命令 `git merge [BRANCH NAME OF CONTENT PROVIDER]`
	 		- 解决冲突
	 			- 第一步：编辑文件，删除特殊符号
	 			- 第二步：把文件修改到满意的程度，保存退出
	 			- 第三步：`git add [FILE NAME]`
	 			- 第四步：`git commit -m "LOG"`，注意此时commit一定不能带具体文件名
5. ## Git原理
	- ### 哈希
		- 哈希是一个系列的加密算法，各个不同的哈希算法虽然加密强度不同，但是有以下几个共同点
			- 不管输入数据的数据量有多大，输入同一个哈希算法，得到的加密结果长度固定
			- 哈希算法确定，输入数据确定，输出数据能够保证不变
			- 哈希算法确定，输入数据有变化，输出数据一定有变化，而且通常变化很大
			- 哈希算法不可逆
		- Git底层采用的是SHA-1算法
		- 哈希算法可以被用来验证文件
	- ### Git保存版本的机制
		- 集中式版本控制工具的文件管理机制：增量式管理。以文件变更列表的方式存储信息，这类系统将它们保存的信息看作是一组基本为你缴纳和每个文件随事件逐步累积的差异
		- Git的文件管理：Git采用快照流，Git把数据看作是小型文件系统的一组快照，每次提交更新时Git都会对当前的全部文件制作一个快照并保存这个快照的索引。为了高效，如果文件没有修改，Git不再重新存储该文件，而是只保留一个连接指向之前存储的的文件
	- ### Git分支管理 机制 
		- 分支的创建
		- 分支的切换
6. ## GitHub
	- ### 账号信息
	- ### 创建远程库
	- ### 创建远程库地址
		- 查看当前所有远程地址别名 `git remote -v [ADDRESS OF REPO]`
		- 存储远程库地址到本地 `git remote add [NIKNAME] [ADDRESS OF REPO]` 
	- ### Push 推送
		- `git push [NIKNAME] [BRANCH NAME]`
	- ### Clone 克隆
		- 命令 `git origin [ADDRESS OF REPO]`
		- 效果
			- 完整的把远程库下载到本地
			- 创建origin远程地址别名
			- 初始化本地库
	- ### Collabaration 团队成员邀请 
