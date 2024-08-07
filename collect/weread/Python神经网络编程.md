---
doc_type: weread-highlights-reviews
bookId: "22655611"
author: 塔里克·拉希德
cover: https://cdn.weread.qq.com/weread/cover/56/YueWen_22655611/t7_YueWen_22655611.jpg
reviewCount: 0
noteCount: 60
readingStatus: 在读
progress: 59%
totalReadDay: 5
readingTime: 2小时35分钟
readingDate: 2023-12-22
isbn: 9787115474810
lastReadDate: 2024-01-02

---
# 元数据
> [!abstract] Python神经网络编程
> - ![ Python神经网络编程|200](https://cdn.weread.qq.com/weread/cover/56/YueWen_22655611/t7_YueWen_22655611.jpg)
> - 书名： Python神经网络编程
> - 作者： 塔里克·拉希德
> - 简介： 本书首先从简单的思路着手，详细介绍了理解神经网络如何工作所必须的基础知识。第一部分介绍基本的思路，包括神经网络底层的数学知识，第2部分是实践，介绍了学习Python编程的流行和轻松的方法，从而逐渐使用该语言构建神经网络，以能够识别人类手写的字母，特别是让其像专家所开发的网络那样地工作。第3部分是扩展，介绍如何将神经网络的性能提升到工业应用的层级，甚至让其在RaspberryPi上工作。
> - 出版时间 2018-04-01 00:00:00
> - ISBN： 9787115474810
> - 分类： 计算机-人工智能
> - 出版社： 人民邮电出版社
> - PC地址：https://weread.qq.com/web/reader/a0b328807159b27ba0ba937

# 高亮划线

## 译者序

> 📌 逻辑的基础其实很简单。 
> ⏱ 2023-12-22 16:30:20 ^22655611-4-1356-1367

## 前言

> 📌 我们使用的最困难运算是梯度演算（gradient calculus） 
> ⏱ 2023-12-23 18:37:34 ^22655611-6-699-733

> 📌 一旦你掌握了神经网络的基本知识，你就可以将神经网络的核心思想运用到许多不同的问题中。 
> ⏱ 2023-12-23 18:37:45 ^22655611-6-827-869

### 1.2 一台简单的预测机

> 📌 是走马观花地浏览了一遍神经网络中学习的核心过程 
> ⏱ 2023-12-23 19:50:46 ^22655611-9-5102-5125

> 📌 我们训练机器，使其输出值越来越接近正确的答案 
> ⏱ 2023-12-23 19:50:50 ^22655611-9-5126-5148

> 📌 并多次改进答案，这是一种非常不同的方法。一些人将这种方法称为迭代，意思是持续地、一点一点地改进答案。 
> ⏱ 2023-12-23 19:51:06 ^22655611-9-5246-5296

### 1.3 分类器与预测器并无太大差别

> 📌 这个问题的答案处于神经网络学习的核心地带。让我们继续看 
> ⏱ 2023-12-23 19:53:50 ^22655611-10-2638-2665

### 1.6 神经元——大自然的计算机器

> 📌 你可以看到，在输入值较小的情况下，输出为零。然而，一旦输入达到阈值，输出就一跃而起。具有这种行为的人工神经元就像一个真正的生物神经元。科学家所使用的术语实际上非常形象地描述了这种行为，他们说，输入达到阈值时，神经元就激发了。 
> ⏱ 2023-12-23 19:56:22 ^22655611-13-2840-2952

> 📌 个函数更自然、更接近现实。自然界很少有冰冷尖锐的边缘！ 
> ⏱ 2023-12-23 19:58:23 ^22655611-13-3054-3081

> 📌 我们使用这种S函数，而不使用其他可以用于神经元输出的S形函数，还有另一个非常重要的原因，那就是，这个S函数比起其他S形函数计算起来容易得多，在后面的实践中，我们会看到为什么。让我们回到 
> ⏱ 2023-12-23 20:00:00 ^22655611-13-4347-4468

> 📌 下图说明了这种组合输入，然后对最终输入总和使用阈值的思 
> ⏱ 2023-12-23 20:00:34 ^22655611-13-4693-4720

> 📌 者带来了一种直观的感觉，即这些神经元也可以进行一些相对复杂、在某种意义上有点模糊的计算。 
> ⏱ 2023-12-23 20:01:06 ^22655611-13-5118-5162

### 1.7 在神经网络中追踪信号

> 📌 但是，计算信号如何经过一层一层的神经元，从输入变成输出，这个过程似乎有点令人生畏，这好像是一种非常艰苦的工作。 
> ⏱ 2023-12-24 15:10:59 ^22655611-14-538-593

> 📌 这层所做的所有事情就是表示输入，仅此而已。 
> ⏱ 2023-12-24 18:11:33 ^22655611-14-2288-2309

> 📌 最终，我们可以使用激活函数y=1 /（1+e -x）计算该节点的输出。 
> ⏱ 2023-12-24 18:12:55 ^22655611-14-3479-3569

> 📌 这个工作真是太伟大了！现在，我们得到了神经网络两个输出节点中的一个的实际输出。 
> ⏱ 2023-12-24 18:12:51 ^22655611-14-3712-3751

> 📌 ！好在计算机在进行大量计算方面表现非常出色，并且不知疲倦和厌烦。 
> ⏱ 2023-12-24 18:13:16 ^22655611-14-4610-4642

### 1.9 使用矩阵乘法的三层神经网络示例

> 📌 最后一层为输出层，中间层我们称之为隐藏层 
> ⏱ 2023-12-24 18:19:52 ^22655611-16-906-926

> 📌 我们对这些节点应用了S激活函数，使得节点对信号的反应更像自然界中节点的反应，你应该记得这一点吧！因此，现在，我们进行这个操作： 
> ⏱ 2023-12-24 18:45:56 ^22655611-16-4204-4267

> 📌 也就是应用激活函数到中间层的组合输入信号上。让我们使用这个新信息，更新下图。 
> ⏱ 2023-12-24 18:46:32 ^22655611-16-5178-5216

> 📌 。因此，需要记住的事情是，不管有多少层神经网络，我们都“一视同仁”，即组合输入信号，应用链接权重调节这些输入信号，应用激活函数，生成这些层的输出信号。 
> ⏱ 2023-12-24 18:47:04 ^22655611-16-5643-5718

> 📌 剩下的工作就是应用S激活函数，这是很容易的一件事情。 
> ⏱ 2023-12-24 18:47:51 ^22655611-16-6913-6939

> 📌 下一步，将神经网络的输出值与训练样本中的输出值进行比较，计算出误差。我们需要使用这个误差值来调整神经网络本身，进而改进神经网络的输出值。 
> ⏱ 2023-12-24 18:48:27 ^22655611-16-7413-7481

> 📌 可能是最难以理解的事情，因此，随着我们继续学习本书，我们将会如春风细雨般详细阐述这个思 
> ⏱ 2023-12-24 18:48:33 ^22655611-16-7698-7741

### 1.10 学习来自多个节点的权重

> 📌 。第二件事情，我们使用权重，将误差从输出向后传播到网络中。我们称这种方法为反向传播，你应该不会对此感到惊讶吧。 
> ⏱ 2023-12-24 19:37:54 ^22655611-17-2006-2061

### 1.13 使用矩阵乘法进行反向传播误差

> 📌 如果这种相对简单的方式行之有效，那么我们就应该坚持这种方法。 
> ⏱ 2023-12-24 20:16:50 ^22655611-20-3834-3864

> 📌 由于链接权重的强度给出了共享误差的最好指示，因此反馈的误差应该遵循链接权重的强度。我们已经做了海量的工作了！ 
> ⏱ 2023-12-24 20:17:06 ^22655611-20-3957-4011

> 📌 下面一节的理论相当酷炫，这需要一个清醒的大脑，因此请好好休息调整一下。 
> ⏱ 2023-12-24 20:34:56 ^22655611-20-4331-4366

### 1.14 我们实际上如何更新权重

> 📌 的输出增加0.5呢 
> ⏱ 2023-12-24 20:52:21 ^22655611-21-1052-1061

> 📌 这个效果也会由于需要调整另一个权重来改进不同的输出节点而被破坏。你不能将此视为无关紧要的事情。 
> ⏱ 2023-12-24 20:52:52 ^22655611-21-1076-1123

> 📌 数学家多年来也未解决这个难题，直到20世纪60年代到70年代，这个难题才有了切实可行的求解办 
> ⏱ 2023-12-24 21:18:35 ^22655611-21-2402-2448

> 📌 这个迟来的发现导致了现代神经网络爆炸性的发展，因此现在的神经网络可以执行一些令人印象深刻的任务。 
> ⏱ 2023-12-24 21:19:27 ^22655611-21-2491-2539

> 📌 神经网络本身可能没有足够多的层或节点，不能正确地对问题的解进行建模。 
> ⏱ 2023-12-24 21:22:14 ^22655611-21-2901-2935

> 📌 如果我们从实际出发可以找到一种办法，虽然这种方法从数学角度而言并不完美，但是由于这种方法没有做出虚假的理想化假设，因此实际上给我们带来了更好的结果。 
> ⏱ 2023-12-24 21:22:54 ^22655611-21-2988-3062

> 📌 大小成比例，那么在接近最小值时，我们就可以采用小步长。这 
> ⏱ 2023-12-24 21:26:24 ^22655611-21-5401-5429

> 📌 记得输出函数吧，神经网络的误差函数取决于许多的权重参数，这些参数通常有数百个呢！ 
> ⏱ 2023-12-24 21:27:33 ^22655611-21-6395-6435

> 📌 神经网络的输出是一个极其复杂困难的函数，这个函数具有许多参数影响到其输出的链接权重 
> ⏱ 2023-12-24 21:29:09 ^22655611-21-7689-7730

> 📌 权重吗？只要我们选择了合适的误差函数，这是完全可以的。 
> ⏱ 2023-12-24 21:29:17 ^22655611-21-7749-7776

> 📌 因此我们可以很容易地把输出函数变成误差函数。 
> ⏱ 2023-12-24 21:29:34 ^22655611-21-7853-7875

> 📌 判断此时网络是否得到了很好的训练，你会看到总和为 
> ⏱ 2023-12-24 21:30:07 ^22655611-21-8221-8245

> 📌 越接近最小值，梯度越小，这意味着，如果我们使用这个函数调节步长，超调的风险就会变得较小。 
> ⏱ 2023-12-24 21:31:35 ^22655611-21-8913-8957

> 📌 于神经网络中的链接权重的。 
> ⏱ 2023-12-24 21:32:26 ^22655611-21-9337-9350

> 📌 误差对链接权重的改变有多敏感？” 
> ⏱ 2023-12-24 21:32:31 ^22655611-21-9365-9381

> 📌 为输入层和隐藏层之间的权重找到类似的误差斜率。 
> ⏱ 2023-12-24 21:45:45 ^22655611-21-15616-15639

> 📌 这种方法虽然很简单，但却是一种很强大的技术，一些天赋异禀的数学家和科学家都使用这种技术。 
> ⏱ 2023-12-24 21:56:59 ^22655611-21-16127-16171

> 📌 我们使用学习因子，调节变化，我们可以根据特定的问题，调整这个学习因子 
> ⏱ 2023-12-24 21:58:14 ^22655611-21-16630-16664

> 📌 符号α是一个因子，这个因子可以调节这些变化的强度，确保不会超调。我们通常称这个因子为学习率。 
> ⏱ 2023-12-24 22:02:05 ^22655611-21-17090-17136

### 1.15 权重更新成功范例

> 📌 虽然这是一个相当小的变化量，但权重经过成百上千次的迭代，最终会确定下来，达到一种布局，这样训练有素的神经网络就会生成与训练样本中相同的输出。 
> ⏱ 2023-12-24 22:10:01 ^22655611-22-2067-2137

### 1.16 准备数据

> 📌 一些问题可以通过改进训练数据、初始权重、设计良好的输出方案来解决。让我们逐个讨 
> ⏱ 2023-12-24 22:10:30 ^22655611-23-571-610

> 📌 于我们使用梯度学习新的权重，因此一个平坦的激活函数会 
> ⏱ 2023-12-24 22:10:45 ^22655611-23-960-986

> 📌 权重的改变取决于激活函数的梯度。小梯度意味着限制神经网络学习的能力。这就是所谓的饱和神经网络。这意味着，我们应该尽量保持小的输入。 
> ⏱ 2023-12-24 22:11:03 ^22655611-23-1036-1101

> 📌 非常大的数字时，可能会丧失精度，因此，使用非常小的值也会出现问题。 
> ⏱ 2023-12-24 22:11:23 ^22655611-23-1205-1238

> 📌 ，匹配激活函数的可能输出，注意避开激活函数不可能达到的值。 
> ⏱ 2023-12-24 22:12:44 ^22655611-23-2013-2042

> 📌 但是由于0.0和1.0这两个数也不可能是目标值，并且有驱动产生过大的权重的风险，因此一些人也使用0.01～0.99的范围。 
> ⏱ 2023-12-24 22:13:03 ^22655611-23-2090-2151

> 📌 由于大的初始权重会造成大的信号传递给激活函数，导致网络饱和，从而降低网络学习到更好的权重的能力，因此应该避免大的初始权重值。 
> ⏱ 2023-12-24 22:13:23 ^22655611-23-2272-2334

> 📌 因此，如果链接更多，那么减小权重的范围，这个经验法则是有道理的。 
> ⏱ 2023-12-24 22:21:23 ^22655611-23-3564-3596

> 📌 再次出现另一组值相等的权重。由于正确训练的网络应该具有不等的权重（对于几乎所有的问题，这是极有可能的情况），那么由于这种对称性，你将永远得不到这种网络，因此这是一种很糟糕的情况。 
> ⏱ 2023-12-24 22:24:08 ^22655611-23-4237-4326

> 📌 值较小，但要避免零值。如果节点的传入链接较多，有一些人会使用相对复杂的规则，如减小这些权重的大小。 
> ⏱ 2023-12-24 22:31:57 ^22655611-23-4903-4952

> 📌 练目标值设置在有效的范围之外，将会驱使产生越来越大的权重，导致网络饱和。一个合适的范围为0.01～0.99。 
> ⏱ 2023-12-24 22:34:35 ^22655611-23-5134-5188

### 2.4 使用Python制作神经网络

> 📌 优秀的程序员、计算机科学家和数学家，只要可能，都尽力创建一般代码，而不是具体的代码。这是一种好习惯，它迫使我们以一种更深更广泛的适用方式思考求解问题 
> ⏱ 2024-01-02 18:52:08 ^22655611-28-2356-2430

# 读书笔记

# 本书评论
