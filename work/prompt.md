AI编程使用探索
一、基础
1.要求关键步骤或阶段在控制台输出日志
2.要求给出清晰的中文注释
3.说明有哪些基础类，工具类，

一、一些痛点记录
1.每次生成完了要检查一遍，
2.新增的话，检查的块；修改的话，检查的慢，甚至不想进行修改
3.要避免在一个文件中写了很多杂乱的逻辑，这样的话会导致选择文件token过多，修改的时候，检查的时候都会有更多困难


二、构想
1.告诉AI项目的目录结构以及每个目录的用途，（目录过多不好整），而且AI也要有找文件的功能才行

三、编码基础命令：
把以下内容写入AIREADME， 每次执行任务之前prompt：
“AIREADME文件记录了你完成任务的原则和当前项目详情，阅读并询问我需要执行什么任务“；
● 代码要有注释
● 模块化设计，单一职责原则
● 交流用中文
● 完成任务之后检查是否需要更新AIREADME文件中的项目信息

二、新建一个项目
1.1. 这是一个Android项目，查看目录结构，列出所有代码所在的文件目录结构，并写在README文件中

三、一个没有上下文的项目
1.最好是模块化的，识别相关文件
2.在问问题之前，因为涉及到本地的项目，远程服务器上的名称，所以AI需要知道远程服务器的名称，需要有一个README_AI文件，来说明项目的场外信息：例如我的数据库用户名密码，或者运行在服务器上的名称，或者架构信息，要求AI在新建文件的时候注意代码架构，
●目录结构和模块，模块使用的架构
●项目依赖
●配置文件说明
●数据流说明
●远程服务器情况：
3.Prompt:"了解项目结构，生成一个AIREADME文件，生成目录结构，项目结构，架构，文件用途等信息， 更新在AIREADME文件中"
四、项目维护：
1.我要做什么
2.涉及到哪些文件

