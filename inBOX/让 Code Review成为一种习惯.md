# 让 Code Review成为一种习惯

来自：火光摇曳www.flickering.cn

链接：http://www.flickering.cn/uncategorized/2014/08/%E8%AE%A9-code-review%E6%88%90%E4%B8%BA%E4%B8%80%E7%A7%8D%E4%B9%A0%E6%83%AF/

**1.开篇**

5月份的时候突然接到 code.oa.com【腾讯内部的一个代码管理平台】 的 summer 的通知， 说广点通的codereview 参与度在公司各部门中表现出色，而我们小组（广点通广告定向小组）的 codereview 综合表现在全公司的小组中排名第一。这让我有点意外，有点无心插柳的感觉。 summer 希望我能写写我们小组做 code review 的一些心得体验， 回想我学习使用 codereview 的经历， 我最先想到的一句话是我几年前看到的关于 codereview 的一句评述：

The biggest thing that makes Google’s code so good is simple: code review.That’s not specific to Google – it’s widely recognized as a good idea, and a lot of people do it. But I’ve never seen another large company where it was such a universal. At Google, no code, for any product, for any project, gets checked in until it gets a positive review.

http://scientopia.org/blogs/goodmath/2011/07/06/things-everyone-should-do-code-review/

这是一段让我记忆深刻的话， 一个Google 工程师写的， 我摘录下来做作为文章的开始，表达我在腾讯这些年和一些出色的工程师合作的过程中一起学习和实践 CodeReview 之后， 对CodeReview 文化的一种由衷的认同和尊敬。

![](net-img-0-20220206181958-5g6j133.png)

**2.Rietveld**

我 2008 年进入腾讯， 主要呆在 R2参与soso 的开发， 从 2008 ~ 2010 年我都很不屑于 R2 的研发质量管理，觉得做得实在很糟糕：没有统一的编码规范、没有unittest，甚至经历过有些人不会用 svn, 不太懂代码的版本控制， 当然我自己也不怎么懂开发流程中的质量管理， 那时候连 codereview 是什么都没听说过。2010年初， 公司开始在搜索上加大投入， 招聘了一些牛人，尤其是 Google 的一些工程师， 而这些人也把 Google 的一些开发文化带入了 R2。虽然 soso 最终的命运并不如人所愿， 但是在 2010~2013年间， 许多soso 的工程师是学习并经历了什么是严谨而强大的开发流程，也见过我们曾经山寨的 Google 的技术， 以及山寨的 Google 的开发流程。 而至今为止，我们和很多同事回忆起来，我们常调侃说我们是见过猪跑也吃过猪肉的。

2010年4月从Google 来到我们部门的一位牛人是 yiwang, 做机器学习的博士， 做了一次 LDA topic modeling 的报告， 令人印象深刻， 接触了几次之后，我主动找了 yiwang 一起合作，学习 LDA 并参与并行 topic modeling 的开发。yiwang 让我做的第一件事就是研究一下 Google 开源的 Rietveld code review 系统， 并找一台机器把 Rietveld 搭建起来。于是一顿折腾之后， 我们搭建了公司的第一套严格意义上的 codereview 系统， 当然那个时候也只是供我们小组内自己使用。 搭建好 Rietveld 之后， 我们做的第二件事就是把 code style 都统一到 Google 发布的 C++ coding style。 最早在这套系统上进行 codereview 的就是4个工程师 yiwang, leostarzhou, charlieyan, 和我。

所有和 yiwang 合作过的人在 codereview 中都苦不堪言， 他是近乎苛刻的严格执行 Google C++ coding style, 我们提交的的 code 中多一个空格，多一个空行；或者是注释符号 “//” 后面少一个空格， 全都被打回来修改；更别说变量、函数名命名不符合规范，随意使用变量缩写这些大的禁区了。对于我们最初的代码， yiwang 挨个细致的指出我们违反规范的地方， 在 comment 中贴上Google C++ 规范的具体要求的 URL anchor， 以说明我们为何违反了规范。 一开始我是有点不以为然的， 来回几次之后我自己也不好意思了， 把 Google coding style 从头到尾仔仔细细的看上3遍， 认真的记住一些细节， 于是很快就开始上手了。

自然， 后续我们合作的工程师逐渐多了起来， 对于 codereview 的必要性，许多人是有质疑和挑战的， 于是在和yiwang合作的几年里， 我反复听他说了三个故事。

