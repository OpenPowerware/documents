# OpenPowerware RT-thread 开发者指南


# 前期准备

- [ ]  注册github账号
- [ ]  下载git客户端，参考[https://www.cnblogs.com/xueweisuoyong/p/11914045.html](https://www.cnblogs.com/xueweisuoyong/p/11914045.html)
- [ ]  安装github客户端，[https://desktop.github.com/](https://desktop.github.com/)

# fork仓库

首先上github网站

[https://github.com/OpenPowerware/rt-thread](https://github.com/OpenPowerware/rt-thread)

点击右上角fork，fork我们的仓库，如下图所示。

![fork](figures/fork.png)

注意勾选copy the openpowerware branch only。随后点击create fork。这里对我们仓库的分支作以下说明：

<aside>
💡 目前我们的仓库共有三个主要的branch，分别是
1. openpowerware，默认分支，用于开发者提交pr的分支
2. master，不可pr，仅用于与rt-thread主仓同步
3. upstream，不可pr，仅用于向rt-thread主仓提交pr

</aside>

由于后面三个都是由管理员负责管理的branch，所以大家在fork的时候只fork默认的分支就好。

熟悉git的同学已经不用看后面的教程了，直接开搞。对于不熟悉git操作的同学，请看下面的步骤。

# 本地开发步骤

## 1. clone仓库到本地

在fork好之后就能在自己的主页看到rt-thread的仓库了，比如我的名字是felixQi7，就能看到如下所示的仓库，点击Code，再点击2所在位置处的复制图标，这个url之后的步骤会用到。

![clone1](figures/clone1.png)

打开github desktop客户端，登录自己的账号之后，按照下图操作。

![clone2](figures/clone2.png)

1. 点击Clone a repository from the Internet
2. 点击URL
3. 粘贴刚刚复制的网址
4. 选择合适的路径，用来存放仓库的本地代码
5. 点击clone

等待片刻，代码自动下载到本地，可以进行下一步操作。

## 2. 创建自己的branch并开发

如下图所示，点击new branch，然后填入开发分支的名字（最好是起一个有意义的名字），点击create branch即可

![newbranch](figures/newbranch.png)

此时你可以按照功能需求开发相应的代码了。注意可以在每一次相对独立的修改之后就可以提交一次本地的commit，并写好title和Description，对修改的内容进行说明。

如下图所示，我修改了readme.md文件，修改完之后进行commit，同时注意写好对应的comment，如下图所示。

![push](figures/push.png)

commit之后可以推送到自己的远程仓库。

## 3. 正式pr

当本地的文件修改完毕之后，测试通过之后并且推送到自己的远程仓库之后，就可以创建PR（Pull-request）。点击如下图所示的位置Create Pull Request。

![pr1](figures/pr1.png)

随后会弹出pr提交网页，如下图所示。

![pr2](figures/pr2.png)

在pr中需要详细地写明进行了哪些修改，并是否通过了测试。可以参考的pr模板为

![pr3](figures/pr3.png)

然后点击create pull request。随后管理员会审阅代码，如果代码没有问题，pr将被merge到仓库，否则将被要求继续修改。

**有几点需要注意（敲黑板）：**

**（1）pr只能从自己的branch发起，pr到openpowerware，即 userbranch → openpowerware。这里的userbranch不能与系统的branch（master，upstream，openpowerware）重名，否则merge的时候会出问题。**

**（2）openpowerware fork到自己的仓之后，可以用github网页上的sync功能与源仓同步。openpowerware这个branch即使在自己的仓也不能自己修改，否则sync的时候会出现冲突。**

**（3）pr前需要在硬件上完成测试，并遵从rt-thread的[代码规范](https://github.com/OpenPowerware/rt-thread/blob/openpowerware/documentation/contribution_guide/coding_style_cn.md)。
可以用这个[formatting tool](https://github.com/mysterywolf/formatting)来自动格式化。注意只需要format改动的代码文件，没有改动的文件不需要formmat，否则会导致copyright栏的时间出错。**

**（4）以上clone操作均基于linux系统。如果你使用linux，请直接用git命令行操作。**


好了，以上就是整个pr流程了。

## 4. 写给管理员

下面这个部分是写给OpenPowerware这个组织的管理员的，非管理员可以不用看。目前这个组织由齐雨（@QY7）和顾云杰（@CuttySark1869）管理，今后我们也会吸纳更多的管理员。

OpenPowerware旨在开发针对电力电子控制的软件生态，目前是基于rt-thread，今后会考虑支持其他的RTOS比如freeRTOS。电力电子控制相比大多数嵌入式应用有非常特殊的地方，因此我们对RTOS的一些修改可能不会merge到主仓，但是我们又希望能够跟主仓尽量的同步以充分利用RTOS的生态。因此我们设计了上述三个branch的结构，其相互关系如下图所示。

![structure](figures/structure.png)

master是rt-thread主仓的影子，负责和主仓同步。openpowerware是我们自己的主branch，负责接收pr。upstream是master和openpowerware之间的桥梁，负责来回merge，由管理员手动完成。所以管理员的任务有三：

（1）定期同步master并把更新merge进upstream和openpowerware；注意master不能直接merge进openpowerware必须以upstream为中介。

（2）review pr，通过或者提出修改意见。

（3）定期检查openpowerware，如果发现其中适合合并进rt-thread主仓的修改，则首先merge进upstream，并从upstream像rt-thread主仓的master提出pr。
