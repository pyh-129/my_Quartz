一般3d human mode()

六个身体部分->六个VQVAE,一个总的Transformer生成，期间能和每个部分有信息交互

六个部分每个训练一个vae
![[Pasted image 20240509182848.png]]

每个transformer生成，引入part coodination模块进行交互
![[Pasted image 20240509182823.png]]

part coodination

![[Pasted image 20240509183048.png]]

![[Pasted image 20240509183139.png]]