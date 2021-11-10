# git-practice

- 设定有多个情况，分别如下：

1. 情况一：`新增文件 docOne`，本地生成一个 branch:team/hodgepodge 上传至 github，不执行 pull request。`（模拟小组 branch 初始化。）`

- 文件 docOne 文本是

```diff
+ This is document 1 created from `team/hodgepodge` branch.
```

- 相关命令：

```bash
$ git clone git@github.com:DonghaoWu/git-practice.git
$ cd git-practice

$ git checkout -b team/hodgepodge
$ git add .
$ git commit -m'created branch team/hodgepodge with docOne.'

$ git push origin team/hodgepodge
```

- :gem: 此时 Github 上有两个 branch。

2. 在 github 对 team/hodgepodge branch 进行修改，`新增文件 docTwo` 。`（模拟有小组成员修改了小组 branch。）`

- 文件 docTwo 文本是

```diff
+ This is document 2 created from Github on branch `team/hodgepodge`.
```

- :gem: 此时 Github 上有两个 branch。

3. 在本地另外一个位置下载代码，然后生成一个新 branch:localTeam，对接情况一的 team/hodgepodge, 然后在 localTeam 的基础上生成一个新的 branch:newFeature，`新增文件 docThree`，上传至 github，执行 pull request，将新 newFeature 合并到 team/hodgepodge 中。 `（模拟本地开发完成新功能，合并到小组的 team/hodgepodge 中。）`

- 文件 docThree 文本是

```diff
+ This is document 3 created from newFeature based on a local branch called localTeam which is based on a remote branch called team/hodgepodge.
```

- 相关命令：

```bash
$ git clone git@github.com:DonghaoWu/git-practice.git
$ cd git-practice

$ git checkout -b localTeam origin/team/hodgepodge #包含了对接两地 branch，下载 remote branch，并转换至 local branch 三个动作。

$ git checkout -b newFeature

$ git add .
$ git commit -m'created branch newFeature with docThree.'
$ git push origin newFeature
```

- 第三行可以替代为：

```bash
$ git pull origin team/hodgepodge:localTeam
$ git checkout localTeam
```

- :gem: 此时 Github 上有三个 branch，可以选择 merge 之后删除 newFeature branch。
- 此时本地 localTeam branch 只有 docOne 和 docTwo，main 一个都没有。

4. 在 github 对 team/hodgepodge branch 进行修改，`新增文件 docFour` 。`（模拟有小组成员修改了小组 branch。）`，然后本地 localTeam 需要进行更新。

- 文件 docFour 文本是

```diff
+ This is document 4 created from Github on branch `team/hodgepodge`.
```

- 相关同步命令。

```bash
$ git checkout localTeam # 就算显示 Your branch is up to date with 'origin/team/hodgepodge'，还是需要执行 git pull 的
$ git branch
$ git pull
```

- 也可以这样

```bash
$ git pull origin team/hodgepodge:localTeam
$ git checkout localTeam
```

- :gem: 此时 Github 上有两个 branch。
- :gem: 操作前 localTeam 只有 docOne 和 docTwo，git pull 加入 docThree 和 docFour，`一共 4 个 doc 文件`。

- :gem: 就算显示 `Your branch is up to date with 'origin/team/hodgepodge'`，还是一定需要执行 git pull.

5. 在本地另外一个位置下载代码，转到 main branch，然后在 main 的基础上生成一个新的 branch:mainNewFeature，`新增文件 docFive`，上传至 github，执行 pull request，将新 branch 合并到 origin 中。`（模拟 main 已经被其他小组修改了）`。

- 文件 docFive 文本是

```diff
+ This is document 5 created from mainNewFeature based on main branch.
```

- 具体命令

```bash
$ git clone git@github.com:DonghaoWu/git-practice.git
$ cd git-practice
$ git checkout main

$ git checkout -b mainNewFeature

$ git add .
$ git commit -m'created branch mainNewFeature with docFive.'
$ git push origin mainNewFeature
```

6.  提出 pull request，把 team/hodgepodge 合并到 orgin 之中。`（模拟小组 branch 准备完好，合并到 main branch 中。）`

- 此时返回步骤 3 的位置，执行以下命令，应该看到在本地 main 中有 5 个 doc 文件

- 具体命令

```bash
$ git checkout main # 无视 Your branch is up to date with 'origin/main'，继续执行以下命令.
$ git pull origin main
```

- :gem: 执行 git pull 之前各个 branch 有以下文件

```diff
+ main: 0
+ localTeam: docOne, docTwo, docThree, docFour
+ newFeature: docOne, docTwo, docThree,
```

- 各个文件的从属关系：

```diff
+ docOne: team/hodgepodge(local)，本地上传
+ docTwo: team/hodgepodge(github)，同事修改小组 branch
+ docThree: newFeature(local)，本地上传
+ docFour: team/hodgepodge(local)，同事修改小组 branch，跟第二项差不多
+ docFive: mainNewFeature branch(local)，同事修改 main
```
