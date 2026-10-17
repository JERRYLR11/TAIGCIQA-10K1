# TAIGCIQA-10K：面向主题图像组的 AIGC 图像质量评价数据集

**TAIGCIQA-10K** 是一个面向主题条件图像组（theme-conditioned image sets）的 AI 生成内容（AIGC）图像质量评价数据集。它以一组具有共同主题和同一主体的四张生成图像为基本单元，旨在评价图像组的整体**主题导向质量（theme-oriented quality）**，并为多维度、组级 AIGC 图像质量评价研究提供数据支持。

与主要评价单张图像视觉质量或图像与文本匹配程度的传统 AIGC 图像质量评价数据集不同，TAIGCIQA-10K 进一步关注同一主体在不同动作和场景下能否保持身份及关键外观属性，以及不同图像之间是否具有合理的场景变化。

## 目录

- [1. 为什么提出 TAIGCIQA-10K？](#1-为什么提出-taigciqa-10k)
- [2. 数据集规模与组成](#2-数据集规模与组成)
- [3. 数据集如何构建？](#3-数据集如何构建)
- [4. 评价维度与标注](#4-评价维度与标注)
- [5. 示例：A lumberjack bear](#5-示例a-lumberjack-bear)
- [6. 数据下载](#6-数据下载)
- [7. 使用建议](#7-使用建议)
- [8. 论文引用](#8-论文引用)
- [9. 许可协议与联系方式](#9-许可协议与联系方式)

## 1. 为什么提出 TAIGCIQA-10K？

随着文本到图像生成模型的发展，内容创作正逐渐从单张图像生成扩展到围绕同一主题生成一组具有动作推进或场景变化的图像。这类图像组可用于故事板设计、连续视觉叙事，以及长视频生成中的关键帧规划。

然而，现有评价方式存在两个主要局限：

1. **单图评价难以刻画组内关系。** 现有 AIGC 图像质量评价方法通常侧重单张图像的视觉质量以及图像与文本提示词的语义对齐，难以充分评价多张图像中的主体身份是否稳定、关键外观属性是否一致，以及场景变化是否合理。
2. **视频评价的关注重点不同。** 视频质量评价方法通常面向时间连续的视频帧，重点关注运动连续性、时序失真和时空一致性；而主题条件图像组由离散生成的图像构成，其质量更依赖于跨图像的主体一致性和场景变化。

因此，需要一个专门针对主题条件图像组的数据集，为研究人员提供相应的图像、文本提示词和主观质量标注。TAIGCIQA-10K 旨在填补这一数据与评价任务上的空缺。

## 2. 数据集规模与组成

| 项目 | 说明 |
|---|---|
| 数据集名称 | TAIGCIQA-10K |
| 研究任务 | 主题条件图像组的 AIGC 图像质量评价 |
| 图像总数 | 10,796 张生成图像 |
| 图像组数量 | 2,699 组（由 10,796 张图像、每组 4 张计算得到） |
| 每组图像数 | 4 张 |
| 主体提示词 | 343 组主体描述 |
| 生成模型/配置 | 8 种 |
| 图像分辨率 | 512 × 512 |
| 主观标注 | 整体主题导向质量，以及感知质量、文本-图像对齐、主角一致性和场景丰富度等细粒度维度 |
| 数据组成 | 主题图像组、对应文本提示词和主观质量评分 |

数据集涵盖人物、动物、物体和环境等多种主体类别，并包含单主体和多主体描述。图像呈现多种视觉风格，例如写实、卡通、油画、抽象及黑白风格等。

### 生成模型

论文中使用了以下 8 种生成模型或配置：

- FLUX1.0
- IP-Adapter
- Stable Diffusion v1.5
- Stable Diffusion XL（SDXL）
- Stable Diffusion 3（SD3）
- StoryDiffusion（J）
- StoryDiffusion（R）
- StoryDiffusion（U）

## 3. 数据集如何构建？

TAIGCIQA-10K 的构建过程主要包括主体提示词设计、动作描述扩展、多模型图像生成和主观质量评价。

### 3.1 主体提示词设计

数据集从 PartiPrompts 和 MS COCO Captions 中选取并整理了 343 组主体描述，覆盖人物、动物、物体和环境等类别。为兼顾描述复杂度与生成多样性，主体提示词包含不同细节层级：

- 约 70% 的提示词主要描述主体本身；
- 约 20% 的提示词进一步描述主体的服装或随身物品；
- 约 10% 的提示词补充服装、物品及其颜色等细节；
- 同时包含多主体描述，以增加样本的内容多样性。

### 3.2 动作描述扩展

在每组主体提示词的基础上，使用 GPT-4o 扩展出四条与主体相对应的动作描述，组成一组主题提示词。动作设置涵盖室内场景、室外场景，以及同时包含室内和室外场景的情况。每组主体提示词的总长度控制在约 10–80 个英文单词。

### 3.3 多模型图像生成

将主体提示词及其对应的四条动作描述输入所选生成模型，得到每组四张围绕同一主体、呈现不同动作或场景的图像。所有图像统一为 **512 × 512** 分辨率。

为增加生成样本的质量和外观多样性，构建过程中还对生成迭代次数和 Classifier-Free Guidance（CFG）参数采用了不同设置：

- **迭代次数：** 40% 使用默认设置，40% 使用默认迭代次数的 75%，20% 使用默认迭代次数的 125%；
- **CFG：** 50% 使用默认值，25% 使用默认值的 0.5 倍，25% 使用默认值的 1.5 倍。

### 3.4 主观质量评价

主观评价参考 ITU-R BT.500-14 建议，邀请 20 名研究生作为观察者，并使用 Gradio 搭建评分界面。实验采用 2560 × 1440 显示器，图像组按随机顺序呈现；同一组中的四张图像同时展示，而非依次展示，以降低观察者比较相似主体时可能出现的顺序依赖或历史依赖偏差。为减轻视觉疲劳，每位参与者每小时评价的 AIGC 图像数量不超过 200 张。

评价分为两个阶段：

1. **整体评价阶段：** 对数据库中的主题条件图像组进行整体主题导向质量评价。
2. **细粒度评价阶段：** 根据提示词长度、主体描述细节以及单主体/多主体等因素，从数据库中抽取 3,200 张图像构建细粒度评价子集，并对感知质量、文本-图像对齐、主角一致性和场景丰富度进行评价。

评分采用 0–5 分量表，等级由低到高分别为 *poor、bad、fair、good、excellent*。随后根据主观评分计算平均意见分数（Mean Opinion Score，MOS），并使用 95% 置信区间处理异常评分，以提高标注的稳健性。

## 4. 评价维度与标注

TAIGCIQA-10K 的评价体系包括一个整体评价目标和四个互补的细粒度维度。

| 维度 | 含义 | 主要关注点 |
|---|---|---|
| **主题导向质量（Theme-Oriented Quality）** | 图像组整体上围绕共同主题表达内容的质量 | 主体是否保持一致、动作和场景是否协调，以及图像内容是否体现提示词的核心语义 |
| **感知质量（Perceptual Quality）** | 单张生成图像的视觉质量 | 清晰度、纹理、视觉失真及其他影响感知的因素 |
| **文本-图像对齐（Text–Image Alignment）** | 生成图像与相应文本提示词的匹配程度 | 图像是否准确呈现提示词描述的主体、动作和语义内容 |
| **主角一致性（Subject Consistency）** | 同一图像组内主体身份和关键外观属性的稳定程度 | 面部或外观特征、服装、形态等是否在不同图像中保持一致 |
| **场景丰富度（Scene Richness）** | 同一主题下图像之间的场景变化程度 | 背景元素、环境内容和空间布局是否存在有意义的变化，而不是重复单调 |

其中，感知质量和文本-图像对齐侧重单张图像的信息；主角一致性和场景丰富度则需要比较同一组内的多张图像。四个维度共同为整体主题导向质量提供互补的信息。

## 5. 示例：A lumberjack bear

下面展示一个主题条件图像组示例。四张图像共享同一个主体提示词 **`A lumberjack bear`**，并分别对应四种动作描述。该示例说明了数据集如何在保持主体主题统一的同时，呈现不同动作和场景。

**主体提示词（Subject Prompt）：** `A lumberjack bear`

<table>
  <thead>
    <tr>
      <th align="center">图像 a</th>
      <th align="center">图像 b</th>
      <th align="center">图像 c</th>
      <th align="center">图像 d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td align="center"><img src="assets/examples/lumberjack_bear_01_chop_wood.png" alt="A lumberjack bear chopping wood" width="180" /></td>
      <td align="center"><img src="assets/examples/lumberjack_bear_02_campfire.png" alt="A lumberjack bear preparing for a campfire" width="180" /></td>
      <td align="center"><img src="assets/examples/lumberjack_bear_03_coffee.png" alt="A lumberjack bear drinking coffee" width="180" /></td>
      <td align="center"><img src="assets/examples/lumberjack_bear_04_next_task.png" alt="A lumberjack bear planning the next task" width="180" /></td>
    </tr>
    <tr>
      <td align="center"><b>动作描述</b><br /><code>chop wood with an axe</code></td>
      <td align="center"><b>动作描述</b><br /><code>prepare for a campfire</code></td>
      <td align="center"><b>动作描述</b><br /><code>drink a cup of coffee</code></td>
      <td align="center"><b>动作描述</b><br /><code>plan the next task</code></td>
    </tr>
    <tr>
      <td align="center">A lumberjack bear chops wood with an axe.</td>
      <td align="center">A lumberjack bear prepares for a campfire.</td>
      <td align="center">A lumberjack bear drinks a cup of coffee.</td>
      <td align="center">A lumberjack bear plans the next task.</td>
    </tr>
  </tbody>
</table>

> 注：示例中将 `a axe` 规范为语法正确的 `an axe`，并将 `plan next task` 调整为更自然的 `plan the next task`。如果需要严格展示数据文件中的原始提示词，请以数据包内的实际文本为准。

## 6. 数据下载

由于完整数据集文件较大，完整压缩包通过百度网盘提供：

- **文件名：** `TAIGCIQA-10K.zip`
- **下载链接：** <https://pan.baidu.com/s/1xkASWd5WJxFgJIOhJVgmeQ?pwd=kh2r>
- **提取码：** `kh2r`
- **压缩包大小：** 约 3.41 GB

完整数据包包含主题条件图像、对应的文本提示词以及主观质量评分标注。具体文件名、目录组织和字段名称请以实际发布的压缩包为准。

## 7. 使用建议

- **以图像组为基本样本。** 每组四张图像应与对应的主体提示词、动作描述和评分标注保持匹配，不建议在读取或预处理时丢失组内对应关系。
- **开展组级质量评价。** 除了分析单张图像的感知质量和文本匹配情况，还可以研究同一主体在不同图像中的身份一致性，以及背景和空间布局的变化。
- **用于模型训练和测试时注意数据划分。** 论文实验采用 8:1:1 的训练集、验证集和测试集划分。复现实验时，请依据实际发布的数据划分文件或论文设置执行，避免训练和测试数据之间发生不恰当的重叠。
- **区分“丰富”与“越多越好”。** 场景丰富度用于刻画图像组内有意义的场景差异，并不意味着场景变化越大，图像组质量就必然越高；场景变化仍需与共同主题相协调。

本 GitHub 仓库用于公开 **TAIGCIQA-10K 数据集及其相关说明**，不包含 ThemeIQA 模型的实现代码。

## 8. 论文引用

如果你在研究中使用 TAIGCIQA-10K，请引用相关论文。以下 BibTeX 为便于引用的基础版本；正式出版后可根据 IEEE Xplore 上的最终记录补充卷、期、页码和 DOI。

```bibtex
@article{liu2026themeiqa,
  title   = {ThemeIQA: Theme-Oriented Generated Image Quality Assessment},
  author  = {Liu, Jing and Liu, Jingrui and Min, Xiongkuo and Yu, Jiexiao and Duan, Huiyu and Zhai, Guangtao},
  journal = {IEEE Transactions on Image Processing},
  year    = {2026}
}
```

## 9. 许可协议与联系方式

- **许可协议：** 请在正式公开前补充明确的数据集许可协议，并在仓库中提供相应的 `LICENSE` 文件。当前 README 不预设数据集可免费商用或可无限制再分发；使用者应遵守最终发布的许可条款以及相关生成模型的适用条款。
- **项目仓库：** <https://github.com/TJUMMG/TAIGCIQA-10K-Dataset>
- **通讯作者：** Jing Liu
- **联系邮箱：** jliu_tju@tju.edu.cn

感谢你对 TAIGCIQA-10K 的关注。希望该数据集能够为主题条件图像组的质量评价、主体一致性分析和场景变化建模研究提供帮助。
