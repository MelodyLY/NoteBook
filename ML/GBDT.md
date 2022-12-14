[TOC]

# 稀疏性问题

假设有1w 个样本， y类别0和1，100维特征，其中10个样本都是类别1，而特征 f1的值为0，1，且刚好这10个样本的f1特征值都为1，其余9990样本都为0(在高维稀疏的情况下这种情况很常见)，我们都知道这种情况在树模型的时候，很容易优化出含一个使用f1为分裂节点的树直接将数据划分的很好，但是当测试的时候，却会发现效果很差，因为这个特征只是刚好偶然间跟y拟合到了这个规律，这也是我们常说的过拟合。
但是当时我还是不太懂为什么线性模型就能对这种 case 处理的好？照理说线性模型在优化之后不也会产生这样一个式子
y = W1*f1 + Wi*fi+….
其中：W1特别大以拟合这十个样本吗，因为反正f1的值只有0和1，W1过大对其他9990样本不会有任何影响。后来思考后发现原因是因为现在的模型普遍都会带着正则项，而lr等线性模型的正则项是对权重的惩罚，也就是 W1一旦过大，惩罚就会很大，进一步压缩 W1的值，使他不至于过大，而树模型则不一样，树模型的惩罚项通常为叶子节点数和深度等，而我们都知道，对于上面这种 case，树只需要一个节点就可以完美分割9990和10个样本，惩罚项极其之小。这也就是为什么在高维稀疏特征的时候，线性模型会比非线性模型好的原因了：带正则化的线性模型比较不容易对稀疏特征过拟合。
