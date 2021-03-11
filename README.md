# simCLR
This is the tnesorflow implementation of the famous paper, "A Simple Framework for Contrastive Learning of Visual Representations", published in ICML'20 [link](https://arxiv.org/pdf/2002.05709.pdf).

## link
1. https://medium.com/@nainaakash012/simclr-contrastive-learning-of-visual-representations-52ecf1ac11fa
2. https://towardsdatascience.com/understanding-contrastive-learning-d5b19fd96607
3. https://www.quora.com/What-is-the-temperature-parameter-in-deep-learning
4. https://medium.com/@majid.ghafouri/why-should-we-use-temperature-in-softmax-3709f4e0161

## Key Takeways
1. SimCLR learns representationsby maximizing agreement between differently augmentedviews of the same data example via a contrastive loss inthe latent space.

## Pipeline
1. A stochasticdata augmentationmodule that transformsany given data example randomly resulting in two cor-related views of the same example. In this work, wesequentially apply three simple augmentations:randomcroppingfollowed by resize back to the original size,ran-dom color distortions, andrandom Gaussian blur. the combination of random crop and color distortion is crucial to achieve a good performance
2. A neural networkbase encoderf(·)that extracts repre-sentation vectors from augmented data example. ResNet-50 as the base encoder net-work a 2-layer MLP projection head, NT-Xent, optimized using LARS with learningrate of 4.8 (= 0.3×BatchSize/256) and weight decay of10−6.  We train at batch size 4096 for 100 epochs.3Fur-thermore,  we use linear warmup for the first 10 epochs,and decay the learning rate with the cosine decay schedulewithout restarts
3. A small neural networkprojection headg(·)that mapsrepresentations to the space where contrastive loss isapplied. We use a MLP with one hidden layer to obtainzi=g(hi) =W(2)σ(W(1)hi)whereσis a ReLU non-linearity. As shown in section 4, we find it beneficial todefine the contrastive loss onzi’s rather thanhi’s
4. Acontrastive loss functiondefined for a contrastive pre-diction task.

## Note
1. We do not sample negative examples explicitly.  Instead,given a positive pair, similar to (Chen et al., 2017), we treatthe other2(N−1)augmented examples within a minibatchas negative example
2. There-fore, it is critical to compose cropping with color distortionin order to learn generalizable features.
3. Thus, our experiments show that unsu-pervised contrastive learning benefits from stronger (color)data augmentation than supervised learning
4. unsupervised learning benefitsmore from bigger models than its supervised counterpart
5. we observe that anonlinear projection is better than a linear projection (+3%),and much better than no projection (>10%).
6. Furthermore, even when nonlinearprojection is used, the layer before the projection head,h,is still much better (>10%) than the layer after,z=g(h),which shows thatthe hidden layer before the projectionhead is a better representation than the layer after.
7. Without L2 normalization, the contrastivetask accuracy is higher, but the resulting representation isworse under linear evaluation
8. We find that, whenthe number of training epochs is small (e.g.  100 epochs),larger  batch  sizes  have  a  significant  advantage  over  thesmaller ones.  With more training steps/epochs, the gapsbetween different batch sizes decrease or disappear, pro-vided the batches are randomly resampled.
9. 
