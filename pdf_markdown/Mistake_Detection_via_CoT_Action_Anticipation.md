# Mistake Detection in Egocentric Procedural Videos via CoT-based Action Anticipation

**Authors:** Yishan Zou, Chris Nugent, Matthew Burns, Shengli Wu, Tianshi Wang, Meng Liu  
**Venue:** IEEE Transactions on Multimedia (accepted author version, 2026)  
**DOI:** 10.1109/TMM.2026.3716085  
**Code:** https://github.com/zou-y23/EgoPV-MD

> This Markdown file is a structured text conversion of the uploaded PDF. Figures are represented by their captions/descriptions; tables and equations are converted to Markdown/LaTeX where practical.

## Abstract

Mistake detection aims to identify deviations from expected action sequences during task execution, a process particularly crucial in egocentric procedural videos to enable reliable task monitoring and the development of intelligent assistive systems. However, existing approaches often depend on preconceived mistake scenarios and require extensive annotations, limiting their scalability and generalizability. To address these challenges, we propose an anticipation-based detection approach that identifies procedural action mistakes by contrasting anticipated actions with actual observed future actions, enabling flexible and robust detection without reliance on large predefined priors. Specifically, our approach integrates three key components: an action recognition module for precise action identification, an action captioning module for complementary semantic description, and an action anticipation module for predicting subsequent actions. Through the interplay of these components, it facilitates open-ended detection of diverse and unforeseen mistakes, effectively circumventing the reliance on preconceived mistake scenarios and extensive annotations. Notably, the final one employs a large language model to capture sequential and contextual dependencies across actions and narrations, leveraging instruction tuning and few-shot chain-of-thought prompts to enhance stepwise action prediction. Comprehensive experiments on widely used benchmarks not only demonstrate the effectiveness and generalizability of our approach but also offer in-depth ablation analysis that validates the role of each module.

**Index Terms:** Procedural mistake detection; egocentric video understanding; action anticipation; large language models; chain-of-thought.

---

# I. Introduction

Procedural tasks, which require the precise execution of a sequence of goal-directed steps, are common across tasks such as cooking [1], [2], furniture assembly [3], [4], laboratory experiments [5], and equipment maintenance [6]–[8]. In these contexts, even minor deviations from the prescribed procedure can result in failure, safety risks, or inefficiency. Consequently, mistake detection, referring to the automatic identification of errors or deviations during task execution, is critical for both human training and the development of intelligent assistive systems.

To advance mistake detection in such tasks, researchers have increasingly turned to egocentric procedural videos [9], [10], most of which are captured from the first-person perspective using head-mounted cameras. Unlike third-person recordings, egocentric videos provide detailed observations of hand-object interactions and the operator’s viewpoint, offering rich information for analyzing action execution and identifying errors [11]. This makes egocentric videos particularly well suited for mistake detection, as they allow close monitoring of the performer’s actions, assessment of procedural adherence, and timely feedback or corrections.

**Figure 1 (PDF p.1).** Example of procedural mistake detection in the “Setup GoPro” task from HoloAssist [6]. The video is divided into procedural action segments and each segment is assessed for correctness. Example mistakes include actions such as closing the battery door at an inappropriate point, pulling the SD card, or turning on the GoPro at the wrong stage.

The mistake detection system processes an egocentric video stream, identifying the action performed at each procedural step and assessing its correctness. For instance, if the performer skips a necessary step, such as closing the battery door before inserting the battery, or performs an incorrect action, such as removing the SD card during insertion or unintentionally turning on the GoPro while changing the lens, the system identifies the action as a mistake and provides timely feedback. In this way, the success rate of procedural task execution is significantly improved, positioning mistake detection as a vital function of practical applications [9], [11].

Recent advances in mistake detection for egocentric procedural videos have given rise to several methodological paradigms. Direct classification approaches [6], [7] train action classifiers to distinguish between correct and mistaken segments. Indirect inference approaches [12], [13] detect mistakes through auxiliary signals or task state estimation. Graph-based approaches [14], [15], meanwhile, model procedural relationships to identify violations. While promising, these approaches have clear limitations: direct classification relies on scarce annotated data, indirect inference increases data collection costs, and graph-based models struggle to handle unforeseen or complex mistakes beyond predefined patterns.

To address these challenges, we introduce an anticipation-based framework for mistake detection in egocentric procedural videos. This framework integrates three tightly coupled modules: an **Action Recognition Module (ARM)** for identifying ongoing actions, an **Action Captioning Module (ACM)** for enriching comprehension with complementary semantics, and an **Action Anticipation Module (AAM)** that employs a Large Language Model (LLM) to predict upcoming actions through contextual reasoning. By contrasting anticipated actions with observed actions, the framework effectively detects procedural anomalies without relying on preconceived mistake scenarios or predefined task structures. In addition, we incorporate a customized few-shot Chain-of-Thought (CoT) prompting strategy to enhance the LLM’s capacity for sequential action reasoning.

Extensive experiments on benchmark datasets show that our approach consistently outperforms baselines, notably achieving a +6% improvement in F1 score on HoloAssist and an approximately 1× increase in recall on Assembly101, underscoring its superior detection and generalizability.

The main contributions are:

