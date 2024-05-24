unsupervised
c

# 对比学习
例：类似图片在特征空间尽量拉近
自监督训练： 视觉领域，通过设计一些巧妙代理任务（pretext task),人为订立规则，用来定义那些图片是相似的哪些不相似，提供监督信号去训练模型。例如：instance discrimination:每张图片自成一类，其他都和这个不相似
![[Pasted image 20240313095219.png]]

动量：加权移动平均：$y_t = m*y_{t-1}+(1-m)x_t$,m很小表示当前输出更多依赖当前输入。Moco利用这种特性缓慢更新编码器，让中间学习的字典的特征尽可能保持一致。

why 相比nlp,cv之前的无监督训练差一些？
+ nlp 语言离散化表示，很方便有tokenize字典(将一个单词对应某个特征)，基于字典无监督很容易展开（能简单将字典key看作一个类别，当作监督信号）
+ 视觉是一个连续的，在高维空间的信号，（not structure for human communications)


![[Pasted image 20240313100207.png]]
positive  negative都通过一个encoder $E_1^2$，然后对比学习希望f11 f12尽可能相近。Moco认为之前对比学习都是在构建一个动态的字典（下面的f12,f2---fN是动态字典）
![[Pasted image 20240313100426.png]]

好的字典：1）大（更好从连续高维视觉空间抽样）2）一致性（key应该用相同或相似的编码器得到， 保持对比尽可能一致）
Moco:
![[Pasted image 20240313100741.png]]
和之前不同：queue & momentum encoder
queue: 字典太大显卡内存吃不消，希望和batch size剥离开：现在minibatch特征入队，最早的mini-batch移出队列，
momentum encoder: 希望一致性。如果队列则当前batch是从当前时刻encoder编码，其他key是不同时刻编码，不一致？->动量设置较大，保证缓慢更新
![[Pasted image 20240313101249.png]]


pretext task:
instance discrimination:
一个图片不同view ,视为query key match
问题：1. 数据集扩大1000倍，提升很小，大规模数据集没有利用起来？


## Related:
Loss:
1. 生成式：（autoencoders) L1/L2 loss 判别式（eight positions,九宫格，例如将5给你，剩下随机挑选一个能否预测出他的位置
2. contrastive：之前的目标是固定的，对比学习中是目标在训练时不停改变，由encoder抽取的特征决定（也就是字典）
3. Adversarial: 主要衡量的是两个概率分布之间的差异->无监督数据生成，还有做representation learning (如果能生成图片按理说学到了底层分布)
pretext tasks:
denoising auto-encoder:重建整张图 context auto-encoders:重建整个patch, colorization给图片上色当自监督信号， pseudo labels
 CPC: 预测性的对比学习，用上下文信息预测未来 CMC : 一个物体不同视角做对比（和colorization（黑白 彩色）很像）


