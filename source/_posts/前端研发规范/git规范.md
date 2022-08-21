分支命名规范：

主分支命名：master

开发分支命名： dev

现在要开发一个版本，版本主分支命名：dev_version

当前版本下的需求分支命名：feat_version_xxFunction

线上bugfix分支命名：bugfix_version_xxFunction

commit 规范
使用type+描述如： "feat: 增加一个新功能"

type用于说明 commit 的提交性质

值	描述
feat	新增一个功能
fix	修复一个Bug
docs	文档变更
style	代码格式（不影响功能，例如空格、分号等格式修正）
refactor	代码重构
perf	改善性能
test	测试
build	变更项目构建或外部依赖（例如scopes: webpack、gulp、npm等）
ci	更改持续集成软件的配置文件和package中的scripts命令，例如scopes: Travis, Circle等
chore	变更构建流程或辅助工具
revert	代码回退
版本开发流程

领取分析需求，判断需要建立多少分支与需求对应。原则上要求每个需求都有单独的分支进行开发，开发完成后提交合并请求并入版本主分支

拉取当前版本的分支，建立需求A对应的分支，feature_version_xxFunction

开发调试完成，提交到本地commit。准备提交远程，如果版本分支已有别人提交的代码，需要先拉取远程分支与当前需求分支合并，处理冲突然后上传。

上传完成后，在gitlab页面向版本主分支提交一个合并请求，合并请求审核通过后，可以在gitLab页面上进行合并。

测试完成后，上线时以最终的dev_version分支为准，上线完成后，将dev_version分支合并入master分支，同时打上上线时间、上线环境的tag，版本开发流程结束。




线上bug修复开发流程：

根据当前环境，拉取对应升级节点tag的代码，以线上代码作为基准建立bugfix分支，bugfix-A。

开发调试完成后，以bugfix分支作为测试分支，提交测试完成上线后，合并入master，打上tag，同步合并进现有的开发分支中。

注意事项：

开发新需求时，以需求为单位建立分支，同项目下多人协作的需求，应该使用同一个需求分支合作开发。

如果基于的版本分支有更新，应该及时拉取更新处理冲突，需求处理完提交合并请求前，先合并基准分支，保证请求合并时不会有冲突。

需求开发完成后，必须提交合并请求，审核通过后才能合并入主分支。

常用的git操作示例

假设现在当前版本分支为 dev_version ，需求为xxFunction，现在开始开发

新建分支：
git checkout dev_version       // 检出dev_version分支
git fetch origin dev_version         // 拉取远程仓库dev_version最新的代码
git checkout -b feature_version_xxFunction      // 基于dev_version 新建分支
git push origin feature_version_xxFunction      // 将新建的分支推送到远程仓库
开发过程：
git add .    // 将工作区的修改保存待提交。
git commit -m “提交信息” // 将当前保存的修改整体作为一次commit进行提交，添加提交信息。此时本地的修改已经完成，等待推送到远程。
更新合并远程代码 ，如果commit在本地没有提交，使用rebase来合并其他代码，
git checkout dev_version    // 切换到当前版本分支
git pull    // 拉取远程代码
git checkout feature_version_xxFunction    // 切换到当前工作分支
git rebase dev_version    // 合并dev_version上的代码到当前工作分支
rebase过程中可能出现冲突，需要手动修改代码来处理冲突，处理完成后：
git add .
git rebase —continue
如果处理冲突出错需要退出合并：
git rebase —abort
如果本地的commit已经push到远程，采用merge方式来合并其他分支的代码
git merge dev_version
// merge中产生冲突，先add 再commit 用一次commit来处理合并冲突
合并完成后，本地代码目前是包含了主分支代码及自己的开发代码，推送到远程
git push origin feature_version_xxFunction
然后在gitLab上提交一个合并请求，表明当前需求你的开发工作已经完成，想要合并入当前版本主分支:

Source branch 选择你当前开发的分支 Target branch 选择你想要合入的分支 提交审核后，可以直接在gitLab页面上选择合并，一般来说之前处理过冲突的话，这里合并不会再产生冲突了

开发合并完成后，删除当前分支：
git checkout master   // 切换到其他分支，在当前分支时不能删除自身
git branch -d testBranch       // 删除本地分支
git push --delete origin testBranch     // 删除远程分支