- We introduce a novel anticipation-based detection approach that identifies procedural mistakes in egocentric videos by comparing anticipated actions with actual observed future actions, significantly reducing reliance on preconceived mistake types and annotated data.
- Technologically, the customized few-shot CoT prompting combined with instruction tuning advances sequential action anticipation in procedural tasks, while the dual-branch architecture in the action recognition module ensures precise action identification, forming a solid foundation for action alignment.
- Our approach outperforms baselines and effectively utilizes the unique contributions of each module, as demonstrated by comprehensive evaluations on the Assembly101 and HoloAssist benchmarks.

---

# II. Related Work

## A. Procedural Mistake Detection

Mistake detection in procedural videos is an emerging area of research that currently lacks a unified framework. Existing approaches are diverse and can be broadly categorized into three main categories: **direct classification** [6], [7], **indirect inference** [12], [13], and **graph-based modeling** [14], [15].

Direct classification approaches formulate mistake detection as a binary classification problem at the frame or action-segment level. For instance, works such as [6] and [7] train supervised classifiers to label action instances as correct or mistaken. While these approaches provide explicit mistake identification, their performance heavily depends on large volumes of labeled data, which is a significant limitation due to the rarity and high annotation cost of mistake instances. Moreover, their success is also contingent on the consistency and quality of manual labels.

Indirect inference approaches assess action correctness by leveraging auxiliary cues rather than direct classification. Schoonbeek et al. [12] analyze task-relevant object states post-action to detect anomalies, while Mazzamuto et al. [13] use gaze-tracking to identify abnormal attention patterns indicative of mistakes. Although these approaches can capture subtle signs of incorrect behavior, they often require additional data sources (e.g., object annotations or eye-tracking), which increases system complexity and cost, ultimately limiting scalability.

Graph-based approaches model procedural tasks as structured graphs to capture temporal and logical dependencies among actions. Seminara et al. [14] learn task graphs by optimizing edge weights from action sequences, enabling seamless integration and sequential mistake detection. Ding et al. [15] construct knowledge graphs derived from task transcripts to detect ordering errors by comparing observed and expected action sequences. However, their reliance on task transcripts, predefined structures, or additional error labels limits their applicability in vision-only settings and reduces adaptability to real-world mistake variability. Although AMNAR [16] alleviates some of these issues by relaxing the strict “next-step” transition constraint, allowing transitions within a broader set of “normal actions”, it remains constrained by a fixed definition of what actions are considered normal. In contrast, LLM-based contrastive approaches can reason beyond predefined action schemas, offering greater flexibility and improved adaptability to diverse real-world scenarios.

Beyond the aforementioned approaches, PREGO [17] and TI-PREGO [18] employ dual-branch architectures for real-time mistake detection. While effective for immediate feedback, these methods prioritize speed over deeper visual analysis and optimal detection performance. In contrast, the proposed approach is tailored for accurate and comprehensive mistake detection in egocentric procedural videos, targeting complex and diverse mistake types without relying on additional sensors or manually labeled mistakes. It combines a task-specific action recognition module for egocentric vision with an LLM-driven action anticipation module that captures temporal and contextual dependencies.

## B. Action Recognition

In recent years, action recognition has seen substantial progress, largely driven by advances in deep learning [19]–[21]. Traditional approaches such as SlowFast [22], MViT [23], StillFast [24], and TwinFormer [25] extract temporal and semantic features from video clips using convolutional or Transformer-based backbones. More recent approaches [26], [27] enhance recognition by modeling relationships among actions, scenes, and interactive objects, or by incorporating causal reasoning.

While these approaches perform well on general videos, growing attention has shifted toward the egocentric domain. Foundation models like EgoVLP [28], EgoVLPv2 [29], and EgoVideo [30], pre-trained on egocentric data, provide rich vision-language aligned representations and have achieved strong performance on tasks such as moment retrieval, step grounding, and long-term anticipation. GPT4Ego [31] further proposes a generalizable, zero-shot recognition framework using Vision-Language Models (VLMs). In this work, EgoVideo-V [30] is adopted as the visual backbone in the action recognition module.

## C. Action Captioning

Action captioning aims to generate accurate, semantically rich descriptions for individual actions or behaviors within videos. Early approaches [32] relied on template-based models, which limited caption diversity and expressiveness due to their dependence on predefined linguistic patterns. With the advent of deep learning, encoder-decoder architectures using LSTM became dominant [33], followed by memory-augmented architectures designed to better capture temporal dependencies and contextual semantics [34].

More recently, large-scale VLMs have gained prominence in this domain [35], excelling at modeling fine-grained interactions between visual content and textual output. Pre-trained models such as ViT-GPT-2 [36], BLIP-2 [37], VideoBLIP [38], and EILEV [39] leverage multimodal learning and instruction tuning to generate more generalizable and expressive captions. EILEV [39] is particularly effective in generating coherent, temporally grounded descriptions for complex egocentric actions and is therefore selected for the video captioning module.

## D. Long-Term Action Anticipation

Long-term action anticipation seeks to predict future actions from short egocentric video segments, as defined in the Ego4D benchmark [40]. Ego4D [40] employs a SlowFast encoder along with classification heads to directly predict verb-noun pairs. HierVL [41] introduces hierarchical contrastive learning to align clip- and video-level features with textual annotations, while ICVAE [42] conditions forecasting on generated scenario summaries.

More recent approaches [43]–[46] leverage LLMs to anticipate actions by reasoning over temporal and semantic context. PALM [46] incorporates a VLM with in-context learning and maximal marginal relevance to enhance procedural knowledge retrieval. Building on this line of work, the proposed method integrates CoT prompting to stimulate LLM reasoning and improve context-aware prediction.

