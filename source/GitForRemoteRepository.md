# 与远程仓库相关的git命令

[toc]

## git clone

远程存储库实际上并不那么复杂。在当今的云计算世界中，人们很容易想到`git remotes`背后有很多魔力，但实际上它们只是您存储库在另一台计算机上的副本。通常，您可以通过Internet与这台其他计算机通信，从而可以来回传输提交。

话虽这么说，远程存储库具有许多出色的属性：

- 首先，远程服务器是一个很好的备份！本地`git`存储库可以将文件还原到以前的状态（如您所知），但是所有这些信息都存储在本地。通过在其他计算机上拥有`git`存储库的副本，您可能会丢失所有本地数据，并且仍然可以从上次中断的地方继续。

- 更重要的是，远程使编码变得社交化！现在，您的项目的副本已托管在其他位置，您的朋友可以非常轻松地为您的项目做出贡献（或获取您的最新更改）。

使用可视化远程仓库（例如`Github`或`Phabricator`）周围活动的网站变得非常流行，但是远程仓库始终充当这些工具的基础。因此，了解它们很重要！



从技术上讲，现实世界中的`git clone`是用于创建远程存储库的本地副本（例如，来自`github`）的命令。但是，我们在学习Git分支中使用此命令的方式有所不同-`git clone`实际上使远程存储库脱离了本地存储库。当然，从技术上讲，它与真实命令的含义相反，但是它有助于在克隆和远程存储库工作之间建立联系，所以现在就让它运行。


![image-20210213201004509](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213201004509.png)

## git 远程仓库分支解释

既然您已经看到了`git clone`的实际应用，那么让我们深入研究一下实际发生的变化。

您可能注意到的第一件事是在我们的本地存储库中出现了一个名为`o / master`的新分支。这种分支称为远程分支。远程分支具有特殊的属性，因为它们具有独特的用途。

远程分支反映了远程存储库的状态（自您上次与这些远程存储库进行交谈以来）。它们可帮助您了解本地工作与公开工作之间的区别-在与他人共享您的工作之前必须采取的关键步骤。

远程分支具有特殊的属性，当您签出它们时，会将它们置于分离的`HEAD`模式。 Git故意这样做是因为您不能直接在这些分支上工作。您必须在其他地方工作，然后与远程共享您的工作（之后将更新远程分支）。

### 什么是o/master?

远程分支也有一个（必需的）命名约定-它们以以下格式显示：

`<远程名称> / <分支名称>`
因此，如果您查看名为`o / master的`分支，则该分支的名称为`master`，而远程的名称为`o`。

实际上，大多数开发人员都将其主要远程来源命名为`“ o”`。这很常见，以至于git克隆存储库时，git实际上将您的远程仓库设置为起`origin`。

