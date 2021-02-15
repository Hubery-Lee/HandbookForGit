# Git基础

[toc]

## 主要命令

- git add
- git commit
- git branch
- git checkout


## 合并

### git merge

结合工作的第一种方法是git merge。在Git中合并会创建一个特殊的提交，该提交具有两个唯一的父级。与两个父母一起进行的承诺本质上意味着“我要包括这名父母在这里，这件事中的所有工作，以及他们所有父母的全部。

### git rebase

将分支机构之间的工作结合起来的第二种方法是重新定级。变基本质上需要一组提交，将它们“复制”，然后将其复制到其他位置；虽然这听起来令人困惑，但是重新定基的优点是它可以用于制作良好的线性提交序列。如果仅允许重新定级，则存储库的提交日志/历史记录将更加整洁

## 提交树中的移动



### 绝对移动

在我们了解Git的一些更高级的功能之前，了解在代表您的项目的提交树中移动的不同方法很重要。

首先我们要谈谈“ HEAD”。 HEAD是当前已检出提交的符号名称-本质上就是您正在处理的提交。

HEAD始终指向工作树中反映的最新提交。大多数更改工作树的git命令都将从更改HEAD开始。

通常，HEAD指向分支名称（例如bugFix）。提交时，bugFix的状态会更改，并且此更改可通过HEAD看到



### 相对移动

通过指定提交哈希值在Git中移动可能会有些乏味。在现实世界中，您的终端旁边没有很好的提交树可视化，因此您必须使用git log来查看哈希。

此外，在实际的Git世界中，哈希值通常也要长得多。例如，引入上一级别的提交的哈希是fed2da64c0efc5293610bdd892f82a58e8cbc5d8。不能完全从舌头上滚下来...

好的方面是，Git对哈希很精明。它仅需要您指定足够的哈希字符，直到它唯一地标识提交为止。因此，我可以键入fed2而不是上面的长字符串。

就像我说的那样，用哈希值来指定提交并不是最方便的事情，这就是Git具有相对引用的原因。他们真棒！

使用相对引用，您可以从一个令人难忘的地方开始（例如分支bugFix或HEAD），然后从那里开始工作。

相对提交功能强大，但是我们将在此处介绍两个简单的提交：

- 一次向上移动一次提交`^`

- 使用`〜<num>`向上移动多次

