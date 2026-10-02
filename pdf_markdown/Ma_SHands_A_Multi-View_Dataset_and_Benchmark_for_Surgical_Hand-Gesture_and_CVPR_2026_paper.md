---
source_pdf: "Ma_SHands_A_Multi-View_Dataset_and_Benchmark_for_Surgical_Hand-Gesture_and_CVPR_2026_paper.pdf"
pages: 12
conversion: "Automatic PDF-to-Markdown text conversion; figures are represented by extracted captions/text rather than embedded images."
---

# SHANDS: A Multi-View Dataset and Benchmark for Surgical Hand-Gesture and Error Recognition Toward Medical Training

<!-- Page 1 -->

Le Ma1 Thiago Freitas dos Santos1 Nadia Magnenat-Thalmann1 Katarzyna Wac2

1MIRALab, 2Quality of Life Technologies Lab (QoL Lab), University of Geneva

le.ma@unige.ch, thiago.freitas@miralab.ch, thalmann@miralab.ch, katarzyna.wac@unige.ch

## Abstract

In surgical training for medical students, proficiency development relies on expert-led skill assessment, which is costly, time-limited, difficult to scale, and its expertise remains confined to institutions with available specialists. Automated AI-based assessment offers a viable alternative, but progress is constrained by the lack of datasets containing realistic trainee errors and the multiview variability needed to train robust computer vision approaches. To address this gap, we present Surgical-Hands (SHANDS), a large-scale multi-view video dataset for surgical hand-gesture and error recognition for medical training. SHANDS captures linear incision and suturing using five RGB cameras from complementary viewpoints, performed by 52 participants (20 experts and 32 trainees) each completing three standardized trials per procedure. The videos are annotated at the frame level with 15 gesture primitives and include a validated taxonomy of 8 trainee error types, enabling both gesture recognition and error detection. We further define standardized evaluation protocols for single-view, multi-view, and cross-view generalization, and benchmark state-of-the-art deep learning models on the dataset. SHANDS is publicly released to support the development of robust and scalable AI systems for surgical training grounded in clinically curated domain knowledge.

## 1. Introduction

Surgical proficiency depends on the ability to execute precise hand movements and handle instruments correctly to prevent technical errors [1, 2]. Traditionally, such proficiency is assessed through expert observation [3, 4], a process that is expensive, time-intensive, and limited in scalability, especially in surgical education, where handson training is essential. Recent advances in computer vision and AI promise scalable and objective skill evaluation [5–7]. However, these methods remain underdeveloped

due to a lack of datasets capturing realistic surgical errors and the variability in hand motion across viewing angles.

Existing surgical video datasets can be categorized into two main groups, each addressing only part of the skill assessment challenge. Robotic-surgery datasets such as JIGSAWS [8] provide synchronized video and kinematic streams but fail to capture the manual hand–tool coordination central to open surgical training, where dexterity develops without robotic mediation. Endoscopic phase datasets, such as Cholec80 [9] and M2CAI16 [10], offer single-view laparoscopic recordings but lack the finegrained gesture boundaries and clinically validated error labels needed for targeted feedback. Moreover, because multi-view recordings of open procedures are largely absent from these datasets, insights from outside the domain help motivate their value. Multi-view action recognition datasets like NTU RGB+D [11] and Assembly101 [12] demonstrate the value of synchronized viewpoints for modeling complex manipulations. Yet, they provide no clinical specificity, expert supervision, or taxonomy of surgical errors.

To address these limitations, we introduce SurgicalHands (SHANDS), a large-scale, multi-view RGB video dataset for open surgical hand-gesture and error recognition. As illustrated in Figure 1, the dataset was recorded using five synchronized cameras capturing complementary viewpoints at 25 frames per second with a resolution of 640×480 pixels, substantially reducing occlusion and enabling cross-view inference. The dataset includes recordings of 52 participants (20 expert surgeons and 32 medical trainees), each performing standardized incision and suturing procedures in three independent trials. Every frame is annotated with 15 gesture primitives representing the fundamental components of surgical action, and with a clinically validated taxonomy of eight error types, including improper grip, incorrect trajectory, tissue damage, and insufficient tension. Data were collected using ex vivo chicken tissue to ensure both realistic tool–tissue interactions and standardization across sessions.

We benchmark several state-of-the-art video recogni-

<!-- Page 2 -->

C2

C3 C4

C5

C1

Gestures

Fine Labels

Incision Suturing

Incision Suturing Transition

I1 I2 I3 I4 I5 S1 S2 S3 S4 S10 ….

C1 C2 C3 C4 C5

**Figure 1. The SHANDS multi-view dataset. A five-camera RGB setup (C1–C5) records synchronized multi-view videos of incision and suturing tasks on ex vivo tissue. The top row shows a gesture-annotated timeline with fine-grained labels (I1–I5 for incision, S1–S10 for suturing) and transition boundaries. The bottom rows show aligned frames from all views for both incision and suturing, highlighting the complementary spatial information captured across cameras.**

tion architectures, including VideoMAE [13] and TimeSformer [14], under standardized protocols for single-view, multi-view, and cross-view generalization. Our experiments demonstrate that integrating multi-view information significantly enhances performance for both gesture recognition and error detection, underscoring the dataset’s value for advancing solutions in AI-driven surgical training. In summary, our main contributions are as follows:

- We introduce SHANDS, to the best of our knowledge, the first multi-view RGB dataset for open surgical hand-gesture and error recognition, captured from five synchronized viewpoints and involving 52 participants across expert and trainee skill levels.
- We provide fine-grained, frame-level annotations for 15 surgical gesture primitives and an eight-category error taxonomy, including synchronized label propagation to support single- and multi-view learning.
- We establish comprehensive benchmarks using stateof-the-art video backbones and multi-view fusion methods, providing standardized evaluation protocols for future research on robust, scalable AI-based surgical skill assessment.

## 2. Related Work

This section reviews prior research most relevant to our work. We organize it into three directions: (i) surgical

video datasets that have enabled automatic skill assessment and tool tracking; (ii) multi-view action recognition studies demonstrating the importance of viewpoint diversity in understanding fine-grained gestures; and (iii) action recognition methods that form the algorithmic foundation for gesture segmentation and temporal modeling. Together, these lines of research highlight the gap of multi-view, clinically validated datasets for open-surgical gesture and error recognition, which motivates the creation of SHANDS.

