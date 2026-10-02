---
source_pdf: "Technical Report for Egocentric Mistake Detection for the HoloAssist Challenge.pdf"
pages: 4
conversion: "Automatic PDF-to-Markdown text conversion; figures are represented by extracted captions/text rather than embedded images."
---

# Technical Report for Egocentric Mistake Detection for the HoloAssist Challenge

<!-- Page 1 -->

Constantin Patsch, Marsil Zakour, Yuankai Wu, Eckehard Steinbach Technical University of Munich

{constantin.patsch, marsil.zakour,yuankai.wu,eckehard.steinbach}@tum.de

## Abstract

In this report, we address the task of online mistake detection, which is vital in domains like industrial automation and education, where real-time video analysis allows human operators to correct errors as they occur. While previous work focuses on procedural errors involving action order, broader error types must be addressed for real-world use. We introduce an online mistake detection framework that handles both procedural and execution errors (e.g., motor slips or tool misuse). Upon detecting an error, we use a large language model (LLM) to generate explanatory feedback. Experiments on the HoloAssist benchmark confirm the effectiveness of our approach, where our approach is placed second on the mistake detection task.

## 1. Introduction

Action detection and recognition methods [9, 10, 13, 15] accurately interpret human actions from video by leveraging spatiotemporal cues. However, a truly intelligent assistant should go beyond recognition and assess the correctness of actions to help users perform tasks accurately. Such systems hold potential for both everyday scenarios like cooking or home maintenance, and industrial applications, such as assembly or mechanical repair. Following recent online mistake detection efforts [4, 5, 11], systems should support real-time inference to identify errors from a continuous video stream, enabling users to react promptly and avoid further consequences. An egocentric perspective is particularly beneficial, capturing task execution from the user’s viewpoint and avoiding occlusions common in static views. Recent methods largely focus on procedural errors—like incorrect sequencing or missed actions [4, 5, 11]—but realworld tasks often involve a broader range of errors. Partial action ordering alone may not fully reflect task success. By also considering execution errors, such as motor mistakes or misuse of tools, a more complete view of task performance emerges. Most current methods further only detect mistakes [4, 11] without providing insights. However, understanding why an action is wrong, such as missing pre-

- requisites or incorrect execution, can help users correct their behavior. To this end, we leverage recent advances in language and vision-language models [6, 7, 12] to generate explanations for detected mistakes using an LLM. Thus, within our approach, we focus on capturing procedural errors that mainly relate to the relative ordering of actions and depend on temporal relations, as well as execution errors, which refer to how the human is performing certain actions. As a result, our approach can better capture the correct execution of a task while being versatile concerning varying error types. Our contribution is three-fold:
- We introduce an online mistake-detection method that captures both procedural and execution errors, enabling a more holistic assessment of task execution beyond temporal ordering alone.
- Our approach generates explanations for detected mistakes, offering users actionable insights and facilitating error resolution.
- Our approach achieves the second place on the HoloAssist [14] benchmark1.

## 2. Methodology

In this section, we give an overview of the design of our approach and explain the individual components with respect to the mistake detection and error explanation tasks.

### 2.1. Mistake Detection

As illustrated in Figure 1, our online setup processes a continuous video stream by dividing the input into frame segments up to the current timestep t. These frames are fed into a visual encoder in combination with a Q-Former, which is based on the architecture introduced in Blip2 [6], to extract visual features. In our implementation, the video encoder is a Vision Transformer (ViT) [3]. We represent the resulting segment of visual features as v = [vt−ts, ..., vt], where v ∈Rts×d1. Here, ts refers to the number of timesteps within the segment, and d1 denotes the dimensionality of the extracted features. Together with learnable queries q ∈Rtq×d2, the feature sequence v is fed into the Video Q-Former. While the

1https://www.codabench.org/competitions/2613/

<!-- Page 2 -->

**Figure 1. Overview: Our method processes a continuous RGB stream using a video encoder and a Q-Former to extract framewise visual relations. A Video Q-Former captures temporal dependencies between the features, using learnable queries (colored squares) inspired by [6]. These features are fed to the mistake classification layer for final predictions. The Video Q-Former output is gated via gm and projected into the LLM embedding space to generate error explanations. Double slashes indicate layers without backpropagation.**

