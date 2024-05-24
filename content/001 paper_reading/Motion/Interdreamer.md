t2m局限：lack of large-scale interaction data and comprehensive descriptions that align with these interactions.
本工作：
1. 通过解耦交互的semantics和dynamics,不直接在text-interaction pair data上训练.
	1. llm 给出high level control
	2. world model理解物理行为：人actions->物体运动
motivation:
解耦交互的semantics和dynamics:
+ 交互的high-level semantics : 由human motion和innitial object pose得到
+ low-level dybnamics:(object接下来的行为) 由人 + 物理约束得到


对于interaction semantics: 可从文本获得先验，独立于text-interaction pair dataset
+ llms 用他的in-context-learning (few0shot prompting和chain-of-thought prompting)
	+ 分解步骤
	+ an initial object pose(利用retrieval-based 算法？？？)
+ low-level:
	+ world model:  给交互，预测object接下来额度state；对有contact部分施加约束（唯一需要train

问题：
以往whole-body: supervised learning方法，受限于data

Method:
1. M = T x m motion分割为T segments，m frames.
	1. object motion $\{o_i\}_{i=1}^M$ -> environmental states $\{s_t\}_{t=1}^T$ 
	2. human motion $\{h_i\}_{i=1}^M$  -> actions $\{a_t\}_{t=1}^T$ 


## High-level
![[Pasted image 20240506174903.png]]



## Low level
t2m:
?? $\pi$ 可以用motion diffusion ?咋做的 不是接受上一个action预测下一个吗 

retrieval-based:
$s_1 \sim R(a_1,g)$
对dataset中， key是接触到的（身体部位，物体类别） value是（身体接触点集，物体接触点集）
gpt给出(身体部位，物体类别)，sample出value,再根据action $a_1$ 确定物体状态$s_1$

## world model


补：
t2m其他task:
+ text-conditioned generation of multiple-person [24, 57, 105] and human-scene interaction [17, 33, 37]


Q: 为什么需要额外optimize  而不是world model加入物理约束loss  节约train成本？
+ 