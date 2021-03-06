## ARTS

### Share

## 【如何讲清楚技术方案】

最近在评审技术方案，和代码review的时候，遇到刚入行的同学们，很多都讲不清楚技术方案。



具体表现是：

– 上来不说需求，直接说算法实现。台下一头雾水，根本不知道设计方案是否合理。

– 描述完需求后，又直接看代码，看表结构，没有交代流程。

– 比较简单的算法，描述的特别绕，让人听不懂。被别人指出后，觉得这东西这么简单，你们为什么听不懂，还很委屈。

– 直接说术语，不给解释。还有自己造术语不给解释的，更混乱的是「复用」已有的术语，让大家理解都不同。

那么程序员如何把技术方案讲清楚呢？下面从实用的角度教大家一些小技巧，在短时间内具备讲清楚的能力。在文末给出通用的方法论学习书籍，供长线学习，达到把所有事情都能交代清楚。

**一、要先交代需求背景。**


为什么要做这个需求，对于实现的要求是什么，产品经理提了哪些边界条件。没有银弹，一个技术方案的好坏与实现要求息息相关，是不能脱钩的。例如，一个接口访问质量统计系统，可以接受一天跑一次脚本生成数据。但是为用户提供服务的消费明细，肯定要能实时展示，并且不能出错。

在评审中，消耗时间比较多的，就是台下的听众问被评审人需求背景。还有台下的人给出了某个建议，然后被被评审人否定，说有个产品的要求我刚才没说。这时对提出建议的人来说，是很伤的。

交代好背景并对齐，是评审技术方案和代码review的基础，否则别人不知道你后面的是否合理，甚至不知道你到底在做什么。技术方案评审就无从谈起了。

**二、介绍技术方案整体架构**


背景知识说完后，说你的做法。要先总后分，先从整体介绍架构设计。有哪些模块，各自负责什么职责，如何衔接……让大家有个整体认识，看到哪部分是主要矛盾，大家把80%的精力花费在20%的重要模块上评审，好钢用在刀刃上。

例如一个发奖活动，最重要的模块是发奖抽奖模块，但是上来不讲整体，而是先讲展示活动规则的模块，而且用掉了大半的时间，是很浪费人力的。

整体架构的描述用架构图、流程图，加上简练的语言，交代明白即可。一般都有架构模板，直接按照模板的要求，参考已有的优秀例子，都不会有大问题。最重要的是这块要先讲，先交代清楚。


**三、介绍协议、库表设计**

整体方案介绍完之后，介绍协议和数据库表设计，开始逐步深入细节。因为这块设计的是否合理，对程序的效率影响比较大。

分清哪些协议、表是重要的，着重讲，其他不太重要的快速讲。

协议的执行流程，要交代清晰，整个协议是怎么在各个模块中流转的，到具体数据修改时，是如何和已有表结构串联起来的。这也是程序执行的流程，如果讲不清楚，会深度怀疑你是否能实现清楚。

这部分要注意，尽量少说术语。因为大家的背景知识不同，一些专门术语大家是不知道的，你要用直白的话语让大家听明白。

例如：有人在描述协议流程时说「我调用server提供的123号命令，返回成功后，把数据库的state字段改为2，就完成发奖了」。但是你说的123是干什么的，state是什么意思，2是什么状态？

大家的疑问太多了，好的说法应该是，「我调用server提供的123号发奖的协议，返回成功后，把数据库中该用户的发奖状态，更新为已发奖」。

**四、描述分支和异常逻辑，讲解代码**


经过前面几部的讲解，方案基本上讲完了。剩下的就是讲分支逻辑，和异常逻辑。一份代码写的好不好，程序员是否有经验，主要是看对于异常处理是否到位。

这部分从架构上主要讲容灾、鲁棒性，例如某个server死掉了，或者某个模块频繁请求，你的系统是否有预警，能够兼容。说白了就是要讲解系统的边界条件和服务能力。

最后上代码，如果是代码review，在这个时候才开始说你的代码。虽然看的时间比较晚，但是大家都知道你的代码是什么功能了，看的速度也会加快。

**五、复盘**


每次评审后，要自己复盘，总结。别人都问题哪些问题，为什么要问？哪些问题是我应该交代没交代的，让人家问了？哪些是我方案的问题，别人提出的挑战？

对于自己没交代的，思考为什么会漏，如果能提前讲清楚，是否能节约很多时间。

根本的心法就是要有同理心。从对方的角度思考，这个问题他会了解吗，我不说他明白吗？**方案评审重要的不是你说完，而是别人听懂。**关注台下人的反应，你的任务不是讲，而是让大家听明白。不是一个劲的说，而是要让大家都理解你的意思，这样别人才能帮你。否则别人会一直问问题，挑战你，最后否定你的方案。

千万不要觉得听众好笨，这么简单都不明白，如果台下的人都不明白，那么一定是你错了。能力强的人是能够把难题讲解的很简单的。美国有专门负责科普的作家，把复杂的科学知识做到「老妪能解」。台下评审的人都是身经百战的，如果他们都反映听不懂，那么会是谁的问题呢？

## 总结

技术方案讲解要先交代背景，再讲整体架构，再细化流程。先主线，再分支，先正确路径，再异常逻辑。要在听众的角度去讲，尽量直白简单，能够让不懂技术的人听懂才是最好的。


