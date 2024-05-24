policy maps observation $o$ to $a$
x

observation: 类比于渲染出来的画面 
state: 类比于计算机内存状态




if given $s_2$, $s_{3}$is conditionally independent of $s_{1}$- > markov property: state: 预测未来的现在状态的信息



goal: given observation, learn policy 


behavior clone: using supervised learning 

![[Pasted image 20240522143612.png]]

可能会有较大偏差（在传统supervised learning中不会发生 因为是iid） 你在此时刻的选择有一点点改动会影响下一时刻的action


WHY behavior cloning fail?-math


