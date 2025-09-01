---
title: 跟同济子豪兄一起学MCP · 研习社
date: '2025-05-31 01:04:42'
updated: '2025-05-31 01:06:23'
---
<font style="color:rgb(206, 206, 206);">同济子豪兄良心整理的MCP大模型协议知识库，包含基本原理、商业和学术机会，保姆级配置及使用教程，还有几个有趣案例</font>

**<font style="color:rgb(49, 106, 191);">MCP：大模型接入万物的通用协议，智能体时代的HTTP！</font>**

<font style="color:rgb(206, 206, 206);">所有智能体都值得用MCP重新做一遍！</font>

<font style="color:rgb(206, 206, 206);">但，当你不看教程be like</font>

![]()

## <font style="color:rgb(206, 206, 206);">MCP是什么？</font>
## <font style="color:rgb(49, 106, 191);">大语言模型 VS 智能体Agent？</font>
<font style="color:rgb(206, 206, 206);">大语言模型，例如DeepSeek，如果不能联网、不能操作外部工具，只能是聊天机器人。除了聊天没什么可做的。</font>

<font style="color:rgb(206, 206, 206);">而一旦大语言模型能操作工具，例如：联网/地图/查天气/函数/插件/API接口/代码解释器/机械臂/灵巧手，它就升级成为智能体Agent，能更好地帮助人类。今年爆火的Manus就是这样的智能体。</font>

![]()

<font style="color:rgb(206, 206, 206);">众多大佬、创业公司，都在All In押注AI智能体赛道。</font>

<font style="color:rgb(206, 206, 206);">也有不少爆款的智能体产品，比如Coze、Manus、Dify。</font>

![]()

## <font style="color:rgb(49, 106, 191);">以前的智能体是怎么实现的？</font>
<font style="color:rgb(206, 206, 206);">在以前，如果想让大模型调用外部工具，需要通过写大段提示词的方法，实现“Function Call”。</font>

<font style="color:rgb(206, 206, 206);">我需要给AI写一大段提示词，包括有哪些动作函数、大模型输出格式、限制，以及大量案例。大模型才会按照我要求的格式回复，进而才能解析出编排出的动作列表。</font>

<font style="color:rgb(206, 206, 206);">提示词如下：</font>

## <font style="color:rgb(49, 106, 191);">靠大段提示词的方法实现的Function Call有什么问题？对开发者（你）来说：</font>
<font style="color:rgb(206, 206, 206);">●</font>

**<font style="color:rgb(191, 53, 53);">要写一大段复杂提示词</font>**<font style="color:rgb(206, 206, 206);">，程序员的语文水平一般都比较捉急</font>

<font style="color:rgb(206, 206, 206);">●</font>

<font style="color:rgb(206, 206, 206);">面对相同的函数和工具，每个开发者都需要</font>**<font style="color:rgb(191, 53, 53);">重新从头造轮子</font>**<font style="color:rgb(206, 206, 206);">，按照自己想要的模型回复格式重新撰写、调试提示词 </font>

<font style="color:rgb(206, 206, 206);">各家厂商训练出的智能体大模型，任务编排能力参差不齐，标准不一致。</font>

<font style="color:rgb(206, 206, 206);">每个软件都要定制开发不同的大模型调用模板。</font>

## <font style="color:rgb(49, 106, 191);">秦王扫六合：MCP协议</font>
![]()

<font style="color:rgb(206, 206, 206);">Anthropic公司（就是发布Claude大模型的公司），在2024年11月，发布了Model Context Protocol协议，简称MCP。</font>

<font style="color:rgb(206, 206, 206);">MCP协议就像Type-C扩展坞，</font>**<font style="color:rgb(191, 53, 53);">让海量的软件和工具，能够插在大语言模型上，供大模型调用。</font>**

<font style="color:rgb(206, 206, 206);">MCP协议是连接【大模型（客户端）】和【各种工具应用（服务端）】的统一接口。</font>

![]()

## <font style="color:rgb(49, 106, 191);">几个MCP的应用案例</font>
<font style="color:rgb(206, 206, 206);">●</font>