### 2.1. Surgical Video Datasets

Datasets in robotic surgery, such as JIGSAWS [8], introduced synchronized kinematic and video data for benchtop tasks, followed by DESK [15] and ROSMA [24], which broadened the coverage of robotic tasks. However, these datasets capture robot-mediated interactions rather than manual hand–tool coordination. Works also center on endoscopic video analysis. Datasets including Cholec80 [9], M2CAI16 [10], CATARACTS [16], and AutoLaparo [25] have advanced phase recognition, tool detection, and workflow segmentation. However, their single-camera laparoscopic setting omits direct visualization of the surgeons’ hands and instruments. Broader operating room datasets such as MM-OR [26], CPR-Coach [27], and 4D-OR [28] model complex team dynamics using multimodal sensors, but their emphasis on holistic scene understanding diverges

<!-- Page 3 -->

Dataset Hours Participants Views Classes Errors Avg. V. Length (s) Domain

Surgical Datasets JIGSAWS [8] 3.5 8 1 15 – 120 Robotic bench-top DESK [15] 5.2 11 1 7 – -Robotic bench-top Cholec80 [9] 50 13 1 Phases only – 2300 Endoscopic OR M2CAI16 [10] 16 -1 Phases only – -Endoscopic OR CATARACTS [16] 9 50 1 Steps/Tools – 656 Microsurgery MVOR [17] – -3 Pose/Boxes – – OR multi-view EgoSurgery [18] 15 8 1 Phases/Tool Boxes – – Open Surgery

Multi-View Action Recognition Datasets NTU RGB+D 120 [19] 200 106 3 120 actions – -General actions PKU-MMD [20] 50 66 3 51 actions – 3.8 General actions Assembly101 [12] 513 53 8 101 coarse/1380 – 426 Assembly tasks

Action Quality Assessment Datasets player videos MTL-AQA [21] 1.5 1412 1 Dive scores – 4.1 Sports (diving) FineDiving [22] 3.5 3000 1 Dive scores – 4.2 Sports (diving) BASKET [23] 4477 32,232 1 20 skills – 500 Sports (basketball)

SHands (Ours) 10 52 5 2 tasks, 15+8 8 660 Open-Surgery Hands

**Table 1. Comparison with existing surgical and multi-view action datasets. Our dataset is the first to provide synchronized multi-view open hand surgery videos with comprehensive gesture and error annotations.**

from our goal of detailed manual technique evaluation. Further specialized datasets (AVOS [29], C2D2 [30], and Surgical-VQLA [31]) investigate tool tracking, crowdsourced skill annotation, and video question answering. Yet these datasets typically lack frame-level gesture boundaries and systematic error taxonomies, both of which are critical for generating automated feedback in training systems. Table 1 provides a consolidated comparison of these datasets alongside multi-view action-recognition and action-quality assessment benchmarks. The overview highlights a consistent pattern: surgical datasets offer clinical realism but remain predominantly single-view or lack systematic error annotation. In contrast, multi-view action datasets provide complementary strengths but no surgical supervision. SHANDS is the first to unify multi-view capture, finegrained gestures, and clinically validated error labeling for open manual surgery.

### 2.2. Multi-View Action Recognition

Outside the surgical domain, extensive research has explored how multi-view capture enhances the recognition of complex human activities. Large-scale benchmarks such as NTU RGB+D [11, 19] and PKU-MMD [20] established cross-view protocols using RGB-D sensors to evaluate viewpoint invariance. For fine-grained manipulation, H2O [32] and Assembly101 [12] focus on hand–object interactions across synchronized views, offering valuable insight into tool usage and coordination. Exploring extended procedural contexts: Ego-Exo4D [33] pairs egocentric and exocentric recordings to study complementary

perspectives, while EgoExoLearn [34] addresses weakly supervised alignment between them. Moreover, instructional video corpora such as ATA [35], HoloAssist [36], LOGO [37], and VidChapters-7M [38] facilitate segmentation and procedural reasoning in non-clinical settings. These studies collectively demonstrate that viewpoint diversity enhances robustness and contextual understanding in action recognition. Motivated by this, we extend the multi-view paradigm to surgical gesture and error recognition, integrating diverse camera capture with clinically validated annotations. The first, to the best of our knowledge, to apply this principle in surgical skill assessment.

### 2.3. Action Recognition Methods

Increasingly powerful architectures drive progress in video understanding. Transformer-based and hierarchical backbones—SlowFast [39], MViT [40], TimeSformer [14], and ViViT [41]—have achieved strong performance on generic datasets. Self-supervised and masked pretraining frameworks such as VideoMAE [13, 42, 43] further improve representation quality with reduced supervision. Specifically in the surgical domain, specialized pretraining approaches such as Endo-FM [44], EndoViT [45], and SurgMAE [46] tailor these architectures for endoscopic imagery but do not address multi-camera open-surgery cases. Multi-view methods extend these architectures to learn view-invariant or view-fused representations. For instance, DVANet [47] disentangles view and action features through contrastive learning, and M3Net [48] fuses embeddings across views for few-shot recognition. Self-supervised vari-

<!-- Page 4 -->

ants such as CVRL [49] and MV2-MAE [50] leverage cross-view reconstruction, while HCTransformer [51] and graph-based reasoning models [52, 53] incorporate human priors to enhance viewpoint robustness. For temporal gesture segmentation, architectures like MS-TCN [54], MS-TCN++ [55], and ASFormer [56] refine predictions through multi-stage temporal modeling. Within surgical applications, transformer-based gesture recognition and phase understanding models, e.g., SKiT [57], LoViT [58], multimodal fusion approaches [59], achieve state-of-the-art results on JIGSAWS and Cholec80, yet remain limited to single-view laparoscopic footage.

## 3. The Surgical Hand-Gesture (SHANDS) Dataset

Despite these advances, current models remain constrained by the limitations of existing datasets. Specifically, their reliance on single-view video and the absence of error labels or open-surgery hand–tool interactions. To address these gaps, we introduce SHANDS, a multi-view video dataset designed for fine-grained surgical hand-gesture recognition and clinically validated error detection in open-surgery training. It captures two foundational manual skills (incision and suturing) performed on ex vivo chicken tissue, providing realistic tool–tissue interactions under standardized and reproducible conditions.

