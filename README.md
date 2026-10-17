# TAIGCIQA-10K: An AIGC Image Quality Assessment Dataset for Theme-Conditioned Image Sets

**TAIGCIQA-10K** is a dataset designed for quality assessment of AI-generated content (AIGC) organized into **theme-conditioned image sets**. Each set consists of four generated images that share a common subject and theme while depicting different actions or scenes. The dataset aims to support research on overall **theme-oriented quality** assessment and multidimensional quality evaluation at the image-set level.

## Contents

- [1. Purpose](#1-purpose)
- [2. Dataset Overview](#2-dataset-overview)
- [3. Dataset Construction](#3-dataset-construction)
- [4. Quality Dimensions and Annotations](#4-quality-dimensions-and-annotations)
- [5. Example: A Lumberjack Bear](#5-example-a-lumberjack-bear)
- [6. Dataset Download](#6-dataset-download)
- [7. Citation](#7-citation)
- [8. License and Contact](#8-license-and-contact)

## 1. Purpose

TAIGCIQA-10K was developed to provide a benchmark for assessing the overall quality of theme-conditioned AIGC image sets, with particular attention to perceptual quality, text–image alignment, subject consistency, and scene richness. It provides generated images, their corresponding text prompts, and subjective quality annotations to support the development and evaluation of objective quality assessment methods.

## 2. Dataset Overview

| Item | Description |
|---|---|
| Dataset name | TAIGCIQA-10K |
| Task | AIGC image quality assessment for theme-conditioned image sets |
| Total number of images | 10,796 generated images |
| Number of image sets | 2,699 sets (10,796 images, with four images per set) |
| Images per set | 4 |
| Subject prompts | 343 subject descriptions |
| Generation models/configurations | 8 |
| Image resolution | 512 × 512 pixels |
| Subjective annotations | Overall theme-oriented quality and fine-grained ratings for perceptual quality, text–image alignment, subject consistency, and scene richness |
| Data components | Theme-conditioned image sets, corresponding text prompts, and subjective quality scores |

The dataset covers a range of subject categories, including people, animals, objects, and environments. It includes both single-subject and multi-subject descriptions, as well as different visual styles, such as realistic, cartoon, oil-painting, abstract, and black-and-white styles.

## 3. Dataset Construction

TAIGCIQA-10K was constructed through subject-prompt design, action-description expansion, image generation, and subjective quality assessment.

### 3.1 Subject-Prompt Design

A total of 343 subject descriptions were selected and organized from the PartiPrompts and MS COCO Captions datasets. They cover categories such as people, animals, objects, and environments. To accommodate different levels of description detail, the subject prompts were designed with varying degrees of specificity:

- Approximately 70% describe primarily the subject itself.
- Approximately 20% describe the subject together with clothing or accompanying items.
- Approximately 10% provide additional details about clothing, items, and their colors.
- Multi-subject descriptions are also included to increase content diversity.

### 3.2 Action-Description Expansion

For each subject prompt, GPT-4o was used to generate four corresponding action descriptions, forming a group of related text prompts. The action descriptions cover indoor scenes, outdoor scenes, and scenarios involving both indoor and outdoor settings. The total length of each subject prompt group was constrained to approximately 10–80 English words.

### 3.3 Image Generation

The subject prompt and its four corresponding action descriptions were supplied to the selected text-to-image generation models to produce four images per set. The images depict the same subject performing different actions or appearing in different scenes. All images were standardized to a resolution of **512 × 512** pixels.

To increase diversity in generation quality and visual appearance, different settings were used for the number of generation iterations and the Classifier-Free Guidance (CFG) parameter:

- **Generation iterations:** 40% of the images used the default number of iterations, 40% used 75% of the default, and the remaining 20% used 125% of the default.
- **CFG:** 50% of the images used the default CFG value, 25% used 0.5 times the default value, and the remaining 25% used 1.5 times the default value.

### 3.4 Subjective Quality Assessment

The subjective study followed the ITU-R BT.500-14 recommendation and recruited 20 graduate students as observers. A Gradio-based interface was used, and the experiments were conducted on a 2560 × 1440 display. Image sets were presented in random order. The four images within a set were displayed simultaneously rather than sequentially, allowing observers to compare them within a unified visual context and helping reduce potential sequence-dependent or history-dependent bias. To mitigate visual fatigue, each participant was restricted to assessing no more than 200 AIGC images per hour.

The assessment consisted of two stages:

1. **Overall assessment:** Observers rated the overall theme-oriented quality of the theme-conditioned image sets.
2. **Fine-grained assessment:** A subset of 3,200 images was sampled according to predefined proportions of prompt length, subject-description detail, and the number of subjects (single-subject or multi-subject cases). This subset was used to collect ratings for perceptual quality, text–image alignment, subject consistency, and scene richness.

Ratings were collected on a 0–5 scale, with the levels ordered from low to high as *poor, bad, fair, good,* and *excellent*. Mean Opinion Scores (MOS) were calculated from the collected ratings. A 95% confidence interval was used to identify and exclude outlier scores, helping improve the robustness of the resulting annotations.

## 4. Quality Dimensions and Annotations

TAIGCIQA-10K includes an overall quality target and four complementary fine-grained dimensions.

| Dimension | Definition | Main aspects |
|---|---|---|
| **Theme-Oriented Quality** | The overall quality with which an image set expresses a unified theme | Subject identity, coordination between actions and scenes, and fidelity to the core semantics of the prompts |
| **Perceptual Quality** | The visual quality of an individual generated image | Clarity, texture, visual distortions, and other perceptually relevant factors |
| **Text–Image Alignment** | The degree of correspondence between a generated image and its associated text prompt | Whether the subject, action, and semantic content described by the prompt are represented in the image |
| **Subject Consistency** | The stability of subject identity and key appearance attributes across images in the same set | Whether facial or other identifying features, clothing, shape, and related attributes remain consistent |
| **Scene Richness** | The degree of scene variation among images that share the same theme | Meaningful variation in background elements, environmental content, and spatial layout rather than repetitive scenes |

Perceptual quality and text–image alignment primarily concern individual images, whereas subject consistency and scene richness require comparisons across images within a set. Together, these dimensions provide complementary information for evaluating overall theme-oriented quality.

## 5. Example: A Lumberjack Bear

The following example illustrates a theme-conditioned image set. All four images share the subject prompt **`A lumberjack bear`**, while each image corresponds to a different action description. The example demonstrates how a common subject and theme can be maintained across images depicting different actions or scenes.

**Subject prompt:** `A lumberjack bear`

<table>
  <thead>
    <tr>
      <th align="center">Image (a)</th>
      <th align="center">Image (b)</th>
      <th align="center">Image (c)</th>
      <th align="center">Image (d)</th>
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
      <td align="center"><b>Action description</b><br /><code>chop wood with an axe</code></td>
      <td align="center"><b>Action description</b><br /><code>prepare for a campfire</code></td>
      <td align="center"><b>Action description</b><br /><code>drink a cup of coffee</code></td>
      <td align="center"><b>Action description</b><br /><code>plan the next task</code></td>
    </tr>
    <tr>
      <td align="center">A lumberjack bear chops wood with an axe.</td>
      <td align="center">A lumberjack bear prepares for a campfire.</td>
      <td align="center">A lumberjack bear drinks a cup of coffee.</td>
      <td align="center">A lumberjack bear plans the next task.</td>
    </tr>
  </tbody>
</table>

*Note:* The action descriptions above use grammatically standard English. If exact reproduction of the original prompt strings is required, please refer to the corresponding text entries in the released dataset.

## 6. Dataset Download

The complete dataset archive is available through Baidu Netdisk:

- **Archive name:** `TAIGCIQA-10K.zip`
- **Download link:** <https://pan.baidu.com/s/1xkASWd5WJxFgJIOhJVgmeQ?pwd=kh2r>
- **Extraction code:** `kh2r`
- **Archive size:** approximately 3.41 GB

The archive contains theme-conditioned images, their corresponding text prompts, and subjective quality annotations. Please refer to the released archive for the exact file names, directory structure, and annotation-field names.

## 7. Citation

If you use TAIGCIQA-10K in your research, please cite the associated paper. The following BibTeX entry provides the basic citation information. Once the final publication record is available, the volume, issue, page range, and DOI can be added based on the official IEEE Xplore record.

```bibtex
@article{liu2026themeiqa,
  title   = {ThemeIQA: Theme-Oriented Generated Image Quality Assessment},
  author  = {Liu, Jing and Liu, Jingrui and Min, Xiongkuo and Yu, Jiexiao and Duan, Huiyu and Zhai, Guangtao},
  journal = {IEEE Transactions on Image Processing},
  year    = {2026}
}
```

## 8. License and Contact

- **License:** Please specify the dataset license before the official release and include the corresponding `LICENSE` file in the repository. This README does not assume that the dataset may be used commercially or redistributed without restriction. Users should comply with the final dataset license and any applicable terms of the generation models.
- **Project repository:** <https://github.com/TJUMMG/TAIGCIQA-10K-Dataset>
- **Corresponding author:** Jing Liu
- **Email:** jliu_tju@tju.edu.cn

Thank you for your interest in TAIGCIQA-10K. We hope this dataset will support research on quality assessment of theme-conditioned image sets, subject consistency, and scene variation.