---

# III. Method

The framework integrates complementary perspectives of past, present, and anticipated future to assess action correctness within context. As illustrated in Figure 2 of the paper (PDF p.3), the framework contains three core modules: ARM, ACM, and AAM, followed by a contrastive mistake detection stage.

## A. Overview

Given an untrimmed egocentric video segmented into a sequence of $N$ action segments,

$$
V = \{v_1, v_2, \ldots, v_N\},
$$

where segment $v_i$ corresponds to an individual action $a_i$ executed over interval $(t_{i-1}, t_i)$, the goal is to determine whether every segment is appropriate in the context of the overall procedure.

The problem is cast as a **segment-level binary classification task**, where each action segment is classified as either correct or mistaken.

The three modules are:

- **ARM:** identifies the executed action $a_i$ in each segment, producing
  $$A = \{a_1, a_2, \ldots, a_N\}.$$
- **ACM:** generates a natural-language description $c_i$ for each segment, producing
  $$C = \{c_1, c_2, \ldots, c_N\}.$$
- **AAM:** uses the preceding $n$ recognized actions $\{a_{i-n}, \ldots, a_{i-1}\}$ and narrations $\{c_{i-n}, \ldots, c_{i-1}\}$ to predict the expected next action $\hat{a}_i$.

## B. Action Recognition Module (ARM)

Accurate recognition of past actions is critical for both mistake identification and future action prediction. ARM is a dual-branch pipeline that independently performs verb and noun recognition for each segmented clip $v_i$.

Given a segment $v_i$, EgoVideo-V [30] encodes the visual content into spatiotemporal feature representations. The dual-branch recognition architecture independently classifies verbs and nouns using Transformer layers and separate classification heads, each specialized in motion and object features.

The action output is:

$$
a_i = \mathrm{ARM}(v_i), \quad i = 1, \ldots, N. \tag{1}
$$

The recognized sequence $A=\{a_1,\ldots,a_N\}$ serves two purposes: it provides the observed current action used in mistake detection and supplies history to the AAM.

ARM is trained independently with cross-entropy losses for verb and noun classification. Let $y_i^v$ and $y_i^n$ denote ground-truth verb and noun labels, and let $\hat{p}_i^v$ and $\hat{p}_i^n$ denote predicted probability distributions. The total loss is:

$$
\mathcal{L}_{\mathrm{ARM}}
= \frac{1}{N}\sum_{i=1}^{N}
\left[
\mathcal{L}_{\mathrm{CE}}(y_i^v, \hat{p}_i^v)
+ \mathcal{L}_{\mathrm{CE}}(y_i^n, \hat{p}_i^n)
\right]. \tag{2}
$$

**Figure 3 (PDF p.4).** ARM extracts video features using a video encoder, passes them through Transformer layers, and uses separate verb and noun classification heads.

## C. Action Captioning Module (ACM)

Discrete action labels provide concise semantic summaries, but they may omit scene-level and contextual cues. ACM therefore generates natural-language descriptions to complement the action labels.

Following PALM [46], a pretrained egocentric captioning model, EILEV [39], is used to generate a narration $c_i$ for each action segment:

$$
c_i = \mathrm{ACM}(v_i), \quad i = 1, \ldots, N. \tag{3}
$$

The narration sequence $C=\{c_1,c_2,\ldots,c_N\}$ and action sequence $A$ are jointly provided to AAM.

## D. Action Anticipation Module (AAM)

AAM predicts upcoming actions using an LLM that models sequential dependencies and contextual relationships over recognized action sequences and segment narrations. The module combines **instruction tuning**, **few-shot learning**, and **Chain-of-Thought prompting**.

The prompt template in Figure 4 (PDF p.5) concatenates historical action labels and corresponding narrations, then adds task-specific instructions for predicting the next action label. Compared with standard in-context prompting, the designed CoT prompt explicitly introduces step-by-step procedural reasoning before the final prediction.

During instruction tuning, LoRA [47] is used for parameter-efficient fine-tuning. The AAM objective is:

$$
\mathcal{L}_{\mathrm{AAM}}
= \frac{1}{N}\sum_{i=1}^{N}
\mathcal{L}_{\mathrm{CE}}(y_i^a, \hat{p}_i^a), \tag{4}
$$

where $y_i^a$ is the ground-truth action label for segment $i$ and $\hat{p}_i^a$ is the predicted probability distribution over action classes.

At inference, AAM receives up to the previous $n$ actions and narrations:

$$
\hat{a}_i = \mathrm{AAM}
\left(
\{a_{i-n}, \ldots, a_{i-1}\},
\{c_{i-n}, \ldots, c_{i-1}\}
\right). \tag{5}
$$

The implementation uses $n=8$, following PALM [46]. If fewer than eight prior segments are available, the full available history is used.

## E. Contrastive Detection

The anticipated action $\hat{a}_i$ is compared with the observed action $a_i$. A mismatch is treated as a procedural mistake:

$$
d_i =
\begin{cases}
1, & \hat{a}_i \ne a_i,\\
0, & \hat{a}_i = a_i.
\end{cases} \tag{6}
$$

The decision sequence is $D=\{d_1,d_2,\ldots,d_N\}$.

### Algorithm 1. Anticipation-based Contrastive Detection