不幸的是，`origin`的全名不适合我们的UI，因此我们将`o`用作速记:(请记住，当您使用真正的git时，您的遥控器可能会被命名为`origin`！

![image-20210213201837657](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213201837657.png)

## git fetch

实际上，使用git远程仓库可以归结为在其他仓库之间来回传输数据。只要我们可以来回发送提交，我们就可以共享git跟踪的任何类型的更新（从而共享工作，新文件，新想法，情书等）。

在本课程中，我们将学习如何从远程仓库中获取数据-为此的命令方便地命名为`git fetch`。

您会注意到，当我们更新远程仓库的表示形式时，我们的远程分支将进行更新以反映该新表示形式。

![image-20210213202626093](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213202626093.png)

### git fetch会做什么？

`git fetch`执行两个主要步骤，并且仅执行两个主要步骤。它：

- 下载远程仓库拥有但本地仓库没有的提交，并...
- 更新我们的远程分支指向的位置（例如`o / master`）
  `git fetch`实质上使我们对远程存储库的本地表示与实际的远程存储库外观保持同步。

如果您还记得上一课，我们说过远程分支反映了自您上次与远程存储库对话以来的远程存储库状态。 `git fetch`是您与这些远程仓库通讯的方式！远程分支和`git fetch`之间的连接很明显。

`git fetch`通常通过Internet（通过类似http：//或git：//的协议）与远程存储库进行对话。

`git fetch`，但是，不会更改您的本地状态。它不会立即更新您的`master`分支或更改有关文件系统外观的任何内容。

了解这一点很重要，因为许多开发人员认为运行`git fetch`将使他们的本地工作反映远程状态。它可能会下载所有必要的数据来执行此操作，但实际上并不会更改您的任何本地文件。我们将在以后的课程中学习命令以实现：D

因此，总而言之，您可以将运行`git fetch`视为下载步骤。

## git pull

现在，我们已经了解了如何使用`git fetch`从远程存储库中获取数据，让我们更新工作以反映这些更改！

实际上，有很多方法可以做到这一点-一旦在本地有新的提交，就可以将它们合并，就好像它们只是其他分支上的普通提交一样。这意味着您可以执行以下命令：

- git cherry-pick o / master
- git rebase o / master
- git merge o / master
- 等等等

实际上，获取远程更改然后合并它们的工作流非常普遍，以至于git实际上提供了同时执行两项操作的命令！该命令是`git pull`。

![image-20210213204352254](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213204352254.png)

![image-20210213204432133](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213204432133.png)

稍后我们将探讨`git pull`的详细信息（包括选项和参数），但现在让我们在级别中进行尝试。

请记住-您实际上可以通过`git fetch`和`git merge`一起来解决此类问题，但这将花费您额外的命令：P、

## git fakeTeamwork

`git fakeTeamwork`是用于模拟团队协作；

前面已经提到过`git fetch`和`git pull`是用于更新本地和远程仓库的同步。也就是说，首先我们得假定，远程仓库某个分支已经被我们的一个同事/朋友/协作者更新了提交。

![image-20210213205507055](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213205507055.png)

![image-20210213205621015](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213205621015.png)

制作一个远程服务器（使用git clone），在该远程服务器上伪造一些更改，提交，然后`pull down`这些更改。

![image-20210213210000375](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213210000375.png)

## git push

好的，所以我已经从远程获取了更改并将其合并到本地工作中。那太好了...但是我如何与其他人分享我的出色作品呢？

好吧，上传共享作品的方式与下载共享作品的相反。 `git pull`的反义词是什么？ `git push`！

`git push`负责将您的更改上传到指定的远程仓库，并更新该远程仓库以合并您的新提交。 `git push`完成后，所有朋友都可以从远程下载您的工作。

您可以将`git push`视为“发布”您的工作的命令。我们很快就会涉及到很多细节，但让我们从婴儿步开始吧...

注意-不带参数的`git push`的行为根据`git`的其中一种设置叫做`push default`的不同而有所不同。此设置的默认值取决于您使用的`git`的版本，但是我们将在课程中使用`upstream`。这不是什么大不了的事，但是值得在启动自己的项目之前检查设置。

![image-20210213211037144](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213211037144.png)

![image-20210213211019920](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213211019920.png)

## 处理工作分歧

想象一下，您在星期一克隆了一个存储库，并开始涉猎附带功能。到星期五，您已经准备好发布您的功能-但是，不！您的同事在一周中写了很多代码，使您的功能过时（且已过时）。他们还将这些提交发布到共享的远程存储库中，因此现在您的工作基于与项目不再相关的旧版本。

在这种情况下，命令`git push`不明确。如果您运行`git push`，则git是否应将远程存储库更改回星期一的状态？它应该尝试在不删除新代码的情况下添加代码吗？还是应该完全忽略您的更改，因为它们已经完全过时了？

因为在这种情况下（历史有所分歧）存在很大的歧义，所以git不允许您进行`push`更改。实际上，它迫使您在可以共享您的工作之前合并了远程仓库的最新状态。

![image-20210213211810351](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213211810351.png)

您如何解决这种情况？这很容易，您需要做的只是基于远程分支的最新版本进行工作。

有几种方法可以做到这一点，但是最直接的方法是通过`rebasing`来移动工作。让我们继续前进，看看是什么样子。

![image-20210213211937398](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213211937398.png)

![image-20210213212007918](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213212007918.png)

远程存储库更新后，还有其他方法可以更新我的工作吗？当然！让我们用`merge`来做相似的事情。

尽管`git merge`不会移动您的工作（而只是创建一个合并提交），但这是一种告诉git您已经合并了来自远程的所有更改的方法。这是因为远程分支现在是您自己分支的祖先，这意味着您的提交反映了远程分支中的所有提交。

让我们看看这个示范...

![image-20210213212322697](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213212322697.png)

![image-20210213212405054](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213212405054.png)

惊人的！有什么方法可以不用输入太多命令就能做到这一点？

当然-您已经知`git pull`是获取`fetch`和合并`merge`的简写。

足够方便的是，`git pull --rebase`是获取`fetch`和`rebase`的简写！

让我们看看这些速记命令在起作用。

![image-20210213212639380](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213212639380.png)

![image-20210213212705753](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213212705753.png)

我们来看看常规的不带参数的`pull`的功能：

![image-20210213212851399](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213212851399.png)命令行为`git pull; git push`

![image-20210213212828289](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213212828289.png)

## 合并要素分支

现在您已经可以轻松获取，拉动和推动了，让我们通过新的工作流程对这些技能进行测试。

对于大型项目的开发人员来说，通常是在功能分支（不在`master`上）上进行所有工作，然后仅在准备就绪后再进行集成。这与上一课（将侧分支推到远程）相似，但是在此我们引入了另外一个步骤。

一些开发人员仅在`master`分支上进行`push` 和 `pull`操作-这样`master`始终保持更新到远程（`o / master`）上的内容。

因此，对于此工作流程，我们结合了两点：

- 将要素分支工作集成到母版上，以及

- 从远程仓库进行`push` 和`pull`操作

![image-20210213215122627](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213215122627.png)

## 为什么不用合并？

为了将新的更新推送到远程，您需要做的就是合并来自远程的最新更改。这意味着您可以在远程分支（例如o / master）中进行`rebase`或`merge`。

因此，如果您可以使用任何一种方法，那么到目前为止，为什么要重点关注`rebase`呢？为什么在使用远程仓库时不喜欢`merge`？

关于在开发社区中`merge`和`rebase`之间的折衷问题，存在很多争论。以下是重新定基`rebase`的一般优点/缺点：

优点：

- `rebase`使您的提交树看起来非常干净，因为一切都在一条直线上

缺点：

- `rebase`修改了提交树的（表观）历史记录。
  例如，提交C1可以在C3之后重新建立`rebase`。这样看来，C1'的工作实际上是在C3之后完成的。

一些开发人员喜欢保留历史，因此喜欢合并。其他人（例如我自己）更喜欢拥有干净的提交树，更喜欢重新定级。全部归结为首选项：D

那让我们看看`merge`是怎么工作的吧![image-20210213214912660](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213214912660.png)

## 远程跟踪分支

关于最后几节课，似乎有些“神奇”的事情是git知道`master`分支与`o / master`有关。确保这些分支具有相似的名称，并且将远程主机上的`master`分支连接到本地`master`分支可能在逻辑上有意义，但是在两种情况下清楚地展示了这种连接：

- 在拉操作期间，提交将下载到`o / master`上，然后合并到`master`分支中。根据此连接确定合并的隐含目标。
- 在推送操作期间，来自master分支的工作被推送到远程的master分支（然后由本地的o / master代表）。推送的目的地由master和o / master之间的连接确定。

==长话短说，`master`和`o / master`之间的这种连接仅通过分支的“远程跟踪”属性来解释。 `master`分支设置为跟踪`o / master`-这意味着`master`分支具有隐式合并目标和隐式推送目的地。==

您可能想知道当您没有运行任何命令指定该属性时如何在`master`分支上设置该属性。好吧，当您使用git克隆存储库时，实际上会自动为您设置此属性。

在克隆期间，git为远程服务器上的每个分支（也就是`o / master`之类的分支）创建一个远程分支。然后，它创建一个本地分支，该分支跟踪在远程服务器上当前处于活动状态的分支，该分支在大多数情况下是`master`。

一旦`git clone`完成，您将只有一个本地分支（因此您不会感到不知所措），但是您可以看到远程服务器上的所有不同分支（如果您很好奇）。这是两全其美！

这也解释了为什么克隆时可能会看到以下命令输出：

```
local branch "master" set to track remote branch "o/master"
```

### 那么我们可以自定义远程跟踪分支吗？

是的你可以！您可以使任意分支跟踪`o / master`，如果这样做，则该分支将具有与`master`相同的隐式推送目标和合并目标。这意味着您可以在名为`totallyNotMaster`的分支上运行`git push`，并将您的工作推送到远程的`master`分支上！

有两种设置此属性的方法。**第一种是使用远程分支作为指定的引用来签出新分支。** 运行：

`git checkout -b totallyNotMaster o / master`

创建一个名为`totalNotMaster`的新分支，并将其设置为跟踪`o / master`。

不多说，让我们看看演示：

![image-20210213220905003](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213220905003.png)

建立新分支`foo`来跟踪远程`master`分支

![image-20210213221015498](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213221015498.png)

同样适用于`git push`

![image-20210213221116920](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213221116920.png)

** 第二种方法 使用 `git branch -u` **，运行：

```
git branch -u o/master foo
```

会将`foo`分支设置为跟踪`o / master`。如果`foo`当前已签出，您甚至可以将其省略：

`git branch -u o / master`

![image-20210213221455617](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213221455617.png)

## 带参数的push

既然您了解了远程跟踪分支，我们就可以开始揭示git push，fetch和pull工作背后的奥秘。我们将一次处理一个命令，但是它们之间的概念非常相似。

首先，我们来看一下`git push`。您在远程跟踪课程中了解到git通过查看当前签出的分支（它“跟踪”的远程）的属性来弄清楚远程和要推送到的分支。这是没有指定参数的行为，但是`git push`可以选择采用以下形式的参数

```
git push <remote> <place>
```

您说的`<place>`参数是什么？我们将很快详细介绍细节，但首先举一个例子。发出命令：

```
git push origin master
```

转到我的存储库中名为“ master”的分支，获取所有提交，然后转到名为“ origin”的远程服务器上的“ master”分支。将缺少的所有提交放在该分支上，然后告诉我完成的时间。

通过将`master`指定为`“ place”`参数，我们告诉git提交来自何处以及提交将往何处。实际上，这是两个存储库之间进行同步的“位置”。

请记住，由于我们告诉git它需要知道的所有内容（通过指定两个参数），因此它完全忽略了我们在哪里签出！

![image-20210213222303402](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213222303402.png)

![image-20210213222325623](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213222325623.png)

那么，如果我们使用不带参数的`git push`呢？

![image-20210213222402743](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210213222402743.png)

### 关于`<place>`变量的更详细的说明

在上一课中，当我们将`master`指定为`git push`的`place`参数时，我们既指定了提交的来源，也指定了提交的目的地。

您可能会想知道-如果我们希望`source`和`destination`不同？如果您想将本地的`foo`分支中的提交推送到远程的`bar`分支上，该怎么办？



为了同时指定`<place>`的源和目标，只需用冒号将两者结合在一起：

```
git push origin <source>：<destination>
```

这通常称为冒号refspec。 Refspec只是git可以找出的位置的一个奇特名称（例如分支`foo`甚至只是`HEAD〜1`）。

一旦分别指定了源和目标，就可以通过远程命令获得相当精确的效果。让我们看一个演示！

![image-20210214001241681](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210214001241681.png)

![image-20210214001302240](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210214001302240.png)

![image-20210214002957754](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210214002957754.png)

## 带参数的fetch

因此，我们已经了解了有关`git push`参数，这个很酷的`<place>`参数甚至冒号refspecs（`<source>：<destination>`）的所有知识。我们是否也可以将所有这些知识用于`git fetch`？

完全正确！ `git fetch`的参数实际上与`git push`的参数非常相似。这是相同类型的概念，但只是在相反的方向上应用（因为现在您正在下载提交而不是上传）。

### `<place>`参数

如果使用以下命令在`git fetch`中指定一个位置：

```
git fetch origin foo
```

Git将转到远程服务器上的`foo`分支，获取本地没有的所有提交，然后将它们放到本地的`o / foo`分支中

![image-20210214003702261](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210214003702261.png)

![image-20210214003728646](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210214003728646.png)

您可能想知道-为什么git将那些提交放到`o / foo`远程分支上，而不是仅仅将它们放到我的本地`foo`分支上？我以为`<place>`参数是本地和远程都存在的地方？

好的，在这种情况下，git会提供一个特殊的例外，因为您可能在`foo`分支上进行了工作，不想弄乱它！这与`git fetch`上的上一课联系在一起-它不会更新本地的非远程分支，它只会下载提交（因此您以后可以检查/合并它们）

“那么，如果我用`<source>：<destination>`显式定义源和目标，会发生什么呢？”

如果您有足够的热情直接将提交获取到本地分支，则可以使用冒号refspec进行指定。您不能将提交提取到已签出的分支上，但是git将允许这样做。

尽管这是唯一的问题-`<source>`现在位于远程位置，而`<destination>`是放置这些提交的本地位置。这与`git push`完全相反，这很有意义，因为我们是在相反的方向上传输数据！

话虽如此，开发人员实际上很少这样做。我主要介绍它是一种概念化的概念，即在相反的方向上提取和推送非常相似。

![image-20210214004209570](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210214004209570.png)

![image-20210214004241079](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210214004241079.png)

![image-20210214004343341](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210214004343341.png)

不带参数的`fetch`

![image-20210214004728987](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210214004728987.png)

![image-20210214004756015](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210214004756015.png)

## `<source>`使用的小技巧

Git以两种奇怪的方式滥用了`<source>`参数。这两种滥用是由于您可以在技术上将`“ nothing”`指定为`git push`和`git fetch`的有效来源。您什么都不指定的方法是通过一个空参数：

- git push origin：side
- git fetch origin：bugFix
  让我们看看它们的作用...

![image-20210214005245927](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210214005245927.png)

![image-20210214005309627](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210214005309627.png)

![image-20210214005335612](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210214005335612.png)

![image-20210214005400154](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210214005400154.png)

## 带参数的pull

既然您几乎了解了有关`git fetch`和`git push`的参数的所有知识，那么几乎没有什么可以覆盖`git pull`了：）

那是因为结束时`git pull`实际上只是获取于合并的简称内容。您可以将其视为使用指定的相同参数运行`git fetch`，然后合并到那些提交最终的位置。即使您也使用疯狂复杂的参数，这也适用。让我们看一些例子：

这是git中的一些等效命令：

`git pull origin foo`等于：

`git fetch origin foo; git merge o / foo`

和...

`git pull origin bar〜1：bugFix`等于：

`git fetch origin bar〜1：bugFix; git merge bug`修复

看？ `git pull`实际上只是`fetch + merge`的简写，而`git pull`关心的只是提交最终的位置（在获取过程中计算出的目标参数）。

让我们看一个演示：

![image-20210214010130225](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210214010130225.png)

![image-20210214010223762](https://gitee.com/hubery_lee123/image-source/raw/master/image-20210214010223762.png)

## 翻译资料及动态演示

[https://oschina.gitee.io/learn-git-branching/](https://oschina.gitee.io/learn-git-branching/)

