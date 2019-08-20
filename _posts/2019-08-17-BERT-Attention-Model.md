---
layout: post
mathjax: true
title:  "预训练语言模型BERT学习之三 - Attention Model"
date:   2019-08-17 11:40:45 +0800
categories: [AI, NLP]
tags: 
 - AI
 - NLP
 - NER
 - Transformer
 - Attention
---

## 什么是Attention机制？
　　最近两年，注意力模型（Attention Model）被广泛使用在自然语言处理、图像识别及语音识别等各种不同类型的深度学习任务中，是深度学习技术中最值得关注与深入了解的核心技术之一。

　　当我们人在看一样东西的时候，我们当前时刻关注的一定是我们当前正在看的这样东西的某一地方，换句话说，当我们目光移到别处时，注意力随着目光的移动也在转移，这意味着，当人们注意到某个目标或某个场景时，该目标内部以及该场景内每一处空间位置上的注意力分布是不一样的。---------（思考：对于图片，会有些特别显眼的场景会率先吸引住注意力，那是因为脑袋中对这类东西很敏感。对于文本，我们大都是带目的性的去读，顺序查找，顺序读，但是在理解的过程中，我们是根据我们自带的目的去理解，去关注的。 注意力模型应该与具体的目的(或者任务)相结合。）

　　从Attention的作用角度出发，我们就可以从两个角度来分类Attention种类：Spatial Attention 空间注意力(图片)和Temporal Attention 时间注意力(序列)。更具实际的应用，也可以将Attention分为Soft Attention和Hard Attention。Soft Attention是所有的数据都会注意，都会计算出相应的注意力权值，不会设置筛选条件。Hard Attention会在生成注意力权重后筛选掉一部分不符合条件的注意力，让它的注意力权值为0，即可以理解为不再注意这些不符合条件的部分。

## Encoder-Decoder框架
i.e. sequence to sequence
序列到序列模型是一个模型，它采用一个目标序列（单词，字母，图像的特征等）并输出另一个目标序列。 经过训练的模型可以这样工作：
<video width="100%" height="auto" loop autoplay controls>
  <source src="/assets/images/Attention_Model/seq2seq_1.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

在神经网络机器翻译中，序列是一系列单词，一个接一个地处理。 输出同样是一系列的词：
<video width="100%" height="auto" loop autoplay controls>
  <source src="/assets/images/Attention_Model/seq2seq_2.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

序列到序列模型由编码器和解码器组成。编码器处理输入序列中的每个项目，它将捕获的信息编译成向量（称为上下文）。 在处理整个输入序列之后，编码器将上下文发送到解码器，解码器逐项开始产生输出序列。
<video width="100%" height="auto" loop autoplay  controls>
  <source src="/assets/images/Attention_Model/seq2seq_3.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>
这同样适用于机器翻译。
<video width="100%" height="auto" loop autoplay  controls>
  <source src="/assets/images/Attention_Model/seq2seq_4.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

目前绝大多数文献中出现的AM(Attention Model)模型是附着在Encoder-Decoder框架下的。

![](/assets/images/Attention_Model/01.png)
图1 抽象的Encoder-Decoder框架

Encoder-Decoder框架可以这么直观地去理解：可以把它看作适合处理由一个句子（或篇章）生成另外一个句子（或篇章）的通用处理模型。对于句子对<X,Y>。 --------（思考：<X,Y>对很通用，X是一个问句，Y是答案；X是一个句子，Y是抽取的关系三元组；X是汉语句子，Y是汉语句子的英文翻译。等等），我们的目标是给定输入句子X，期待通过Encoder-Decoder框架来生成目标句子Y。X和Y可以是同一种语言，也可以是两种不同的语言。而X和Y分别由各自的单词序列构成：

$$
\begin{equation}
  \mathbf{X} =
  \left(
  \begin{array}{ccc}
          x_{1},x_{2} &
          \cdots &
          x_{m}
  \end{array}
  \right)
\end{equation} 
$$

$$
\begin{equation}
  \mathbf{Y} =
  \left(
  \begin{array}{ccc}
          y_{1},y_{2} &
          \cdots &
          y_{n}
  \end{array}
  \right)
\end{equation} 
$$

Encoder顾名思义就是对输入句子X进行编码，将输入句子通过非线性变换转化为中间语义表示C(C为一个张量或者其他表示)：