```text
Input:  Egocentric video V with N segments
Output: Mistake decision sequence D

1. Initialize action sequence A, narration sequence C, and mistake sequence D.
2. For each segment v_i in V:
3.     Recognize current action a_i using ARM and append it to A.
4.     Generate narration c_i using ACM and append it to C.
5.     If i > 1:
6.         s <- max(1, i - n)
7.         P <- A[s : i-1]          # up to n previous actions
8.         Q <- C[s : i-1]          # up to n previous narrations
9.         Predict expected next action a_hat_i using AAM(P, Q).
10.        If a_hat_i != a_i:
11.            Append 1 to D.
12.        Else:
13.            Append 0 to D.
14.    Else:
15.        Append 0 to D.
16. Return D.
```

---

# IV. Experiments

## A. Experimental Settings

### 1. Datasets

The paper compares publicly available procedural-video datasets with mistake annotations and selects **HoloAssist [6]** and **Assembly101 [7]** because both provide mistake annotations, egocentric video, and verb-noun action labels.

### Table I. Public datasets for procedural mistake detection

| Dataset | Mistake | Ego | V-N Labels | #Tasks | #Videos | #Hours |
|---|---:|---:|---:|---:|---:|---:|
| EPIC-Tent [3] | ✓ | ✓ | ✓ | 1 | 24 | 5.4 |
| Assembly101 [7] | ✓ | ✓ | ✓ | 1 | 1,425 | 167 |
| ATA [48] | ✓ | ✗ | ✓ | 3 | 1,152 | 24.8 |
| HoloAssist [6] | ✓ | ✓ | ✓ | 20 | 2,221 | 166 |
| IndustReal [12] | ✓ | ✓ | ✓ | 1 | 84 | 5.8 |
| EgoPER [9] | ✓ | ✓ | ✗ | 5 | 386 | 28 |
| EgoOops [11] | ✓ | ✓ | ✗ | 5 | 50 | 6.8 |

**HoloAssist.** The training set contains over 11 million frames, with approximately 94% labeled correct and 6% labeled as mistakes. The test set contains around 1.7 million frames with approximately 95% correct and 5% mistakes. HoloAssist also provides fine/coarse labels, coarse-grained action sentences, and long-form task descriptions.

**Assembly101.** Assembly101 contains 362 complete assembly and disassembly procedures involving toy vehicles. It includes fine- and coarse-grained action labels, 3D hand poses, participant skill levels, and mistake labels at the coarse segment level. The experiments use egocentric camera views and coarse-grained action segments.

### 2. Baselines

Representative baselines are selected per dataset with matched task settings and metrics. Wang et al. [6] and Sener et al. [7] provide classification-based mistake detection baselines for HoloAssist and Assembly101. Mazzamuto et al. [13] use gaze information; the comparison uses their “One-Class” setting. Patsch et al. [49] introduce hand-pose features; only their RGB-only setting is reported. Ding et al. [15] use no visual input and therefore receive ground-truth action labels at evaluation; they are compared against the proposed Oracle setting.

Two internal baseline variants are also defined:

- **Base:** vanilla action prediction model using standard in-context prompting at inference.
- **PEFT:** parameter-efficient fine-tuned model using standard in-context prompts.
- **PEFT-CoT:** full model combining PEFT with the designed CoT prompting.

### 3. Evaluation Metrics

Mistake detection is evaluated with **Recall, Precision, and F1 score**. Module-specific ablations use **Top-1/Top-5 accuracy** for verb, noun, and composite action recognition/anticipation. Caption quality is evaluated with **edit distance**, following PALM [46].

### 4. Implementation Details

The modules are developed independently and integrated at inference.

- **ARM:** EgoVideo-V [30] encoder followed by Transformer layers and classification heads.
- **ACM:** EILEV [39], pretrained on Ego4D [40].
- **AAM:** DeepSeek-V2-Lite, fine-tuned separately on each dataset.

For ARM training, the paper reports 3 NVIDIA L20 GPUs, 100 epochs, batch size 64, learning rate $5\times 10^{-3}$, weight decay $1\times 10^{-4}$, and Adam optimizer.

For AAM, LoRA with 8-bit quantization is used. It is fine-tuned for 20 epochs with effective batch size 32 (4 GPUs, gradient accumulation 8), learning rate $1\times 10^{-4}$, cosine decay, and warmup ratio 0.1. Test-time experiments are conducted on NVIDIA L20 GPUs.

## B. Performance Comparison

### Table II. Mistake detection results on HoloAssist and Assembly101

| Method | HoloAssist Recall ↑ | HoloAssist Precision ↑ | HoloAssist F1 ↑ | Assembly101 Recall ↑ | Assembly101 Precision ↑ | Assembly101 F1 ↑ |
|---|---:|---:|---:|---:|---:|---:|
| Wang et al. [6] | 26.9 | 12.9 | 17.6 | - | - | - |
| Sener et al. [7] | - | - | - | 46.6 | 30.8 | 37.1 |
| Mazzamuto et al. [13] | 59.0 | 14.0 | 22.6 | - | - | - |
| Patsch et al. [49] | 21.1 | 11.4 | - | - | - | - |
| Ding et al. [15] - Oracle | 43.7 | 13.9 | 21.3 | 67.3 | 48.3 | 54.8 |
| Ours - Base | 77.8 | 13.1 | 22.4 | 69.2 | 31.5 | 43.3 |
| Ours - PEFT | 79.1 | 15.3 | 25.6 | 88.3 | 33.1 | 48.2 |
| Ours - PEFT-CoT | **81.3** | **16.8** | **27.8** | **90.9** | **38.3** | **53.9** |
| Ours - PEFT-CoT - Oracle | 83.3 | 23.1 | 36.2 | 91.0 | 47.6 | 62.5 |