<font style="color:rgb(206, 206, 206);">调用Playwright的MCP接口，让AI自己操作网页。（后面的保姆级教程讲的就是这个）</font>

<font style="color:rgb(206, 206, 206);">只要“扩展坞”上插的“工具”够多，每个人都能几分钟，搭积木手搓出，类似Manus的智能体</font>

## <font style="color:rgb(49, 106, 191);">MCP解决的核心问题：统一了大模型调用工具的方法</font>
<font style="color:rgb(206, 206, 206);">MCP为【大模型】与【外部数据和工具】的【无缝集成】提供了标准化协议和平台。</font>

**<font style="color:rgb(191, 53, 53);">不需要用户写提示词。</font>**

**<font style="color:rgb(191, 53, 53);">极大降低了大模型调用外部海量工具、软件、接口的难度。</font>**

![]()

<font style="color:rgb(206, 206, 206);">Unity和百度地图，看上去截然不同的软件，但都可以让大模型按照相同的协议去调用各自的功能。AI一眼就知道有哪些工具，每个工具是什么含义。</font>

<font style="color:rgb(206, 206, 206);">点点鼠标，就可以把同一个大模型，挂载到不同的软件和工具上。</font>

![]()

<font style="color:rgb(206, 206, 206);">在上图中，上方代表MCP客户端软件，比如Cusor、Claude Desktop，下方代表MCP服务端，比如海量的软件和API接口。</font>

## <font style="color:rgb(49, 106, 191);">用HTTP协议做类比</font>
<font style="color:rgb(206, 206, 206);">MCP客户端软件（例如Cursor）就相当于网页浏览器。</font>

<font style="color:rgb(206, 206, 206);">不同的浏览器，用相同的HTTP协议，就可以访问海量的网站。</font>

<font style="color:rgb(206, 206, 206);">不同的大模型，用相同的MCP协议，就可以调用海量的外部工具。</font>

<font style="color:rgb(206, 206, 206);">互联网催生出搜索、社交、外卖、打车、导航、外卖等无数巨头。</font>

<font style="color:rgb(206, 206, 206);">MCP同样可能催生出繁荣的智能体生态。</font>

**<font style="color:rgb(191, 53, 53);">类比互联网的HTTP协议，所有的智能体都值得用MCP重新做一遍。</font>**

![]()

<font style="color:rgb(206, 206, 206);">参考资料</font>

<font style="color:rgb(206, 206, 206);">几张图片来自公众号：西二旗生活指北</font>

## <font style="color:rgb(206, 206, 206);">MCP带来哪些新机会</font>
## <font style="color:rgb(49, 106, 191);">1、 MCP带来的商业影响和机会</font>
### <font style="color:rgb(206, 206, 206);">最直接的影响</font>
**<font style="color:rgb(49, 106, 191);">通过搭积木、调接口，每个人都能几分钟手搓出类似Manus的应用（绝对不是危言耸听）</font>**

### <font style="color:rgb(206, 206, 206);">商业机会</font>
**<font style="color:rgb(191, 53, 53);">不需要用户写提示词。</font>**

**<font style="color:rgb(191, 53, 53);">极大降低了大模型调用外部海量工具、软件、接口的难度和门槛。</font>**

**<font style="color:rgb(191, 53, 53);">构建出真正的智能体生态。</font>**

<font style="color:rgb(206, 206, 206);">MCP市场将成为新的应用分发渠道</font>

<font style="color:rgb(206, 206, 206);">整合各种MCP服务的提供商（Zapier、mcp.SO、千帆AppBuilder） 将涌现</font>

<font style="color:rgb(206, 206, 206);">未来所有人机交互应用（地图、外卖、聊天、办公、剪辑、建模、编程），全都会发布自己的MCP服务。</font>

### <font style="color:rgb(206, 206, 206);">有的在升起，有的在坠落</font>
<font style="color:rgb(206, 206, 206);">有些大语言模型对MCP的支持更好（比如Claude、OpenAl），但有些支持得不够好（比如绝大多数国产大模型，包括特别有名的那几个）</font>