$$
\begin{equation}
  \mathcal{C} =
  \mathcal{F}
  \left(
  \begin{array}{ccc}
          x_{1},x_{2} &
          \cdots &
          x_{m}
  \end{array}
  \right)
\end{equation} 
$$

对于解码器Decoder来说，其任务是根据句子X的中间语义表示C和之前已经生成的历史信息 \\( y_1,y_2….y_{i-1} \\) 来生成i时刻要生成的单词 \\( y_i \\) ：

$$
\mathrm{y}_{\mathrm{i}}=\mathcal{G}\left(\mathrm{C}, \mathrm{y}_{1}, \mathrm{y}_{2} \dots \mathrm{y}_{\mathrm{i}-1}\right)
$$

每个\\( y_i \\) 都依次这么产生，那么看起来就是整个系统根据输入句子X生成了目标句子Y。 ------（其实这里的Encoder-Decoder是一个序列到序列的模型seq2seq，这个模型是对顺序有依赖的。）

Encoder-Decoder是个非常通用的计算框架，至于Encoder和Decoder具体使用什么模型都是由研究者自己定的，常见的比如 CNN / RNN / BiRNN / GRU / LSTM / Deep LSTM 等，这里的变化组合非常多。

## Attention Model
以上介绍的Encoder-Decoder模型是没有体现出“注意力模型”的，所以可以把它看作是注意力不集中的分心模型。以下目标句子Y中每个单词的生成过程如下：

$$
\begin{aligned} 
  \mathbf{y}_{1} =\mathbf{f}(\mathbf{C}) \\ \mathbf{y}_{2} =\mathbf{f}\left(\mathbf{C}, \mathbf{y}_{1}\right) \\ \mathbf{y}_{3} =\mathbf{f}\left(\mathbf{C}, \mathbf{y}_{1}, \mathbf{y}_{2}\right) 
\end{aligned}
$$

其中f是decoder的非线性变换函数。从这里可以看出，在生成目标句子的单词时，不论生成哪个单词，是y1,y2也好，还是y3也好，他们使用的句子X的语义编码C都是一样的，没有任何区别。而语义编码C是由句子X的每个单词经过Encoder 编码产生的，这意味着不论是生成哪个单词，y1,y2还是y3，其实**句子X中任意单词对生成某个目标单词yi来说影响力都是相同的**，没有任何区别（其实如果Encoder是RNN的话，理论上越是后输入的单词影响越大，并非等权的，估计这也是为何Google提出Sequence to Sequence模型时发现把输入句子逆序输入做翻译效果会更好的小Trick的原因）。这就是为何说这个模型没有体现出注意力的缘由。

引入AM模型，以翻译一个英语句子举例：输入X：Tom chase Jerry。 理想输出：汤姆追逐杰瑞。

应该在翻译“杰瑞”的时候，体现出英文单词对于翻译当前中文单词不同的影响程度，比如给出类似下面一个概率分布值(权重)：

（Tom,0.3）（Chase,0.2）（Jerry,0.5）

每个英文单词的概率代表了翻译当前单词“杰瑞”时，注意力分配模型分配给不同英文单词的注意力大小(即原始单词X对当前词的翻译的权重大小)。这对于正确翻译目标语单词肯定是有帮助的，因为引入了新的信息。同理，目标句子中的每个单词都应该学会其对应的源语句子中单词的注意力分配概率信息。这意味着在生成每个单词Yi的时候，原先都是相同的中间语义表示C会替换成根据当前生成单词而不断变化的Ci。理解AM模型的关键就是这里，即由固定的中间语义表示C换成了根据当前输出单词来调整成加入注意力模型的变化的\\( C_i \\)。

![](/assets/images/Attention_Model/02.png)

即生成目标句子单词的过程成了下面的形式：

$$
\begin{aligned}
  \mathbf{y}_{1} =\mathbf{f} \mathbf{1}\left(\mathbf{C}_{1}\right) \\ \mathbf{y}_{2} =\mathbf{f 1}\left(\mathbf{C}_{2}, \mathbf{y}_{1}\right) \\ \mathbf{y}_{3} =\mathbf{f} \mathbf{1}\left(\mathbf{C}_{3}, \mathbf{y}_{1}, \mathbf{y}_{2}\right) 