第一个是关于他自己的，他从小开始 coding, 高中的时候已经拿到工程师的认证，进入 Google 之前 coding 无数， 进入 Google 之后写的第一个100行的程序， 被贴的 comment 超过了100行。 所以不要抱怨我们的标准严格， 严格的标准才能产出漂亮的代码， 我们今天在腾讯做的 codereview 真是不算啥。

第二个是关于Google 内部 codereview 的争议的， 据说 Google 内部建立 codereview 制度的时候，也是很多工程师不乐意， 尤其是年长的工程师， 写了多少年代码了， 被一群年轻的小屁孩在代码上指指点点，实在是受不了。 最后说是 Google 的一位极有地位的人物出来说话了：我们要想把工程质量控制好， 无论是资历深浅，都一定要遵守共同的代码规范，于是 codereview 制度得以建立。

第三个是解释为何要用 Google C++ coding style 的， Google 的代码规范是一群顶尖的 coder 制定的， 变量名用小写， 函数名用驼峰式， 那都是有严格的视觉美学讲究的，让人看了很舒服的，甚至是做过视觉对照研究的。虽然我对此存疑, 我还是认同 Google C++ coding style 是我见过的最好的 coding style。 而我们腾讯的一位 HR 在和我闲聊的时候也表示过同样的观点 :-)。

Coding style 的规则制定很多都是相对的， 而Google style做得 NB 的地方就是， 他设定的大多数规则的目的他并不是要证明自己是正确的， 而是详细的列出各规则的优缺点(pros and cons)，让你更深刻的理解这些规则。著名的统计学家 George E.P. Box 在数据建模中说过一句广为流传的话： All models are wrong, but some are useful. 套用这句话到 code review 的 coding style 中， 我想说 All coding rules are wrong, but some are useful。当然 Google style 好的一个额外的原因是 Google 提供了 cpplint 这个工具自动检查你的 code 中是否有违反规范的地方， 大部分的低级错误都会被检查出来。

**3.推广 CodeReview**

我们小组刚开始把 Google 的 Rietveld 在内部搭建起来并在部门内推广使用以后， 发现了一些问题， 主要包括：

1.issue 提交之后如何通过 email 发送到公司的 outlook 帐号

2.Rietveld 提供的提交 issue 的工具 upload.py 对中文的支持不好

3.底层用的文本文件做存储， 访问量大的时候，提交 issue 多的时候， 速度不行

前面两个问题charlieyan 和我对 Rietveld 的 python 源代码做了一些修改， 很快解决了。 第3个问题我们一直解决得不好。 不过当时来说支撑我们几个小组的开发还是 OK的。 所以 yiwang 开始继续推动 Rietveld 在 R2 的使用。 到了 2011 年的时候这套工具已经在 R2 的多个部门被使用了。 下面一张图是我们曾经统计过的各个部门当时的使用情况![]()

![](net-img-0-20220206181958-0i2edfp.png)

2010 年我们在推广 codereview 的时候， yiwang 也联系了研发管理部的同事， 也就是 code.oa.com 的维护人员， 当时 code.oa.com 并没有严格意义的 codereview 功能。 它提供一个代码 checkin 之后进行 codereview 的功能， 所以也没有发送 codereview 的 upload.py 脚本；这个功能自然是有用的，但是事后的 review 往往有缺陷， 很难替代事前的 review。事实上更重要的问题是， 公司内部当时并没有形成 codereview 的文化。 yiwang 和研发管理部的人交流的时候强烈的推荐了 Rietveld 系统， 不久之后code.oa.com 也仿照 Rietveld 开发了正式的 codereview 功能，而用于提交issue 的 tcr.py 也正是基于Rietveld 的 upload.py 做的修改。

