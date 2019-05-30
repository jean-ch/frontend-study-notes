- git reset 回退版本   
- git checkout -- file ```git checkout -- readme.txt```把readme.txt文件在工作区的修改全部撤销, 让这个文件回到最近一次git commit或git add时的状态  
- git checkout -b [branch-name] 创建并且切换branch 
- branch    
	- git branch  查看当前branch  
	- git [branch-name] 创建branch     
	- git merge [branch-name] 合并branch到当前checkout的branch   
	- git branch -d [branch-name] 删除branch    
	- git branch -D [branch-name] 强行删除没有merge过的branch   
- conflict： git merge之后手动resolve再commit   
- git stash, git stash apply/pop    
- rebase: 分叉提交变成一条直线     