\end{aligned}
$$

每个\\( C_i \\)可能对应着不同的源语句子单词的注意力分配概率分布，比如对于上面的英汉翻译来说，其对应的信息可能如下：


其中，f2函数代表Encoder对输入英文单词的某种变换函数，比如如果Encoder是用的RNN模型的话，这个f2函数的结果往往是某个时刻输入xi后隐层节点的状态值；g代表Encoder根据单词的中间表示合成整个句子中间语义表示的变换函数，一般的做法中，g函数就是对构成元素加权求和，也就是常常在论文里看到的下列公式：

$$
c_{i}=\sum_{j=1}^{T_{x}} \alpha_{i j} h_{j}
$$

$$
\alpha_{i j}=\frac{\exp \left(e_{i j}\right)}{\sum_{k=1}^{T_{x}} \exp \left(e_{i k}\right)}
$$

$$
e_{i j}=a\left(s_{i-1}, h_{j}\right)
$$

\\( \alpha_{i j} \\) 是一个softmax模型输出，概率值的和为1。 \\( e_{i j} \\) 表示一个对齐模型，用于衡量encoder端的位置j个词，对于decoder端的位置i个词的对齐程度（影响程度），换句话说：decoder端生成位置i的词时，有多少程度受encoder端的位置j的词影响。对齐模型 \\( e_{i j} \\) 的计算方式有很多种，不同的计算方式，代表不同的Attention模型，最简单且最常用的的对齐模型是dot product乘积矩阵，即把target端的输出隐状态ht与source端的输出隐状态进行矩阵乘。常见的对齐计算方式如下：

$$
\text{score}(\mathbf{h}_t,\bar{\mathbf{h}_s}) = \begin{cases} \mathbf{h}_t^T \bar{\mathbf{h}_s} \qquad\qquad\qquad dot \\ \mathbf{h}_t^T \mathbf{W}_a \bar{\mathbf{h}_s} \qquad\qquad general \\ \mathbf{v}_a^T \text{tanh}[\mathbf{h}_t^T : \bar{\mathbf{h}_s}] \quad concat \\ \end{cases}
$$

其中, \\( \operatorname{score}\left(h_{t}, h_{s}\right) = a_{ij} \\) 表示源端与目标单单词对齐程度。可见，常见的对齐关系计算方式有，点乘（Dot product），权值网络映射（General）和concat映射几种方式。

![](/assets/images/Attention_Model/03.png)

假设Ci中那个i就是上面的“汤姆”，那么Tx就是3，代表输入句子的长度，h1=f(“Tom”)，h2=f(“Chase”),h3=f(“Jerry”)，对应的注意力模型权值分别是0.6,0.2,0.2，所以g函数就是个加权求和函数。如果形象表示的话，翻译中文单词“汤姆”的时候，数学公式对应的中间语义表示Ci的形成过程类似下图：

![](/assets/images/Attention_Model/04.png)
图3 Ci的形成过程

这里还有一个问题：生成目标句子某个单词，比如“汤姆”的时候，你怎么知道AM模型所需要的输入句子单词注意力分配概率分布值呢？就是说“汤姆”对应的概率分布：

　　注意力权重获取的过程（Tom,0.3）（Chase,0.2）（Jerry,0.5）是如何得到的呢？

　　为了便于说明，我们假设对图1的非AM模型的Encoder-Decoder框架进行细化，Encoder采用RNN模型，Decoder也采用RNN模型，这是比较常见的一种模型配置，则图1的图转换为下图：


图4 RNN作为具体模型的Encoder-Decoder框架
注意力分配概率分布值的通用计算过程：


图5 AM注意力分配概率计算
对于采用RNN的Decoder来说，如果要生成 yi 单词，在时刻 i ，我们是可以知道在生成 Yi 之前的隐层节点i时刻的输出值 Hi 的，而我们的目的是要计算生成 Yi 时的输入句子单词“Tom”、“Chase”、“Jerry”对 Yi 来说的注意力分配概率分布，那么可以用i时刻的隐层节点状态 Hi 去一一和输入句子中每个单词对应的RNN隐层节点状态 hj 进行对比，即通过函数 F(hj,Hi) 来获得目标单词 Yi 和每个输入单词对应的对齐可能性，这个F函数在不同论文里可能会采取不同的方法，然后函数F的输出经过Softmax进行归一化就得到了符合概率分布取值区间的注意力分配概率分布数值（这就得到了注意力权重）。图5显示的是当输出单词为“汤姆”时刻对应的输入句子单词的对齐概率。绝大多数AM模型都是采取上述的计算框架来计算注意力分配概率分布信息，区别只是在F的定义上可能有所不同。

