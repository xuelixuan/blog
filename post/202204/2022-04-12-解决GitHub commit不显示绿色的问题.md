Contributions 未被 Github 计入的几个常见原因
进行 Commits 的用户没有被关联到你的 Github 帐号中。
不是在这个版本库的默认分支进行的 Commit。
这个仓库是一个 Fork 仓库，而不是独立仓库。
我的是独立仓库，查了一下发现我的 git 记录指向另一个账号，原来是 commit 的邮箱和用户名不对。可以用 git show 发现邮箱那里跟 github 的账号邮箱不一样。解决步骤：

使用 git show 查看本地端的邮箱
在 GitHub 的个人账号的设置里，找到 Email，找到 Add email address，把本地邮箱填上去
添加并绑定验证，刷新，绿色格子就出来了！
当然也可以修改本地 git 配置

git config --global user.name “username”
git config --global user.email “username@mail.com”
