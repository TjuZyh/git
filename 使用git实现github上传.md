#### 使用git实现github上传

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
//""中为登录github的邮箱
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

###### 5.将生成好的ssh配置到github中

步骤：进入到github首页，点击右上角的头像，进入**settings/SSH and GPG Keys** ，新建SSH key，title随意，key直接ctrl+v即可

###### 6.验证是否ssh添加成功

```
ssh -T git@github.com

//若出现Hi TjuZyh! You've successfully authenticated, but GitHub does not provide shell access.即添加成功
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

直接在github中创建即可，添加repository name以及description，**一定要勾选add a README file**

###### 2.查看改动

```
git status
```

###### 3.添加至本地仓库

```
//.表示更改所有的改动
git add .
```

###### 4.提交本地仓库

```
//""中填写改动内容
git commit -m “修改信息”
```

###### 5.配置github仓库路径

```
//后面的ssh地址可以从github目标仓库的code/clone/SSH中拷贝
git remote add origin git@github.com:TjuZyh/blockChain-study.git

//出现fatal: remote origin already exists报错
git remote rm origin
```

###### 6.push代码

```
//push到github仓库的master分支中
git push -f origin master
```





