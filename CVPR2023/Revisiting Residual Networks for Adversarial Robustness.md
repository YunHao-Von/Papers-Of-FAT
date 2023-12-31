# Revisiting Residual Networks for Adversarial Robustness


本文的主要目标是通过(i)系统地研究架构组件对对抗鲁棒性的贡献，(ii)确定有助于对抗鲁棒性的关键设计选择，以及(iii)最终构建一个新的对抗鲁棒性网络，该网络可以作为研究对抗鲁棒性的基线和测试平台，来弥合这一知识差距。我们采用实证方法，并进行大量精心设计的实验来实现这一目标。  

本文提出的新看法:  
+ 与标准ERM训练中使用的后激活相比，在卷积层之前放置激活函数(即预激活)通常更有利于对抗性训练。有时，它可以严重影响块结构，如wrn中使用的基本块。  
+ 瓶颈块提高了wrn中使用的事实基本块的对抗鲁棒性。此外，在标准ERM训练下得到的聚合卷积和分层卷积在对抗训练下都有改进。  
+ 直接应用SE[16]会降低对抗鲁棒性。请注意，这与标准的ERM训练不同，在标准的ERM训练中，当SE被合并到残差网络中时，会持续提高大多数视觉任务的性能。  
+ 平滑激活函数的性能严重依赖于对抗性训练(AT)设置和数据集。特别是，从权值衰减中去除BN仿射参数对于AT下平滑激活函数的有效性至关重要。  
+ 在相同的FLOPs下，深度和狭窄的剩余网络相对于广度和浅层网络具有更强的鲁棒性。具体来说，深度和宽度的最佳比例是7:3。  
+ 总之，架构设计对对抗鲁棒性有重要贡献，特别是块拓扑和网络缩放因子。  

本文的贡献：  
+ 我们提出了一种简单而有效的SE变体，称为个体SE，用于对抗训练。从经验上讲，我们证明了它会导致跨多个数据集、攻击和模型能力的残余网络的对抗性鲁棒性的一致改进。  
+ 我们提出了RobustResBlock，这是一种用于对抗鲁棒性的新型剩余块拓扑。它在多个数据集、攻击和模型容量上始终优于 WRN 中事实上的基本块约 3% 的稳健准确度。  
+ 我们提出了 RobustScaling，这是第一个复合缩放规则，以有效地缩放网络深度和宽度以实现对抗鲁棒性。从技术上讲，RobustScaling 可以扩展任何架构。在实验上，我们证明了RobustScaling在缩放WRN方面非常有效，其中缩放模型在鲁棒精度上产生了一致的改进，同时在标准WRN上的可学习参数方面提高了约2倍。  
+ 我们提出了一种新的残差网络家族，称为RobustResNets，实现了最先进的AutoAttack[5]鲁棒准确率为61.1%，没有生成或外部数据，500K外部数据为63.7%，在参数方面更紧凑2倍。  

Cazenavette 等人。表明残差连接显着有助于对抗鲁棒性 [3]； (2) Xie 等人。表明平滑激活函数导致 ImageNet [36] 上的对抗鲁棒性更好，Pang 等人的观察结果类似。在 ResNet-18 [21] 的 CIFAR-10 上； (3) Dai 等人。确定参数化激活函数具有更好的鲁棒性属性[6]。然而，这些研究都没有验证它们在不同模型容量和数据集上的相应观察结果。  

我们认为对于对抗鲁棒性，预激活优于后激活。  
基本块在低模型容量区域更有效，而瓶颈块在高模型容量区域更有效。最后，由于倒置瓶颈在任何模型容量下都不会优于其他两个块，因此我们不会进一步考虑它。附录中提供了其他结果。   

SE添加到残余网络中时，SE始终如一地提高了大多数视觉任务的性能。我们假设这可能是由于SE层过度抑制或放大通道。因此，我们提出了SE的另一种变体，称为残差SE，用于对抗鲁棒性。图5c对比了BAT下有剩余SE和没有剩余SE的基本区块和瓶颈区块。结果表明，我们的残差SE一致地提高了两个块在不同模型容量区域的对抗鲁棒性。  

我们分解了每个识别的拓扑增强的贡献，即预激活，聚合和分层卷积，以及残差SE表1。我们演示了所有这些增强都可以自然地集成到瓶颈拓扑中。根据经验，我们的最终拓扑在基线AT下比wrn中使用的基本块提高了~ 3%。   