　　上述内容就是论文里面常常提到的Soft Attention Model（任何数据都会给一个权值，没有筛选条件）的基本思想，你能在文献里面看到的大多数AM模型基本就是这个模型，区别很可能只是把这个模型用来解决不同的应用问题。那么怎么理解AM模型的物理含义呢？一般文献里会把AM模型看作是单词对齐模型，这是非常有道理的。目标句子生成的每个单词对应输入句子单词的概率分布可以理解为输入句子单词和这个目标生成单词的对齐概率，这在机器翻译语境下是非常直观的：传统的统计机器翻译一般在做的过程中会专门有一个短语对齐的步骤，而注意力模型其实起的是相同的作用。在其他应用里面把AM模型理解成输入句子和目标句子单词之间的对齐概率也是很顺畅的想法。


图6 Google 神经网络机器翻译系统结构图
图6所示即为Google于2016年部署到线上的基于神经网络的机器翻译系统，相对传统模型翻译效果有大幅提升，翻译错误率降低了60%，其架构就是上文所述的加上Attention机制的Encoder-Decoder框架，主要区别无非是其Encoder和Decoder使用了8层叠加的LSTM模型。

当然，从概念上理解的话，把AM模型理解成影响力模型也是合理的，就是说生成目标单词的时候，输入句子每个单词对于生成这个单词有多大的影响程度。这种想法也是比较好理解AM模型物理意义的一种思维方式。

Rush用AM模型来做生成式摘要给出的一个AM的一个非常直观的例子。




图7 句子生成式摘要例子
这个例子中，Encoder-Decoder框架的输入句子X是：“russian defense minister ivanov called sunday for the creation of a joint front for combating global terrorism”。对应图中纵坐标的句子。系统生成的摘要句子Y是：“russia calls for joint front against terrorism”，对应图中横坐标的句子。可以看出模型已经把句子主体部分正确地抽出来了。矩阵中每一列代表生成的目标单词对应输入句子每个单词的AM分配概率，颜色越深代表分配到的概率越大。这个例子对于直观理解AM是很有帮助作用。

4. Attention机制的本质思想
如果把Attention机制从上文讲述例子中的Encoder-Decoder框架中剥离，并进一步做抽象，可以更容易看懂Attention机制的本质思想。


图9 Attention机制的本质思想
我们可以这样来看待Attention机制（参考图9）：将Source中的构成元素想象成是由一系列的<Key,Value>数据对构成，此时给定Target中的某个元素Query，通过计算Query和各个Key的相似性或者相关性，得到每个Key对应Value的权重系数，然后对Value进行加权求和，即得到了最终的Attention数值。所以本质上Attention机制是对Source中元素的Value值进行加权求和，而Query和Key用来计算对应Value的权重系数。即可以将其本质思想改写为如下公式：


其中，Lx=||Source||代表Source的长度，公式含义即如上所述。上文所举的机器翻译的例子里，因为在计算Attention的过程中，Source中的Key和Value合二为一，指向的是同一个东西，也即输入句子中每个单词对应的语义编码，所以可能不容易看出这种能够体现本质思想的结构。

　　当然，从概念上理解，把Attention仍然理解为从大量信息中有选择地筛选出少量重要信息并聚焦到这些重要信息上，忽略大多不重要的信息，这种思路仍然成立。聚焦的过程体现在权重系数的计算上，权重越大越聚焦于其对应的Value值上，即权重代表了信息的重要性，而Value是其对应的信息。

　　至于Attention机制的具体计算过程，如果对目前大多数方法进行抽象的话，可以将其归纳为两个过程：第一个过程是根据Query和Key计算权重系数，第二个过程根据权重系数对Value进行加权求和。而第一个过程又可以细分为两个阶段：第一个阶段根据Query和Key计算两者的相似性或者相关性；第二个阶段对第一阶段的原始分值进行归一化处理；这样，可以将Attention的计算过程抽象为如图10展示的三个阶段。