### 3.1. Multi-View Capture System

As depicted in Figure 1, the capture rig comprises five static RGB cameras (Canon PowerShot A2500; 25 fps, 640×480 px) mounted on a rigid frame surrounding the surgical workspace. These cameras are positioned to provide complementary viewpoints, combining top-down and oblique angles to minimize occlusions and facilitate crossview inference. Frame-accurate temporal alignment is guaranteed through hardware-level synchronization using the Canon Hack Development Kit (CHDK). Supported by a measurable verification procedure, this synchronization mechanism eliminates the need for post-hoc registration and enables direct pixel-level correspondence across all views. Ultimately, this setup produces a rich and multi-perspective representation of the procedure, effectively capturing the fine nuances of hand–tool–tissue interactions.

### 3.2. Participants and Experiment Protocol

The dataset comprises 52 participants: 20 certified surgeons with clinical experience and 32 medical trainees at different training stages. This composition provides natural variability in performance and technique, which is essential for developing models that generalize across skill levels. Before data collection, participants provided written informed consent under an ethical protocol approved

by the University of Geneva’s Ethical Committee. To ensure anonymity, only participants’ hands and forearms were recorded. For the medical students, each session followed a standardized training and evaluation protocol. Before the recording began, students first watched a reference video showing an expert performing the target procedures: linear incision and suturing. A trained member of the research team then provided verbal guidance, following the instructions of a medical professional, explaining the correct sequence of gestures, proper instrument handling, and the expected surgical technique. After this brief instructional phase, students independently performed both procedures, repeating each three times to capture performance variability and enable consistent evaluation. The expert surgeons followed the same repetition protocol—three trials per procedure—without the pre-training step. All trials were carried out at each participant’s self-determined pace, without external intervention, to preserve natural execution variability rather than constrained or scripted motions. This experiment protocol aims to ensure that SHANDS reflects the realistic range of surgical performance, from trainee learning behaviors to expert fluency, under ethically compliant and reproducible conditions.

### 3.3. Gesture and Error Annotation

SHANDS introduces a two-level annotation taxonomy, developed in collaboration with surgical educators, to support gesture recognition and error detection. This hierarchical scheme comprises: (1) 15 Gesture Primitives(Table 2), which decompose incision (I1–I5) and suturing (S1–S10) into clinically meaningful units with frame-level temporal boundaries; and (2) 8 Error Categories(Table 3), reflecting expert-validated trainee mistake criteria. For suturing, trials are standardized to require the completion of three knots. To ensure temporal completeness, a Background/Idle class is defined for segments containing no meaningful gestures or unlabeled motions. Annotations were performed by a single annotator (guided by an educator’s definitions) and verified by an expert surgeon to ensure label consistency. This dual-layered approach facilitates research in multi-view fusion, temporal segmentation, and expert-interpretable error recognition.

### 3.4. Dataset Statistics

SHands comprises five synchronized RGB streams per recording. Trainees contributed 90.9% of the footage, while experts provided 9.1%. We annotated 55% of trainee and 92% of expert recordings to balance data diversity with high-quality benchmarking; unlabelled sequences remain available for semi-supervised learning (Figure 2). The dataset features 15 gesture classes and 8 error types. Su-

<!-- Page 5 -->

Task ID Description

I1 Grasping scalpel and forceps I2 Positioning toward tissue site I3 Stabilizing surrounding tissue I4 Executing incision with blade I5 Retracting incised tissue

Incision

S1 Grasping forceps and needle holder S2 Securing needle in holder S3 Positioning toward tissue site S4 Elevating wound edge S5 Passing needle through tissue S6 Pulling suture with needle holder S7 Making a knot S8 Drop the needle holder, grasp a scissors S9 Cutting excess suture S10 Re-grasping needle holder

Suturing

**Table 2. Surgical gesture taxonomy for incision and suturing. Each gesture represents a clinically meaningful primitive motion used in open-surgery training.**

Task ID Error Description

Incision II1 Improper instrument grip II2 Incorrect angle II3 Repetitive cutting

IS1 Incorrect needle handling IS2 Excessive force application IS3 Faulty knot technique IS4 Manual thread manipulation IS5 Result incorrect

Suturing

**Table 3. Common error taxonomy curated by surgical educators. Each category denotes a clinically relevant technical deviation observed during incision or suturing.**

turing dominates the temporal distribution due to its iterative nature, with each trial requiring exactly three knots. A dedicated No Gesture/Idle class accounts for periods of inactivity or non-meaningful motion. Temporal Dynamics: Gestures exhibit significant duration variance, reflecting authentic skill gaps. For example, S7-knot averages 40s ± 5.8s (range: 30–60s), while S8-transition lasts 3s ± 0.8s. Total: ∼900K frames, 520K labeled (58%). These temporal features are clinically valuable for skill assessment. Expert-defined frame-level boundaries ensure precision, while the error taxonomy captures any heterogeneous sub-motions that deviate from standard techniques.

## 4. Benchmark Evaluation

We evaluate SHANDS on three core tasks that align directly with the requirements articulated by surgical-

education professionals and supported by prior research highlighting the need for data-driven, objective, and scalable assessment methods for surgical skill training [60,61]. First, gesture recognition, classifying video clips into one of 15 gesture primitives, measured by Top-1 accuracy and Macro-F1. Second, error detection, a multi-label classification problem covering eight error categories, reported with Top-1 accuracy and per-class F1. Third, cross-view generalization, assessing whether models can transfer to unseen camera viewpoints without re-training, thereby testing the view-invariance of learned representations. Data splits and protocols. We adopt a cross-subject evaluation, dividing the 52 participants between expert surgeons and medical trainees, ensuring that no subject in the test set appears during training. In single-view experiments, one camera (C1–C5) is used for both training and testing. In multi-view experiments, all five synchronized cameras are available during both phases. In cross-view generalization, models are trained on C1–C3 and evaluated on C4–C5 to test robustness to unseen viewpoints. For error detection, evaluation focuses on the trainee subset, where error frequency and clinical relevance are highest. This protocol enables a comprehensive assessment of model performance under single-view, multi-view, and cross-view conditions. Train/Val/Test = 31/11/10 participants (60/20/20), with expert/trainee breakdown: Train (12E/19T), Val (4E/7T), Test (4E/6T). Baseline methods. We benchmark a diverse set of stateof-the-art video recognition architectures covering both convolutional and transformer paradigms. For single-view gesture recognition, we include convolutional backbones R3D [62], SlowFast [39], and X3D-M [63], alongside transformer-based models TimeSformer [14], ViViT-B [41], MViTv2-B [64], and VideoMAE in both base and large configurations [42]. All single-view models are pretrained on Kinetics-400 or ImageNet-21K before fine-tuning on SHANDS. For multi-view learning, we evaluate architectures that explicitly capture cross-view dependencies, including MVAction [65] (view-specific and shared pathways), ViewCLR [66] (cross-view contrastive objectives), ViewCon [67] (view-consensus fusion), and DVANet [47] (disentangled view-invariant representations). These multiview methods are pretrained on NTU RGB+D and subsequently adapted for surgical gesture on SHANDS.

