
![[Pasted image 20240312135359.png]]
这是hinge loss
In summary, the SVM loss function wants the score of the correct class $yi$to be larger than the incorrect class scores by at least by $\Delta$
The most appealing property is that penalizing large weights tends to improve generalization, because it means that no input dimension can have a very large influence on the scores all by itself.
Since the L2 penalty prefers smaller and more diffuse weight vectors, the final classifier is encouraged to take into account all input dimensions to small amounts rather than a few input dimensions and very strongly.
this effect can improve the generalization performance of the classifiers on test images and lead to less _overfitting_.