The full PEFT-CoT model achieves the strongest reported non-oracle performance across recall, precision, and F1 on both datasets. On HoloAssist, its F1 is 6.1 percentage points higher than Mazzamuto et al. [13]. On Assembly101, it is 16.8 points higher than Sener et al. [7]. The method particularly improves recall while maintaining competitive precision.

Compared with Ding et al. [15], the Oracle variant has slightly lower precision on Assembly101 but higher recall and F1. The authors argue that Assembly101’s highly structured, narrow task scope particularly benefits graph-based methods, whereas LLM-based prediction shows advantages on the more diverse HoloAssist benchmark.

## C. Ablation Study

All module ablations are conducted on HoloAssist.

### 1. Effect of Action Recognition

Several egocentric video encoders are compared using frozen encoders.

### Table III. ARM ablation on HoloAssist

| Encoder | Verb Top-1 ↑ | Verb Top-5 ↑ | Noun Top-1 ↑ | Noun Top-5 ↑ | Action Top-1 ↑ | Action Top-5 ↑ |
|---|---:|---:|---:|---:|---:|---:|
| SlowFast [22] | 20.52 | 29.98 | 16.60 | 20.31 | 8.82 | 13.51 |
| MViT [23] | 21.88 | 30.01 | 22.35 | 33.79 | 15.50 | 18.17 |
| EgoVLPv2 [29] | 33.88 | 53.14 | 35.63 | 58.79 | 23.15 | 40.13 |
| EgoVideo-V [30] | **49.06** | **67.52** | **70.80** | **82.25** | **41.93** | **60.12** |

EgoVideo-V performs best across all listed metrics and is therefore selected as the ARM visual backbone.

### 2. Effect of Action Captioning

The captioning comparison uses off-the-shelf models without task-specific fine-tuning. Lower edit distance is better.

### Table IV. ACM ablation on HoloAssist

| Model | Verb ED ↓ | Noun ED ↓ | Action ED ↓ |
|---|---:|---:|---:|
| ViT-GPT-2 [36] | 0.7079 | 0.6547 | 0.7905 |
| BLIP-2 [37] | 0.6543 | 0.6342 | 0.7904 |
| VideoBLIP [38] | 0.6319 | 0.6115 | 0.7356 |
| EILEV [39] | **0.5021** | **0.5558** | **0.5826** |

EILEV has the lowest edit distance and is selected as ACM.

### 3. Effect of Action Anticipation

#### a. Different LLMs

The paper first compares LLMs using ground-truth action labels and coarse-grained narrations under standard few-shot in-context prompting without task-specific fine-tuning.

### Table V. Different LLMs in AAM on HoloAssist

| LLM | Verb Top-1 ↑ | Verb Top-5 ↑ | Noun Top-1 ↑ | Noun Top-5 ↑ | Action Top-1 ↑ | Action Top-5 ↑ |
|---|---:|---:|---:|---:|---:|---:|
| Llama-3.1-8B [51] | 26.3 | 29.2 | 39.2 | 55.9 | 31.7 | 38.8 |
| Llama-3.3-70B [51] | 35.4 | 39.2 | 49.2 | 56.3 | 40.9 | 49.1 |
| DeepSeek-V2-Lite [52] | 33.1 | 39.6 | 45.5 | 53.7 | 35.1 | 48.3 |
| DeepSeek-V3 [53] | 38.8 | 45.3 | **53.3** | 61.1 | 41.9 | 50.6 |
| gpt-4o-mini [54] | 33.8 | 36.2 | 41.3 | 56.1 | 35.9 | 48.6 |
| gpt-4o [54] | **41.9** | **47.3** | 51.2 | **63.1** | **43.1** | **52.0** |

Larger models generally perform better. The final implementation uses DeepSeek-V2-Lite (16B) as a balance among performance, availability, fine-tunability, and efficiency.

#### b. Prompting Strategies

### Table VI. DeepSeek-V2-Lite under prompting/fine-tuning settings

| Model | Prompt | Setting | Verb Top-1 ↑ | Verb Top-5 ↑ | Noun Top-1 ↑ | Noun Top-5 ↑ | Action Top-1 ↑ | Action Top-5 ↑ |
|---|---|---|---:|---:|---:|---:|---:|---:|
| Vanilla | Standard | ZS | 20.3 | 25.1 | 31.9 | 35.3 | 25.5 | 31.0 |
| Vanilla | Standard | FS | 33.1 | 39.6 | 45.5 | 53.7 | 35.1 | 48.3 |
| Vanilla | CoT | ZS | 23.1 | 28.3 | 30.8 | 37.1 | 25.9 | 35.2 |
| Vanilla | CoT | FS | 35.6 | 41.3 | 47.1 | 56.6 | 38.3 | 51.7 |
| PEFT | Standard | ZS | 33.5 | 41.8 | 49.6 | 53.3 | 37.5 | 49.1 |
| PEFT | Standard | FS | 36.3 | 45.1 | 51.0 | 57.3 | 41.9 | 51.7 |
| PEFT | CoT | ZS | 35.2 | 43.1 | 50.3 | 55.1 | 40.6 | 48.3 |
| PEFT | CoT | FS | **37.7** | **46.3** | **53.1** | **58.9** | **43.8** | **52.1** |