## 5. Results

We assess SHANDS across single-view and multi-view gesture recognition, error detection, and cross-view generalization. Overall, transformer-based models outperform convolutional baselines in the single-view setting (Table 4), while multi-view methods that explicitly model cross-view dependencies deliver consistent gains (Table 5). On the trainee subset, multi-view fusion also improves error detec-

<!-- Page 6 -->

Labeled 92%

Surgeons

9.1%

Labeled Coverage

Medical Trainees

90.9%

Labeled 55%

Incision Suturing

Improper Incision Improper Suturing

**Figure 2. Overview of dataset composition and annotation coverage. The pie chart on the left illustrates the distribution of total recording time contributed by medical trainees (90.9%) and surgeons (9.1%). The middle plots report the proportion of annotated footage, showing 92% labeled coverage for surgeons and 55% for trainees. The bar plots on the right present gesture distribution for surgeons (top) and trainees (bottom), illustrating variability across incision (I1–I5) and suturing (S1–S10) categories, as well as error types (II1–II3, IS1–IS5).**

tion (Table 6). Cross-view transfer experiments show that DVANet achieves the best performance on unseen cameras (Table 7), and a boundary-focused analysis corroborates the precision of our frame-level annotations (Figure 3).

### 5.1. Single-View Gesture Recognition

**Table 4 summarizes single-view baselines. Among CNNs, SlowFast achieves 55.7%, followed by X3D-M (53.9%) and R3D (52.3%). Transformers consistently outperform CNNs: TimeSformer reaches 58.4%, ViViTB 59.2%, and MViTv2-B 61.8%; masked pretraining improves performance further with VideoMAE-B (63.4%), VideoMAE-L (65.9%), and InternVideo2 (68.9%). These moderate accuracies, despite strong backbones, highlight the difficulty of fine-grained tool–tissue interactions and the domain gap with generic action datasets.**

### 5.2. Multi-View Gesture Recognition

Using all five synchronized views yields substantial gains (Table 5). Pretrained on NTU RGB+D 120 and finetuned on SHANDS, MVAction achieves Macro-F1 0.704, ViewCLR 0.717, and ViewCon 0.723. DVANet achieves 0.736 by disentangling view-specific and view-invariant factors. These results confirm that leveraging complementary viewpoints is critical for robust recognition under occlusions and viewpoint changes.

### 5.3. Error Detection Performance

To evaluate the eight error categories, we train models on SHANDS’ trainee subset using a VideoMAE-B backbone

and vary the number of synchronized cameras (Table 6). Multi-view configurations consistently yield higher accuracy than single-view setups: the single-view VideoMAEB reaches 60.4%, while ViewCon improves to 66.2% and DVANet achieves the highest accuracy of 68.5% using all five views. Per-class results for DVANet indicate strong performance on Improper instrument grip (77.3%) and Incorrect result (76.2%), whereas more nuanced categories such as Incorrect needle handling (60.9%) and Excessive force application (61.8%) remain challenging. These findings suggest that while cross-view fusion improves robustness, recognizing subtle procedural errors still requires refined temporal and contextual modeling.

### 5.4. Cross-View Generalization

**Table 7 analyzes the challenging cross-view transfer setting, where models trained on cameras C1–C3 are evaluated on unseen viewpoints C4–C5. On the training views, ViewCon achieves 68.4% (Macro-F1: 0.662), and DVANet reaches 72.8% (Macro-F1: 0.706). When tested on held-out cameras, ViewCon drops to 63.2% (92.4% retention), whereas DVANet remains more stable with 68.5% (94.1% retention). These results highlight DVANet’s superior cross-view generalization, confirming its ability to learn view-invariant representations that preserve performance across novel camera placements. Such robustness has direct practical value: surgical training centers could deploy pre-trained models under different camera setups without requiring additional labeled data from those viewpoints, significantly reducing the cost of**

<!-- Page 7 -->

Method Pretrain Dataset Top-1 (%)

CNN-based architectures: R3D [62] K400 52.3 ± 0.8 X3D-M [63] K400 53.9 ± 0.9 SlowFast [39] K400 55.7 ± 1.2

Transformer-based architectures: TimeSformer [14] K400 58.4 ± 1.1 ViViT-B [41] K400 59.2 ± 0.7 MViTv2-B [64] K400 61.8 ± 0.9 VideoMAE-B [42] K400 63.4 ± 0.6 VideoMAE-L [42] K400 65.9 ± 0.8 InternVideo2 [68] Im21K+K400 68.9 ± 0.8

**Table 4. Single-view gesture classification on SHands. Models trained and evaluated on a single-view camera. K400 = Kinetics400; Im21K = ImageNet-21K.**

Method Pretrain Dataset Top-1 (%) Macro-F1

MVAction [65] NTU RGB 120 70.4 ± 0.9 0.704 ViewClr [66] NTU RGB 120 71.1 ± 0.7 0.717 ViewCon [67] NTU RGB 120 72.5 ± 0.6 0.723 DVANet [47] NTU RGB 120 73.9 ± 0.6 0.736

**Table 5. Multi-view gesture classification on SHands. Models trained and evaluated with 5 synchronized cameras following a cross-subject protocol.**

Method Camera Views Top-1

VideoMAE-B single-view 60.4 ViewCon [67] All views 66.2 DVANet [47] All views 68.5