standard Q-Former focuses on capturing spatial information within individual frames, the Video Q-Former is designed to model temporal dependencies across multiple frame-wise features. Despite this extension, the Q-Former retains the overall architecture proposed in [6], similar to the adaptation shown in [16], and is likewise built upon a BERT encoder [2]. The output of this process is a set of temporallyaware features denoted as f ∈Rtq×d2. The features f are passed to the mistake classification layer to obtain the final mistake logits m ∈Rtq. The error explanation generation process is initiated once a mistake is identified based on those logits.

### 2.2. Error Explanation

The error explanation component aims to offer textual reasoning that clarifies why a particular action is incorrect. These explanations address both execution and procedural mistakes detected within the analyzed segment. The Video Q-Former features f, extracted during the mistake detection phase, are passed through a gating mechanism gm to generate these explanations. This is formally defined as:

( 1, if σ(m) ≥τ 0, otherwise (1)

gm(σ(m)) =

where σ denotes the sigmoid function. When the predicted logit exceeds a predefined threshold τ, the features from the Video Q-Former are passed to a projection layer. This projection layer is a linear transformation that maps the features into the LLM embedding space. The projected fea-

tures, along with a prompt, are then input to the LLM to generate the final explanation. This mechanism ensures that explanations are produced only when an error is detected.

## 3. Experiments

We provide qualitative and quantitative results of the mistake detection and explanation generation tasks.

### 3.1. Mistake Detection

For the Holo Assist dataset, we report results via the official competition server, as test annotations are unavailable. As shown in Table 1, our approach significantly outperforms the RGB-only TSformer[1] and improves over GazeCompl [8] by 3.8% without relying on eye gaze input. The dataset’s frequent background segments and mix of procedural and execution errors highlight the robustness of our approach under challenging conditions.

### 3.2. Error Explanation

We further evaluate the explanation capabilities of our approach on the Holo-Assist [14] dataset, which includes detailed error descriptions like ”The battery is upside down” or ”The person accidentally turned on the GoPro.” As shown in Table 2, our method matches or surpasses baseline performance, with notable improvements in semantic similarity, particularly reflected by higher CIDEr scores. Figure 2 shows a qualitative example of HoloAssist.

<!-- Page 3 -->

Holo Assist

Methods Modality F1-Score Correct Mistake Prec Rec Prec Rec

TSformer (Baseline) RGB 35.1 82.6 51.8 12.9 26.9 TSformer (Baseline) RGB + H(GT) 36.2 85.5 43.1 9.7 11.5 GazeCompl (CVPR24) RGB + E 51.0 95.0 92.0 6.0 9.0 266097 (CVPR25) RGB 54.0 96.0 86.0 9.0 26.0 MR-CAS (CVPR25) RGB 57.0 97.0 60.0 8.0 63.0 Ours RGB 55.0 96.0 91.0 11.0 21.0

**Table 1. Performance comparison on Holo Assist [14]. TSformer [1] + H(GT)[14] denotes the TimeSformer model combined with ground truth hand pose as reported by [14]. E denotes the eye gaze information.**

**Figure 2. Qualitative results on a video cutout of the HoloAssist dataset, where exemplary frames are sampled from the indicated segments. The confidence scores indicating the mistake detection probabilities ˆy and the ground truth indicating the mistake annotations are visualized over time.**

Holo Assist

Methods BLEU ROUGEL CIDEr

GT Sampling 0.46 0.50 0.30 Videollama [16] 0.52 0.57 0.68 Video-ChatGPT [7] 0.55 0.58 0.61 Ours 0.53 0.58 0.76

**Table 2. Performance comparison on the error explanation generation task for the Holo Assist [14] dataset.**

## 4. Conclusion

We introduce a versatile online mistake detection approach that also generates mistake explanations. Upon error detection, features are projected into the LLM embedding space to produce explanations. By modeling both spatial execution and temporal action dependencies, our method detects execution and procedural errors.

Experiments on HoloAssist show strong results on both detection and explanation tasks, currently placed second on the benchmark leaderboard. Future work could address very short mistakes and explore additional modalities to improve detection robustness.

<!-- Page 4 -->

## 5. Acknowledgement

We gratefully acknowledge the funding of the Lighthouse Initiative Geriatronics by StMWi Bayern (Project X, grant no. 5140951) and LongLeif GaPa GmbH (Project Y, grant no. 5140953). 1 1

## References

[1] Gedas Bertasius, Heng Wang, and Lorenzo Torresani. Is space-time attention all you need for video understanding? In ICML, page 4, 2021. 2, 3 [2] Jacob Devlin, Ming-Wei Chang, Kenton Lee, and Kristina Toutanova. Bert: Pre-training of deep bidirectional transformers for language understanding. In Proceedings of the 2019 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies, Volume 1 (Long and Short Papers), pages 4171–4186, 2019. 2 [3] Alexey Dosovitskiy, Lucas Beyer, Alexander Kolesnikov, Dirk Weissenborn, Xiaohua Zhai, Thomas Unterthiner, Mostafa Dehghani, Matthias Minderer, Georg Heigold, Sylvain Gelly, Jakob Uszkoreit, and Neil Houlsby. An image is worth 16x16 words: Transformers for image recognition at scale. In International Conference on Learning Representations, 2021. 1 [4] Alessandro Flaborea, Guido Maria D’Amely di Melendugno, Leonardo Plini, Luca Scofano, Edoardo De Matteis, Antonino Furnari, Giovanni Maria Farinella, and Fabio Galasso. Prego: online mistake detection in procedural egocentric videos. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 18483– 18492, 2024. 1 [5] Shih-Po Lee, Zijia Lu, Zekun Zhang, Minh Hoai, and Ehsan Elhamifar. Error detection in egocentric procedural task videos. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 18655– 18666, 2024. 1 [6] Junnan Li, Dongxu Li, Silvio Savarese, and Steven Hoi. Blip-2: Bootstrapping language-image pre-training with frozen image encoders and large language models. In International conference on machine learning, pages 19730– 19742. PMLR, 2023. 1, 2 [7] Muhammad Maaz, Hanoona Rasheed, Salman Khan, and Fahad Shahbaz Khan. Video-chatgpt: Towards detailed video understanding via large vision and language models. In Proceedings of the 62nd Annual Meeting of the Association for Computational Linguistics (ACL 2024), 2024. 1, 3

[8] Michele Mazzamuto, Antonino Furnari, and Giovanni Maria Farinella. Eyes wide unshut: Unsupervised mistake detection in egocentric procedural video by detecting unpredictable gaze. arXiv preprint arXiv:2406.08379, 2024. 2 [9] Constantin Patsch and Eckehard Steinbach. Self-attention based action segmentation using intra-and inter-segment representations. In ICASSP 2023-2023 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP), pages 1–5. IEEE, 2023. 1 [10] Thinh Phan, Khoa Vo, Duy Le, Gianfranco Doretto, Donald Adjeroh, and Ngan Le. Zeetad: Adapting pretrained visionlanguage model for zero-shot end-to-end temporal action detection. In Proceedings of the IEEE/CVF winter conference on applications of computer vision, pages 7046–7055, 2024. 1 [11] Luigi Seminara, Giovanni Maria Farinella, and Antonino Furnari. Differentiable task graph learning: Procedural activity representation and online mistake detection from egocentric videos. In The Thirty-eighth Annual Conference on Neural Information Processing Systems, 2024. 1 [12] Hugo Touvron, Louis Martin, Kevin Stone, Peter Albert, Amjad Almahairi, Yasmine Babaei, Nikolay Bashlykov, Soumya Batra, Prajjwal Bhargava, Shruti Bhosale, et al. Llama 2: Open foundation and fine-tuned chat models. arXiv preprint arXiv:2307.09288, 2023. 1 [13] Xiang Wang, Shiwei Zhang, Zhiwu Qing, Yuanjie Shao, Zhengrong Zuo, Changxin Gao, and Nong Sang. Oadtr: Online action detection with transformers. In Proceedings of the IEEE/CVF International Conference on Computer Vision, pages 7565–7575, 2021. 1 [14] Xin Wang, Taein Kwon, Mahdi Rad, Bowen Pan, Ishani Chakraborty, Sean Andrist, Dan Bohus, Ashley Feniello, Bugra Tekin, Felipe Vieira Frujeri, Neel Joshi, and Marc Pollefeys. Holoassist: an egocentric human interaction dataset for interactive ai assistants in the real world. In Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV), pages 20270–20281, 2023. 1, 2, 3 [15] Le Yang, Junwei Han, and Dingwen Zhang. Colar: Effective and efficient online action detection by consulting exemplars. In Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, pages 3160–3169, 2022. 1 [16] Hang Zhang, Xin Li, and Lidong Bing. Video-llama: An instruction-tuned audio-visual language model for video understanding. In Proceedings of the 2023 Conference on Empirical Methods in Natural Language Processing: System Demonstrations, pages 543–553, 2023. 2, 3