Few-shot prompting improves over zero-shot prompting in the vanilla setting. CoT further improves action prediction. Fine-tuning produces substantial gains, and the combination of PEFT with few-shot CoT produces the strongest result.

#### c. Ensembling LLMs

The paper evaluates soft-voting combinations of several LLMs using the same ARM/ACM outputs. The individual LLMs use base few-shot prompting without fine-tuning and their Top-5 predictions are aggregated.

### Table VII. Soft-voting LLM ensembles on HoloAssist

| Model Combination | Recall ↑ | Precision ↑ | F1 ↑ |
|---|---:|---:|---:|
| D2L (baseline) | 77.8 | 13.1 | 22.4 |
| D2L + L8B | 78.0 | 14.3 | 24.1 |
| D2L + G4m | 78.5 | 14.1 | 23.9 |
| D2L + D3 | 78.2 | 14.1 | 23.9 |
| D2L + L70B | 78.6 | 16.2 | 26.1 |
| D2L + G4 | 78.9 | 15.8 | 26.3 |
| D2L + L8B + G4m | 80.3 | 15.5 | 26.1 |
| D2L + D3 + L70B | 80.8 | 18.3 | 29.9 |
| D2L + D3 + G4 | 80.1 | 18.3 | 29.8 |
| D2L + L70B + G4 | 81.1 | 19.3 | 31.2 |
| D2L + D3 + L70B + G4 | 79.7 | 19.6 | 31.5 |
| D2L + L8B + G4m + D3 + L70B + G4 | **81.2** | **20.1** | **32.2** |

Here D2L = DeepSeek-V2-Lite, D3 = DeepSeek-V3, L8B = Llama-3.1-8B, L70B = Llama-3.3-70B, G4m = GPT-4o-mini, and G4 = GPT-4o.

The six-model ensemble improves F1 by about 9.8 percentage points over the single D2L baseline. The authors note that diversity among models matters: model similarity can limit ensemble benefits, and carefully selected three- or four-model ensembles can already perform strongly.

## D. Qualitative Results

**Figure 5 (PDF p.9)** presents two HoloAssist examples.

In the first example, the narration is “The person is using a capsule coffee machine to make coffee.” After pulling the lever, the correct next action should be discarding the used coffee capsule rather than pressing the lever again to close the coffee chamber. The proposed model detects the sequence error, while Ding et al. [15] with Oracle action inputs outputs “correct.”

In the second example, the narration is “The person is replacing paper in a printer.” ARM recognizes the current action as “push paper tray,” and PEFT-CoT predicts “push paper tray” as the expected action, yielding a correct non-mistake decision. The Base model predicts “press button” and PEFT with standard prompting predicts “align paper,” leading both variants to false positives.

The qualitative examples are used to argue that narrations and CoT prompting improve procedural reasoning, and that LLM-based prediction can still recover useful expectations even when action recognition is occasionally imperfect.

---

# V. Conclusion

The paper proposes an anticipation-based contrastive framework for procedural mistake detection in egocentric videos. It integrates action recognition, action captioning, and LLM-based action anticipation, and detects mistakes by directly comparing the anticipated next action with the observed current action. Parameter-efficient fine-tuning and Chain-of-Thought prompting improve next-action prediction and, in turn, mistake detection performance.

Experiments on Assembly101 and HoloAssist demonstrate the effectiveness of the approach. The authors also note that the system remains dependent on the quality of its individual modules and can be affected by factors such as occlusion and complex interactions. Future work is proposed in self-supervised learning, enhanced LLM inference, and multimodal fusion.

---

# References

[1] L. Zhou, C. Xu, and J. Corso, “Towards automatic learning of procedures from web instructional videos,” in AAAI, 2018, pp. 7590–7598.

[2] D. Zhukov, J.-B. Alayrac, R. G. Cinbis, D. Fouhey, I. Laptev, and J. Sivic, “Cross-task weakly supervised learning from instructional videos,” in CVPR, 2019, pp. 3537–3545.

[3] Y. Jang, B. Sullivan, C. Ludwig, I. Gilchrist, D. Damen, and W. Mayol-Cuevas, “Epic-tent: An egocentric video dataset for camping tent assembly,” in ICCVW, 2019, pp. 1–9.

[4] Y. Ben-Shabat, X. Yu, F. Saleh, D. Campbell, C. Rodriguez-Opazo, H. Li, and S. Gould, “The ikea asm dataset: Understanding people assembling furniture through actions, objects and pose,” in WACV, 2021, pp. 847–859.

[5] T. Nishimura, K. Sakoda, A. Hashimoto, Y. Ushiku, N. Tanaka, F. Ono, H. Kameko, and S. Mori, “Egocentric biochemical video-and-language dataset,” in ICCV, 2021, pp. 3129–3133.

[6] X. Wang, T. Kwon, M. Rad, B. Pan, I. Chakraborty, S. Andrist, D. Bohus, A. Feniello, B. Tekin, F. V. Frujeri et al., “Holoassist: An egocentric human interaction dataset for interactive AI assistants in the real world,” in ICCV, 2023, pp. 20270–20281.

