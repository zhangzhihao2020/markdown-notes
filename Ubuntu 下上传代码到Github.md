# Ubuntu 下上传代码到Github
第一步：建立git仓库

cd到你的本地项目根目录下，执行git命令，此命令会在当前目录下创建一个.git文件夹。
```
git init
```
第二步：将项目的所有文件添加到仓库中
```
git add ./
```
添加单个文件
```
git add filename
````
这个命令会把当前路径下的所有文件，添加到待上传的文件列表中。
如果想添加某个特定的文件，只需把.换成特定的文件名即可

第三步：将add的文件commit到仓库
```
git config --global user.email "2970541981@qq.com"
git config --global user.name "zhangzhihao2020"
git commit -m "注释"
```
第四步：
```
git remote add origin git@github.com:tangqian-cqu/zzh-robot.git
```
第五步，上传代码到github远程仓库
```
git push -u origin master
```
执行完后，如果没有异常，等待执行完就上传成功了，中间可能会让你输入Username和Password，你只要输入github的账号和密码就行了.

第一次上传有可能会遇到push失败的情况，那是因为跟SVN一样，github上有一个README.md 文件没有下载下来 。我们得先
```
git pull --rebase origin master
```
然后执行：       
```
git push -u origin master
```

参考网址https://www.jianshu.com/p/e61c4ceec96c