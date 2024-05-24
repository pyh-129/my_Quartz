## Abs
1. 2 ways for pretrained lm to down-stream tasks:
	1. ElMo: 基于特征 对每一个下游任务构造一个跟这个任务相关的神经网络。预训练好的表示（额外的特征）和预训练一起输入，作为特征表达
	2. GPT： fine-tuning 下游，权重微调
	二者单向
2. 贡献
	1. 双向
	2. 微调


## related
unsupervised fine-tuning -> GPT

unlabeled  句子对->训练好bert模型，对每一个下游任务训练，权重初始化来源于训练好的模型。labeled数据

## Method

1. 可学习参数
	1. 输入是字典大小 输出（hidden size) H![[Pasted image 20231112162118.png]]
	2. ![[Pasted image 20231112162147.png]]输入30K,embedding后是H，self-attention 头（H*（H/h)\*3 一共有h个头
2. embedding
	1.  片段 子序列
	2. CLS 开头 表示句子的信息
	3. 区分每个句子：
		1. 在每个句子后面放一个词SEP
		2. 学一个嵌入层 学习是第一个句子还是第二个
	4. ![[Pasted image 20231114090807.png]]


## 预训练和微调不一样
1.train 替换为mask
2. 不能做seq2seq 的生成式（机器翻译，文本摘要）做分类很好做



![[Pasted image 20231114093124.png]]即使不同token 有相同type,每一个token有一个embedding-> contextualized word embedding
![[Pasted image 20231114093340.png]]

### ELMO
预测下一个token ->contextualized word embedding
![[Pasted image 20231114093727.png]]

训练一个反向

根据不同任务学出不同的weight 
![[Pasted image 20231114094056.png]]

### Bert
>作为character输入 更合适（字典大小固定

![[Pasted image 20231114094602.png]]

cls经过classifier之后  输出两个句子是否接在一起(CLS看到整个句子的信息)
![[Pasted image 20231114094742.png]]

同时使用qpproach 1 & 2

Linear从头学，Bert微调：
![[Pasted image 20231114095110.png]]![[Pasted image 20231114095233.png]]
![[Pasted image 20231114095247.png]]
QA: 输出答案在文章中的位置

![[Pasted image 20231114095503.png]]![[Pasted image 20231114095717.png]]
output 矛盾->无解

### GPT
![[Pasted image 20231114101527.png]]

![[Pasted image 20231114101547.png]]

