# Motivation
+ 预测3D HOI中
	+ 缺少和动态物体全面的全身交互
		+ floating contact and penetration. 这种artifacts在生成长过程的动作序列会累计错误
		+ •对于3d meshed预测全身运动：确保符合物理规律（先前有1.事后注入物理约束2.建立物理simulator  都需要建立物理simulation environment，而数据集中缺少这种特征
方法：
+ interaction dffusion
+ interaction correction 注入物理先验 (缺少的话，易造成floating contact and penetration ，解决：事后优化；构建physics simulation 进行HOI 合成)
二者可分开train
aim:
model and forecast 3D mesh-based whole-body movements and object dynamics simultaneously

key observation:
物体与人交互接触点遵循简单确定的模式 （overall interaction复杂）



## Method
### Problem
$x = [x^1,\cdots,x^{H+F}]$: H history N future frames
![[Pasted image 20240421100016.png]]
condition c: Given object shape information c
diffusion : 预测$x_0$而不是噪音

### Interaction difffusion model
 之前human motion diffusion : 过去的motion融入每次需要预测的x
 本工作：将过去的x作为条件信息更好。和object shape 一起经过encoder
 预测clean signal $\tilde x$ 而不是 $\epsilon$  
 
![[Pasted image 20240421100622.png]]
## Interaction correction

diffusion中间步骤，进行correction
motivation: diffusion 产生中间$\tilde x$ , 可以将合理的动态融入后产生 无缝衔接的结果
基于inductive bias: 尽管人和物体运动复杂 ，但在合适的参考系下是简单的

### Correction schedule

 在late iteration时进行早，因早期产生噪声太多

 mesh: 对未来的frame：有渗透或无接触时：判断需要修正：
![[Pasted image 20240506160524.png]]
![[Pasted image 20240506160536.png]]

skeletal: ill defined


修正：
STGNN预测： ![[Pasted image 20240506164714.png]]
对于人与物体的M个markers和一个世界坐标系 $D_o$是物体状态$o_i$的features数量（注意物体本身形状是c 通过条件注入，知道features列如中心点位置 旋转角度等能够获得物体点云）成为一个图
# 补
## HOI task:
Generating whole-body interactions, such as approaching and manipulating static [47, 82, 99, 120], articulated [48, 106], and dynamic objects [21] has also been a growing topic




diffuison应用于：
skeleton-based  mesh-based 


Object pose state oi has Do features, including e.g., the position of the center, and the rotation of the object w.r.t. the template
Given the past object motion and the trajectories of human markers/joints in both past and future, we now predict future object motions under different references. We first apply coordinate transformations to the past object motion (which is by default under the ground reference system) and obtain relative motions with respect to all markers/joints. Then we formulate the object motions, either under the ground reference system or relative to each marker/joint, collectively as a spatial-temporal graph G1:H . For example, given |M| markers, we define G1:H ∈ RH×(1+|M|)×Do , where Do is the number of features for object poses as defined previously. Here, 1 + |M| correspond to 1 ground reference system and |M| marker-based reference systems. Given the spatial-temporal graph G1:H , we use a spatialtemporal graph neural network (STGNN) [105] to process the past motion graph G1:H and obtain GH:H+F that represents future object motions in these systems. Then, we acquire a specific object relative motion GH:H+F [s + 1], under the reference system s specified in Sec. 3.2.1. We transform this predicted reference motion back to the ground system. The resulting object motion is defined as ˆ x = P( ̃ x, s), where the interaction predictor P performs the above operations. We blend the original denoised HOI  ̃ x with this newly obtained HOI ˆ x, denoted as  ̃ x× t T +ˆ x × (1 − t T ).