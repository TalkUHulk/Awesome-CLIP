# Awesome-CLIP

- [Awesome-CLIP](#awesome-clip)
  - [Train](#train)
  - [Improvement \& Innovation](#improvement--innovation)
  - [Data](#data)
  - [Distillation](#distillation)
  - [Loss](#loss)
  - [Zero-Shot \& Few-Shot \& Classification](#zero-shot--few-shot--classification)
  - [Retrieval](#retrieval)
  - [Segmentation](#segmentation)
  - [Captioning](#captioning)
  - [Generation](#generation)
  - [Other](#other)


## Train


| Title | Abstract | Intro | Useful Links |
|:----| :---:| :----: | :---:|
|2022|
| [![Star](https://img.shields.io/github/stars/Sense-GVT/DeCLIP.svg?style=social&label=Star)](https://github.com/Sense-GVT/DeCLIP) <br> **SUPERVISION EXISTS EVERYWHERE: A DATA EFFICIENT CONTRASTIVE LANGUAGE-IMAGE PRE-TRAINING PARADIGM** <br>| 本文提出一种创新的CLIP训练方式--Data efficient CLIP (DeCLIP)，来解决CLIP训练对文本-图像pair数据量的需求.  核心思想就是增加对图像-文本对的supervision(增加更多约束)，更有效地学习通用的视觉特征. 作者增加了以下监督：1.每个模态内的self-supervision;2.跨模态的多视图supervision(数据增强后的view);3.来自其他相似对的最近邻supervision.  实验证明，与base CLIP相比，更少的训练数据取得了更高的表现.  <br><br>🧟‍♂️:Nearest-Neighbor Supervision处设计了一个FIFO的队列，个人觉得借鉴了MoCo的思想，很有意思👍 |<img src="./images/DeCLIP.png"  width="640px"/>| [[Github](https://github.com/Sense-GVT/DeCLIP)] <br> [[Paper](https://arxiv.org/pdf/2110.05208)] |
| [![Star](https://img.shields.io/github/stars/microsoft/RegionCLIP.svg?style=social&label=Star)](https://github.com/microsoft/RegionCLIP) <br> **RegionCLIP: Region-based Language-Image Pretraining** <br>|虽然CLIP在zero-shot和transfer learning settings这样的图片分类任务上表现不俗，但如果直接将CLIP应用到目标检测这种识别图像区域的任务上，表现很差. 原因是存在domain shift，CLIP基于image-text pair的数据集训练，建立的是图片整体和文本描述的联系，与文本没有做局部、细粒度的对齐 . 为了解决这个问题，本文提出regionCLIP ，将图片局部与对应描述对齐，在特征空间实现region-text 对齐. 实验证明，在下游检测任务上，与CLIP相比性能明显提升.  <br><br>🧟‍♂️: pretrain期间显式对齐图像区域和文本标记，有个点，无需人工标注region区域caption，使用的是伪标签，具体做法，建立一个object类别候选池，通过预训练的CLIP，可以将region（RPN提取）与object match起来，然后利用固定的prompt模版生成caption，比如region是一只狗，描述就是 a photo of dog.  这里如果借助LLaVa等模型生成caption，效果会不会更好？当然pretrain成本也会更高，需要balance. | <img src="./images/regionCLIP.png" width="640px"/> | [[Github](https://github.com/microsoft/RegionCLIP)] <br> [[Paper](https://arxiv.org/pdf/2112.09106)] |
|2023|
| **Less is More: Removing Text-regions Improves CLIP Training Efficiency and Robustness** <br>| CLIP没有区分嵌入在图像中的文本区域的视觉语义和意义. 当嵌入区域中的文本与图像的视觉外观不匹配时，这可能导致不鲁棒性. 文章提出两种有效的方法来提高CLIP训练的效率和鲁棒性：1. 在保持相同数量的优化步骤的同时增强训练数据集；2.过滤掉图像中包含文本区域的样本.  在ImageNet和CoCo等数据集上测试，文章方法提高了Clip在下游任务的分类和检索准确率.  |<img src="./images/LessisMore.png"  width="640px"/>| [[Paper](https://arxiv.org/pdf/2305.05095)] |
| [![Star](https://img.shields.io/github/stars/UCSC-VLAA/CLIPA.svg?style=social&label=Star)](https://github.com/UCSC-VLAA/CLIPA) <br> **CLIPA: An Inverse Scaling Law for CLIP Training** <br>| 文章提出了一个令人惊讶的发现，即CLIP训练存在inverse scaling law，即使用的图像/文本编码器越大，可以用于训练的图像/文本tokens的序列长度越短. 此外，减少图像/文本tokens长度的策略，在确定这种缩放定律的质量方面起着至关重要的作用. 文章在有限的资源下成功训练了Clip. |<img src="./images/CLIPA.png"  width="640px"/>| [[Github](https://github.com/UCSC-VLAA/CLIPA)] <br> [[Paper](https://arxiv.org/pdf/2305.07017)] |
| [![Star](https://img.shields.io/github/stars/UCSC-VLAA/CLIPA.svg?style=social&label=Star)](https://github.com/UCSC-VLAA/CLIPA) <br> **CLIPA-v2: Scaling CLIP Training with 81.1% Zero-shot ImageNet Accuracy within a $10,000 Budget; An Extra $4,000 Unlocks 81.8% Accuracy** <br>| 在CLIPA基础上，验证了full resolution 的token微调模型时，inverse scaling law也适用;同时验证各种不同训练参数下模型的能力，包括模型大小、数据和training schedule. |<img src="./images/CLIPA-v2.png"  width="640px"/>| [[Github](https://github.com/UCSC-VLAA/CLIPA)] <br> [[Paper](https://arxiv.org/pdf/2306.15658)] |
| [![Star](https://img.shields.io/github/stars/facebookresearch/flip.svg?style=social&label=Star)](https://github.com/facebookresearch/flip) <br> **Scaling Language-Image Pre-training via Masking** <br>| 文章提出了一种简单而有效的CLIP训练方法---FLIP（Fast Language-Image Pre-training）.该方法只需要在训练时随机Mask掉一部分图像. 实验证明，与标准CLIP详细，该方法在训练速度和模型精度方面都有提升. 文章受到MAE的启发. 引入masking，使模型在“how carefully we look at a sample pair” 和 “how many sample pairs we can process”之间做trade-off. 因为Vit encoder只用于visible patches，当mask掉一部分图像时，可以节约相应的显存，这样降低了计算量，可以使用更大的batchsize，对contrastive loss更加友好.  同时，masking作为一种形式的噪声和正则化可以提高鲁棒性.  |<img src="./images/flip.png"  width="640px"/>| [[Github](https://github.com/facebookresearch/flip)] <br> [[Paper](https://arxiv.org/pdf/2212.00794)] |
| [![Star](https://img.shields.io/github/stars/LijieFan/LaCLIP.svg?style=social&label=Star)](https://github.com/LijieFan/LaCLIP) <br> **Improving CLIP Training with Language Rewrites** <br>| 在CLIP训练过程中，只对图像数据做了数据增强，而文本数据保持不变. 针对此问题，作者提出了Language augmented CLIP (LaCLIP), 利用LLM的上下文学习能力，重新描述训练集的captions，增加文本的多样性. 实验表明，在下游zero-shot任务上，性能有明显提升.|<img src="./images/LaCLIP.png"  width="640px"/>| [[Github](https://github.com/LijieFan/LaCLIP)] <br> [[Paper](https://arxiv.org/pdf/2305.20088)] |
| [![Star](https://img.shields.io/github/stars/zjukg/Structure-CLIP.svg?style=social&label=Star)](https://github.com/zjukg/Structure-CLIP) <br> **Structure-CLIP: Towards Scene Graph Knowledge to Enhance Multi-modal Structured Representations** <br>|CLIP在结构化的文本-图像匹配上表现不够，如通过clip score并不能区别一张图是人咬狗和狗咬人. 作者认为造成这个问题的原因是CLIP在学习多模态场景中的representations时未能充分利用结构化知识.  文章提出 Structure-CLIP ，一个端到端的框架，通过集成场景图知识来增强多模态结构化表示. <br><br>🧟‍♂️: 1.增加难例负样本; 2. 实际工作中个人也想过类似方法，大概是通过分析负责描述的词性等，利用最短依赖路径等方法拆分句子，然后做enhance.| <img src="./images/Structure-CLIP.png"  width="640px"/> | [[Github](https://github.com/zjukg/Structure-CLIP)] <br> [[Paper](https://arxiv.org/pdf/2305.06152)] |
| [![Star](https://img.shields.io/github/stars/mertyg/vision-language-models-are-bows.svg?style=social&label=Star)](https://github.com/mertyg/vision-language-models-are-bows) <br> **WHEN AND WHY VISION-LANGUAGE MODELS BEHAVE LIKE BAGS-OF-WORDS, AND WHAT TO DO ABOUT IT?** <br>|  尽管VLM在下游任务表现不错，但对模型如何学习类别和属性的组合关系，目前尚不清楚.  文章首先提出一个Attribution, Relation, and Order(ARO) 的benchmark，系统评估VLM理解不同类型的关系、属性和顺序信息的能力. 然后对使用VLM实现的检索任务和对比预训练做了深刻解析，提出解释了几个问题. 最后提出NegCLIP提高模型对 attributes and relations的理解.  <br><br>🧟‍♂️: ICLR 2023 Oral，初看觉得没什么意思，但精读后收获很多. 比如实际工作中，利用clip实现多模态检索，虽然clip不能很好的学习object和对应属性的关系，但通过触发key words，依旧可以获得很好的表现. 文章对此做了解读. 以及NegCLIP，方法直接简单，其实实际工作中常用类似方法. 总之是一篇值得细品的文章👍| <img src= "./images/NegCLIP.png"  width="640px"/> | [[Github](https://github.com/mertyg/vision-language-models-are-bows)] <br> [[Paper](https://openreview.net/pdf?id=KRLUvxh8uaX)] |
|2024|
| [![Star](https://img.shields.io/github/stars/YichaoCai1/CLAP.svg?style=social&label=Star)](https://github.com/YichaoCai1/CLAP) <br> **CLAP: Isolating Content from Style through Contrastive Learning with Augmented Prompts** <br>|直接使用[作者的回答](https://www.zhihu.com/question/660698707/answer/3550999896)： 从causality理论出发，CLAP旨在提升pretrained VLM在distribution shift场景下的generalization能力，CLAP仅需在text modality用较小成本去fine-tune CLIP模型，可以使pretrained representations更聚焦于content（object）本身，从而提升模型zero-shot/few-shot表现, 以及domain adaptation和adversarial resilience的能力.  |<img src="./images/CLAP.png"  width="640px"/>| [[Github](https://github.com/YichaoCai1/CLAP)] <br> [[Paper](https://arxiv.org/pdf/2311.16445)] |
| [![Star](https://img.shields.io/github/stars/apple/ml-tic-clip.svg?style=social&label=Star)](https://github.com/apple/ml-tic-clip) <br> **TIC-CLIP: CONTINUAL TRAINING OF CLIP MODELS** <br> |随着数据的不断积累和更新，如何低成本的训练模型，与最新的数据同步. 文章提出一种简单的rehearsal-based的方案，与标准的预训练方法相比，提速2.5x. | <img src="./images/TIC-CLIP.png"  width="640px"/>| [[Github](https://github.com/apple/ml-tic-clip)] <br> [[Paper](https://arxiv.org/pdf/2310.16226)] |
| [![Star](https://img.shields.io/github/stars/facebookresearch/MetaCLIP.svg?style=social&label=Star)](https://github.com/facebookresearch/MetaCLIP) <br> **DEMYSTIFYING CLIP DATA[MetaCLIP]** <br>|文章揭示了CLIP训练数据的管理方法，提出CLIP数据管理算法，来简化和产生高质量的训练数据.  并且介绍了Metadata-Curated Language-Image Pre-training (MetaCLIP)---CLIP pro版.  <br><br>🧟‍♂️:建议大规模数据集管理食用，数据质量比数量重要得多～| | [[Github](https://github.com/facebookresearch/MetaCLIP)] <br> [[Paper](https://arxiv.org/pdf/2309.16671)]|
| [![Star](https://img.shields.io/github/stars/dsam99/pc_clip.svg?style=social&label=Star)](https://github.com/dsam99/pc_clip) <br> **FINETUNING CLIP TO REASON ABOUT PAIRWISE DIFFERENCES** <br> |文章提出PC-CLIP (Pairwise Comparison CLIP)，提高CLIP推理差异的能力. 具体而言，借助LLM得到图片之间差异的文本描述，text encoder将上述描述编码，同时image encoder分别编码两张图片，计算image embedding的差异，对齐两个模态的差异. | <img src="./images/PC-CLIP.png"  width="640px"/>| [[Github](https://github.com/dsam99/pc_clip)] <br> [[Paper](https://arxiv.org/pdf/2409.09721)] |
| **FFF: Fixing Flawed Foundations in contrastive pre-training results in very strong Vision-Language models** <br> |噪声和Caption的质量对VLM模型的训练十分重要. 文章首先研究分析了影响训练的两个因素：负样本的错误分配，以及质量的Caption和多样性较低. 针对以上问题，作者提出了一种基于图像-文本、图像-图像和文本-文本的positive pair的相似性挖掘算法，减少训练数据中由于语义相似的图像或是相似描述而产生的false negative的数量.  额外的，作者建议使用sigmoid loss训练.  | <img src="./images/FFF.png"  width="640px"/>| [[Paper](https://arxiv.org/pdf/2405.10286)] |


## Improvement & Innovation


| Title | Abstract | Intro | Useful Links |
|:----| :---:| :----: | :---:|
|2022|
| [![Star](https://img.shields.io/github/stars/FlagAI-Open/FlagAI.svg?style=social&label=Star)](https://github.com/FlagAI-Open/FlagAI) <br> **AltCLIP: Altering the Language Encoder in CLIP for Extended Language Capabilities** <br>|文章提出一个概念上简单而有效的方法来训练一个强大的双语/多语多模态表示模型. 使用预训练的多语言文本编码器XLMR替换Clip的文本编码器，通过两阶段的训练模式(Teacher Learning; Contrastive Learning)对齐文本和图像表征. | <img src="./images/AltCLIP.png"  width="640px"/> | [[Github](https://github.com/FlagAI-Open/FlagAI)] <br> [[Paper](https://arxiv.org/pdf/2211.06679)] |
|2023|
| [![Star](https://img.shields.io/github/stars/google-research/big_vision.svg?style=social&label=Star)](https://github.com/google-research/big_vision) <br> **CLIPPO: Image-and-Language Understanding from Pixels Only** <br>| 文章对使用纯基于像素的模型进行文本和图像的多模态学习进行探索。CLIPPO是一个单独的视觉 Transformer，它处理视觉输入或文本，或两者一起，所有都呈现为 RGB 图像（文本在空白图像上渲染，作为纯图像处理）. 所有模态都使用相同的模型参数，包括低级特征处理；也就是说，不存在特定于模态的初始卷积、tokenization 算法或输入嵌入表. CLIPPO仅用一个任务训练--对比学习. | <img src="./images/CLIPPO.png"  width="640px"/> | [[Github](https://github.com/google-research/big_vision)] <br> [[Paper](https://arxiv.org/pdf/2212.08045)] |
| [![Star](https://img.shields.io/github/stars/SunzeY/AlphaCLIP.svg?style=social&label=Star)](https://github.com/SunzeY/AlphaCLIP) <br> **Alpha-CLIP: A CLIP Model Focusing on Wherever You Want** <br>|Clip无法关注到局部区域，针对此问题，文章提出一个增强版本的CLIP，名为Alpha-CLIP. Alpha-CLIP带有一个辅助alpha通道来提示注意区域，并通过构造数百万个RGBA区域-文本对进行了微调。Alpha-CLIP不仅保留了CLIP的视觉识别能力，而且可以精确控制图像内容的重点. 它证明了在各种任务中的有效性，包括但不限于开放世界识别、多模态大语言模型和条件2D /3D生成. 它具有强大的潜力，可作为图像相关任务的通用工具. | <img src="./images/Alpha-CLIP.png"  width="640px"/> | [[Github](https://github.com/SunzeY/AlphaCLIP)] <br> [[Paper](https://arxiv.org/pdf/2312.03818)] |
| [![Star](https://img.shields.io/github/stars/OFA-Sys/Chinese-CLIP.svg?style=social&label=Star)](https://github.com/OFA-Sys/Chinese-CLIP) <br> **Chinese CLIP: Contrastive Vision-Language Pretraining in Chinese** <br>|文章提出中文CLIP预训练模型，并给出了两阶段预训练法: 1.将预训练Clip的图像编码器固定，使用中文RoBERTA替换文本编码器，训练RoBERTA; 2.文本、图像编码器同时训练.| <img src="./images/Chinese-CLIP.png"  width="640px"/> | [[Github](https://github.com/OFA-Sys/Chinese-CLIP)] <br> [[Paper](https://arxiv.org/pdf/2211.01335)] |
| [![Star](https://img.shields.io/github/stars/baaivision/EVA.svg?style=social&label=Star)](https://github.com/baaivision/EVA) <br> **EVA-CLIP: Improved Training Techniques for CLIP at Scale** <br>|大力出奇迹. | <img src="./images/EVA-CLIP.png"  width="640px"/> | [[Github](https://github.com/baaivision/EVA)] <br> [[Paper](https://arxiv.org/pdf/2303.15389)] |
|2024|
| [![Star](https://img.shields.io/github/stars/baaivision/EVA.svg?style=social&label=Star)](https://github.com/baaivision/EVA) <br> **EVA-CLIP-18B: Scaling CLIP to 18 Billion Parameters** <br>|大力出奇迹. | <img src="./images/EVA-CLIP-18B.png"  width="640px"/> | [[Github](https://github.com/baaivision/EVA)] <br> [[Paper](https://arxiv.org/pdf/2402.04252)] |
| [![Star](https://img.shields.io/github/stars/xmed-lab/CLIP_Surgery.svg?style=social&label=Star)](https://github.com/xmed-lab/CLIP_Surgery) <br> **ACloser Look at the Explainability of Contrastive Language-Image Pre-training** <br>|文章发现了CLIP的可解释性有两个问题：1.可视化结果和人的感知是反的；2.可视化有非常多的噪声响应. 针对上述问题，文章阐述了原因，并给出一个train-free的解决方法. <br><br>🧟‍♂️:工作上遇到一个问题，使用clip做相似对对比，cos相似度基本都在0.2+，这篇论文给了答案，同时Cam图的结果提升也很大.👍 | <img src="./images/CLIP_Surgery.png"  width="640px"/> | [[Github](https://github.com/xmed-lab/CLIP_Surgery)] <br> [[Paper](https://arxiv.org/pdf/2304.05653)]  [[知乎](https://www.zhihu.com/question/595372017/answer/2982207851)]|
| [![Star](https://img.shields.io/github/stars/beichenzbc/Long-CLIP.svg?style=social&label=Star)](https://github.com/beichenzbc/Long-CLIP) <br> **Long-CLIP: Unlocking the Long-Text Capability of CLIP** <br>| CLIP的文本token长度被限制为77，而研究表明实际有效长度甚至不到20. 这使得CLIP无法处理详细的描述,限制了其在图像检索和文本到图像生成方面的应用. 本文提出Long-CLIP作为CLIP的即插即用替代方案，它支持长文本输入，保留甚至超越其zero-shot的泛化能力，并调整CLIP潜在空间，使其易于取代CLIP，而无需在下游框架中进行任何进一步的调整.| <img src="./images/Long-CLIP.png"  width="640px"/> | [[Github](https://github.com/beichenzbc/Long-CLIP)] <br> [[Paper](https://arxiv.org/pdf/2403.15378)]|
| [![Star](https://img.shields.io/github/stars/apple/ml-mobileclip.svg?style=social&label=Star)](https://github.com/apple/ml-mobileclip) <br> **MobileCLIP: Fast Image-Text Models through Multi-Modal Reinforced Training** <br>| 文章提出了mobile版的CLIP，与标准的ViT-B/16 CLIP相比，速度提升2.3倍，在38个测试集上accuracy平均提高2.9%. 与标准CLIP相比，训练效率提升10-1000倍. 主要的一些点包括: 文本/图像encoder的重新选择和设计、借助CoCa对训练集生成多个caption进行数据集增强、多个大模型（CLIP）的Model ensembling，以及基于此设计的loss.| <img src="./images/MobileCLIP.png"  width="640px"/> | [[Github](https://github.com/apple/ml-mobileclip)] <br> [[Paper](https://arxiv.org/pdf/2311.17049)]|
| [![Star](https://img.shields.io/github/stars/deepglint/RWKV-CLIP.svg?style=social&label=Star)](https://github.com/deepglint/RWKV-CLIP) <br> **RWKV-CLIP: A Robust Vision-Language Representation Learner** <br>| 本文从数据和模型架构的角度进一步探讨了 CLIP.  同时为了解决噪声数据的问题，提高从互联网爬取的大规模图像-文本数据的质量，作者借助LLM，提出了一个多样化的描述生成框架；此外，作者将RWKV引入VLM中，将 Transformer 的有效并行训练与 RNN 的高效推理相结合. 实验证明其在多个下游任务中实现了最先进的性能.  <br><br>🧟‍♂️: RWKV架构是国人（chinese）原创的，通过高效的线性扩展解决了 Transformer 中的内存瓶颈和二次扩展问题，同时保留了并行训练和强大扩展性的表现特性. 关于RWKV 可查阅[RWKV: Reinventing RNNs for the Transformer Era](https://arxiv.org/pdf/2305.13048)| <img src="./images/RWKV-CLIP1.png"  width="640px"/> <br> <img src="./images/RWKV-CLIP2.png"  width="640px"/> | [[Github](https://github.com/deepglint/RWKV-CLIP)] <br> [[Paper](https://arxiv.org/pdf/2406.06973)]|



## Data


| Title | Abstract | Intro | Useful Links |
|:----| :---:| :----: | :---:|
|2024|
| [![Star](https://img.shields.io/github/stars/apple/ml-veclip.svg?style=social&label=Star)](https://github.com/apple/ml-veclip) <br> **VeCLIP: Improving CLIP Training via Visual-enriched Captions** <br>| 针对网络爬虫的文本-图像数据对，进行caption重写。使用LLaVA生成caption，然后与爬虫得到的描述（AltTexts）做融合，送入Vicuna-1.1得到重写后的caption.  |<img src="./images/VeCLIP.png"  width="640px"/>|  [[Github](https://github.com/apple/ml-veclip)] <br> [[Paper](https://arxiv.org/pdf/2310.07699)] |
| [![Star](https://img.shields.io/github/stars/hammoudhasan/SynthCLIP.svg?style=social&label=Star)](https://github.com/hammoudhasan/SynthCLIP) <br> **SynthCLIP: Are We Ready for a Fully Synthetic CLIP Training?** <br>| 使用全合成文本图像对训练 CLIP 模型，与先前依赖于真实数据的方法有显著区别，SynthCLIP 实现了与在真实数据集上训练的 CLIP 模型相媲美的性能.  |<img src="./images/SynthCLIP.png"  width="640px"/>|  [[Github](https://github.com/hammoudhasan/SynthCLIP)] <br> [[Paper](https://arxiv.org/pdf/2402.01832)] |



## Distillation


| Title | Abstract | Intro | Useful Links |
|:----| :---:| :----: | :---:|
|2023|
| [![Star](https://img.shields.io/github/stars/microsoft/Cream.svg?style=social&label=Star)](https://github.com/microsoft/Cream/tree/main/TinyCLIP) <br> **TinyCLIP: CLIP Distillation via Affinity Mimicking and Weight Inheritance** <br>|文章提出了一种面向大规模语言图像预训练模型的跨模态蒸馏方法:TinyCLIP. TinyClip包括两个核心技术: affinity mimicking and weight inheritance. 基于多级渐进式方案进行affinity mimicking和Weight inheritance，完成Clip模型的压缩及性能保真，在速度和准确度上做了较好的平衡. | <img src="./images/TinyCLIP.png"  width="640px"/> | [[Github](https://github.com/microsoft/Cream/tree/main/TinyCLIP)] <br> [[Paper](https://arxiv.org/pdf/2211.01335)] |
|2024|
| [![Star](https://img.shields.io/github/stars/winycg/CLIP-KD.svg?style=social&label=Star)](https://github.com/winycg/CLIP-KD) <br> **CLIP-KD: An Empirical Study of CLIP Model Distillation** <br>|文章核心目的是利用一个大型的教师CLIP模型来监督一个小型的学生CLIP模型，使得学生CLIP模型可以在保持轻量的前提下显著提升性能. 文章从关系、特征、梯度和对比模式的角度来检验CLIP-知识蒸馏的有效性 . 最后的消融实验表明，使用简单的MSE进行特征蒸馏实现了最好的蒸馏性能. | <img src="./images/CLIP-KD.png"  width="640px"/> | [[Github](https://github.com/winycg/CLIP-KD)] <br> [[Paper](https://arxiv.org/pdf/2307.12732)] |



## Loss


| Title | Abstract | Intro | Useful Links |
|:----| :---:| :----: | :---:|
|2022|
| [![Star](https://img.shields.io/github/stars/goel-shashank/CyCLIP.svg?style=social&label=Star)](https://github.com/goel-shashank/CyCLIP) <br> **CYCLIP: Cyclic Contrastive Language-Image Pretraining** <br>|Clip的目标函数仅使用了跨模态的对比loss，对于单个模态内部和跨模态的i2t、t2i的对称性约束稍显不足，可能会导致图像和文本之前的inconsistent predictions. 如果对称化两个不匹配的图像-文本对之间的相似性以及图像-图像对和文本-文本对之间的相似性，则可以消除图像和文本空间中的不一致（看图片更好理解）. 论文提出了cross-modal consistency和in-modal consistency两种loss，与标准clip相比，在下游的zero-shot分类任务中，准确率有10% − 24%的提升. | <img src="./images/AltCLIP.png"  width="640px"/> | [[Github](https://github.com/goel-shashank/CyCLIP)] <br> [[Paper](https://arxiv.org/pdf/2205.14459)] |
|2023|
| [![Star](https://img.shields.io/github/stars/google-research/big_vision.svg?style=social&label=Star)](https://github.com/google-research/big_vision) <br> **SigLip:Sigmoid Loss for Language Image Pre-Training** <br>|文章提出了一种用于语言图像预训练（SigLIP）的简单成对 Sigmoid 损失. 与使用 Softmax 归一化的标准对比学习不同，Sigmoid 损失仅对图像-文本对进行操作，并且不需要对归一化的成对相似性进行全局视图.  Sigmoid 损失同时允许进一步扩大批量大小，同时在较小的批量大小下也能表现更好. | <img src="./images/SigLip.png"  width="640px"/> | [[Github](https://github.com/google-research/big_vision)] <br> [[Paper](https://arxiv.org/pdf/2303.15343)] |



## Zero-Shot & Few-Shot & Classification


| Title | Abstract | Intro | Useful Links |
|:----| :---:| :----: | :---:|
|2021|
| [![Star](https://img.shields.io/github/stars/gaopengcuhk/CLIP-Adapter.svg?style=social&label=Star)](https://github.com/gaopengcuhk/CLIP-Adapter) <br> **CLIP-Adapter: Better Vision-Language Models with Feature Adapters** <br>| CLIP Adapter是一个为few-shot classfication任务设计的一个插入式模块.在冻住的clip特征上添加一个残差连接的微调器，使得CLIP能更好地应对分类等下游任务.|<img src="./images/CLIP-Adapter.jpg"  width="640px"/>| [[Github](https://github.com/gaopengcuhk/CLIP-Adapter)] <br> [[Paper](https://arxiv.org/pdf/2110.04544)] |
| **CMA-CLIP: Cross-Modality Attention CLIP for Image-Text Classification** <br>| 利用CLIP分类. 作者认为CLIP的训练仅涉及全局图像和文本特征，因此没有对图像块和文本标记之间的细粒度关系进行建模. 这种关系在精细训练的分类任务中非常有用，特别是在只有一小部分图像块或文本标记与分类任务相关的情况下. 并且训练集中不可避免的存在噪声数据.  针对上述问题，作者提出了Cross-Modality Attention CLIP (CMA-CLIP)，实验主要使用社媒和电商领域数据集.  具体的，1.提出sequence-wise 模块来捕获图像块和文本标记之间的细粒度关系，做法就是把clip编码后的图像和文本拼到一起组成新的embedding，送到后续的网络中; 2.设计了modality-wise attention模块，学习各模态的权重，这里直接用了《Multimodal Keyless Attention Fusion for Video Classification》中的Keyless Attention；3. task specific modality-wise attentions， 其实就是多头做multi-task.  整体没啥可说的地方.| <img src="./images/CMA-CLIP.png"  width="640px "/>| [[Paper](https://arxiv.org/pdf/2112.03562)] |
|2022|
| [![Star](https://img.shields.io/github/stars/gaopengcuhk/Tip-Adapter.svg?style=social&label=Star)](https://github.com/gaopengcuhk/Tip-Adapter) <br> **Tip-Adapter: Training-free Adaption of CLIP** <br>|为了提高Clip的few-shot能力，文章提出一种免训练的方法，名为Tip-Adapter. 通过从少样本监督中构建query-key缓存模型来获取适配器的权重. 通过缓存模型，与传统的finetune方法相比，Tip-Adapter表现出极高的效率. |<img src="./images/Tip-Adapter.png"  width="640px"/>| [[Github](https://github.com/gaopengcuhk/Tip-Adapter)] <br> [[Paper]( https://arxiv.org/pdf/2207.09519)]  |
| [![Star](https://img.shields.io/github/stars/ZiyuGuo99/CALIP.svg?style=social&label=Star)](https://github.com/ZiyuGuo99/CALIP) <br> **CALIP: Zero-Shot Enhancement of CLIP with Parameter-free Attention** <br>| 文章出发点是如何在不finetune的情况下，提升clip在下游任务上的zero-shot能力.文章提出了一种parameter-free的注意力模块(CALIP)，引导视觉和文本表示相互交互，并通过注意力探索跨模式信息特征.通过这种方式，图像与文本两个模态特征相互感知，以实现更好的自适应零样本对齐.|<img src="./images/CALIP.png"  width="640px"/>| [[Github](https://github.com/ZiyuGuo99/CALIP)] <br> [[Paper](https://arxiv.org/pdf/2209.14169)]  |
| [![Star](https://img.shields.io/github/stars/KaiyangZhou/CoOp.svg?style=social&label=Star)](https://github.com/KaiyangZhou/CoOp)   <br> **CoOp: Learning to Prompt for Vision-Language Models** <br>| 受NLP领域prompt learning的启发，文章提出了Context Optimization(CoOp)，用于将类CLIP式的视觉语言模型迁移到下游图像识别任务.具体而言，CoOp将预训练模型参数freeze，使用可学习向量对提示的上下文单词进行建模.作者在11个下游任务上验证CoOp的有效性，结果显示CoOp的性能明显好于原始预训练模型如CLIP.|<img src="./images/CoOp.png"  width="640px"/>| [[Github](https://github.com/KaiyangZhou/CoOp)] <br> [[Paper](https://arxiv.org/pdf/2109.01134)] |
| [![Star](https://img.shields.io/github/stars/KaiyangZhou/CoOp.svg?style=social&label=Star)](https://github.com/KaiyangZhou/CoOp)   <br> **CoCoOp: Conditional Prompt Learning for Vision-Language Models** <br>|针对CoOp泛化性差的问题，即:学习到的上下文对数据集中unseen classes的泛化性不好.文章提出Conditional Context Optimization (CoCoOp)，在CoOp基础上，引入一个轻量级的网络，名为Meta-Net:为每张图像生成input-conditional tokens. input-conditional tokens与 CoOp中的learnable vectors叠加，共同参与训练.大量实验表明，对于unseen classes，CoCoOp 比 CoOp 的泛化能力要好得多，甚至显示出超越单个数据集的可迁移性， 并产生更强的领域泛化性能 |<img src="./images/CoCoOp.png"  width="640px"/>| [[Github](https://github.com/KaiyangZhou/CoOp)] <br> [[Paper](https://arxiv.org/pdf/2203.05557)] |
| [![Star](https://img.shields.io/github/stars/LightDXY/FT-CLIP.svg?style=social&label=Star)](https://github.com/LightDXY/FT-CLIP)   <br> **CLIP Itself is a Strong Fine-tuner:Achieving 85.7% and 88.0% Top-1 Accuracy with ViT-B and ViT-L on ImageNet** <br>| 文章通过一系列试验，验证使用不同超参数finetune clip后在下游分类任务的表现.  |<img src="./images/FT-CLIP.png"  width="640px"/>| [[Github](https://github.com/LightDXY/FT-CLIP)] <br> [[Paper](https://arxiv.org/pdf/2212.06138)] |
|[![Star](https://img.shields.io/github/stars/xmed-lab/CLIPN.svg?style=social&label=Star)](https://github.com/xmed-lab/CLIPN)   <br> **CLIPN for Zero-Shot OOD Detection: Teaching CLIP to Say No** <br>| 文章的motivation是通过向CLIP提供positive的语义提示和negative的语义提示，以此让CLIP拥有区分OOD（Out-of-distribution）和ID（in-distribution）样本的能力。具体来说,文章设计了一个可学习的"否定"提示及一个针对"否定"的文本编码器,以捕捉图像中的否定语义.|<img src="./images/CLIPN.png"  width="640px"/>| [[Github](https://github.com/xmed-lab/CLIPN)] <br> [[Paper](https://arxiv.org/pdf/2308.12213v2)] |
|[![Star](https://img.shields.io/github/stars/muzairkhattak/multimodal-prompt-learning.svg?style=social&label=Star)](https://github.com/muzairkhattak/multimodal-prompt-learning)   <br> **MaPLe: Multi-modal Prompt Learning** <br>|诸如CLIP的VLM模型，对文本提示很敏感，需要仔细选择prompt才能发挥良好作用.  针对下游任务，近期比较常用学习提示作为文本输入的方案（learn prompts）微调CLIP.  作者认为只在单个branch中使用提示来调整 CLIP的表示并不是最优的，因为它不允许灵活地动态调整下游任务的两个表示空间.  为解决这个问题，作者提出了MaPLe，在image和text两个分支都进行prompt learning，提高两个模态的表征一致性. 与CoCoOp 相比，MaPLe 表现出了良好的性能，在 11 个不同的图像识别数据集性能平均明显提升. |<img src="./images/MaPLe.png"  width="640px"/>| [[Github](https://github.com/muzairkhattak/multimodal-prompt-learning)] <br> [[Paper](https://arxiv.org/pdf/2210.03117)] |
|2023|
| [![Star](https://img.shields.io/github/stars/linyq2117/TagCLIP.svg?style=social&label=Star)](https://github.com/linyq2117/TagCLIP) <br> **TagCLIP: A Local-to-Global Framework to Enhance Open-Vocabulary Multi-Label Classification of CLIP Without Training** <br>|CLIP在开放词汇分类任务上表现出众，但在多标签分类任务表现不足，因为CLIP的图像编码器中的token经过训练以捕获全局特征，以此区分由对比损失监督的不同文本描述，这对于单标签分类非常有效. 多标签分类恰恰相反，全局特征往往由最突出的类别主导，而 softmax 操作的对比性质加剧了这种情况.  针对以上问题，作者提出TagCLIP ，无需额外训练提升CLIP 的多标签分类能力.  具体而言，1. 作者假设并实验证明，CLIP的图像编码器最后一层的attention操作破坏了空间信息，即前面的CLIP特征保有spatial information ，因此TagCLIP忽略最后一个self-attention操作来维持spatial information.  2.提出一种local-to-global 的方法，即首先经过patch级的分类得到粗标签(很巧妙，输入的text prompt为标签，CLIP的text encoder当作分类器，基于contrastive loss得到的相似度矩阵即为score)，然后使用dual-masking attention refinement（DMAR）优化分类score（直接借助Vit，因为self-attention学习了图像patch之间的关系，可直接用来refine patch之间的相似度），最后经过class-wise reidentification (CWR) 模块得到最终结果（patch对应原图区域送到CLIP，变成一个单标签分类问题，做double check）.|<img src="./images/TagCLIP.png"  width= "640px"/>| [[Github](https://github.com/linyq2117/TagCLIP)] <br> [[Paper](https://arxiv.org/pdf/2312.12828)] |
| [![Star](https://img.shields.io/github/stars/alibaba-mmai-research/CLIP-FSAR.svg?style=social&label=Star)](https://github.com/alibaba-mmai-research/CLIP-FSAR) <br> **CLIP-guided Prototype Modulating for Few-shot Action Recognition** <br>| 文章提出名为CLIP-FSAR的框架，将CLIP引入小样本动作识别任务（few-shot action recognition）. CLIP-FSAR充分利用了 CLIP 模型的多模态知识，并设计了视频-文本对比loss来模拟原始CLIP的训练模式，以及prototype modulation来生成可靠的原型.|<img src="./images/CLIP-FSAR.png"  width= "640px"/>| [[Github](https://github.com/alibaba-mmai-research/CLIP-FSAR)] <br> [[Paper](https://arxiv.org/pdf/2303.02982)] |
|2024|
| [![Star](https://img.shields.io/github/stars/TJU-sjyj/AMU-Tuning.svg?style=social&label=Star)](https://github.com/TJU-sjyj/AMU-Tuning) <br> **AMU-Tuning: Effective Logit Bias for CLIP-based Few-shot Learning** <br>| 提出了一种名为AMU-Tuning的方法，用于改进基于CLIP模型的小样本学习性能. 该方法通过分析关键组件——logit特征、logit预测器和logit融合——来学习有效的logit偏差，并通过利用辅助特征、多分支训练的特征初始化线性分类器以及基于不确定性的融合策略，将logit偏差有效地整合到CLIP中，以提高小样本分类的准确性. |<img src="./images/AMU-Tuning.png"  width="640px"/>| [[Github](https://github.com/TJU-sjyj/AMU-Tuning)] <br> [[Paper](https://arxiv.org/pdf/2404.089588)] |
| [![Star](https://img.shields.io/github/stars/SegoleneMartin/transductive-CLIP.svg?style=social&label=Star)](https://github.com/SegoleneMartin/transductive-CLIP) <br> **Transductive Zero-Shot and Few-Shot CLIP** <br>| 将转导推理（Transductive Inference）引入到VLM中，改进小样本分类问题.  具体做法是利用支持集（support set）中可用的监督（label）来预测查询样本（query）的类别.  <br><br>🧟‍♂️: 转导推理（Transductive Inference）是一种通过观察特定的训练样本，进而预测特定的测试样本的方法.  文章中的公式很多，个人没有完全读懂... |<img src="./images/transductive-CLIP.png"  width="640px"/>| [[Github](https://github.com/SegoleneMartin/transductive-CLIP)] <br> [[Paper](https://hal.science/hal-04534868v1/document)] |
| [![Star](https://img.shields.io/github/stars/The-Shuai/DeIL.svg?style=social&label=Star)](https://github.com/SegoleneMartin/The-Shuai/DeIL) <br> **DeIL : Direct-and-Inverse CLIP for Open-World Few-Shot Learning** <br>| 针对开放世界少样本学习-Open-World Few-Shot Learning (OFSL)问题，作者引入Direct-and-Inverse的概念 ，提出了一种创新的方法DeIL，借助CLIP解决OFSL问题.  DeIL由以下几个模块构成，1. DeIL-Pretrainer：使用CLIPN计算目标图片不属于label类别的概率（Inverse-phase），找到噪声图片，然后利用CLIP修正噪声图片的label（Direct-phase） ; 2. 数据增强模块：借助DALL-E ，对改正后的训练数据集做增强; 3. DeIL-Adapter ：CLIP+Adaptor分类器，loss包括两部分，分类器直接预测的分类损失以及使用 CLIPN 的逆向推理后选择的负样本正样本的对比损失.  注意的是，在推理阶段，仅需要 DeIL-Adapter 内的分类器.... <br><br>🧟‍♂️:  真的绕|<img src="./images/DeIL.png"  width="640px"/>| [[Github](https://github.com/SegoleneMartin/The-Shuai/DeIL)] <br> [[Paper](https://openaccess.thecvf.com/content/CVPR2024/papers/Shao_DeIL_Direct-and-Inverse_CLIP_for_Open-World_Few-Shot_Learning_CVPR_2024_paper.pdf)] |
| [![Star](https://img.shields.io/github/stars/Visual-AI/FROSTER.svg?style=social&label=Star)](https://github.com/Visual-AI/FROSTER) <br> **FROSTER: FROZEN CLIP IS A STRONG TEACHER FOR OPEN-VOCABULARY ACTION RECOGNITION** <br>|本文的研究课题是开集动作识别（open-vocabulary action recognition），具体来说就是测试集中的视频动作类别与训练集动作类别基本没有重叠或重叠程度很小，因此这需要模型具备较高的泛化性能. 目前视频领域主流的做法是基于图像-文本对预训练的模型（主要是CLIP）先在视频数据集上进行fine-tuning，然后再进行测试集的验证. 通过实验探索，我们发现：尽管fine-tuning可以让CLIP具备不错的视频特征提取的能力，但这也会让它失去大规模预训练所得到的泛化性能.  具体的表现就是，那些在闭集（closed-set）场景下优秀的视频分类器们，一到了开集场景下实验性能便大大缩水，甚至不如原先的预训练CLIP模型了. 因此如何让视频模型在fine-tuning的同时还能保持住预训练的知识，成为了本文的研究重点.  作者认为一个基于CLIP的开集动作识别模型应该具备以下特点：1.由于CLIP预训练是没有使用视频数据集的，因此模型需要学习视频域的相关知识（video-specific），用于弥补CLIP在时域建模方面的不足; 2模型需要能保持住预训练CLIP的能力，这对于泛化性能力的保持很重要.   作者提出了一种新的结构FROSTER用来同时实现以上两个目标：针对第一点（时域建模），直接采用cross-entropy loss对finetune模型进行监督. 针对第二点（泛化性特征保持），将frozen clip作为teacher模型对fine-tune模型的特征进行蒸馏，借此希望预训练的能力能够得到很好地保持. 蒸馏过程类似于一个正则化项，确保fine-tune特征不会偏离frozen clip的特征太远. 因为有两个不同的目标，我们需要在它们之间平衡特征学习. |<img src="./images/FROSTER.png"  width="640px"/>| [[Github](https://github.com/Visual-AI/FROSTER)] <br> [[Paper](https://hal.science/hal-04534868v1/document)] <br> [知乎](https://zhuanlan.zhihu.com/p/681708426) |



## Retrieval


| Title | Abstract | Intro | Useful Links |
|:----| :---:| :----: | :---:|
|2022|
| [![Star](https://img.shields.io/github/stars/mzhaoshuai/CenterCLIP.svg?style=social&label=Star)](https://github.com/mzhaoshuai/CenterCLIP) <br> **CenterCLIP: Token Clustering for Efficient Text-Video Retrieval** <br>| 利用CLIP进行文本-视频检索任务，由于视频时域的连续性，会产生很多同质化的token，增加了计算成本. 为了减少冗余视频token的数量，作者提出a multi-segment token clustering算法，找到最具代表性的token并丢弃非必要的. 具体而言，将视频分片，每片单独聚类，且只保留簇的center tokens. 将所有center tokens拼接组成新的visual sequence送如入transformer进行训练. |<img src="./images/CenterCLIP.png"  width="640px"/>  | [[Github](https://github.com/mzhaoshuai/CenterCLIP)] <br> [[Paper](https://arxiv.org/pdf/2205.00823)] |
|2023|
| [![Star](https://img.shields.io/github/stars/ABaldrati/CLIP4Cir.svg?style=social&label=Star)](https://github.com/ABaldrati/CLIP4Cir) <br> **Composed Image Retrieval using Contrastive Learning and Task-oriented CLIP-based Features** <br>| 使用clip进行检索. 分为两步：1.微调clip的text encoder 和image encoder；2.设计一个combiner，将两个模态特征fusion，用这个特征做retrieval. |<img src="./images/CLIP4Cir1.png"  width="640px"/>   <img src="./images/CLIP4Cir2.png"  width="640px"/>| [[Github](https://github.com/ABaldrati/CLIP4Cir)] <br> [[Paper](https://arxiv.org/pdf/2308.11485)] | 
| [![Star](https://img.shields.io/github/stars/aneeshan95/Sketch_LVM.svg?style=social&label=Star)](https://github.com/aneeshan95/Sketch_LVM) <br> **CLIP for All Things Zero-Shot Sketch-Based Image Retrieval, Fine-Grained or Not** <br>| 将CLIP应用于零样本草图检索任务（ZS-SBIR）,针对细粒度的检索做出改进. 主要包括1. prompt learning; 2. 引入一个正则项，使跨类别（数据集）的相对距离一致（最小化每个类别对之间的相对距离分布之间的KL散度）；3. patch shuffling，草图-图片数据对分成NxN的patch，随机做相同的shuffle. 作者认为打乱的草图应该更接近具有相同排列顺序打乱的图片，远离不同排列的图片. 这种permutation-invariance 有助于模型细粒度的理解.  通过这些设计，我与之前最先进的技术相比，性能显着提高了 26.9%.| <img src="./images/Sketch_LVM.png"  width="640px"/>| [[Github](https://github.com/aneeshan95/Sketch_LVM)] <br> [[Paper](https://arxiv.org/pdf/2303.13440)] |
|2024|
| **JINA CLIP: Your CLIP Model Is Also Your Text Retriever** <br>| 传统的text embedding模型，在文本到文本检索中出色，但无法执行cross-modal任务. 诸如Clip之类的模型，有效地对齐图像和文本嵌入，但由于其训练方法和上下文限制，因此未针对文本到文本检索进行优化. 文章提出了一种新颖的多任务对比训练方法，在单个模型中实现了state-of-the-art的文本到文本和文本到图像检索能力. |<img src="./images/JINA-CLIP.png"  width="640px"/>   | [[huggingface](https://huggingface.co/jinaai/jina-clip-v1)] <br> [[Paper](https://arxiv.org/pdf/2405.20204))] |



## Segmentation


| Title | Abstract | Intro | Useful Links |
|:----| :---:| :----: | :---:|
|2022|
| [![Star](https://img.shields.io/github/stars/raoyongming/DenseCLIP.svg?style=social&label=Star)](https://github.com/raoyongming/DenseCLIP) <br> **DenseCLIP: Language-Guided Dense Prediction with Context-Aware Prompting** <br>| 文章提出一种新的框架，将clip的预训练知识迁移到下游分割、目标检测等密集任务. 作者将CLIP 中的图像-文本匹配问题转换为像素文本匹配问题，并使用像素-文本匹配问题，使用像素-文本匹配得分(pixel-text score maps)来指导密集预测模型的学习.  通过进一步使用图像中的上下文信息来提示语言模型，促进模型更好地利用预训练的知识. |<img src="./images/DenseCLIP.png"  width="640px"/>| [[Github](https://github.com/raoyongming/DenseCLIP)] <br> [[Paper](https://arxiv.org/pdf/2112.01518)] |


## Captioning


| Title | Abstract | Intro | Useful Links |
|:----| :---:| :----: | :---:|
|2021|
| [![Star](https://img.shields.io/github/stars/rmokady/CLIP_prefix_caption.svg?style=social&label=Star)](https://github.com/rmokady/CLIP_prefix_caption) <br> **ClipCap: CLIP Prefix for Image Captioning** <br>| 作者提出了CLIPCap模型来生成image captions.具体而言，借助CLIP提取图像embeadding，训练一个mapping network，为每一个caption生成前缀.直接和caption embedding 做结合（concatenation），形成新的embedding，送入GPT-2生成captions. |<img src="./images/ClipCap.png"  width="640px"/>| [[Github](https://github.com/rmokady/CLIP_prefix_caption)] <br> [[Paper](https://arxiv.org/pdf/2111.09734)] |
|2022|
| [![Star](https://img.shields.io/github/stars/DavidHuji/CapDec.svg?style=social&label=Star)](https://github.com/DavidHuji/CapDec) <br> **CapDec: Text-Only Training for Image Captioning using Noise-Injected CLIP** <br>| 文章认为，Clip模型的训练，就是将抽取的文本和图片特征尽可能相似. 基于这个观察，只需要设计一个decoder，仅利用文本数据学习如何将文本特征“翻译”到文本，即可实现图片captioning.  <br><br>🧟‍♂️:脑洞很大，又很合理，喜欢这篇文章~👍|<img src="./images/CapDec.png"  width="640px"/>| [[Github](https://github.com/DavidHuji/CapDec)] <br> [[Paper](https://arxiv.org/pdf/2211.00575)] |
|2023|
| [![Star](https://img.shields.io/github/stars/dhg-wei/DeCap.svg?style=social&label=Star)](https://github.com/dhg-wei/DeCap) <br> **DECAP: DECODING CLIP LATENTS FOR ZERO-SHOT CAPTIONING VIA TEXT-ONLY TRAINING** <br>|文章提出一个简单的框架来实现Zero-shot Captioning. clip的 text encoder作为输入，使用text-only数据训练一个text decoder。同时，为了解决多模态对比学习中的modality gap问题，作者将 image embedding 送入 text decoder 中解码，实现 Zero-shot Captioning.  |<img src="./images/DeCap.png"  width="640px"/>| [[Github](https://github.com/dhg-wei/DeCap)] <br> [[Paper](https://openreview.net/pdf?id=Lt8bMlhiwx2)] |


## Generation


| Title | Abstract | Intro | Useful Links |
|:----| :---:| :----: | :---:|
|2022|
| [![Star](https://img.shields.io/github/stars/hila-chefer/TargetCLIP.svg?style=social&label=Star)](https://github.com/hila-chefer/TargetCLIP) <br> **Image-based CLIP-Guided Essence Transfer** <br>| StyleGAN与CLIP结合实现人脸的风格迁移. <br><br>🧟‍♂️: 略读了一下，之前做过一段时间的Gan，对StyleGan还是比较了解的.  CLIP之前，基于StyleGan做一些人脸属性编辑，方法基本大同小异，效用encoder把图片映射到latent空间，然后在这个latent空间对这个向量做一些手脚，比如可以某个方向偏移、额外利用一些分类、或者做fusion等.  本文依旧是latent空间去加一个shift vector，文章称作essence vector，然后利用CLIP 的image encoder去施加监督. 文章给的生成case看着效果还可以，具体还需要测试. | <img src="./images/TargetCLIP.png"  width="640px"/>  | [[Github](https://github.com/hila-chefer/TargetCLIP)] <br> [[Paper](https://arxiv.org/pdf/2110.12427)] |
| [![Star](https://img.shields.io/github/stars/yael-vinker/CLIPasso.svg?style=social&label=Star)](https://github.com/yael-vinker/CLIPasso) <br> **CLIPasso: Semantically-Aware Object Sketching** <br>| SIGGRAPH 2022 (Best Paper Award) 一种将图像中的物体转换为草图的方法，允许不同层次的抽象.  <br><br>🧟‍♂️关键词: 显着性引导的初始化(a saliency-guided initialization)、基于局部注意力图（based on the local attention maps）、B´ezier curves（贝塞尔曲线是通过一组控制点（P0...Pn）来定义的，P0和Pn是曲线的起点和终点,中间控制点通常不在曲线上）. 首先通过显著图初始化草图的初始点(通过热力图)，然后去迭代更新B´ezier curves 参数（那一组点），通过一个[rasterizer](https://dl.acm.org/doi/pdf/10.1145/3414685.3417871)绘制出草图，借助CLIP计算loss，更新那一组点 ，直到收敛| <img src="./images/CLIPasso.png"  width="640px"/>  | [[Github](https://github.com/yael-vinker/CLIPasso)] <br> [[Paper](https://arxiv.org/pdf/2202.05822)] |

## Other


| Title | Abstract | Intro | Useful Links |
|:----| :---:| :----: | :---:|
|2022|
| [![Star](https://img.shields.io/github/stars/Sense-GVT/DeCLIP.svg?style=social&label=Star)](https://github.com/Sense-GVT/DeCLIP) <br> **Democratizing Contrastive Language-Image Pre-training: A CLIP Benchmark of Data, Model, and Supervision** <br>| 文章提出CLIP-benchmark，是第一个对CLIP及其变体进行评估、分析和测试的基准. 同时，作者提出三个观点，1.数据质量对性能有很大影响；2..某些supervision对卷积网络（ConvNets）和视觉变换器（ViT）有不同的影响. 适当的supervision可以有效地提高CLIP的性能; 3.减少文本编码器可以降低训练成本，但对最终性能影响不大.  此外，作者将DeCLIP与FLIP结合，得到一个性能较好的CLIP变体: DeFILIP.|| [[Github](https://github.com/Sense-GVT/DeCLIP)] <br> [[Paper](https://arxiv.org/pdf/2203.05796)] |
| [![Star](https://img.shields.io/github/stars/IceClear/CLIP-IQA.svg?style=social&label=Star)](https://github.com/IceClear/CLIP-IQA) <br> **Exploring CLIP for Assessing the Look and Feel of Images** <br>| 借助CLIP做IQA，文章实验证明，CLIP 捕获了有意义的priors，可以很好地推广到不同的感知评估 . | <img src="./images/CLIP-IQA.png" width="640px"/> | [[Github](https://github.com/IceClear/CLIP-IQA)] <br> [[Paper](https://arxiv.org/pdf/2207.12396)] |
|2024|
| [![Star](https://img.shields.io/github/stars/MrChenFeng/CLIPCleaner_ACMMM2024.svg?style=social&label=Star)](https://github.com/MrChenFeng/CLIPCleaner_ACMMM2024) <br> **CLIPCleaner: Cleaning Noisy Labels with CLIP** <br>| 文章首次提出利用 VL模型进行样本选择来解决噪声标签学习 (LNL) 问题.  作者使用CLIP，借助其天然的多模态和zero-shot能力，提出一种CLIPCleaner的方法来进行样本选择，并从理论和经验上证实了其有效性（大量公式警告） . | <img src="./images/CLIPCleaner.png" width="640px"/> | [[Github](https://github.com/MrChenFeng/CLIPCleaner_ACMMM2024)] <br> [[Paper](https://www.arxiv.org/pdf/2408.10012)] |