而在搜搜内部，状况则有些不同。 Google 来的朱会灿带领的搜索云平台(搜索基础架构部）对 codereview 文化建设做了强力的推动， 基础架构部门的几个牛人开始使用这套系统之后， wujie, phong 他们接管了这套 CodeReview 系统， 当时上面提到的第三个问题当时已经成为了一个严重的问题， wujie， phong 开始改进 Rietveld 的性能问题， 把底层的文本文件存储替换为 mysql 数据库， 使用 apache 服务器替换了 Rietvled 基于 Django 的 HttpServer, 所以Rietveld 第三个和性能相关的问题也彻底被解决了。 后续 huanyu 申请了 codereview.soso.oa.com 这个域名专门提供 codereview 服务， 于是直到搜搜和搜狗合并， 搜搜内部的好几个部门的兄弟们一直基于 Rietveld 系统做 codereview。

![](net-img-0-20220206181958-re6vkdy.png)

**4.C++ Readability**

搜索广告部门的情境广告中心应该是我经历的 code review 文化建立得最彻底完善的地方。 当时 yiwang 是这个中心的总监，我和 huan 各负责一个小组， 我们中心要负责开发一套 AFC(ads for content) 的情境广告产品， 和现在广点通有很多相似之处。 huan, yiwang, 包括我们的 GM paulyan 都是 Google 的工程师， 而 AFC 这个产品几乎是从零开始开发， 所以很自然的， 许多开发流程都是山寨的 Google 的， 虽然 AFC 这个产品最终由于各种原因并没有在公司内部成功， 但是许多参与开发的工程师对于我们当时建立起来的开发流程应该是印象深刻的。 当然 codereview 是其中很重要的一个环节。 Huan 借鉴 Google 的CodeReview 流程， 在中心推动了一个 C++ readability 制度的建立。 一个语言的 readability 表示工程师对这个语言和相应的编码规范的熟悉， 这个 C++ readability 最初只授予有限几个高职级的工程师。 每一个 codereview issue 至少需要得到一位具有 C++ readabiltiy 的工程师的认可， 这个 issue 才能够提交到代码库中。

![](net-img-0-20220206181958-3pbtunt.png)

而每一个没有 C++ readability 的工程师要想获得这个 readability, 需要提交一个 codereview issue 做专门的申请， 这个issue 的标题必须包含 “Apply for C++ Readability”, issue 至少包含三个文件， xxx.cc, xxx.h, xxx_test.cc, 如果这个 issue 通过严格的 codereview 被通过了， 就可以授予这个工程师 C++ readability 。 而通常这种申请的 review 过程大家都会特别的认真细致。

![](net-img-0-20220206181958-kkc4wwq.png)

**5.在广点通 CodeReview**

![](net-img-0-20220206181958-rdgx4c4.png)

2013年10月， 搜索广告平台部并入广点通成立了SNG 的效果广告平台部， 原来AFC 的系统由于这个合并事实上基本废弃， 只是在开发过程中很多模块组件被逐步迁移到了广点通的系统上。 我们刚进入广点通的时候， 广点通使用 code.oa.com 平台做 codereview, 也建立了一些 codereview 的文化， 但是有很多地方是执行不到位的。 coding style 虽然也推崇 Google coding style, 但是执行上不严格， 所以整个代码库的代码风格是很不统一的。 这也给我们的 codereview 执行带来很大的挑战：

●旧的代码风格不统一，我们如何统一代码风格， 旧的代码是否需要基于新的风格重写？

●整个产品都在高速的开发迭代中，老大对于产品功能的开发演进有明确的期望， 产品经理天天在开发人员后面催进度， 我们是否要执行严格的 code review ？

广点通最后还是明确的规定 coding style 统一到 Google C++ coding style， 我们团队的 CodeReview 也整体转战 code.oa.com, 我们自己不再使用 Rietveld（不过这套系统并没有被废弃， wujie 2014年一月的时候把这套系统搭建起来服务于微信， 并申请了域名 cr.oa.com), code.oa.com 的 CodeReview 功能经过几年的改进， 也确实变得很好用。 考虑到产品迭代的节奏， 我们广告质量中心在推进的过程是渐进的：

●所有新提交的文件必须使用 google style

●所有新修改的代码必须使用 google style

 **●** 在指定的时间范围内， 重要模块的 code 必须重构为 google style

为了让 codereview 文化在广点通能更加彻底的建立起来、执行到位， 几位总监和架构师也迅速推动 code.oa.com 中实现了 code owner 的机制， 每个目录下面明确设置 code owner, 每个change issue 要想提交， 必须有 code owner 通过才可以。 而刚开始执行的时候， 广告质量中心为了保证 code review 执行到位， code review 是非常集权的， 所有相关目录的 owner 只写上总监。所以质量中心发出的所有 code review issue 只有总监通过了之后才可以提交， 在一定的时间内这确实影响开发效率， 不过对教育所有开发人员的 code review 意识是高效的。 在经历了近三周的严格控制之后， 目录的owner 才加上组长， 然后逐步放开到组内的一些骨干成员。