在第一个阶段，可以引入不同的函数和计算机制，根据Query和某个 Keyi ，计算两者的相似性或者相关性，最常见的方法包括：求两者的向量点积、求两者的向量Cosine相似性或者通过再引入额外的神经网络来求值，即如下方式：


第一阶段产生的分值根据具体产生的方法不同其数值取值范围也不一样，第二阶段引入类似SoftMax的计算方式对第一阶段的得分进行数值转换，一方面可以进行归一化，将原始计算分值整理成所有元素权重之和为1的概率分布；另一方面也可以通过SoftMax的内在机制更加突出重要元素的权重。即一般采用如下公式计算：


第二阶段的计算结果 ai 即为 Valuei 对应的权重系数，然后进行加权求和即可得到Attention数值：


通过如上三个阶段的计算，即可求出针对Query的Attention数值，目前绝大多数具体的注意力机制计算方法都符合上述的三阶段抽象计算过程。

5. Self Attention模型
通过上述对Attention本质思想的梳理，我们可以更容易理解本节介绍的Self Attention模型。Self Attention也经常被称为intra Attention（内部Attention），最近一年也获得了比较广泛的使用，比如Google最新的机器翻译模型内部大量采用了Self Attention模型。

　　在一般任务的Encoder-Decoder框架中，输入Source和输出Target内容是不一样的，比如对于英-中机器翻译来说，Source是英文句子，Target是对应的翻译出的中文句子，Attention机制发生在Target的元素和Source中的所有元素之间。而Self Attention顾名思义，指的不是Target和Source之间的Attention机制，而是Source内部元素之间或者Target内部元素之间发生的Attention机制，也可以理解为Target=Source这种特殊情况下的注意力计算机制。其具体计算过程是一样的，只是计算对象发生了变化而已，所以此处不再赘述其计算过程细节。

Self Attention与传统的Attention机制非常的不同：传统的Attention是基于source端和target端的隐变量（hidden state）计算Attention的，得到的结果是源端的每个词与目标端每个词之间的依赖关系。但Self Attention不同，它分别在source端和target端进行，仅与source input或者target input自身相关的Self Attention，捕捉source端或target端自身的词与词之间的依赖关系；然后再把source端的得到的self Attention加入到target端得到的Attention中，捕捉source端和target端词与词之间的依赖关系。因此，self Attention比传统的Attention mechanism效果要好，主要原因之一是，传统的Attention机制忽略了源端或目标端句子中词与词之间的依赖关系，相对比，self Attention可以不仅可以得到源端与目标端词与词之间的依赖关系，同时还可以有效获取源端或目标端自身词与词之间的依赖关系，如图7所示


图11 可视化Self Attention实例

图12 可视化Self Attention实例
从两张图（图11、图12）可以看出，Self Attention可以捕获同一个句子中单词之间的一些句法特征（比如图11展示的有一定距离的短语结构）或者语义特征（比如图12展示的its的指代对象Law）。

　　很明显，引入Self Attention后会更容易捕获句子中长距离的相互依赖的特征，因为如果是RNN或者LSTM，需要依次序序列计算，对于远距离的相互依赖的特征，要经过若干时间步步骤的信息累积才能将两者联系起来，而距离越远，有效捕获的可能性越小。

　　但是Self Attention在计算过程中会直接将句子中任意两个单词的联系通过一个计算步骤直接联系起来，所以远距离依赖特征之间的距离被极大缩短，有利于有效地利用这些特征。除此外，Self Attention对于增加计算的并行性也有直接帮助作用。这是为何Self Attention逐渐被广泛使用的主要原因。



## 更多参考资源:

[Recurrent Models of Visual Attention](http://papers.nips.cc/paper/5542-recurrent-models-of-visual-attention.pdf)    
[Learning Phrase Representations using RNN Encoder–Decoder for Statistical Machine Translation](https://arxiv.org/pdf/1406.1078.pdf)    
[Recurrent Continuous Translation Models](https://www.aclweb.org/anthology/D13-1176)    
[Sequence to Sequence Learning with Neural Networks](http://papers.nips.cc/paper/5346-sequence-to-sequence-learning-with-neural-networks.pdf)    
[Visualizing A Neural Machine Translation Model (Mechanics of Seq2seq Models With Attention)](https://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/)



