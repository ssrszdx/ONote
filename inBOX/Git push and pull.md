
使用[Git](http://lib.csdn.net/base/git "Git知识库")  直接提交的话   直接 push

获取最新版本  有两种  拉取 和 获取 pull 和 fetch

git  pull     从远程拉取最新版本 到本地  自动合并 merge            git pull origin master

git  fetch   从远程获取最新版本 到本地   不会自动合并 merge    git fetch  origin master       git log  -p master ../origin/master     git merge orgin/master

实际使用中  使用git fetch 更安全    在merge之前可以看清楚 更新情况  再决定是否合并

[^1]: 是