![](net-img-0-20220206181958-7iiz7st.png)

对于我们小组而言， 有一半是之前 AFC的开发人员， 经历过严格的 codereview 训练； 有一半是广点通原来的开发人员，没有经历过严格的 codereview 训练。 为了教育大家的 codereview 意识，在小组的周会上我要反复的强调 codereview 的重要性；同时我明确的指出每个人的考评是和 codereview 的表现相关的。

对于广告业务的工程输出主要包括项目中的设计文档， 代码实现， 线上A/B test实验和实验结果跟进分析， 以及日常在小组内的分享(平常积极的邮件讨论，wiki 建设， 组内的 techtalk 分享等)。 所有的一线工程师， 无论职级高低，最重要的工程输出原则还是 show me the code, 而 codereview 是最能够反应这个客观输出的。 在小组内部， 我们尽量让每个人的 codereview 参与状况都公开透明， 所以我们要求

●每个 codereview issue 要明确发送到你的项目合作者， 项目合作者 review 之后才允许提交

●每个 issue 都必须全员转发到小组内所有成员， 小组内的任何人只要愿意，都可以去review 其他人的代码

由于所有的 codereview issue 都会发送到小组内所有成员， 所以我是能够在 code.oa.com 上看到所有人的 code 产出情况的。我通过给大家做 codereview 判断小组每个工程师的工程输出， code.oa.com 上大致是可以看到小组内每个人参与 codereview 的程度的， 包括他发出的 issue, 他给组内成员做的 codereview comments 等等。 code.oa.com 上提供了一个功能， 可以 dump 出所有人的 codereview issue, 所以考评的时候我会检查每个成员在半年之内的 codereview 输出状况，检查组内成员提交的 issue 的质量。

不过 code.oa.com 上查看每个人的 codereview 参与状况的功能现在还是太弱， 如果code.oa.com 能提供一个统计分析功能, 使得每个人可以看到自己的一些统计指标， 包括

●我修改的文件总数，修改和提交的代码总行数

●我给合作者做了多少次 codereview, 提供了多少次 comments

●合作者给我做了多少次 comments

如果小组的组长能够看到组内成员的这些统计指标，无疑对于考评每一个小组成员的工程输出会提供很多客观公正的信息。

**6.CodeReview 感悟**

做了4年的 CodeReview, 基本上这个流程在我们的团队已经深入骨髓，成为了小组工程工作中的种行为习惯。 如果有一天我离开了腾讯， 我相信我在腾讯所经历的相对严格的 CodeReview 训练会帮我做好下一个项目的工程质量管理。 为何要做 CodeReview 网络上有很多的论证， 之前和 yiwang 合作的时候， 团队内总结的几点如下(主要都是 yiwang 总结的):

![](net-img-0-20220206181958-u9tzfyq.png)

●Code Review保证开发质量，整个过程有章可循，无需顾及“面子”，能够修正很多平时不注意或者明知故犯的小毛病

●Code Review高效沟通交流，不需电话和视频会议，让北京深圳两地的同事们一起协同工作

●Code Review帮助知识传递，团队内细粒度的知识分享， 大家一起逐行的点评代码比用技术交流会以及KM投稿更细致

●Code Review保证代码的可持续使用，代码风格不一致导致一个系统每次移交负责人都被重写一次

●Code Review和敏捷开发相容，而不是相斥，敏捷不是片面追求发布次数，而是保证质量的发布

●CodeReview 是一种社交的途径， 鼓励小组成员之间多做技术交流

●CodeReview 对新人的成长极其有利。 很多新人第一次做 codereview 的时候感觉都是惨不忍睹， 被拍晕了， 我见过的最惨的 issue 被要求修改了20多次之后才被允许提交。然而这种方式对新人的成长非常高效，一个coding 能力不错的工程师， 几个 codereview issue 之后， 很快就能写出合格的代码。

●CodeReview 增加团队内代码的可维护性, 同一份代码至少会有两个人熟悉——代码的作者和审阅者。

**如何建设一个团队的 code review 文化， 跟随 Google 的脚步， 也有一些具体的意见：**

●需要培养合格的reviewer。 建立各种编程语言的readability认证机制，从一组“种子”reviewer开始，让更多同事获得认证成为合格的reviewers

●需要细化和公开各种语言的code style。 评注中可以给出建议的出处的URL，使得保持代码风格和可维护性落到实处

