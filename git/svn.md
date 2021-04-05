svn log: 用来展示svn 的版本作者、日期、路径等等。

svn diff: 用来显示特定修改的行级详细信息。

svn cat: 取得在特定版本的某文件显示在当前屏幕。

svn list: 显示一个目录或某一版本存在的文件。

------------------------------------

在本地副本中创建一个 my_branch 分支  
svn copy trunk/ branches/my_branch

接着我们就到 my_branch 分支进行开发  
cd branches/my_branch/

切换到 trunk，执行 svn update，然后将 my_branch 分支合并到 trunk 中。   
svn merge ../branches/my_branch/

--------------------------------------

我们在本地工作副本创建一个 tag  
svn copy trunk/ tags/v1.0

上面的代码成功完成，新的目录将会被创建在 tags 目录下。  
 ls tags/v1.0/


提交tag内容。  
svn commit -m "tags v1.0" 

---------------------------------------