# typora+picgo+阿里云+git实现云存储文档

PicGo最大的特点是，可以和Typora结合使用，配置好关联之后，Typora写文章时，如果需要穿插图片，只需要将图片复制粘贴到Typora的编辑区域，就自动通过PicGo上传到指定图床，得到外网能访问的URL并展示。

1、第一步：安装PicGo

2、在PicGo中打开PicGo设置，找到设置Server，点击设置，点击开启Server，点击确定即可。

3、配置Typora。我们在Typora中打开文件-偏好设置，选择PicGo(app)，路径就是安装PicGo的文件夹，选中PicGo.exe即可

![image-20240528165425624](https://karse-typora-pictures.oss-cn-guangzhou.aliyuncs.com/img/202405281654221.png)

1、注册阿里云账号后,开通对象储存，进入对象存储OSS的控制台。

2、在左侧选择概览，然后在右侧Bucket管理中创建一个新的bucket

3、创建Bucket具体配置。Bucket名字不能有大写字母、地域就近选择、存储类型选择标准存储，读写权限公共读

4、创建AccessKey。页面右上角，鼠标放在头像处，在弹出的框里选择AccessKey管理，在弹出的选项框里，选择继续使用。

5、目前我们购买了下OSS存储包，但是依旧要为访问图床的流量付钱，仔细算过以后，作为图床的数据量其实很小的，我们使用默认的0.12元/1GB/1个月，一年共计1.44元，远低于40GB的9元购买年储存包。最后记得给阿里云账户充值几块钱，估计2块钱就可以用一年左右。

6、配置PicGo。我们打开打开PicGo的主界面,在图床设置里面选择阿里云OSS，依照下面注意事项填写信息。

设定Keyld：填写我们在第三步中获得的AccessKeyID

设定KeySecret：填写我们在第三步中获得的AccessKeyIDSecret

设定储存空间名：填写我们在第二步中填写的bucket名称

确认存储区域：填写我们在第二步中查看的地域节点，注意复制的格式：只需要复制oss-cn-Xxxx即可，不需要后面的.aliyuncs.com

指定存储路径：其实就是自定义一个文件夹的名字，以/结尾，它会自动在你的bucket里面创建一个文件夹，并把图片上传进去。

![image-20240528165501272](https://karse-typora-pictures.oss-cn-guangzhou.aliyuncs.com/img/202405281655363.png)

8、到此设置完毕。现在我们可以使用Markdown开始写文章了，图片、截图会在粘贴之后，自动通过PicGo上传到了远端图床



9、实现typora文档云存储。登陆github, 并新建一个远程仓库, 用于存放文档

给仓库取一个名字, 选择Public, 而README, [gitignore](https://so.csdn.net/so/search?q=gitignore&spm=1001.2101.3001.7020)文件都可以后续再添加, 这里我也全部忽略

10、创建本地仓库。在对应的项目目录中打开命令工具, win10可以直接在当前目录shift右击, 选择 **在此处打开Powershell窗口**

![image-20240528180240106](https://karse-typora-pictures.oss-cn-guangzhou.aliyuncs.com/img/202405281802217.png)

此时, 我们就成功新建了一个空的本地仓库. 同时, 在目录下也会新增一个 **隐藏格式的** .git的文件

添加.gitignore文件, 用于过滤掉不用上传的文件, 仍然是在当前目录的命令行工具中, 执行`New-Item .gitignore`

用记事本打开刚刚创建的.gitignore文件, 写上相应的**过滤规则**(咨询度娘), 我这里希望忽略本地的图片资源, 所以可以写入如下内容并保存

![image-20240528174200524](https://karse-typora-pictures.oss-cn-guangzhou.aliyuncs.com/img/202405281742001.png)

11、关联。

- 在目录的命令行工具中执行

   `git remote add origin git@github.com:[githubID]/[repositoryName].git`, 关联过程中会要求你输入账号密码, 当然如果已经与本地计算机建立了 **SSH**协议就不用了

- 关联成功的话, 我们可以通过`git remote -v`进行查看

![image-20240528180700225](https://karse-typora-pictures.oss-cn-guangzhou.aliyuncs.com/img/202405281807212.png)

12、将本地文件同步到远程。

- 在目录的命令行工具中依此执行`git add .`
- `git commit -m "some description"`
- `git push -u origin master`

命令解释

```
git add 文件名

git commit -m"备注"

git push origin master
```