●需要建立Code Lab机制。 帮助工程师们入门开发流程和规范

●需要建立change approval机制。 鼓励组内更多工程师改进代码（敏捷），但是修改在提交前除了必须通过review，还需要相关责任人的approval；在加速开发滚动的同时，保证权责分明

●引导轻量 review。我们都经历过有些工程师发一个 change issue 的时候， 几十个文件一并发出来， 一个 issue 可能是累积一周的修改， reviewer 看到这种 issue 几乎都是崩溃的，不可能保证 review 质量。 所以好的 issue 应该是轻量的， 大的 issue 应该拆分为小的 issue 发送。下图是 Cisco 公司的codereview 报告中摘出的， 如果一个 issue 的修改代码行数超过 400， 基本上 reviewer 是不可能认真 review 帮你找出 bug 的 。

![](net-img-0-20220206181959-fggkmjt.png)

●明确issue 能否提交是工程师自己的责任。开发人员有时候抱怨别人 review 得慢， 拖延时间。 我们小组内部通常明确指出：一个 issue 能否提交是开发人员自己的责任。 开发人员自己要积极推动合作人员帮忙 review issue， 推动 issue 被通过并及时提交。

**7.结语**

学术界的所有高质量论文都是 peer review 之后才有可能发表的。 写过 paper 的硕士博士都有一个体会， 接到的各种 review 意见有时候令你非常的痛苦， 然而无论是你拒绝了 reviewer 的意见，还是接受意见修改了文章， 你都会经历一个认真思考的过程去提升你文章的质量。 高质量的论文是科研人员严格 review 出来的， 我相信高质量的 code 也可以通过工程师的 review 产生。 把CodeReview 打造成团队的习惯，而我们逐渐会发现这种习惯所释放出来的正能量。 希望有一天, 我们也能做到：

At Tencent, no code, for any product, for any project, gets checked in until it gets a positive review。

来源： [[http://mp.weixin.qq.com/s?__biz=MjM5NzA1MTcyMA==](http://mp.weixin.qq.com/s?__biz=MjM5NzA1MTcyMA==&mid=205557687&idx=3&sn=627a9e51fb0bd53d039a74593c645263&scene=2&from=timeline&isappinstalled=0#rd)​[mid=205557687](http://mp.weixin.qq.com/s?__biz=MjM5NzA1MTcyMA==&mid=205557687&idx=3&sn=627a9e51fb0bd53d039a74593c645263&scene=2&from=timeline&isappinstalled=0#rd)​[idx=3](http://mp.weixin.qq.com/s?__biz=MjM5NzA1MTcyMA==&mid=205557687&idx=3&sn=627a9e51fb0bd53d039a74593c645263&scene=2&from=timeline&isappinstalled=0#rd)​[sn=627a9e51fb0bd53d039a74593c645263](http://mp.weixin.qq.com/s?__biz=MjM5NzA1MTcyMA==&mid=205557687&idx=3&sn=627a9e51fb0bd53d039a74593c645263&scene=2&from=timeline&isappinstalled=0#rd)​[scene=2](http://mp.weixin.qq.com/s?__biz=MjM5NzA1MTcyMA==&mid=205557687&idx=3&sn=627a9e51fb0bd53d039a74593c645263&scene=2&from=timeline&isappinstalled=0#rd)​[from=timeline](http://mp.weixin.qq.com/s?__biz=MjM5NzA1MTcyMA==&mid=205557687&idx=3&sn=627a9e51fb0bd53d039a74593c645263&scene=2&from=timeline&isappinstalled=0#rd)​[isappinstalled=0#rd](http://mp.weixin.qq.com/s?__biz=MjM5NzA1MTcyMA==&mid=205557687&idx=3&sn=627a9e51fb0bd53d039a74593c645263&scene=2&from=timeline&isappinstalled=0#rd)](%5Bhttp://mp.weixin.qq.com/s?__biz=MjM5NzA1MTcyMA==&amp;mid=205557687&amp;idx=3&amp;sn=627a9e51fb0bd53d039a74593c645263&amp;scene=2&amp;from=timeline&amp;isappinstalled=0#rd%5D(http://mp.weixin.qq.com/s?__biz=MjM5NzA1MTcyMA==&mid=205557687&idx=3&sn=627a9e51fb0bd53d039a74593c645263&scene=2&from=timeline&isappinstalled=0#rd))