<font style="color:rgb(206, 206, 206);">DeepSeek V3对MCP的支持非常好。</font>

<font style="color:rgb(206, 206, 206);">有些IDE对MCP的支持更好（比如Cursor），但有些支持得不够好（不点名了）</font>

<font style="color:rgb(206, 206, 206);">因为MCP，我现在更愿意用Claude和Cursor，而不是国产AI编程插件</font>

<font style="color:rgb(206, 206, 206);">Coze和Dity这类"智能体“平台，插件生态是较为封闭的，客观上自己重复造轮子，而且插件数量有限，肯定无法和MCP生态相比。</font>

## <font style="color:rgb(49, 106, 191);">2、 MCP带来的科研学术机会</font>
<font style="color:rgb(206, 206, 206);">对于在校硕博来说，用MCP可以发不少论文。</font>

<font style="color:rgb(206, 206, 206);">下面是同济子豪兄列举的一些能想到的科研创新点。</font>

### <font style="color:rgb(206, 206, 206);">模型</font>
<font style="color:rgb(206, 206, 206);">如何训练出专门用来做MCP的大语言模型和端侧大模型（用于智能家居）</font>

### <font style="color:rgb(206, 206, 206);">改进MCP协议本身</font>
<font style="color:rgb(206, 206, 206);">如何节省MCP消耗的token</font>

<font style="color:rgb(206, 206, 206);">安全性：MCP的鉴权、漏洞注入、信息泄露、用户隐私、数据脱敏、防止DDos攻击</font>

<font style="color:rgb(206, 206, 206);">是否需要像HTTP一样多次握手</font>

<font style="color:rgb(206, 206, 206);">是否需要像HTTPS一样，引入密码学和加密技术，让通信更安全</font>

<font style="color:rgb(206, 206, 206);">目前的MCP是一对一，能否一对多或多对多</font>

<font style="color:rgb(206, 206, 206);">是否需要引入数据压缩技术</font>

### <font style="color:rgb(206, 206, 206);">各个学科的MCP客户端软件及应用</font>
<font style="color:rgb(206, 206, 206);">MCP为大模型与外部数据源和工具的无缝集成提供了标准化协议和平台</font>

<font style="color:rgb(206, 206, 206);">特别是对于AI4S方向，可能有巨大机会：自动化实验设计、多模态数据融合</font>

<font style="color:rgb(206, 206, 206);">医学、经济学、生物信息学、数学、物理学、科研学术助手、文科</font>

<font style="color:rgb(206, 206, 206);">例如：生物学中的基因序列（文本）、细胞图像（图像）、蛋白质结构（三维模型）</font>

### <font style="color:rgb(206, 206, 206);">物理AI和具身智能</font>
<font style="color:rgb(206, 206, 206);">具身智能机器人的MCP（运动控制、强化学习、大脑小脑、复合机器人）</font>

<font style="color:rgb(206, 206, 206);">现有的具身智能更多是强化学习和sim2real，而非大语言模型，MCP可能是把大模型能真正用在具身智能的起点</font>

### <font style="color:rgb(206, 206, 206);">人机交互</font>
<font style="color:rgb(206, 206, 206);">让智能体操作游戏开发、3D建模、强化学习Agent</font>

<font style="color:rgb(206, 206, 206);">让智能体实现工业控制或鱼缸管理</font>

### <font style="color:rgb(206, 206, 206);">其它的一些idea</font>
<font style="color:rgb(206, 206, 206);">• 基于MCP的测评Benchmark</font>

<font style="color:rgb(206, 206, 206);">• AI自己探索感知，生成某个现有工具的MCP Server文件</font>

<font style="color:rgb(206, 206, 206);">• 多个MCP智能体上下游协作（图生文、文生图、图生视频、视频传B站、点赞评论一条龙）</font>

## <font style="color:rgb(49, 106, 191);">3、 MCP的局限和上限</font>
<font style="color:rgb(206, 206, 206);">MCP有点被自媒体追捧过头了（类似Manus）。</font>

<font style="color:rgb(206, 206, 206);">MCP只是大模型通信协议，使用MCP开发不出新的应用范式，只能降低应用开发难度。</font>