Per-class accuracy of DAVNet (5 views): II1 Improper instrument grip 77.3 II2 Incorrect angle 69.8 II3 Repetitive cutting 66.2 IS1 Incorrect needle handling 60.9 IS2 Excessive force application 61.8 IS3 Faulty knot technique 73.5 IS4 Manual thread manipulation 65.1 IS5 Result incorrect 76.2

**Table 6. Error detection on SHands student subset. The accuracy classification over eight error types.**

system adaptation. The remaining 5–6% gap between seen and unseen views suggests residual camera-specific bias, motivating future research on stronger view-invariance constraints and meta-learning strategies for domain transfer. Quality Analysis. To assess the precision of our temporal annotations, we analyze the gesture probability distributions predicted by the multi-view DVANet model in temporal neighborhoods surrounding ground-truth bound-

Configuration Method Top-1 (%) Macro-F1

Train: C1 + C2 + C3, Test: Same views ViewCon 68.4 0.662 DVANet 72.8 0.706

Train: C1 + C2 + C3, Test: C4 + C5 (unseen) ViewCon 63.2 0.613 DVANet 68.5 0.651

Performance retention (DVANet) 94.1% 92.2%

**Table 7. Cross-view generalization on SHands. Models trained on cameras C1–C3 and evaluated on seen views (same cameras) versus unseen views (C4–C5). Retention ratio measures the preservation of performance across novel viewpoints.**

aries. Figure 3 illustrates two representative transition points (frames 61 and 109), where sharp probability shifts between gesture classes validate the accuracy of manual annotations. The shaded regions represent uncertainty windows, while the before/after probability measures (computed over 10- and 15-frame intervals, respectively) quantify the model’s confidence on either side of each transition. The clear separation between dominant gesture probabilities confirms that SHANDS provides temporally precise and unambiguous boundaries, enabling robust model training and reliable evaluation of segmentation accuracy.

## 6. Discussion & Future Work

The findings above highlight the strengths and design rationale of SHANDS as a practical and pedagogically grounded benchmark for assessing surgical skill. Our decision to capture RGB video from five time-synchronized cameras was driven by educational constraints and by the need for realistic deployment. In both formal training facilities and personal study environments, standard webcams and smartphones provide an affordable and easy-to-deploy setup for recording training sessions. By focusing on RGB only, we emphasize an anytime, anywhere training paradigm in which models can be deployed with a single camera while still benefiting from knowledge distilled from multi-view “teacher” systems. We note that although depth or force sensors can provide additional geometric/interaction cues, adopting these modalities requires significantly more complex hardware integration and precise multi-sensor calibration. Depth systems in particular are sensitive to occlusions and reflective surgical instruments, making them unreliable in realistic training environments. These limitations render such sensor-dependent setups impractical for routine use in clinical training for medical trainees. Moreover, our focus on linear incision and suturing reflects their foundational role in surgical curricula and their

<!-- Page 8 -->

**Figure 3. Annotation quality analysis. Gesture probability distributions predicted by DVANet around two annotated boundary frames (61 and 109). Sharp probability transitions and narrow uncertainty regions indicate high temporal precision and consistency of the manual annotations, with strong classification confidence on either side of each gesture change.**

ubiquity in skills-lab training. The resulting taxonomy of 15 gesture primitives captures the key hand–tool configurations identified by the project’s medical educators. At the same time, the eight error categories were selected for their direct observability in RGB and instructional actionability, capturing information about grip stability, approach angle, tissue control, force application, and knot security.

Looking ahead, we will extend our approach toward holistic skill assessment. This involves aggregating temporal gesture predictions into interpretable global ratings (e.g., excellent, good, average, poor) with calibrated confidence and pairwise ranking between expert and trainee sessions. Such modeling opens the path toward a continuous feedback loop in which students practice in physical or VR simulators, a low-latency recognizer detects gestures and errors in real time, and a language model translates these detections into actionable, context-aware guidance. The proposed teacher–student distillation paradigm (multi-view teacher →single-view student) aligns naturally with singlecamera or head-mounted configurations, enabling scalable, low-infrastructure deployment across training institutions.

Finally, SHANDS highlights the intrinsic complexity of real surgical behavior. Gesture imbalance and ambiguous temporal boundaries arise not from annotation errors but from the intrinsic variability of human motion. Rather than simplifying this variability, our dataset preserves it to reflect realistic conditions. Future work can address these challenges through temporal uncertainty modeling, curriculumaware sampling strategies, and active annotation protocols that refine label consistency without compromising realism.

## 7. Conclusion

This paper presented OPEN-SURGERY HANDS (SHANDS), a synchronized multi-view dataset and benchmark for open-surgical gesture and error recognition. The dataset encompasses two fundamental tasks, incision and suturing, decomposed into 15 gesture primitives and eight clinically validated error categories. Experimental analyses demonstrate that multi-view supervision significantly enhances robustness and boundary precision, while RGB-only sensing enables realistic and privacy-preserving deployment for surgical training. SHANDS establishes a foundation for data-driven, interpretable, and scalable assessment of technical surgical skills, bridging the gap between computer vision research and medical education.

## 8. Ethics Statement

All data collection procedures were approved by the institutional ethics committee, and written informed consent was obtained from the participants. Recordings captured only hands and forearms, with no identifiable personal information. Participants were informed of the research objectives and the intended use of the data for training AI models. The dataset will be released under a Creative Commons Attribution 4.0 International License, permitting academic and commercial use while prohibiting any application involving surveillance, biometric identification, or nonconsensual monitoring.

<!-- Page 9 -->

## Acknowledgments

Supported by IDS (100.133 IP-ICT) and INDUX-R (GA No. 101135556; DOI: 10.3030/101135556). Funded by the European Union and the Swiss State Secretariat for Education, Research and Innovation (SERI). Disclaimer: Opinions expressed are the authors’ alone and do not necessarily represent the EU or CINEA. Neither the EU nor the granting authority is responsible for the content.

## References

[1] J. Martin, G. Regehr, R. Reznick, H. Macrae, J. Murnaghan, C. Hutchison, and M. Brown, “Objective structured assessment of technical skill (osats) for surgical residents,” British journal of surgery, vol. 84, no. 2, pp. 273–278, 1997. 1

