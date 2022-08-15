#### 使用git实现github/gitee上传

##### 一、初始化git

###### 1.安装git

```
//安装后测试git是否可用
git --version
```

###### 2.定位到目标文件夹，初始化git（这里以test为例）

```
//定位到test文件夹
cd ./desktop/test

//初始化git
git init
```

###### 3.配置ssh

```
//""中为登录github或gitee的邮箱
//将会在你选择的路径下上生成 ssh key，如果你直接点击回车，会在默认路径下创建 ssh 。

ssh-keygen -t rsa -C “tju_zhaoyihan@163.com”

//输入后，会出现选择路径，直接使用括号中的路径即可
//输入路径后，会要求输入密码，直接回车则是不设置密码。点击回车不设置密码；然后会让你重复密码，也是直接回车。
```

出现 **The key's randomart image is :  SHA256**则表示ssh已经生成

###### 4.拷贝ssh代码

```
//注意后面的路径是上面的路径
pbcopy < ~/.ssh/id_rsa.pub
```

###### 5.将ssh配置到远程仓库

步骤：

- 进入到github首页，点击右上角的头像，进入**settings/SSH and GPG Keys** ，新建SSH key，title随意，key直接ctrl+v即可
- 进入gitee，选择ssh公钥，添加公钥

###### 6.验证是否ssh添加成功

```
ssh -T git@github.com
//Hi TjuZyh! You've successfully authenticated, but GitHub does not provide shell access.

ssh -T git@gitee.com
//Hi tju_zhaoyihan! You've successfully authenticated, but GITEE.COM does not provide shell access.
```

###### 7.配置github的登录名以及登录邮箱

```
//”“中填github用户名
git config –global user.name “TjuZyh”
//”“中填github登录邮箱
git config –global user.email “tju_zhaoyihan@163.com”
```

##### 二、上传github

###### 1.配置一个空的仓库

直接在github中创建即可，添加repository name以及description，**最好勾选add a README file（不勾选也可以后期建）**

```
git init
```

###### 2.更新本地仓库

**注意如果是团队合作push更改后的代码，需要执行**

- 如果不冲突，会把本地未修改的部分覆盖
- 如果冲突，会提示*Your local changes to the following files would be overwritten by merge*，本次pull失败，不会改动本地代码

```
git pull
```

因为在你push之前，可能有人在你之前push过了新的一版代码，你需要将自己本地未修改的代码pull成最新的

###### 3.添加至本地仓库

```
//查看修改信息
git status

//.表示更改所有的改动
git add .
```

###### 4.提交本地仓库

```
//""中填写改动内容
git commit -m “修改信息”
```

###### 5.（若第一次创建，需要配置远程仓库路径）

```
//后面的ssh地址可以从github目标仓库的code/clone/SSH中拷贝
git remote add origin git@github.com:TjuZyh/blockChain-study.git

//出现fatal: remote origin already exists报错，因为已经连接到远程了，不需要执行这一步了，除非你想连接其他仓库
git remote rm origin
```

###### 6.push代码

```
git push

git push origin main

//push到github仓库的main分支中
//-f为强制上传覆盖远程文件
//团队开发时不要使用该模式！
git push -f origin main
```

###### 7.问题解决

**1、报错：因为在github上创建仓库存在readme文件，而本地没有**

```
zyh@yihandeMacBook-Pro javaSE % git push origin main
To github.com:TjuZyh/javaSE.git
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'git@github.com:TjuZyh/javaSE.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

**解决：**

- 在本地生成一个Readme文件

  ```
  git pull --rebase origin master     本地生成ReadMe文件
  git push origin master
  ```

- 强制上传

  ```
  git push -f origin master
  ```

**2、 error: src refspec xxx does not match any / error: failed to push some refs to**

**原因：在github上创建的工程默认分支名为main，而本地为master；导致了远程与本地的仓库关联不上**

**解决：统一远程和本地的仓库名称即可**

```
//git branch -m oldBranchName newBranchName
//将本地的master变为远程的main
git branch -m master main
//再push即可
git push origin main
```

**3、在执行git branch -M main 时 报error: refname refs/heads/master not found  fatal: Branch rename failed**

**原因：要先初始化、add、commit之后，在进行更多操作**

##### 三、简易手册

**1、针对于在本地写好的项目第一次上传github时：**

```
git init
git add .
git commit -m "meg"
//改名操作，因为github现在以main作为默认分支（原来为master）
git branch -M main
//连接远程仓库，只需要第一次连接
git remote add origin XXXX.git
git push -u origin main
```

**2、针对于个人项目迭代**

```
git add .
git commit -m "meg"
//可以先看一下本地分支
git branch
git push origin main
```

**3、针对于团队开发**

```
//拉一下最新的代码
git pull
git add .
git commit -m "meg"
//可以先看一下本地分支
git branch
git push origin main
```

##### 四、分支

###### 1.查看分支

```
// 查看远程分支
git branch -a

// 查看本地分支
git branch
```

###### 2.切换分支

```
// 切换本地分支 自动查询远程分支
git checkout objBranch
// 查看本地当前分支
git checkout
```

##### 五、关于忽略文件

###### 1. 新建.gitignore文件

在该文件中，填写需要省略的文件或文件夹

```.gitignore
//忽略.idea目录下的全部文件
.idea/

//忽略.idea目录下的xxxx.txt
.idea/xxxx.txt 

//不忽略do.txt文件
!do.txt

//忽略所有的.jar文件
*.jar
```

注意：如果要忽略的文件已被git管理，则需要删除对应缓存

```
git rm -r --cached .idea/    //-r为递归
```

删除后即可，可以通过status查看删除的文件，后add、commit、push等操作

##### 六、删除原有git信息

可能存在这样的情况：一个项目已经被上传在其他仓库中，想要上传到自己的仓库

###### 1. 查看.git文件夹

```
ls -a
```

###### 2. 删除.git文件夹

```
rm -fr .git
```

删除后，即可init初始化，重新上传至仓库了