<font style="color:rgb(206, 206, 206);">调用MCP的智能体效果取决于基座大模型，例如Unity游戏开发和Blender三维建模，效果依赖于基座模型对3D世界和交互的理解。</font>

<font style="color:rgb(206, 206, 206);">目前的MCP只是能用，还达不到好用。</font>

<font style="color:rgb(206, 206, 206);">大模型需要支持Function Call或者Tool Use才能使用MCP</font>

<font style="color:rgb(206, 206, 206);">只有某些大模型对MCP的支持比较好（Claude、OpenAI），绝大多数国产大模型，包括特别有名的那几个，对MCP的支持都不够好。</font>

<font style="color:rgb(206, 206, 206);">例如OpenRouter，虽然模型写着支持Function Call和Tool Use，但是这家的接口里面直接告诉你只能有几个模型可以用。</font>

<font style="color:rgb(206, 206, 206);">MCP的开发和使用门槛较高。你自己亲自跑一下后面教程的环境配置和使用就知道了。</font>

<font style="color:rgb(206, 206, 206);">MCP是美国，特别是Anthropic和OpenAl主导的规范。</font>

## <font style="color:rgb(206, 206, 206);">MCP的技术细节（看不懂可跳过）</font>
## <font style="color:rgb(49, 106, 191);">MCP协议的通信双方</font>
**<font style="color:rgb(49, 106, 191);">MCP Host：</font>**<font style="color:rgb(206, 206, 206);">人类电脑上安装的客户端软件，一般是Cursor、Claude Desktop、Cherry Studio、Cline，软件里带了大语言模型，后面的教程会带你安装配置。</font>

**<font style="color:rgb(49, 106, 191);">MCP Server：</font>**<font style="color:rgb(206, 206, 206);">各种软件和工具的MCP接口，比如：</font><font style="color:rgb(206, 206, 206);">百度地图、</font><font style="color:rgb(206, 206, 206);">高德地图、游戏开发软件Unity、三维建模软件Blender、浏览器爬虫软件Playwrights、聊天软件Slack。尽管不同软件有不同的功能，但都是以MCP规范写成的server文件，大模型一眼就知道有哪些工具，每个工具是什么含义。</font>

<font style="color:rgb(206, 206, 206);">有一些MCP Server是可以联网的，比如百度地图、高德地图。而有一些MCP Server只进行本地操作，比如Unity游戏开发、Blender三维建模、Playwright浏览器操作。</font>

![]()

## <font style="color:rgb(49, 106, 191);">MCP的Host、Client、Server是什么关系？</font>
<font style="color:rgb(206, 206, 206);">Host就是Cursor、Cline、CherryStudio等MCP客户端软件。</font>

![]()

<font style="color:rgb(206, 206, 206);">如果你同时配置了多个MCP服务，比如百度地图、Unity、Blender等。每个MCP服务需要对应Host中的一个Client来一对一通信。Client被包含在Host中。</font>

## <font style="color:rgb(49, 106, 191);">大模型是怎么知道有哪些工具可以调用，每个工具是做什么的？</font>
<font style="color:rgb(206, 206, 206);">每个支持MCP的软件，都有一个MCP Server文件，里面列出了所有支持调用的函数，函数注释里的内容是给AI看的，告诉AI这个函数是做什么用的。</font>

<font style="color:rgb(206, 206, 206);">每个以</font>`<font style="color:rgb(206, 206, 206);">@mcp.tool()</font>`<font style="color:rgb(206, 206, 206);">开头的函数，都是一个百度地图支持MCP调用的功能。</font>

![]()![]()

<font style="color:rgb(206, 206, 206);">你也可以按照这个规范，自己开发MCP Server，让你自己的软件支持MCP协议，让AI能调用你软件中的功能。</font>

<font style="color:rgb(206, 206, 206);"></font>

> <font style="color:rgb(206, 206, 206);">来自: </font>[跟同济子豪兄一起学MCP · 研习社](https://modelscope.cn/learn/1121?pid=1114)
>





