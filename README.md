# 论文Continuous, Subject-Specific Attribute Control in T2I Models by Identifying Semantic Directions复现

## 摘要

近年来,文本到图像(Text to Image,T2I)扩散模型的发展显著提升了生成图像的质量.然而,由于自然语言的局限性(例如"人”和"老人”之间缺少连续的中间描述集),实现对特定对象的属性的细粒度控制仍然是一个挑战.尽管引入了许多方法来增强模型或图像生成过程以实现这种控制,但无需为模型提供参考图像的方法仅限于实现全局细粒度属性控制或针对特定对象的粗略属性控制,而不是同时实现.这篇文章指出,在常用的令牌级CLIP文本嵌入中存在一些变化方向,可以实现对文本到图像模型中的特定对象的语义上的属性进行细粒度的控制.基于这一观察,提出了高效的无优化和基于鲁棒优化的两种方法,用于从对抗文本中识别特定对象的属性的这些变化方向.相关实验结果表明,这些方向可用于以组合方式(即控制单个对象的多个属性)对特定对象的属性进行细粒度控制,从而增强文本输入的表达能力,且无需调整扩散模型.

## 说明

本文作者给出了原代码，Github链接：https://github.com/CompVis/attribute-control

同时，作者写了详细的README文档，如遇问题请进一步参考作者的README文档

本次复现工作主要对人的年龄属性进行研究，对其余对象与属性的结果做了简单验证，这里不再给出

## 依赖

本次实验使用主要的环境依赖：

- python3.9.20
- CUDA 12.4

其他请参考requirements.txt文件

可以通过以下语句快速配置：

```bash
pip install -r requirements.txt
```

## 使用模型

本次复现工作沿用了作者使用的stable-diffusion-xl-base-1.0

## 训练微调

输入以下命令进行标准微调：

```bash
python learn_delta.py device=cuda:0 model=sdxl prompts=people/age
```

输入以下命令在性能有限的环境下微调：

```bash
python learn_delta.py device=cuda:0 model=sdxl prompts=people/age model.compile=False +model.gradient_checkpointing=True
```

## 验证结果

运行notebooks/inference_sdxl.ipynb文件进行验证，可以通过修改block3以下语句以载入期望的微调结果：

```python
state_dict = torch.load('enter your delta result here')
#使用你期望载入的结果的路径
```

## 已有结果说明

本次只给出本文作者的结果和复现中部分有代表性的结果

- 目录pretrained_deltas下为本文作者提前微调好的结果

- 目录outputs下为复现的结果

  - outputs\learn_delta\people\age\runs\2024-11-24\02-28-23

    - 修改部分超参数的复现结果

      ```python
      #部分超参数
      max_steps: 1000
      batch_size: 1
      grad_accum_steps: 8
      scale_batch_size: 4
      ```

  - outputs\learn_delta\people\age\runs\2024-12-06\14-25-43

    - 修改部分超参数的复现结果

      ```python
      #部分超参数
      max_steps: 2000
      batch_size: 1
      grad_accum_steps: 8
      scale_batch_size: 4
      ```

  - outputs\learn_delta\people\age\runs\2025-01-07\07-35-18

    - 增加学习率余弦败火机制后的复现结果

      ```python
      #部分超参数
      max_steps: 1000
      batch_size: 1
      grad_accum_steps: 5
      scale_batch_size: 4
      ```

  - outputs\learn_delta\people\age\runs\2025-01-08\08-57-38

    - 增加学习率余弦败火机制后的复现结果

      ```python
      #部分超参数
      max_steps: 500
      batch_size: 1
      grad_accum_steps: 8
      scale_batch_size: 4
      ```