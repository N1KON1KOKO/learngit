[教程](https://liaoxuefeng.com/books/git/introduction/index.html)
# 创建版本库

版本库可以视为一个目录，里面所有文件都被git管理

> 首先cd一个位置
> 
> 然后==mkdir learngit==创建一个目录（**pwd可以显示当前目录**）
> 
> 再使用==git init==把这个目录变成git可以管理的仓库

>[!danger] 然后此目录下会多一个.git目录，这个目录是git来跟踪管理版本库的，不要乱改

所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，版本控制系统可以告诉你每次的改动。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。如果要真正使用版本控制系统，就要以纯文本方式编写文件。

如果想要添加文本文件到git仓库，可以按如下步骤：

> 首先在git仓库目录下或子目录下创建一个xxx.txt
> 
> 然后==git add xxx.txt==将文本文件添加到暂存区
> 
> 最后==git commit -m “commit instructions”==将对暂存区的更改提交到本地仓库

> [!danger] 文本文件一定要在git仓库目录下创建，否则找不到

> [!attention] git commit一次提交所有更改的文件到仓库，-m “commit instructions”是对修改的说明，双引号内是说明的内容

# 时光机穿梭

==git status==命令可以让我们时刻掌握仓库当前的状态

如果我们记不清文件被如何修改，可以通过==git diff xxx.txt==命令来查看修改内容（**注意这里是工作区与暂存区之间差异**），知道了修改内容就可以放心提交，再通过==git add xxx.txt==就可以添加修改到暂存区（此时可以==git status==查看xxx.txt是否将要被提交），再使用==git commit -m “commit instructions”==将修改提交到仓库（此时可以通过==git status==查看是否成功提交）

## 版本回退

每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为==commit==。一旦你把文件改乱了，或者误删了文件，还可以从==最近的一个commit==恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

在实际工作中，我们脑子里怎么可能记得一个几千行的文件每次都改了什么内容，不然要版本控制系统干什么。版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用==git log==命令查看。如果嫌输出信息太多，看得眼花缭乱的，可以试试在==git log==后面加上==--pretty=oneline==参数

我们看到的一大串类似==1094adb...==的是==commit id（版本号）==，可以避免大家都用序号1、2、3...导致冲突

git中用==HEAD==表示当前版本，==HEAD^==表示上一个版本，==HEAD^^==表示上上一个版本，……，==HEAD~100==表示往上100个版本

如果我们要回退版本，可以使用==git reset 状态参数 某个版本==命令回退到某个版本的某个状态，状态参数有：
> [!quote] ==--hard==会回退到上个版本的已提交状态，而==--soft==会回退到上个版本的未提交状态，==--mixed==会回退到上个版本已添加但未提交的状态。

如若我们回退之后想在回来，需要找到新版本号根据版本号再回去，具体命令为==git reset 状态参数 版本号(**可简写前几位**)==，如果找不到或者记不得版本号，可以使用==git reflog==命令查看过去所有命令，找到对应版本号
## 工作区和暂存区

名词解释：
工作区：就是能在电脑里看到的目录，例如learngit
版本库：工作区learngit里的隐藏目录.git

Git的版本库里存了很多东西，其中最重要的就是称为==stage（或者叫index）==的暂存区，还有Git为我们自动创建的==第一个分支master==，以及==指向master的一个指针叫HEAD==。

因为我们创建Git版本库时，Git自动为我们创建了==唯一一个master分支==，所以，==git commit==就是==往master分支上提交更改==。


可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。
## 管理修改

在git add后在工作区进行修改，此时再git commit只会提交被git add的版本，而不会提交git add之后再做修改的版本，此时可以用==git diff head==查看工作区和版本库里提交的最新版本的区别，==git diff head -- xxx.txt==是查看工作区和版本库最新版本的特定文件区别，==git diff==是查看工作区与暂存区的区别

## 撤销修改

### 工作区撤销

==git checkout -- xxx.txt==可以丢弃工作区修改，恢复到最近一次提交状态

### 暂存区撤销

==git reset head xxx.txt==可以将暂存区撤销而不影响工作区，工作区中的修改仍然保留

## 删除文件

当工作区文件被删除时，工作区和版本库就不一致，git status可以看到。现在有两个选择，一是彻底删除（但版本库还能找回上次提交状态）：==git rm xxx.txt==再==git commit -m “commit instructions”==彻底删除；二是错删想要恢复：==git checkout -- test.txt==从版本库恢复。

==git rm==会删除==工作区和暂存区==中的文件，若已经修改并存到暂存区则需要==git rm -f强制删除==，git rm且git commit后想要恢复只能根据上一次提交的版本号从版本库中恢复

# 远程仓库

## github与密钥

github提供git仓库托管服务，本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

1. ：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：````
```
$ ssh-keygen -t ed25519 -C "your_email@example.com"
```
你需要把邮件地址换成你自己的邮件地址，然后保存路径默认，设置密码确认密码都是盲打，生成成功后生成随机图案，然后可以通过
```
cat ~/.ssh/id_ed25519.pub
```
查看公钥内容，然后完整复制公钥内容

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

2. 登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容，点“Add Key”，你就应该看到已经添加的Key

3. 添加完成后，回到你的终端，输入以下命令测试是否配置成功：
```
ssh -T git@github.com
```
若不行则尝试使用 HTTPS 端口，首先打开git bash，然后输入以下命令来编辑 SSH 配置文件：
```
nano ~/.ssh/config
```
将以下内容复制并粘贴到文件中：
```
Host github.com
    Hostname ssh.github.com
    Port 443
    User git
    IdentityFile ~/.ssh/id_ed25519  # 或者是你创建的私钥文件，如 id_rsa
```
保存并退出：
- **nano 编辑器**：按 `Ctrl + X`，然后按 `Y` 确认保存，最后按 `Enter` 确认文件名。
- **vim 编辑器**：按 `Esc`，然后输入 `:wq`，再按 `Enter`。
再次测试连接：
```
ssh -T git@github.com
```
现在你应该能看到成功的提示了：
```
Hi your_username! You've successfully authenticated, but GitHub does not provide shell access.
```
这表示你的 SSH 密钥已经成功设置，现在你可以使用 SSH 协议安全地推送和拉取代码了！

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

如果你不想让别人看到Git库，有两个办法，一个是交点保护费，让GitHub把公开的仓库变成私有的，这样别人就看不见了（不可读更不可写）。另一个办法是自己动手，搭一个Git服务器，因为是你自己的Git服务器，所以别人也是看不见的。

## 添加远程库

如果你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作

1. 登录你的账户。
2. 点击 **New** 或 **Create repository** 按钮。
3. 填写仓库名称（如 `my-project`），选择公开（Public）或私有（Private）。
4. **非常重要：不要勾选 “Initialize this repository with a README”**。我们希望从一个空的仓库开始，以便推送现有的本地代码。
5. 点击创建。
6. 创建成功后，平台会提供一个仓库的 URL。它有两种协议：
 ```
 HTTPS URL: https://github.com/你的用户名/仓库名.git
 SSH URL: git@github.com:你的用户名/仓库名.git
  ```
7. 打开Git Bash，导航到你的本地项目根目录。
```
git remote add origin <远程仓库的URL>
```
- `origin`：这是你为远程仓库起的**默认别名**（shortname）。你可以用其他名字（如 `upstream`），但 `origin` 是通用惯例。
- `<远程仓库的URL>`：替换为你从上一步复制的 URL。
8. 验证远程库是否添加成功：
```
git remote -v
```
如果添加成功，你会看到类似下面的输出。它显示了你对远程库的**推送（push）** 和**拉取（fetch）** 地址。
```
origin  git@github.com:your-username/my-project.git (fetch)
origin  git@github.com:your-username/my-project.git (push)
```
9. 推送本地代码到远程库
现在关联已经建立，你需要将本地的代码和历史（commit）推送到远程。

如果你是**第一次推送**，并且你的本地仓库已经有了一些提交（commits），你需要使用 `-u` 选项：
```
git push -u origin main
```

- `-u` (或 `--set-upstream`)：这个选项会将本地的 `main` 分支与远程的 `origin/main` 分支关联起来。**之后你只需要简单地使用 ==git push== 和 ==git pull== 即可，无需再指定分支名。**

- `origin`：我们刚才设置的远程库别名。

- `main`：你要推送的**本地分支名**。如果你的旧项目主分支叫 `master`，请将这里的 `main` 替换为 `master`。
## 删除远程库

如果添加的时候地址写错了，或者就是想删除远程库，可以用`git remote rm <name>`命令。使用前，建议先用`git remote -v`查看远程库信息：
```
$ git remote -v
origin  git@github.com:your-username/my-project.git (fetch)
origin  git@github.com:your-username/my-project.git (push)
```
然后，根据名字删除，比如删除`origin`：
```
$ git remote rm origin
```
此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。

## 从远程库克隆

第 1 步：获取远程仓库的 URL

1. 打开 GitHub（或 GitLab/Gitee）上的项目页面。

2. 点击绿色的 **「Code」** 按钮。

3. 选择你想要的协议（**HTTPS** 或 **SSH**），然后点击复制图标。


- **SSH URL** (推荐): `git@github.com:用户名/仓库名.git`

- **HTTPS URL**: `https://github.com/用户名/仓库名.git`


第 2 步：执行克隆命令

打开Git Bash，**不需要**先 `cd` 到一个目录，因为 `git clone` 会自己创建文件夹。

**导航到你希望存放项目的父目录**，然后执行克隆命令：
```
# 示例：使用 SSH 方式克隆
git clone git@github.com:username/repository-name.git

# 示例：使用 HTTPS 方式克隆
git clone https://github.com/username/repository-name.git
```
第 3 步：进入项目目录

克隆完成后，会自动创建一个与仓库同名的文件夹。你需要进入这个文件夹才能开始工作。
```
# 进入克隆下来的项目文件夹
cd repository-name

# 现在你可以查看文件列表了
ls -la
# 或 Windows 上用
dir
```

执行完 `git clone` 后，你的本地环境已经一切就绪：

1. **所有文件**：远程仓库的所有文件和历史版本都下载到了本地。

2. **完整的 Git 历史**：你可以使用 `git log` 查看所有的提交记录。

3. **远程库已自动添加**：名为 `origin` 的远程地址已经指向了你克隆的来源。可以用 `git remote -v` 查看。

4. **分支已关联**：本地默认分支（通常是 `main` 或 `master`）已经自动跟踪了远程的 `origin/main` 或 `origin/master` 分支。
# 分支管理

分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

## 创建与合并分支

在版本回退里，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即`master`分支。`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点

每次提交，`master`分支都会向前移动一步，这样，随着你不断提交，`master`分支的线也越来越长。

当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上

你看，Git创建一个分支很快，因为除了增加一个`dev`指针，改改`HEAD`的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变

假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并

所以Git合并分支也很快！就改改指针，工作区内容也不变！

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支

### 创建与合并分支实战

首先，我们创建`dev`分支，然后切换到`dev`分支：
```
$ git checkout -b dev
Switched to a new branch 'dev'
```
`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：
```
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```
然后，用`git branch`命令查看当前分支：
```
$ git branch
* dev
  master
```
`git branch`命令会列出所有分支，当前分支前面会标一个`*`号。

然后，我们就可以在`dev`分支上正常提交，比如对`readme.txt`做个修改，加上一行：
```
Creating a new branch is quick.
```
然后提交：
```
$ git add readme.txt 
$ git commit -m "branch test"
[dev b17d20e] branch test
 1 file changed, 1 insertion(+)
```
现在，`dev`分支的工作完成，我们就可以切换回`master`分支：
```
$ git checkout master
Switched to branch 'master'
```
切换回`master`分支后，再查看一个`readme.txt`文件，刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变

现在，我们把`dev`分支的工作成果合并到`master`分支上：
```
$ git merge dev
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```
`git merge`命令用于合并指定分支到当前分支。合并后，再查看`readme.txt`的内容，就可以看到，和`dev`分支的最新提交是完全一样的。

注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。

当然，也不是每次合并都能`Fast-forward`

合并完成后，就可以放心地删除`dev`分支了：
```
$ git branch -d dev
Deleted branch dev (was b17d20e).
```
删除后，查看`branch`，就只剩下`master`分支了：
```
$ git branch
* master
```

### 切换分支

我们注意到切换分支使用`git checkout <branch>`，而前面讲过的撤销修改则是`git checkout -- <file>`，同一个命令，有两种作用，确实有点令人迷惑。

实际上，切换分支这个动作，用`switch`更科学。因此，最新版本的Git提供了新的`git switch`命令来切换分支：

创建并切换到新的`dev`分支，可以使用：
```
$ git switch -c dev
```
直接切换到已有的`master`分支，可以使用：
```
$ git switch master
```

## 解决冲突

当两个分支各自都有新的提交时，无法快速合并，只能试图把各自的修改合并起来，但这种合并就可能会有冲突。此时我们必须把Git合并失败的文件手动编辑为我们希望的内容，再git add，再git commit，此时你在哪个分支提交的，就在这个分支上新产生一个提交，此分支指向这个提交，这个提交成为其他冲突分支的后继，即其他分支指向提交仍然是原来提交，如图所示：
```
                            HEAD
                              │
                              ▼
                           master
                              │
                              ▼
                            ┌───┐
                         ┌─▶│   │
┌───┐    ┌───┐    ┌───┐  │  └───┘
│   │───▶│   │───▶│   │──┤
└───┘    └───┘    └───┘  │  ┌───┐
                         └─▶│   │
                            └───┘
                              ▲
                              │
                          feature1
```
```
                                     HEAD
                                       │
                                       ▼
                                    master
                                       │
                                       ▼
                            ┌───┐    ┌───┐
                         ┌─▶│   │───▶│   │
┌───┐    ┌───┐    ┌───┐  │  └───┘    └───┘
│   │───▶│   │───▶│   │──┤             ▲
└───┘    └───┘    └───┘  │  ┌───┐      │
                         └─▶│   │──────┘
                            └───┘
                              ▲
                              │
                          feature1
```
最后可以删除featurel分支

用==git log --graph==命令可以看到分支合并图

## 分支管理策略

通常，合并分支时，如果可能，Git会用==Fast forward==模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

下面我们实战一下==--no-ff==方式的`git merge`：
1. 创建一个新分支dev
2. 修改文件并提交一个新的commit
3. 切换回master
4. 使用==git merge --no-ff -m "merge with no-ff" dev==合并dev分支
> [!attention] `--no-ff`参数，表示禁用`Fast forward`
> 因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去
5. 用git log查看分支历史，可以看到不使用`Fast forward`模式，merge后就像这样：
```
                                 HEAD
                                  │
                                  ▼
                                master
                                  │
                                  ▼
                                ┌───┐
                         ┌─────▶│   │
┌───┐    ┌───┐    ┌───┐  │      └───┘
│   │───▶│   │───▶│   │──┤        ▲
└───┘    └───┘    └───┘  │  ┌───┐ │
                         └─▶│   │─┘
                            └───┘
                              ▲
                              │
                             dev
```

实际开发中我们应按照几个基本原则进行分支管理：

1. `master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

2. 干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

所以，团队合作的分支看起来就像这样：
![git-br-policy](https://liaoxuefeng.com/books/git/branch/policy/branches.png)

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

## Bug分支

当你正在一个分支上工作，此时接到一个修复bug的任务，自然想要创建一个分支来修复它，但你在当前分支上的工作还没有提交，并不是不想提交，而是工作还没完成，此时修复bug任务又非常紧急。此时可以使用git提供的==stash功能==，可以把当前工作现场”储藏“起来，等以后恢复现场后继续工作。==git stash==后用git status查看工作区就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug

修复完成后回到原分支，再使用git status可以看到工作区是干净的，因为git把stash内容存在某个地方了，需要恢复一下，有两个办法：
1. 用==git stash apply==恢复，但是恢复后，`stash`内容并不删除，你需要用==git stash drop==来删除
2. 用==git stash pop==，恢复的同时把`stash`内容也删了

你可以多次stash，stash内容可以用==git stash list==查看，恢复的时候可以先查看再使用命令==git stash apply 某个stash==或==git stash pop 某个stash==恢复指定的stash，比如stash@{0}

在`master`分支上修复了bug后，我们要想一想，`dev`分支是早期从`master`分支分出来的，所以，这个bug其实在当前`dev`分支上也存在。

那怎么在`dev`分支上修复同样的bug？

同样的bug，要在`dev`上修复，我们只需要把修复bug的这个提交所做的修改“复制”到`dev`分支。注意：我们只想复制修复bug的这个提交所做的修改，并不是把整个`master`分支merge过来。

为了方便操作，Git专门提供了一个==cherry-pick==命令，让我们能复制一个特定的提交到当前分支：
```
$ git branch
* dev
  master
$ git cherry-pick 4c805e2
[master 1d4b803] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```
Git自动给`dev`分支做了一次提交，注意这次提交的commit是`1d4b803`，它并不同于`master`的`4c805e2`，因为这两个commit只是改动相同，但确实是两个不同的commit。用==git cherry-pick==，我们就不需要在`dev`分支上手动再把修bug的过程重复一遍。

### 思考

master修复bug后直接merge过来和cherry-pick修复bug后所做的提交有什么区别：

方案一：使用merge：

创建**合并提交**，历史变得复杂、耦合，**集成**两个分支的发展线，可能引入不相关的更改或冲突

方案二：使用cherry-pick：

**移植**一个特定的修改，创建**新提交**（内容与修复bug的提交完全一致），历史保持线性、清晰，两个分支不会汇合，仍然各自发展

## Feature分支

如果master分支要开发一个新功能，我们要在master分支上创建一个feature分支用于开发功能。开发完毕并且提交成功，准备合并，此时这个功能被上级取消，我们需要销毁这个分支，仅仅使用==git branch -d feature==命令销毁分支会显示：
> [!danger] 销毁失败。Git友情提醒，`feature`分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的`-D`参数。

我们需要使用==git branch -D feature==命令强行删除

## 多人协作

当你从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。

要查看远程库的信息，用==git remote==：
```
$ git remote
origin
```
或者，用==git remote -v==显示更详细的信息：
```
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
```
上面显示了可以抓取和推送的`origin`的地址。如果没有推送权限，就看不到push的地址

### 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：
```
$ git push origin master
```
如果要推送其他分支，比如`dev`，就改成：
```
$ git push origin dev
```

### 抓取分支

多人协作时，大家都会往`master`和`dev`分支上推送各自的修改。

当从远程库clone时，默认情况下，只能看到本地的`master`分支。其实其他分支都下载到你的本地仓库，只是没有自动为你创建对应的本地分支来“跟踪”它们。这是 Git 的设计，旨在保持简洁。它假设你最初只关心项目的主要分支（默认分支），而不是所有的分支。这样可以避免你的本地仓库一下子被几十个分支弄得混乱不堪。

1. 查看所有远程分支：==git branch -r==

2. 本地创建一个分支，并让它“跟踪”远程的 `origin/dev` 分支（其实就是建立本地dev分支和origin/dev分支的链接）：==git switch -c dev origin/dev==

3. 完成后，验证一下：==git branch -vv== 的输出会显示你的本地 `dev` 分支正在跟踪 `origin/dev`：
```
* dev     a1b2c3d [origin/dev] Some commit message
  main    e4f5g6h [origin/main] Initial commit
```

要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地即创建分支跟踪远程origin的dev分支，这样就可以在`dev`上继续修改，然后，时不时地把`dev`分支`push`到远程：==git push origin dev==

如果你要push时发现push失败，说明远程分支比你的本地更新，先用==git pull==把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送。如果==git pull==失败了，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接：==git branch --set-upstream-to=origin/dev dev==，再==git pull==，这回就pull成功了，如果合并有冲突，需要手动解决，参考**分支管理的解决冲突**，解决后，本地提交，再push

### 远程库多分支

|方面|单分支模式（仅 `main`）|多分支模式（`main` + `dev` + ...）|
|---|---|---|
|**代码稳定性**|**差**。所有提交，包括半成品和实验代码，都直接混入主分支。|**优**。`main` 分支始终稳定，可随时发布。|
|**协作难度**|**高**。多人直接向同一分支推送，极易互相干扰和产生冲突。|**低**。各自在特性分支开发，通过合并请求（PR/MR）有序集成。|
|**发布与开发**|**耦合**。想发布时可能代码不稳定，想开发时又怕影响发布。|**解耦**。`main` 用于发布，`dev` 用于集成，互不干扰。|
|**问题排查**|**困难**。所有历史交织在一起，很难定位引入问题的提交。|**容易**。特性分支和合并请求将更改隔离，便于审查和回滚。|
|**风险性**|**高**。一次错误的推送就可能直接破坏主分支。|**低**。`main` 分支受保护，更改必须通过审查才能合并。|
#### 为什么推荐多分支？（Git Flow / GitHub Flow 核心思想）

多分支模式的核心是**职责分离**和**降低风险**。最常见的模式是：

1. **`main` / `master` 分支（主分支）**
    
    - **职责**：存放**完全稳定**、**可随时发布**的代码。
        
    - **保护**：通常设置为“受保护分支”，禁止开发者直接 `push`，只能通过**合并请求（Pull Request / Merge Request）** 将其他分支的代码合并进来。这强制进行了代码审查（Code Review）。
        
2. **`dev` / `develop` 分支（开发分支）**
    
    - **职责**：存放**最新开发成果**的集成版本。功能相对完整，但可能包含未完全测试的代码。
        
    - **来源**：由各个功能分支（`feature/*`）合并而来。
        
3. **`feature/*` 分支（功能分支）**
    
    - **职责**：开发者为了某个特定功能或任务**从 `dev` 分支拉取**的临时分支。
        
    - **流程**：开发者在此分支上独立工作，完成后向 `dev` 分支发起合并请求。

### 多人协作一般流程

一、协同开发的准备工作 🍉
1、新建一个仓库（当然

2、创建主分支，即上传项目的初始内容到master分支

3、团队内成员进行分工（各个成员之间负责的内容用尽量不冲突）

二、开发阶段 🍇
1、每个成员将远程仓库主（master）分支的内容clone下来

2、按照分工进行自己负责的工作

三、提交阶段 🍍
这个阶段是问题发生最严重的的阶段，下面的步骤都是按照有管理员的情况下阐述的。管理员的作用。

1️⃣每个成员在完成自己的工作后，首先要注意远程仓库的变化
(1) 为什么要注意远程仓库的变化？
一开始团队内每个成员都是基于最开始的master分支进行工作的，随着时间的流逝，会有成员上传自己的内容，如果成员甲push了自己的内容到master分支后，那么master分支中的内容就会被甲所提交的内容所覆盖，这时如果有个成员乙不注意远程仓库的变化，也直接将自己的内容push到远程的master分支上，那么远程仓库的主分支就会被成员乙策内容所替换，也就是成员甲所push的内容会被成员乙提交的内容覆盖掉。

(2) 该如何注意远程仓库的变化？
方式一：拉取远程分支（最常用）

即使用pull指令,该指令可以理解为两个步骤：

获取远程分支
将获取的远程分支与本地分支合并
拉取后，由于自动合并，就会将远端的内容合并到本地分支，此时再push上去，本地内容将远端分支覆盖后就保留了成员甲的内容。但是合并过程中可能会出现一些错误。

方式二：获取远程分支（最安全）

即使用fetch指令，该指令就是获取远端指定分支的最新版到本地（即在本地创建一个新分支内容为远端指定分支的最新版）。获取分支后就可以比较、查看远程分支的内容，随后若想push，可选择与获取的分支进行merge（合并）再push。

2️⃣获取远程仓库的最新版，与本地进行合并
合并时会产生的问题：合并冲突
会产生的情况：

两个人对同一项目的不同文件进行了修改☑️
两个人对同一项目的同一文件的不同区域进行了修改☑️
对同一项目的同一文件的同一区域进行了不同修改❌

上述三种情况的前两种都可以由Git进行自动合并，而第三种情况无法进行自动合并，需要**手动合并**。

合并过后，就可以上传（push）到远程仓库自己的分支了

## Rebase(不建议使用)

构造两个分支master和feature，其中feature是在提交点B处从master上拉出的分支

master上有一个新提交M，feature上有两个新提交C和D
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/11b5dfb6fcb1b5e2e3d68d726b7a07bb.png)
此时我们切换到feature分支上，执行rebase命令，相当于是想要把master分支合并到feature分支（这一步的场景就可以类比为我们在自己的分支feature上开发了一段时间了，准备从主干master上拉一下最新改动。

下图为变基后的提交节点图，解释一下其工作原理：
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/0e5ced3de53e575c3af477e2dd8a0ce6.png)
- feature：待变基分支、当前分支
- master：基分支、目标分支

结合例子解释：当在feature分支上执行git rebase master时，git会从master和featuer的共同祖先B开始提取feature分支上的修改，也就是C和D两个提交，先提取到。然后将feature分支指向master分支的最新提交上，也就是M。最后把提取的C和D接到M后面，注意这里的接法，官方没说清楚，实际是会依次拿M和C、D内容分别比较，处理冲突后生成新的C’和D’。一定注意，这里新C’、D’和之前的C、D已经不一样了，是我们处理冲突后的新内容，feature指针自然最后也是指向D’（注意是分别处理==C和M==以及==D和M==的冲突生成C’、D’。手动处理冲突后，执行如下命令即可：
```
# 先处理完C，会继续报D的冲突，所以下面命令一共会执行两次
git add file
git rebase --continue
```
）

通俗解释（重要！！）：rebase，变基，可以直接理解为改变基底。feature分支是基于master分支的B拉出来的分支，feature的基底是B。而master在B之后有新的提交，就相当于此时要用master上新的提交来作为feature分支的新基底。实际操作为把B之后feature的提交先暂存下来，然后删掉原来这些提交，再找到master的最新提交位置，把存下来的提交再接上去（接上去是逐个和新基底处理冲突的过程），如此feature分支的基底就相当于变成了M而不是原来的B了。（注意，如果master上在B以后没有新提交，那么就还是用原来的B作为基，rebase操作相当于无效，此时和git merge就基本没区别了，差异只在于git merge会多一条记录Merge操作的提交记录）

上面的例子可抽象为如下实际工作场景：远程库上有一个master分支目前开发到B了，张三从B拉了代码到本地的feature分支进行开发，目前提交了两次，开发到D了；李四也从B拉到本地的master分支，他提交到了M，然后合到远程库的master上了。此时张三想从远程库master拉下最新代码，于是他在feature分支上执行了git pull origin master:feature --rebase（注意要加–rebase参数），即把远程库master分支给rebase下来，由于李四更早开发完，此时远程master上是李四的最新内容，rebase后再看张三的历史提交记录，就相当于是张三是基于李四的最新提交M进行的开发了。（但实际上张三更早拉代码下来，李四拉的晚但提交早）

**大部分公司其实会禁用rebase，不管是拉代码还是push代码统一都使用merge，虽然会多出无意义的一条提交记录“Merge … to …”，但至少能清楚地知道主线上谁合了的代码以及他们合代码的时间先后顺序**

# 标签管理

发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

Git有commit，为什么还要引入tag？

“请把上周一的那个版本打包发布，commit号是6a5819e...”

“一串乱七八糟的数字不好找！”

如果换一个办法：

“请把上周一的那个版本打包发布，版本号是v1.2”

“好的，按照tag v1.2查找commit就行！”

所以，tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

## 创建标签

在Git中打标签非常简单，首先，切换到需要打标签的分支上

然后，敲命令==`git tag <name>`==就可以打一个新标签

可以用命令==`git tag`==查看所有标签

默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了：
```
$ git log --pretty=oneline --abbrev-commit
12a631b (HEAD -> master, tag: v1.0, origin/master) merged bug fix 101
4c805e2 fix bug 101
e1e9c68 merge with no-ff
f52c633 add merge
cf810e4 conflict fixed
5dc6824 & simple
14096d0 AND simple
b17d20e branch test
d46f35e remove test.txt
b84166e add test.txt
519219b git tracks changes
e43a48b understand how stage works
1094adb append GPL
e475afc add distributed
eaadf4e wrote a readme file
```
比方说要对`add merge`这次提交打标签，它对应的commit id是`f52c633`，敲入命令：
```
$ git tag v0.9 f52c633
```
再用命令`git tag`查看标签

注意，标签不是按时间顺序列出，而是按字母排序的。可以用==`git show <tagname>`==查看标签信息

还可以创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：
```
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
```

## 操作标签

如果标签打错了，也可以删除
```
git tag -d <tagname>
```
因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令`git push origin <tagname>`

或者，一次性推送全部尚未推送到远程的本地标签：`git push origin --tags`

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：`git tag -d <tagname>`

然后，从远程删除。删除命令也是push，但是格式如下：`git push origin :refs/tags/<tagname>`