[7] F. Sener, D. Chatterjee, D. Shelepov, K. He, D. Singhania, R. Wang, and A. Yao, “Assembly101: A large-scale multi-view video dataset for understanding procedural activities,” in CVPR, 2022, pp. 21096–21106.

[8] F. Ragusa, A. Furnari, S. Livatino, and G. M. Farinella, “The meccano dataset: Understanding human-object interactions from egocentric videos in an industrial-like domains,” in WACV, 2021, pp. 1569–1578.

[9] S.-P. Lee, Z. Lu, Z. Zhang, M. Hoai, and E. Elhamifar, “Error detection in egocentric procedural task videos,” in CVPR, 2024, pp. 18655–18666.

[10] R. Peddi, S. Arya, B. Challa, L. Pallapothula, A. Vyas, B. Gouripeddi, Q. Zhang, J. Wang, V. Komaragiri, E. Ragan et al., “CaptainCook4D: A dataset for understanding errors in procedural activities,” in NeurIPS, 2024, pp. 135626–135679.

[11] Y. Haneji, T. Nishimura, H. Kameko, K. Shirai, T. Yoshida, K. Kajimura, K. Yamamoto, T. Cui, T. Nishimoto, and S. Mori, “EgoOops: A dataset for mistake action detection from egocentric videos with procedural texts,” arXiv:2410.05343, 2024.

[12] T. J. Schoonbeek, T. Houben, H. Onvlee, F. Van der Sommen et al., “IndustReal: A dataset for procedure step recognition handling execution errors in egocentric videos in an industrial-like setting,” in WACV, 2024, pp. 4365–4374.

[13] M. Mazzamuto, A. Furnari, Y. Sato, and G. M. Farinella, “Gazing into missteps: Leveraging eye-gaze for unsupervised mistake detection in egocentric videos of skilled human activities,” in CVPR, 2025, pp. 8310–8320.

[14] L. Seminara, G. M. Farinella, and A. Furnari, “Differentiable task graph learning: Procedural activity representation and online mistake detection from egocentric videos,” arXiv:2406.01486, 2024.

[15] G. Ding, F. Sener, S. Ma, and A. Yao, “Spatial and temporal beliefs for mistake detection in assembly tasks,” CVIU, p. 104338, 2025.

[16] W. Huang, Y. Li, Z. Xia, Y. Tang, K. Lin, J. Hu, and W. Zheng, “Modeling multiple normal action representations for error detection in procedural tasks,” in CVPR, 2025, pp. 27794–27804.

[17] A. Flaborea, G. M. D. Di Melendugno, L. Plini, L. Scofano, E. De Matteis, A. Furnari, G. M. Farinella, and F. Galasso, “PREGO: Online mistake detection in procedural egocentric videos,” in CVPR, 2024, pp. 18483–18492.

[18] L. Plini, L. Scofano, E. De Matteis, G. M. D. di Melendugno, A. Flaborea, A. Sanchietti, G. M. Farinella, F. Galasso, and A. Furnari, “TI-PREGO: Chain of thought and in-context learning for online mistake detection in procedural egocentric videos,” arXiv:2411.02570, 2024.

[19] Y. Zhao, H. Zhang, Z. Gao, W. Gao, M. Wang, and S. Chen, “A novel action saliency and context-aware network for weakly-supervised temporal action localization,” TMM, pp. 8253–8266, 2023.

[20] M. Liu, X. Wang, L. Nie, X. He, B. Chen, and T.-S. Chua, “Attentive moment retrieval in videos,” in ACM SIGIR, 2018, pp. 15–24.

[21] M. Liu, X. Wang, L. Nie, Q. Tian, B. Chen, and T.-S. Chua, “Cross-modal moment localization in videos,” in ACM MM, 2018, pp. 843–851.

[22] C. Feichtenhofer, H. Fan, J. Malik, and K. He, “SlowFast networks for video recognition,” in ICCV, 2019, pp. 6202–6211.

[23] H. Fan, B. Xiong, K. Mangalam, Y. Li, Z. Yan, J. Malik, and C. Feichtenhofer, “Multiscale vision transformers,” in ICCV, 2021, pp. 6824–6835.

[24] F. Ragusa, G. M. Farinella, and A. Furnari, “StillFast: An end-to-end approach for short-term object interaction anticipation,” in CVPR, 2023, pp. 3636–3645.

[25] J. Zhou, K.-Y. Lin, Y.-K. Qiu, and W.-S. Zheng, “TwinFormer: Fine-to-coarse temporal modeling for long-term action recognition,” TMM, pp. 2715–2728, 2023.

[26] X. Zhang, Z. Wu, and Y.-G. Jiang, “SAM: Modeling scene, object and action with semantics attention modules for video recognition,” TMM, pp. 313–322, 2021.

[27] Y. Liu, F. Liu, L. Jiao, Q. Bao, L. Li, Y. Guo, and P. Chen, “A knowledge-based hierarchical causal inference network for video action recognition,” TMM, pp. 9135–9149, 2024.

[28] K. Q. Lin, J. Wang, M. Soldan, M. Wray, R. Yan, E. Z. Xu, D. Gao, R.-C. Tu, W. Zhao, W. Kong et al., “Egocentric video-language pretraining,” in NeurIPS, 2022, pp. 7575–7586.

