##本地仓库如何与github远程仓库关联

1、本地仓库初始化 git init

2、添加文件到本地仓库 git add .

3、提交文件到本地仓库 git commit -m "first commit"

4、关联远程仓库 git remote add origin https://github.com/yourname/yourproject.git

5、更改master为main git branch -m main (如果报错，改为-M)

6、拉取远程仓库文件到本地仓库 git pull origin main (如果报错 refusing to merge unrelated histories ， 加上--allow-unrelated-histories参数)

7、上传远程仓库git push  origin main
