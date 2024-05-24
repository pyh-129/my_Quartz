
t2m:
以往的方法： text->映射到code：生成motions:问题：
1. 难以fine-grained control（没有完全结合spatial 和temporal信息；以往有工作用llm 给出补充的信息，但缺少监督很难对齐；有加入时序考虑但忽略了low-level时序信息（TEACH），）generator必须解耦出code中的body parts表示
2. motion edit: 


本方法：可以instructions,细微差别可控
每个body part生成对应code






## Method

![[Pasted image 20240506193454.png]]


## Encoder-decoder
 预定义好一个codebook：每个code $c_n$ 表示一个语义：（身体部位）"left hand and left foot close" 被分为K 个类
 Encoder: $Z = \mathcal{E}(X) = \{z_i\}_{i=1}^L$  $z_i$ 是k hot N维向量（也就是每个种类只有一个1）
 parser P 根据given pose code 评估是否达到某个threshold,不用训练encoder model 直接分解：$Z = \mathcal{E}(X) = \{\{\mathcal{P}(c_n,x_{i\times l})\}_{n=1}^N \}$
 相加得 latent features, decoder解码为motion
![[Pasted image 20240506194933.png]]


## generator
$z_i^n$ 独立伯努利随机变量
 变为多标签问题：
 最大化

## Editor



new-task:
文中说：dialog-based motion generation.

Q:
1. 没法保证k-hot预测仅出现一次？
想法： 
1. codebook限制太大 
2. llm的 part没有和身体部位很好结合？可以用在parco
3. instruction : 参考坐标系？