[2] A. M. Shayan, S. Singh, J. Gao, R. E. Groff, J. Bible, J. F. Eidt, M. Sheahan, S. S. Gandhi, J. V. Blas, and R. Singapogu, “Measuring hand movement for suturing skill assessment: A simulation-based study,” Surgery, vol. 174, no. 5, pp. 1184–1192, 2023. 1

[3] R. G. Olsen, M. F. Gen´et, L. Konge, and F. Bjerrum, “Crowdsourced assessment of surgical skills: a systematic review,” The American Journal of Surgery, vol. 224, no. 5, pp. 1229–1237, 2022. 1

[4] R. G. Olsen, A. G. Andersen, A. J. Hung, M. B. S. Svendsen, J. A. Dagnæs-Hansen, L. Konge, A. Røder, and F. Bjerrum, “Untangling surgical gesture analysis—are we even speaking the same language? a systematic review,” Surgical Endoscopy, pp. 1–20, 2025. 1

[5] R. Pedrett, P. Mascagni, G. Beldi, N. Padoy, and J. L. Lavanchy, “Technical skill assessment in minimally invasive surgery using artificial intelligence: a systematic review,” Surgical endoscopy, vol. 37, no. 10, pp. 7412–7424, 2023. 1

[6] L. Ma, H. Kang, N. Magnenat-Thalmann, and K. Wac, “Transsg: a spatial-temporal transformer for surgical gesture recognition,” in Computer Graphics International Conference. Springer, 2024, pp. 151–165. 1

[7] D. Power, C. Burke, M. G. Madden, and I. Ullah, “Automated assessment of simulated laparoscopic surgical skill performance using deep learning,” Scientific Reports, vol. 15, no. 1, p. 13591, 2025. 1

[8] Y. Gao, S. S. Vedula, C. E. Reiley, N. Ahmidi, B. Varadarajan, H. C. Lin, L. Tao, L. Zappella, B. B´ejar, D. D. Yuh, C. C. G. Chen, R. Vidal, S. Khudanpur, and G. D. Hager, “Jhu-isi gesture and skill

assessment working set (jigsaws): A surgical activity dataset for human motion modeling,” in Modeling and Monitoring of Computer Assisted Interventions (M2CAI) – MICCAI Workshop, vol. 3, 2014, p. 3. 1, 2, 3

[9] A. P. Twinanda, S. Shehata, D. Mutter, J. Marescaux, M. de Mathelin, and N. Padoy, “Endonet: A deep architecture for recognition tasks on laparoscopic videos,” IEEE Transactions on Medical Imaging, vol. 36, no. 1, pp. 86–97, 2017. 1, 2, 3

[10] Y. Jin, Q. Dou, H. Chen, L. Yu, J. Qin, C.-W. Fu, and P.-A. Heng, “Sv-rcnet: Workflow recognition from surgical videos using recurrent convolutional network,” IEEE Transactions on Medical Imaging, vol. 37, no. 5, pp. 1114–1126, 2018. 1, 2, 3

[11] A. Shahroudy, J. Liu, T.-T. Ng, and G. Wang, “NTU RGB+D: A large scale dataset for 3D human activity analysis,” in Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2016, pp. 1010–1019. 1, 3

[12] F. Sener, D. Chatterjee, D. Shelepov, K. He, D. Singhania, R. Wang, and A. Yao, “Assembly101: A largescale multi-view video dataset for understanding procedural activities,” in Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), 2022, pp. 21 066–21 076. 1, 3

[13] Z. Tong, Y. Song, J. Wang, and L. Wang, “Videomae: Masked autoencoders are data-efficient learners for self-supervised video pre-training,” in Advances in Neural Information Processing Systems (NeurIPS), 2022. 2, 3

[14] G. Bertasius, H. Wang, and L. Torresani, “Is spacetime attention all you need for video understanding?” in Proceedings of the 38th International Conference on Machine Learning (ICML), ser. PMLR, vol. 139, 2021. 2, 3, 5, 7

[15] N. Madapana, M. M. Rahman, N. Sanchez-Tamayo, M. V. Balakuntala, G. Gonzalez, J. P. Bindu, L. V. Venkatesh, X. Zhang, J. B. Noguera, T. Low et al., “Desk: A robotic activity dataset for dexterous surgical skills transfer to medical robots,” in 2019 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS). IEEE, 2019, pp. 6928– 6934. 2, 3

[16] H. A. Hajj, M. Lamard, P. Conze, B. Cochener, and G. Quellec, “CATARACTS: Challenge on automatic tool annotation for cataract surgery,” Medical Image Analysis, vol. 52, pp. 24–41, 2019. 2, 3

<!-- Page 10 -->

[17] V. K. Srivastav, T. Issenhuth, A. Kadkhodamohammadi, M. de Mathelin, A. Gangi, and N. Padoy, “Mvor: A multi-view rgb-d operating room dataset for 2d and 3d human pose estimation,” in MICCAI 2018 Satellite Workshop, Granada, Spain, september 16-20 2018. Springer, 2018. 3

[18] R. Fujii, M. Hatano, H. Saito, and H. Kajita, “Egosurgery-phase: a dataset of surgical phase recognition from egocentric open surgery videos,” in International Conference on Medical Image Computing and Computer-Assisted Intervention. Springer, 2024, pp. 187–196. 3

[19] J. Liu, A. Shahroudy, M. Perez, G. Wang, L.-Y. Duan, and A. C. Kot, “Ntu rgb+ d 120: A large-scale benchmark for 3d human activity understanding,” IEEE transactions on pattern analysis and machine intelligence, vol. 42, no. 10, pp. 2684–2701, 2019. 3

[20] C. Liu, Y. Hu, Y. Li, S. Song, and J. Liu, “Pkummd: A large scale benchmark for continuous multimodal human action understanding,” arXiv preprint arXiv:1703.07475, 2017. 3

[21] P. Parmar and B. T. Morris, “What and how well you performed? a multitask learning approach to action quality assessment,” in Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), 2019, pp. 304–313. 3

[22] J. Xu, Y. Rao, X. Yu, G. Chen, J. Zhou, and J. Lu, “Finediving: A fine-grained dataset for procedureaware action quality assessment,” in Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), 2022, pp. 2949–2958. 3

