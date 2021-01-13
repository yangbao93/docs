# git常用命令，常见问题
Git配置用户邮箱，用户名
1. 配置用户邮箱：git config (双横线)- - global user.email ***@***.com
2. 配置用户名：git config (双横线)- - global user.name “****"

Git生成ssh秘钥

1. 生成代码：ssh-keygen -t rsa -f ~/.ssh/id_rsa.***
2. 一路回车
3. 获取公钥：cat ~/.ssh/id_rsa.***.pub
Ssß

常用命令：

1.从仓库下载代码
git pull origin develop（仓库分支）

2.上传代码
git push

3.合并代码
git fetch

4.下载代码
git clone ssh的url

5.查看分支
git branch -a(全部)
git branch -r

6.切换分支
git checkout branchName

常见问题：

1.Git Pull Failed
You have not concluded your merge (MERGE_HEAD exists).
Exiting because of unfinished merge.
存在未完成的merge操作
解决办法：①保留本地修改 git merge --abort ; git reset --merge ; 合并后记得提交这个本地合并，然后获取线上仓库; git pull
②抛弃本地修改，获取线上版本：git fetch --all ; git reset --hard origin/master ; git fetch

2.合并后的文件内容混乱，抛弃这次合并，回到合并前的内容：git reset --hard HEAD