![](https://gitee.com/hubery_lee123/image-source/raw/master/Screenshot%202021-02-09%20145111.png)



## 在Git中回溯

逆转Git的变化
有很多方法可以逆转Git中的更改。就像提交一样，撤消Git中的更改既具有低级组件（分段单个文件或块），又具有高级组件（如何实际撤消更改）。我们的应用程序将专注于后者。

撤消Git中的更改有两种主要方法-一种是使用`git reset`，另一种是`git revert`。我们将在下一个对话框中查看每个

### git reset

`git reset`通过将分支引用及时向后移到较早的提交来恢复更改。从这个意义上讲，您可以将其视为“重写历史”； `git reset`将向后移动一个分支，就好像从来没有进行过提交一样。

![](https://gitee.com/hubery_lee123/image-source/raw/master/Screenshot%202021-02-09%20150140.png)

### git revert

虽然`git reset`重置非常适合您自己计算机上的本地分支，但其“重写历史记录”方法不适用于其他人正在使用的远程分支。

为了撤消更改并与他人共享那些撤消的更改，我们需要使用`git revert`。让我们看看它的作用

![](https://gitee.com/hubery_lee123/image-source/raw/master/Screenshot%202021-02-09%20145952.png)



## 工作树结构拼接

到目前为止，我们已经介绍了git的基础知识–在源代码树中进行提交，分支和移动。仅这些概念就足以利用git存储库90％的功能并满足开发人员的主要需求。

但是，剩下的10％在复杂的工作流程中（或使您陷入困境时）可能非常有用。我们将要讨论的下一个概念是“四处移动” –换句话说，这是开发人员以精确，雄辩，灵活的方式说“我想要在这里工作并在那里工作”的一种方式。

### git cherry-pick

本系列的第一个命令称为`git cherry-pick`。它采用以下形式：

`git cherry-pick <提交1> <提交2> <...>`
这是一种非常简单的说法，即您要在当前位置（HEAD）下复制一系列提交。我个人很喜欢`cherry-pick`，因为涉及的魔术很少，而且很容易理解

![](https://gitee.com/hubery_lee123/image-source/raw/master/Screenshot%202021-02-09%20150851.png)

### git rebase -i

当您知道想要的提交（并且知道它们对应的哈希）时，`Git cherry-pick`很棒。不得不夸赞其简单性。

但是，如果您不知道想要哪个提交，情况又会如何呢？幸运的是，git也覆盖了那里！我们可以为此使用`interactive rebase`-这是查看将要rebase的一系列提交的最佳方法。

所有交互式`rebase`手段都是将`rebase`命令与`-i`选项一起使用。

如果包含此选项，git会打开一个UI，以向您显示哪些提交将被复制到变基目标下方。它还显示了他们的提交哈希和消息，这对于了解提交是什么是非常有用的。



交互式重新设置对话框打开时，您可以执行3件事：

- 您可以简单地通过在UI中更改提交的顺序来重新排列提交（在我们的窗口中，这意味着可以使用鼠标拖放）。
- 您可以选择完全省略一些提交。这是由`pick`指定的`--toggling pick off表示您要放弃提交。
- 最后，您可以压缩提交。

![](https://gitee.com/hubery_lee123/image-source/raw/master/Screenshot%202021-02-09%20163915.png)

## 一些技巧

### 只抓取一次提交

本地堆积的提交 `locally stacked commits`
这是一个经常发生的开发情况：我正在尝试查找一个错误，但是它很难捉摸。为了协助我的侦探工作，我输入了一些调试命令和一些打印语句。

所有这些调试/打印语句均在其自己的提交中。最终，我找到了该错误，进行了修复，并为之高兴！

唯一的问题是，我现在需要将`bugFix`重新带回`master`分支。如果我只是简单地转发`master`，那么`master`将得到我所有的调试语句，这是不希望的。必须有另一种方式...

我们需要告诉git仅复制一次提交。就像前面介绍的工作层次一样，我们可以使用相同的命令：

```
git rebase -i
git cherry-pick
```

为了实现这个目标

### Juggling Commits 杂耍犯

这是另一种很常见的情况。您有一些更改（newImage）和另一组相关的更改（caption），因此它们在存储库中彼此堆叠（又名一个又一个）。

棘手的是，有时您需要对早期提交进行一些小的修改。在这种情况下，设计人员希望我们稍微更改newImage的维度，即使该提交已追溯到我们的历史中！！

我们将通过以下操作克服这一困难：

- 我们将对提交进行重新排序，以便我们要更改的提交在最上面 `git rebase -i`
- 我们将`commit--amend`进行稍作修改
- 然后，我们将使用`git rebase -i`重新将提交重新排序到以前的状态
- 最后，我们将`master`移动到树的此更新部分以完成（通过您选择的方法）

有很多方法可以实现此总体目标（我看到您在盯着`cherry-pick`），稍后我们将看到更多方法，但现在让我们集中讨论此技术。最后，请注意此处的目标状态-由于我们两次移动了提交，因此它们都附加了撇号。为我们修改的提交添加了另一个撇号，这使我们有了树的最终形式

话虽如此，我现在可以根据结构和相对撇号差异来比较级别。只要您树的主分支具有相同的结构和相对的撇号差异，我将给予全部功劳

### git commit --amend

>  **对以往的commit进行修改，注意修改之前需要调整commit的顺序使得需要修改的**

````
git commit --amend
````

调整commit顺序的方法有两个`rebase -i`和`cherry-pick`

如您在上一级别中所见，我们使用`rebase -i`对提交进行了重新排序。一旦我们要更改的提交排在最前面，我们就可以轻松地-对其进行修改`--amend`，然后重新排序回我们的首选顺序。

这里唯一的问题是正在进行大量的重新排序，这可能会引入重新设置冲突。让我们看一下`git cherry-pick`的另一种方法。



## Git标签

### git tag

正如您从先前的课程中学到的那样，分支很容易移动，并且在完成工作时经常引用不同的提交。分支很容易变异，通常是临时的，并且总是在变化。

如果是这样，您可能想知道是否存在一种在项目历史中永久标记历史点的方法。对于主要版本和大型合并之类的事情，是否有任何方法可以用分支以外的其他永久性标记这些提交？

对有这种功能！ Git标签支持这种确切的用例-它们（某种程度上）将某些提交永久标记为“里程碑”，您可以像分支一样引用它们。

但是，更重要的是，它们不会随着创建更多提交而移动。您不能“签出”标签，然后完成该标签上的工作-标签作为提交树中的锚点存在，用于指定某些位置。

让我们看看实际中的标签是什么样的。

![](https://gitee.com/hubery_lee123/image-source/raw/master/Screenshot%202021-02-09%20174518.png)

## Git描述

### git describe

因为标签在代码库中充当了很好的“锚”，所以git有一个命令来描述您相对于最接近的“锚”（aka标签）的位置。该命令称为`git describe`！

`git describe`可以帮助您在历史上向前或往后移动许多提交之后获得帮助。当您完成git bisect（调试搜索）后，或者坐在刚从假期回来的同事的计算机上坐下时，可能会发生这种情况。

Git描述采用以下形式：

`git describe <参考>`

哪里`<ref>`是git可以解析为提交的任何内容。如果您未指定`ref`，则git仅使用您现在已签出的位置（`HEAD`）。

命令的输出如下所示：

`<标签> _ <numCommits> _g <哈希>`

其中`tag`是历史记录中最接近的祖先标记，`numCommits`是该标记离开的提交数量，而`<hash>`是所描述的提交的哈希值。

![](https://gitee.com/hubery_lee123/image-source/raw/master/Screenshot%202021-02-09%20175606.png)

## 一些高级功能

### 重新布置多个分支

伙计，我们这里有很多分支！让我们将这些分支的所有工作重新部署到`master`上。

但是，高层管理人员却使这变得有些棘手-他们希望所有提交都按顺序排列。因此，这意味着我们的最终树应在底部具有`C7'`，在其上方具有`C6'`，依此类推，所有这些都应按顺序进行。

如果您一路搞砸，请随时使用`reset`重新开始。确保检查出我们的解决方案，看看是否可以用更少的命令来完成！

![](https://gitee.com/hubery_lee123/image-source/raw/master/Screenshot%202021-02-09%20180847.png)

### 指定父母

像`〜`修饰符一样，`^`修饰符也接受一个可选数字。

`^`上的修饰符没有指定要返回的世代数（`〜`花费了多少），而是指定了从合并提交中跟随哪个父引用。请记住，合并提交有多个父级，因此选择的路径是不确定的。

Git通常会从合并提交中向上跟随“第一个”父对象，但是用^指定一个数字会更改此默认行为。

![](https://gitee.com/hubery_lee123/image-source/raw/master/Screenshot%202021-02-09%20181149.png)

![](https://gitee.com/hubery_lee123/image-source/raw/master/Screenshot%202021-02-09%20181312.png)

![](https://gitee.com/hubery_lee123/image-source/raw/master/Screenshot%202021-02-09%20181348.png)

![](https://gitee.com/hubery_lee123/image-source/raw/master/Screenshot%202021-02-09%20181440.png)

## Branch Spaghetti错综复杂的更新

WOAHHHhhh Nelly! We have quite the goal to reach in this level.



`master`上在一些分支`one` `two` and `three`前有一些提交。无论出于何种原因，我们都需要使用`master`上最后几次提交的修改版本来更新其他三个分支。

Branch `one` 需要重新排序和删除 `C5`. `two` 需要纯粹的重新排序, and `three` 只需一次提交！

我们将让您弄清楚如何解决这一问题-确保稍后通过 `show solution`.检查我们的解决方案。

![](https://gitee.com/hubery_lee123/image-source/raw/master/Screenshot%202021-02-09%20182442.png)