[23] Y. Pan, C. Zhang, and G. Bertasius, “Basket: A largescale video dataset for fine-grained skill estimation,” in Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), 2025. 3

[24] I. Rivas-Blanco, C. J. P. Del-Pulgar, A. Mariani, G. Tortora, and A. J. Reina, “A surgical dataset from the da vinci research kit for task automation and recognition,” in 2023 3rd International Conference on Electrical, Computer, Communications and Mechatronics Engineering (ICECCME). IEEE, 2023, pp. 1–6. 2

[25] Z. Wang, B. Lu, Y. Long, F. Zhong, T.-H. Cheung, Q. Dou, and Y. Liu, “Autolaparo: A new dataset of integrated multi-tasks for image-guided surgical automation in laparoscopic hysterectomy,” in Medical Image Computing and Computer-Assisted Intervention – MICCAI 2022, ser. Lecture Notes in Computer Science. Springer, 2022, pp. 486–496. 2

[26] E. ¨Ozsoy, C. Pellegrini, T. Czempiel, F. Tristram, K. Yuan, D. Bani-Harouni, U. Eck, B. Busam, M. Keicher, and N. Navab, “Mm-or: A large multimodal operating room dataset for semantic understanding of high-intensity surgical environments,” in Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), 2025, arXiv:2503.02579. 2

[27] S. Wang, S. Wang, D. Yang, M. Li, H. Kuang, X. Zhao, L. Su, P. Zhai, and L. Zhang, “Cpr-coach: Recognizing composite error actions based on singleclass training,” in Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2024, pp. 18 782–18 792. 2

[28] E. ¨Ozsoy, E. P. ¨Ornek, U. Eck, T. Czempiel, F. Tombari, and N. Navab, “4d-or: Semantic scene graphs for or domain modeling,” in Medical Image Computing and Computer-Assisted Intervention – MICCAI 2022, ser. Lecture Notes in Computer Science, vol. 13437. Springer, 2022, pp. 475–485. 2

[29] A. P. Sharghi, A. Haugerud, D. Oh, and N. Mohareri, “Automatic operating room surgical activity recognition for robot-assisted surgery,” in International Conference on Medical Image Computing and ComputerAssisted Intervention (MICCAI), ser. Lecture Notes in Computer Science, vol. 12263. Springer, 2020, pp. 385–395. 3

[30] A. Malpani, C. Lea, C. C. G. Chen, and G. D. Hager, “System events: readily accessible features for surgical phase detection,” International Journal of Computer Assisted Radiology and Surgery, vol. 11, no. 6, pp. 1201–1209, 2016. 3

[31] L. Bai, M. Islam, L. Seenivasan, and H. Ren, “Surgical-vqla: Transformer with gated visionlanguage embedding for visual question localizedanswering in robotic surgery,” arXiv preprint arXiv:2305.11692, 2023. 3

[32] T. Kwon, B. Tekin, J. St¨uhmer, F. Bogo, and M. Pollefeys, “H2o: Two hands manipulating objects for first person interaction recognition,” in Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV), 2021, pp. 10 138–10 148. 3

[33] K. Grauman, A. Westbury, L. Torresani, K. Kitani, J. Malik, T. Afouras, K. Ashutosh, V. Baiyya, S. Bansal, B. Boote et al., “Ego-exo4d: Understanding skilled human activity from first-and third-person perspectives,” in Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, 2024, pp. 19 383–19 400. 3

<!-- Page 11 -->

[34] Y. Huang, G. Chen, J. Xu, M. Zhang, L. Yang, B. Pei, H. Zhang, L. Dong, Y. Wang, L. Wang, and Y. Qiao, “Egoexolearn: A dataset for bridging asynchronous ego- and exo-centric view of procedural activities in real world,” in CVPR, 2024. 3

[35] R. Ghoddoosian, I. Dwivedi, N. Agarwal, and B. Dariush, “Weakly-supervised action segmentation and unseen error detection in anomalous instructional videos,” in ICCV, 2023, introduces the ATA dataset. 3

[36] X. Wang, T. Kwon, M. Rad, B. Pan, I. Chakraborty, S. Andrist, D. Bohus, A. Feniello, B. Tekin, F. V. Frujeri et al., “Holoassist: an egocentric human interaction dataset for interactive ai assistants in the real world,” in Proceedings of the IEEE/CVF International Conference on Computer Vision, 2023, pp. 20 270– 20 281. 3

[37] S. Zhang, W. Dai, S. Wang, X. Shen, J. Lu, J. Zhou, and Y. Tang, “Logo: A long-form video dataset for group action quality assessment,” in CVPR, 2023. 3

[38] A. Yang, A. Nagrani, I. Laptev, J. Sivic, and C. Schmid, “Vidchapters-7m: Video chapters at scale,” in Advances in Neural Information Processing Systems, vol. 36, 2023, pp. 49 428–49 444. 3

[39] C. Feichtenhofer, H. Fan, J. Malik, and K. He, “Slowfast networks for video recognition,” in Proceedings of the IEEE/CVF international conference on computer vision, 2019, pp. 6202–6211. 3, 5, 7

[40] H. Fan, B. Xiong, K. Mangalam, Y. Li, Z. Yan, J. Malik, and C. Feichtenhofer, “Multiscale vision transformers,” in Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV), 2021.

3

[41] A. Arnab, M. Dehghani, G. Heigold, C. Sun, M. Luˇci´c, and C. Schmid, “Vivit: A video vision transformer,” in Proceedings of the IEEE/CVF international conference on computer vision, 2021, pp. 6836– 6846. 3, 5, 7

[42] L. Wang, B. Huang, Z. Zhao, Z. Tong, Y. He, Y. Wang, Y. Wang, and Y. Qiao, “Videomae v2: Scaling video masked autoencoders with dual masking,” in Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), 2023. 3, 5, 7

[43] K. Hu, Y. Xiao, Y. Zhang, and X. Gao, “Multi-view masked contrastive representation learning for endoscopic video analysis,” Advances in Neural Information Processing Systems, vol. 37, pp. 47 987–48 014, 2024. 3

[44] Z. Wang, C. Liu, S. Zhang, and Q. Dou, “Foundation model for endoscopy video analysis via large-scale self-supervised pre-train,” in Medical Image Computing and Computer Assisted Intervention – MICCAI 2023, ser. Lecture Notes in Computer Science, vol. 14223. Springer, 2023, pp. 101–111. 3

