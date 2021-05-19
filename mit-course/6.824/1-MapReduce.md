- [MapReduce](#mapreduce)
# MapReduce
在MapReduce最经典的[论文](http://static.googleusercontent.com/media/research.google.com/zh-CN//archive/mapreduce-osdi04.pdf)中，有MapReduce的完整说明，详细可以参见该链接。<br>为了学习MapReduce以及完成官方的[Lab1实验](https://pdos.csail.mit.edu/6.824/labs/lab-mr.html)，我们还需要对MapReduce中的其他内容进行学习，例如 **golang中的RPC协议**

为了一步一步的梳理整个子单元的内容，本笔记以记录学习思考过程为主，逐步加深该知识点的印象。<br>本笔记的叙述流程如下：
1. MapReduce的基本概念是什么？
2. 将最经典的MapReduce例子的工作流程拆解一遍
3. MapReduce中的Coordinator和Worker
4. RPC的基本定义
5. 开始拆解Lab1代码
6. 实现Worker、RPC和Coordinator
7. 继续完善：MapReduce需要额外注意哪几个指标？
8. 完善代码，全部测试一遍