[29] S. Pramanick, Y. Song, S. Nag, K. Q. Lin, H. Shah, M. Z. Shou, R. Chellappa, and P. Zhang, “EgoVLPv2: Egocentric video-language pre-training with fusion in the backbone,” in ICCV, 2023, pp. 5285–5297.

[30] B. Pei, G. Chen, J. Xu, Y. He, Y. Liu, K. Pan, Y. Huang, Y. Wang, T. Lu, L. Wang et al., “EgoVideo: Exploring egocentric foundation model and downstream adaptation,” arXiv:2406.18070, 2024.

[31] G. Dai, X. Shu, W. Wu, R. Yan, and J. Zhang, “GPT4Ego: Unleashing the potential of pre-trained models for zero-shot egocentric action recognition,” TMM, pp. 401–413, 2025.

[32] C. Yan, Y. Tu, X. Wang, Y. Zhang, X. Hao, Y. Zhang, and Q. Dai, “STAT: Spatial-temporal attention mechanism for video captioning,” TMM, pp. 229–241, 2019.

[33] L. Gao, Z. Guo, H. Zhang, X. Xu, and H. T. Shen, “Video captioning with attention-based LSTM and semantic consistency,” TMM, pp. 2045–2055, 2017.

[34] S. Jing, H. Zhang, P. Zeng, L. Gao, J. Song, and H. T. Shen, “Memory-based augmentation network for video captioning,” TMM, pp. 2367–2379, 2023.

[35] Y. Xing, Q. Wu, D. Cheng, S. Zhang, G. Liang, P. Wang, and Y. Zhang, “Dual modality prompt tuning for vision-language pre-trained model,” TMM, pp. 2056–2068, 2023.

[36] A. Kumar, “The illustrated image captioning using transformers,” 2022.

[37] J. Li, D. Li, S. Savarese, and S. Hoi, “BLIP-2: Bootstrapping language-image pre-training with frozen image encoders and large language models,” in ICML, 2023, pp. 19730–19742.

[38] K. P. Yu, “VideoBLIP is a large vision-language model based on BLIP-2 that can generate texts conditioned on videos,” GitHub, 2021.

[39] K. P. Yu, Z. Zhang, F. Hu, and J. Chai, “Efficient in-context learning in vision-language models for egocentric videos,” arXiv:2311.17041, 2023.

[40] K. Grauman, A. Westbury, E. Byrne, Z. Chavis, A. Furnari, R. Girdhar, J. Hamburger, H. Jiang, M. Liu, X. Liu et al., “Ego4D: Around the world in 3,000 hours of egocentric video,” in CVPR, 2022, pp. 18995–19012.

[41] K. Ashutosh, R. Girdhar, L. Torresani, and K. Grauman, “HierVL: Learning hierarchical video-language embeddings,” in CVPR, 2023, pp. 23066–23078.

[42] E. V. Mascaro, H. Ahn, and D. Lee, “Intention-conditioned long-term human egocentric action anticipation,” in WACV, 2023, pp. 6048–6057.

[43] G. Chen, Y.-D. Zheng, J. Wang, J. Xu, Y. Huang, J. Pan, Y. Wang, Y. Wang, Y. Qiao, T. Lu et al., “VideoLLM: Modeling video sequence with large language models,” arXiv:2305.13292, 2023.

[44] S. Wang, Q. Zhao, M. Q. Do, N. Agarwal, K. Lee, and C. Sun, “VAMOS: Versatile action models for video understanding,” arXiv:2311.13627, 2023.

[45] Q. Zhao, S. Wang, C. Zhang, C. Fu, M. Q. Do, N. Agarwal, K. Lee, and C. Sun, “AntGPT: Can large language models help long-term action anticipation from videos?” arXiv:2307.16368, 2023.

[46] S. Kim, D. Huang, Y. Xian, O. Hilliges, L. Van Gool, and X. Wang, “PALM: Predicting actions through language models,” in ECCV, 2024, pp. 140–158.

[47] E. J. Hu, Y. Shen, P. Wallis, Z. Allen-Zhu, Y. Li, S. Wang, L. Wang, W. Chen et al., “LoRA: Low-rank adaptation of large language models,” in ICLR, 2021, pp. 1–13.

[48] R. Ghoddoosian, I. Dwivedi, N. Agarwal, and B. Dariush, “Weakly-supervised action segmentation and unseen error detection in anomalous instructional videos,” in ICCV, 2023, pp. 10128–10138.

[49] C. Patsch, Y. Wu, M. Zakour, D. Salihu, and E. Steinbach, “MistSense: Versatile online detection of procedural and execution mistakes,” in ICCV, 2025, pp. 14528–14537.

[50] J. Kirkpatrick, R. Pascanu, N. Rabinowitz, J. Veness, G. Desjardins, A. A. Rusu, K. Milan, J. Quan, T. Ramalho, A. Grabska-Barwinska et al., “Overcoming catastrophic forgetting in neural networks,” in PNAS, 2017, pp. 3521–3526.

[51] A. Grattafiori, A. Dubey et al., “The Llama 3 herd of models,” arXiv:2407.21783, 2024.

[52] DeepSeek-AI, A. Liu et al., “DeepSeek-V2: A strong, economical, and efficient mixture-of-experts language model,” arXiv:2405.04434, 2024.

[53] DeepSeek-AI, “DeepSeek-V3 technical report,” arXiv:2412.19437, 2025.

[54] OpenAI, J. Achiam et al., “GPT-4 technical report,” arXiv:2303.08774, 2023.