[45] D. Bati´c, F. Holm, E. ¨Ozsoy, T. Czempiel, and N. Navab, “Endovit: pretraining vision transformers on a large collection of endoscopic images,” International Journal of Computer Assisted Radiology and Surgery, vol. 19, no. 6, pp. 1085–1091, 2024. 3

[46] M. A. Jamal and O. Mohareri, “Surgmae: Masked autoencoders for long surgical video analysis,” arXiv preprint arXiv:2305.11451, 2023. 3

[47] N. Siddiqui, P. Tirupattur, and M. Shah, “Dvanet: Disentangling view and action features for multi-view action recognition,” in Proceedings of the AAAI Conference on Artificial Intelligence, vol. 38, no. 5, 2024, pp. 4873–4881. 3, 5, 7

[48] H. Tang, J. Liu, S. Yan, R. Yan, Z. Li, and J. Tang, “M3net: multi-view encoding, matching, and fusion for few-shot fine-grained action recognition,” in Proceedings of the 31st ACM international conference on multimedia, 2023, pp. 1719–1728. 3

[49] R. Qian, J. Targarona, O. ¨Ozdenizci, J. Echevarria, Y. Shen, C. Liu, S. Sclaroff, K. Saenko, B. Wu, and R. Feris, “Spatiotemporal contrastive video representation learning,” in Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), 2021. 4

[50] K. Shah, R. Crandall, J. Xu, P. Zhou, M. George, M. Bansal, and R. Chellappa, “Mv2mae: Multi-view video masked autoencoders,” 2024. 4

[51] K.-Y. Lin, J. Zhou, and W.-S. Zheng, “Human-centric transformer for domain adaptive action recognition,” in IEEE Transactions on Pattern Analysis and Machine Intelligence, 2024. 4

[52] D. Ho and S. Madden, “Dejavid: Encoder-agnostic learned temporal matching for video classification,” in Proceedings of the Computer Vision and Pattern Recognition Conference, 2025, pp. 24 023–24 032. 4

[53] Y. Lin, J. Lu, Y. Yong, and J. Zhang, “Mv-gmn: State space model for multi-view action recognition,” arXiv preprint arXiv:2501.13829, 2025. 4

[54] Y. A. Farha and J. Gall, “Ms-tcn: Multi-stage temporal convolutional network for action segmentation,” in

<!-- Page 12 -->

Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition, 2019, pp. 3575–3584. 4

[55] S. Li, Y. AbuFarha, Y. Liu, M. Cheng, and J. Gall, “MS-TCN++: Multi-stage temporal convolutional network for action segmentation,” IEEE Transactions on Pattern Analysis and Machine Intelligence, 2020. 4

[56] F. Yi, H. Wen, and T. Jiang, “Asformer: Transformer for action segmentation,” in British Machine Vision Conference (BMVC), 2021. 4

[57] Y. Liu, J. Huo, J. Peng, R. Sparks, P. Dasgupta, A. Granados, and S. Ourselin, “Skit: a fast key information video transformer for online surgical phase recognition,” in Proceedings of the IEEE/CVF international conference on computer vision, 2023, pp. 21 074–21 084. 4

[58] Y. Liu, M. Boels, L. C. Garcia-Peraza-Herrera, T. Vercauteren, P. Dasgupta, A. Granados, and S. Ourselin, “Lovit: Long video transformer for surgical phase recognition,” Medical Image Analysis, vol. 99, p. 103366, 2025. 4

[59] K. Weerasinghe, S. H. R. Roodabeh, K. Hutchinson, and H. Alemzadeh, “Multimodal transformers for real-time surgical activity prediction,” in 2024 IEEE International Conference on Robotics and Automation (ICRA). IEEE, 2024, pp. 13 323–13 330. 4

[60] K. Lam, J. Chen, Z. Wang, F. M. Iqbal, A. Darzi, B. Lo, S. Purkayastha, and J. M. Kinross, “Machine learning for technical skill assessment in surgery: a systematic review,” NPJ digital medicine, vol. 5, no. 1, p. 24, 2022. 5

[61] D. A. Hla and D. I. Hindin, “Generative ai & machine learning in surgical education,” Current problems in surgery, vol. 63, p. 101701, 2025. 5

[62] D. Tran, H. Wang, L. Torresani, J. Ray, Y. LeCun, and M. Paluri, “A closer look at spatiotemporal convolutions for action recognition,” in Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), 2018, pp. 6450–6459. 5, 7

[63] C. Feichtenhofer, “X3d: Expanding architectures for efficient video recognition,” in Proceedings of the IEEE/CVF conference on computer vision and pattern recognition, 2020, pp. 203–213. 5, 7

[64] Y. Li, C.-Y. Wu, H. Fan, K. Mangalam, B. Xiong, J. Malik, and C. Feichtenhofer, “Mvitv2: Improved multiscale vision transformers for classification and detection,” in CVPR, 2022. 5, 7

[65] S. Vyas, Y. S. Rawat, and M. Shah, “Multi-view action recognition using cross-view video prediction,” in European Conference on Computer Vision. Springer, 2020, pp. 427–444. 5, 7

[66] S. Das and M. S. Ryoo, “Viewclr: Learning selfsupervised video representation for unseen viewpoints,” in Proceedings of the IEEE/CVF Winter Conference on Applications of Computer Vision, 2023, pp. 5573–5583. 5, 7

[67] K. Shah, A. Shah, C. P. Lau, C. M. de Melo, and R. Chellappa, “Multi-view action recognition using contrastive learning,” in Proceedings of the ieee/cvf winter conference on applications of computer vision, 2023, pp. 3381–3391. 5, 7

[68] Y. Wang, J. Chen, K. Li, Z. Pan, J. Li, Z. Wen, S. Li, H. Wang, H. Lu, and Y. Qiao, “Internvideo2: Scaling foundation models for multimodal video understanding,” in Computer Vision – ECCV 2024, ser. Lecture Notes in Computer Science, A. Leonardis, E. Ricci, S. Roth, O. Russakovsky, T. Sattler, and G. Varol, Eds. Springer, Cham, 2025, vol. 15143, pp. 370–389. 7
