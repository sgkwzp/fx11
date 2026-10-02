---
source_pdf: "ProcObject-10K Benchmarking Object-Centric Procedural Understanding in Instructional Videos.pdf"
pages: 12
conversion: "Automatic PDF-to-Markdown text conversion; figures are represented by extracted captions/text rather than embedded images."
---

# ProcObject-10K: Benchmarking Object-Centric Procedural Understanding in Instructional Videos

<!-- Page 1 -->

Wenliang Guo Michigan State University Yu Kong Michigan State University

## Abstract

Procedural activities are fundamentally driven by object state transitions, yet existing instructional video benchmarks remain action-centric and cannot evaluate whether models reason about how objects evolve toward task completion. In this work, we introduce PROCOBJECT-10K, the first benchmark that jointly evaluates object-centric reasoning and temporal evidence grounding in instructional videos, across both egocentric and exocentric views. It comprises 10,522 open-ended VideoQA pairs grounded in 1,799 video clips, spanning 137 tasks across 9 domains and five reasoning types covering preconditions, state evolution, counterfactuals, mistakes, and readiness. Benchmarking 13 leading MLLMs reveals a substantial answering-grounding gap: models produce plausible answers while failing to localize the supporting evidence (mIoU < 45%), exposing their reliance on linguistic priors rather than fine-grained object dynamics. As a step toward closing this gap, we further provide an object-centric supervised fine-tuning baseline with pseudo object-level supervision and spatial-temporal constraints. Models fine-tuned on PROCOBJECT-10K not only improve on the benchmark itself, but also transfer effectively to other grounded VideoQA and embodied planning tasks. The dataset, annotations, and evaluation toolkit will be publicly released to support future research on object-centric procedural understanding.

## 1 Introduction

Human daily activities are inherently procedural, spanning scenarios such as cooking [7, 35], assembly [39], and medical procedures [33]. These activities are governed by an underlying procedural structure, where actions operate on objects to induce state transitions, forming causal dependencies across steps toward task completion. For example, cracking an egg enables mixing, and mixing produces a liquid state required for subsequent cooking. Understanding such structure requires not only recognizing actions [4], but reasoning about how object states evolve over time [43] and how these changes constrain future actions. This capability is fundamental for future intelligent systems that aim to assist humans on procedural tasks by watching and learning from instructional videos.

Existing work on procedural understanding predominantly adopts an action-centric perspective. Representative approaches construct symbolic graphs over action sequences to model dependencies [17, 2, 42, 41, 16, 20], or rely on masked modeling to recover missing action segments and implicitly learn structure [28, 22, 56]. Despite methodological differences, these approaches share a common assumption that procedural structure is determined by action transitions alone.

However, this assumption overlooks a key property of procedural activities whose goal is achieved through progressive transformation of object states under human interaction. Procedural causality is therefore not fully captured by action sequences, but is also reflected in how object states evolve over time. Direct evidence is that the same action can produce different outcomes depending on execution conditions [12], which in turn alters the set of valid subsequent actions [29]. As a result,

Preprint.

<!-- Page 2 -->

**Table 1: Dataset comparison. PROCOBJECT-10K provides a comprehensive evaluation of openended question answering, temporal evidence grounding, object-centric reasoning, and mistake understanding across both egocentric and exocentric instructional videos.**

Datasets View Object-centric QA Type Evidence Mistake Video Domain

COIN [44] Exo ✗ -✗ ✗ Instructional ChangeIt [43] Exo ✓ -✗ ✗ Instructional EgoSchema [27] Ego ✗ Multi-choice ✗ ✗ Human Activity EgoPER [21] Ego ✗ -✗ ✓ Instructional CaptainCook4D [35] Ego ✗ -✗ ✓ Instructional ProMQA [15] Exo ✗ Open-ended ✗ ✓ Instructional REXTIME [5] Exo ✗ Multi-choice ✓ ✗ Generic VideoInfer [45] Exo ✓ Open-ended ✓ ✓ Human Activity MULTIHOP-EGOQA [6] Ego ✗ Open-ended ✓ ✗ Human Activity TrackVerse [48] Exo ✓ -✗ ✗ Generic EPIC-KITCHENS-100-MQA [36] Ego ✗ Open-ended ✓ ✓ Instructional PROCOBJECT-10K Ego+Exo ✓ Open-ended ✓ ✓ Instructional

Previous QA Object-centric Instructional QA (Ours)

Action-focused Precondition Grounding Object-state Evolution Counterfactual Reasoning Mistake Recognition

Question: How does coffee be prepared before grinding for 20 seconds?

Question: How does the state of the egg change from adding to bowl to being microwaved?

Question: What is the next action after adding coffee ground into dripper? Answer: After adding coffee ground, the next step is adding water on coffee ground. Evidence Time-spans: None

Answer: Coffee are first portioned after weighting and then poured into grinder. Evidence Time-spans: 124-131s, 141-145s

Answer: The egg is liquid after stirring and solidify into fluffy curd after microwaving. Evidence Time-spans: 57-69s, 311-325s

Question: What would happen if peeling the 2 garlic cloves were not performed on the garlic?

Question: What mistake occurs on the butter added to the bowl, and why is it a mistake?

Question: What mistake occurs on the butter added to the bowl, and why is it a mistake?

Answer: It will cause peel-laced garlic pieces that don’t blend smoothly into the sauce. Evidence Time-spans: 68-75s, 152-171s, 267-270s

Answer: Butter remains as chunks instead of integrating with the corn. Evidence Time-spans: 470-482s

Answer: Butter remains as chunks instead of integrating with the corn. Evidence Time-spans: 470-482s

**Figure 1: QA types in PROCOBJECT-10K dataset, which centers on object-level understanding to capture temporal and spatial dynamics, enabling benchmarking from diverse perspectives.**

action-centric representations of procedural structure lack sufficient object-state awareness, limiting their ability to support consistent reasoning about procedural progress and causal dependencies.

In this work, we study the procedural understanding of instructional videos from an object-centric perspective, where object state dynamics serves as an observable signal for modeling procedural structure. We formulate a procedure as a sequence of temporally grounded object state transitions with localized evidence and precondition constraints, enabling explicit evaluation of whether models can identify relevant objects, localize state changes, and reason about their causal roles. However, previous benchmarks fail to support such evaluation, because existing instructional video datasets [44, 21, 35, 15, 36] focus on action sequences without modeling object state changes, while objectcentric datasets [43, 45, 48] lack goal-driven procedural structure or object-level causal dependencies. Consequently, current benchmarks fail to evaluate whether models reason over object state dynamics, instead allowing strong performance through reliance on action patterns.

To bridge this gap, we introduce PROCOBJECT-10K, an instructional video benchmark for objectcentric procedural understanding. It is formulated as grounded VideoQA task [8, 6, 49, 13] where each sample requires reasoning over temporally localized evidence of object state changes. The dataset contains 1,799 video clips and 10,522 question-answer pairs with aligned evidence spans, covering 137 diverse tasks across 9 domains. It is constructed via a semi-automated pipeline with VLM generated annotations followed by model-based and manual verification and refinement for temporal and causal consistency. To systematically evaluate procedural reasoning, we define five types of questions shown in Figure 1: Precondition Grounding, Object State Evolution, Counterfactual Reasoning, Mistake Recognition, and Readiness Assessment, covering complementary aspects of procedural structures. Our benchmarking results expose a critical Answering-Grounding Gap: while leading MLLMs achieve plausible performance in language answering, they consistently fail to pinpoint the underlying temporal evidence, with grounding mIoU generally remaining below 45%. This discrepancy reveals that existing models often rely on linguistic priors rather than achieving a fine-grained object-centric understanding of procedural evolution.

To further validate the diagnostic value of the benchmark, we introduce an object-centric supervised fine-tuning (SFT) framework that explicitly encourages MLLMs to attend to the dynamic evolution of action-relevant objects. To this end, we construct object-level pseudo labels using an object grounding

Readiness Assessment

<!-- Page 3 -->

model [25] and a vision-language model [37], and apply spatial and temporal attention constraints as supervision signals to guide models toward relevant objects and key frames. This training strategy improves both grounding accuracy and reasoning performance, suggesting that explicit supervision on object state dynamics addresses the failure modes exposed by the benchmark.

Experimental results show that the learned object-centric representation not only improves performance on PROCOBJECT-10K, but also exhibits strong generalization. The model fine-tuned on PROCOBJECT-10K transfers effectively to other procedural VideoQA benchmarks [6] under zeroshot settings, and achieves improved performance on embodied instruction following tasks [19, 40] when used as zero-shot planners. These results indicate that reasoning over object state dynamics yields a transferable capability that extends beyond the object-centric benchmark setting, highlighting the broader value of PROCOBJECT-10K for procedural understanding.

In summary, our contributions are as follows:

- We introduce PROCOBJECT-10K, a benchmark for object-centric procedural understanding, featuring 10,522 VideoQA pairs grounded in 1,799 video clips across 137 tasks and 9 domains.

- We propose an SFT framework that incorporates object-level supervision and spatial-temporal constraints to implicitly model procedural structure and learn object-centric representation.

- We empirically show that object-centric understanding not only improves procedural reasoning and grounding, but also generalizes to other VideoQA and even embodied planning tasks.

## 2 Related Work

Procedural Understanding. Instructional videos depicting goal-driven activities such as cooking [7, 35, 21] and assembly [39, 14] have been widely studied for procedural understanding. Prior work has primarily focused on action-centric tasks, including action recognition [4], action segmentation [54], and action anticipation [10]. Beyond procedural understanding, recent studies in general video understanding have explored object-centric representations through finer-grained modeling of object dynamics and state transitions [50, 48, 45]. Since procedural activities inherently involve dense human-object interactions, object-centric modeling has also shown strong potential for instructional video understanding, benefiting downstream tasks such as object state prediction [53], procedural planning [29], and mistake detection [12]. These works highlight the importance of modeling object dynamics for procedural reasoning. However, existing approaches are typically designed for specific downstream tasks [12, 29] or only capture short-term local object state changes [43], lacking a general capability for understanding procedural structure from an object-centric perspective. These limitations motivate the development of foundation models capable of object-centric procedural reasoning across more applications in this work.

Video Question Answering. VideoQA has emerged as an effective task for evaluating spatialtemporal reasoning over videos through natural language interaction [18, 52]. More recent grounded VideoQA benchmarks further require models to localize supporting temporal evidence for the answers, enabling evaluation beyond language generation toward evidence-aware reasoning [8, 6, 49, 13]. Existing benchmarks have advanced long-form reasoning [27] and temporal causal understanding [5], while instructional VideoQA datasets such as ProMQA [15, 14] and MultiHop-EgoQA [6] study procedural reasoning over human-object interactions. However, these benchmarks primarily focus on action sequences or event-level reasoning, without explicitly modeling the causal dependencies induced by object state transitions across procedural steps. In contrast, we introduce a grounded VideoQA benchmark in this work for object-centric procedural understanding that explicitly evaluates object state evolution, temporal causal structure, and evidence localization in instructional videos.

## 3 PROCOBJECT-10K Benchmark

Existing benchmarks fail to jointly capture object state dynamics and the underlying procedural structure. To bridge this gap, we introduce PROCOBJECT-10K, a benchmark dedicated to objectcentric procedural reasoning. It features open-ended, temporally grounded QA pairs that explicitly require models to localize relevant objects and infer causal state transitions across multiple execution steps. In this section, we first detail the data generation pipeline in Section 3.1, then analyze the dataset statistics in Section 3.2, and finally outline the evaluation metrics in Section 5.2.

<!-- Page 4 -->

Grounded QA Generation

Procedural Video Collection

CaptainCook4D (Egocentric ⋅Cooking)

HoloAssist (Egocentric ⋅Assembly)

Sys & User

Sparse Frames

Prompts

EgoPER (Egocentric ⋅Cooking)

COIN (Exocentric ⋅Daily & Lab)

Action Sequence Sampling

Question: How does the egg change before the final mixing and cooking begins?

Action #1 Action #2 Action #3 Action #4

Answer: Egg is cracked into a bowl and whisked until it forms a smooth, yellow liquid mixture

Evidence Time-spans: 0-13s, 34-45s

… … …

Data Verification & Filtering

Seq #1 Seq #2 Seq #3

Answer-Evidence Consistency

Dense Caption for Each Sequence

Commonsense Answerability

! second: Shaping tortilla dough ...

Manual Review & Refinement

! + 1 second: Preparing tortilla on ...

(a) Data generation pipeline.

Dense Captions

(b) Dataset statistics.

**Figure 2: Overview of PROCOBJECT-10K: (a) data generation pipeline, and (b) distribution of tasks across all videos (upper chart) and distribution of video sources and QA types (lower chart).**

3.1 Data Generation

We construct the PROCOBJECT-10K benchmark dataset using a four-stage semi-automated pipeline, as illustrated in Figure 2a. To scale the benchmark dataset, we first collect instructional videos based on existing datasets. Then we process the raw videos and sample action sequences to maintain consistent object appearance and change while preserving procedural structure. We employ visionlanguage models [37] to generate QA pairs via multimodal prompting. Finally, we perform modelbased and manual post verification and filtering to ensure data quality.

Stage 1: Procedural Video Collection. We curate videos from datasets characterized by complex hand-object interactions, including the egocentric cooking datasets CaptainCook4D [35] and EgoPER [21], as well as the egocentric assembly dataset HoloAssist [47]. In particular, these three datasets contain instances of erroneous actions and procedural failures, such as object mishandling or omission of critical steps, which provide a natural testbed to evaluate mistake detection [21, 20, 12, 34]. This capability is essential for assessing whether models capture the underlying procedural structure beyond surface level action recognition. To further broaden procedural coverage, we incorporate COIN [44], a large scale Internet video dataset spanning diverse domains including daily activities, industrial operations, and scientific experiments. We apply a set of video filtering criteria to ensure data quality, including the removal of corrupted videos, elimination of highly similar procedures, and exclusion of overly long videos (≥30 minutes). The resulting collection comprises 1,943 videos (before QA filtering in Stage 4) and serves as a basis for subsequent data generation.

Stage 2: Action Sequence Sampling. Untrimmed procedural videos involve multiple interacting objects, making them unsuitable for directly constructing questions about specific object-state dynamics. To ensure that each video captures meaningful and causally coherent state transitions, we sample temporally contiguous action sequences. Concretely, we segment each video into consecutive action clips and apply a temporal sliding window over N segments to extract sub-action sequences, e.g., “Place tortilla on a cutting board →Pour egg mixture on tortilla →Roll the tortilla.” To balance object consistency while preserving rich state transitions, we set N ∈{2, 3, 4, 5}, enabling windows of varying lengths to yield diverse procedural sequences. To obtain object information, we prompt Qwen3.5 [37] to generate dense captions conditioned on action annotations, focusing on key objects such as the tortilla. These captions capture fine-grained object states, facilitating subsequent QA generation that emphasizes object dynamics rather than actions or environmental context.

Stage 3: Grounded QA Generation. For each video clip corresponding to a sampled action sequence from Stage 2, we prompt Qwen3.5 [37] to generate questions, answers, and supporting temporal evidence (i.e., start and end timestamps). For each question type shown in Figure 1, we define task-specific system prompts to specify the requirements. The user prompt combines action annotations describing the sequence with predefined question templates, e.g., How does the [OBJECT] change from [ACTION A] to [ACTION B]? for Object-State Evolution questions. To represent the video content, we provide the object-centric captions obtained in Stage 2 together

<!-- Page 5 -->

with a small set of sparsely sampled frames as multimodal input to Qwen3.5, supplying visual context such as scene and environment. This multimodal conditioning ensures that the generated QA pairs remain consistent with the underlying video content and aligned with the intended QA objectives.

Stage 4: Verification and Filtering. Due to VLM hallucination [24], the generated QA pairs may suffer from issues such as object-irrelevant questions, mismatched answers, misaligned evidence segments, or low overall quality. As fully manual curation is costly, we design a data cleaning pipeline that combines model-based and human verification and refinement, consisting of the following steps:

- Step 1: Commonsense Filtering. We identify questions answerable without video grounding by prompting a large language model (LLM) [51] to generate answers using only the question text. If the text-only prediction achieves a high-quality evaluation (e.g., an LLM-as-judge score ≥4, as defined in Section 5.2), the instance is discarded. Such questions are likely solvable via pure commonsense reasoning and would artificially inflate benchmark performance. This step filters out approximately 30% (∼4,500) of the initial QA samples.
- Step 2: Answer-Evidence Alignment. We verify alignment by inputting the question, answer, and the video segment cropped using evidence timestamps into GPT-4o mini [30], which outputs a binary decision indicating whether the evidence supports the answer. Accepted instances proceed to manual review, while rejected ones are directly sent for human refinement in Step 3.
- Step 3: Human Review and Refinement. We conduct human review and refinement by students with AI backgrounds majoring in computer science. First, each QA instance is independently verified twice by different student reviewers, and only those instances that pass both verifications are included in the final dataset. The others then are manually refined by correcting the questions, answers, or/and evidences. This step removes QA ambiguity, tightens evidence grounding, and improves linguistic clarity, ensuring the reliability of the benchmark.

3.2 Dataset Statistics

As shown in Figure 2b, after data cleaning, PROCOBJECT-10K comprises 10,522 grounded QA pairs sourced from 1,799 procedural videos, covering 137 tasks across 9 domains. The dataset spans 211.28 hours of video in total, with an average duration of 72.29 seconds and a median of 72.33 seconds as shown in Figure 3a. We adopt a video-disjoint split with 9,472 training and 1,050 testing samples, ensuring that there is no overlap in videos. Beyond the five types of QA (Figure 1), we further categorize samples by temporal search patterns, as shown in Figure 3b: (1) Multi-hop Reasoning, which requires aggregating evidence across multiple or temporally separated segments, and (2) Needle-in-a-Haystack, which involves identifying short but critical moments within long videos. Multi-hop reasoning dominates in forward prediction and counterfactual reasoning, whereas mistake recognition and readiness assessment more often follow the needle-in-a-haystack pattern.

## 4 Object-centric Supervised Finetuning

Mean 72.29s Median 72.33s

1000

800

# QA clips

600

400

200

0

0 30 60 90 120 150 180 Sampled clip duration (seconds)

(a) Video length distribution.

Multi-hop Reasoning Needle-in-a-Haystack

2500

2143 2152

2000

# QA pairs

1500

1297

1067 1191

937

1000

556

379 524

500

276

Counterfactual Mistake Readiness 0

Precondition State Evolution

(b) Reasoning categories.

**Figure 3: Statistics of PROCOBJECT-10K.**

Standard VideoQA fine-tuning optimizes answer generation, but does not explicitly supervise which objects and temporal states support the answer. To encourage object-centric procedural reasoning, we introduce a supervised fine-tuning framework with pseudo object-level supervision and complementary spatial-temporal constraints. In this following, we introduce the construction of pseudo-labels in Section 4.1, the auxiliary constraints in Section 4.2, and the overall training objective in Section 4.3.

4.1 Pseudo Object-label Construction

To avoid expensive manual annotations, we construct pseudo object-labels as proxy supervision for object-centric reasoning. For each training sample, we first use a vision-language model [37] to

<!-- Page 6 -->

extract a set of supporting object phrases from the question-answer pair. These phrases correspond to the key objects required to answer the question. We then use the extracted phrases as text queries for an object detection model [25], which localizes the corresponding objects in sampled video frames.

Assume that each input video includes T frames, and each frame is divided into P visual patches by MLLM. For each frame, the detection model returns bounding boxes together with the associated confidence scores. We convert these detection outputs into two forms of pseudo supervision.

Soft Patch-level Masks ˜M ∈[0, 1]T ×P . Each entry ˜mt,p measures the overlap ratio between patch p in frame t and the detected object regions. A value of 0 indicates no overlap, 1 indicates full coverage by a bounding box, and intermediate values represent partial overlap.

Object Presence Indicators ˜y ∈{0, 1}T . Each entry ˜yt indicates whether frame t contains the supporting objects with sufficiently high confidence. Specifically, ˜yt = I[¯st > τ], where ¯st denotes the average confidence score of all bounding boxes in frame t, τ is a confidence threshold, and I[·] is the indicator function.

Together, ˜M provides spatial supervision over where relevant objects appear, while ˜y provides temporal supervision over when they appear. To improve robustness, we further incorporate detection confidence into the loss weighting to reduce the influence of unreliable pseudo-labels during training.

4.2 Spatial and Temporal Constraints

Based on the pseudo object-labels, we introduce two auxiliary constraints on the visual representation to encourage the model to attend to question-relevant objects and their temporal dynamics.

Spatial Constraint: We introduce a lightweight spatial head after the MLLM vision encoder to predict αt,p ∈R for the p-th patch in frame t, indicating the probability that the patch overlaps with the supporting object regions. Given the pseudo-label soft mask ˜M, spatial constraint is defined as:

T X

P X

Lspl = 1 TP

t=1

p=1 wt [ ˜mt,p log(αt,p) + (1 −˜mt,p) log(1 −(αt,p))] , (1)

where wt ∈[0, 1] is a frame-level reliability weight derived from the average confidence score of all bounding boxes in frame t. This constraint encourages the model to focus on spatial regions associated with supporting objects. However, spatial localization alone is insufficient for objectcentric procedural reasoning, because critical object states may only appear at specific moments in the procedure. We therefore introduce a complementary temporal constraint.

Temporal Constraint: We introduce another lightweight temporal head after the vision encoder to predict βt ∈R for the t-th frame, indicating the probability that frame t contains supporting objects relevant to the question. Given the pseudo-label object presence indicator ˜y, the temporal constraint is defined as:

T X

Ltmp = 1

T

t=1 [˜yt log σ(βt) + (1 −˜yt) log(1 −σ(βt))] . (2)

This constraint encourages the model to focus on specific frames demonstrating the dynamics of key objects. Together, the spatial and temporal constraints encourage the model to encode not only which objects are relevant, but also where and when the relevant object states appear to get the answer.

4.3 Training Objectives

Since our primary goal is fine-tuning MLLM, we retain the standard generative language modeling loss, Lgen, as the primary supervision signal for answer generation. To further encourage objectcentric reasoning, we augment this objective with the spatial and temporal constraints as auxiliary losses. The final training objective is formulated as: L = Lgen + λsplLspl + λtmpLtmp, (3) where λspl and λtmp control the strength of the auxiliary constraints. In practice, these weights are kept small and gradually warmed up during training, allowing the auxiliary supervision to shape the visual representation without overwhelming the answer-generation objective. Notably, the auxiliary heads are only used during training to encourage the learning of object-centric visual representations. During inference, all auxiliary modules are removed to maintain efficiency.

<!-- Page 7 -->

## 5 Experiments

5.1 Benchmark Models

To comprehensively evaluate existing models on PROCOBJECT-10K, we benchmark 13 models with diverse architectures, parameter sizes, and input modalities.

Blind LLMs: To explore whether commonsense reasoning can solve our benchmark task, we benchmark several language-only models, including GPT-5.4-Mini [32], Claude-Sonnet-4.6 [1], Qwen3-30B-A3B [51], and Llama-3.2-3B [9]. These models only receive textual prompts and questions without video input, forcing them to rely on internal priors to address procedural questions.

MLLMs: We benchmark representative commercial and open-source multimodal models that takes both video and text as input. Commercial models include Gemini-3.1-Flash-Lite [11], GPT-5.4-Mini [32], and Claude-Sonnet-4.6 [1]. Open-source baselines include variants of the InternVL3.5 family (4B, 8B, and 38B) [46] and the Qwen3-VL family (4B, 8B, and 30B-A3B) [3].

Fine-tuned Model: We also benchmark the Qwen3-VL-4B model [3] fine-tuned using object-centric SFT on PROCOBJECT-10K training data to demonstrate its effectiveness.

5.2 Evaluation Metrics

Our benchmark includes a comprehensive evaluation that jointly measures open-ended question answering and temporal grounding accuracy, enabling rigorous assessment of model performance.

Answering Metrics. We evaluate the generated responses using a bi-level quantitative approach. At the textual-embedding level, we use Sentence Similarity (S.) [38] and BERT-Score F1 (B.) [55] to provide reproducible measurements. At the natural-language level, we employ the LLM-asJudge paradigm to assess linguistic coherence. Following previous works [23, 26], we utilize GPT-5-mini [31], Qwen3 (2B) [51] and Llama-3.2 (3B) [9] to evaluate answers independently from four dimensions using a 0–5 scoring scale: Contextual Integration for factual consistency, Detail Orientation for fine-grained completeness, Contextual Understanding for narrative and causal flow, and Temporal Understanding for event ordering and state transitions. The final LLM-as-judge score (J.) is the average across all four dimensions and all LLM-as-judge models.

Grounding Metrics. To evaluate the ability to localize visual evidence, we measure the alignment between the predicted and ground-truth evidence of temporal segments. Since each QA pair in PROCOBJECT-10K may be supported by multiple non-overlapping intervals, we follow previous work [6] and adopt a set-level Intersection-over-Union (IoU) metric. For each sample with m predicted spans ˆT = { ˆTi}m i=1 and n ground-truth spans T = {Tj}n j=1:

IoU(T , ˆT ) =

Pm i=1 Pn j=1 | ˆTi ∩Tj| Sm i=1 ˆTi ∪Sn j=1 Tj . (4)

In the following experiments, we report the mean IoU across all samples. We additionally report mean IoP and mean IoG [6], which replace the denominator with Sm i=1 ˆTi and Sn j=1 Tj respectively, serving as precision- and recall-style counterparts.

5.3 Quantitative Results and Analysis

**Table 2 and Figure 4 demonstrate the experimental results on PROCOBJECT-10K. We have the following findings: 1) Overall grounding performance remains constrained: Across all benchmark models, temporal grounding precision is notably low, with mIoU scores consistently falling below 45%. 2) A sharp answering-grounding gap reveals reliance on priors: There is a significant discrepancy between answering plausibility and grounding precision, suggesting that models often rely on internal linguistic and commonsense priors rather than actual visual evidence. 3) Blind LLMs show competitive linguistic performance: The reliance on commonsense reasoning is further validated by language-only models, such as GPT-5.4-Mini (Blind), which achieves an Answering Judge score of 3.14 despite having no video input, only marginally lower than its MLLM counterpart’s 3.42. 4) Parameter scaling yields limited gains: Increasing model size does not inherently solve**

<!-- Page 8 -->

**Table 2: Evaluation results on testing set of PROCOBJECT-10K. Answering is evaluated by Sentence Similarity (S.), BERT-Score F1 (B.), and LLM-as-Judge score (J.). Grounding is evaluated by mean IoU%, mean IoP%, and mean IoG%. The Best and Second-best results are highlighted.**

Methods Multi-hop Reasoning Needle-in-a-Haystack All

Answering Grounding Answering Grounding Answering Grounding

S. B. J. IoU IoP IoG S. B. J. IoU IoP IoG S. B. J. IoU IoP IoG

Blind LLM GPT-5.4-Mini 74.0 89.2 3.13 -71.7 89.0 3.15 -73.1 89.1 3.14 -Claude-Sonnet-4.6 74.5 88.3 3.51 -69.2 87.6 3.08 -72.4 88.0 3.34 -Qwen3-30B-A3B 71.0 88.8 3.07 -68.9 88.7 2.98 -70.2 88.8 3.04 -Llama-3.2-3B 65.2 88.1 2.50 -60.5 87.9 2.45 -63.4 88.0 2.48 -Closed-source MLLMs Gemini-3.1-Flash-Lite 68.3 89.7 3.34 38.7 71.3 48.2 75.4 88.9 2.45 31.0 46.2 50.8 71.1 89.4 2.99 35.7 61.5 49.2 GPT-5.4-Mini 72.1 88.9 3.58 45.8 68.5 62.0 75.6 88.6 3.16 32.6 44.9 60.7 73.5 88.8 3.42 40.7 59.3 61.5 Claude-Sonnet-4.6 65.4 85.6 3.86 48.1 70.8 62.6 70.7 85.6 3.30 32.5 46.3 55.1 67.5 85.6 3.64 42.0 61.3 59.7 Open-source MLLMs InternVL3.5-4B 70.8 88.9 2.30 19.8 46.4 32.9 65.4 88.3 1.86 12.6 29.8 32.6 68.7 88.7 2.13 17.0 39.9 32.8 InternVL3.5-8B 76.6 90.2 2.88 22.4 66.5 27.6 69.4 89.5 2.29 16.0 49.1 21.9 73.8 89.9 2.65 19.9 59.7 25.4 InternVL3.5-38B 76.2 90.4 2.84 27.2 72.8 33.1 70.4 89.9 2.39 22.8 52.8 30.8 73.9 90.2 2.66 25.5 65.0 32.2 Qwen3-VL-4B 70.7 89.5 3.08 43.3 71.4 56.5 77.5 88.6 2.34 31.3 50.2 50.2 73.3 89.2 2.79 38.6 63.1 54.1 Qwen3-VL-8B 71.5 89.8 3.28 40.9 72.3 52.8 78.7 88.6 2.51 30.3 50.7 47.1 74.3 89.3 2.98 36.8 63.9 50.6 Qwen3-VL-30B-A3B 71.1 90.0 3.35 42.4 71.2 54.6 78.9 88.9 2.67 32.9 53.7 48.5 74.1 89.6 3.09 38.7 64.4 52.2 Object-centric Fine-tuned Qwen3-VL-4B 78.0 92.2 3.79 51.4 73.1 62.5 82.4 91.6 3.07 33.6 54.5 44.6 79.7 92.0 3.51 44.5 65.9 55.5

Gemini-3.1-Flash-Lite

GPT-5.4-Mini

Precondition

Claude-Sonnet-4.6

InternVL3.5-4B

4.8

4.4

InternVL3.5-8B

4.0

3.6

3.2

2.8

2.4

Readiness

Counterfactual

2.0

1.6

1.2

0.8

0.4

Evolution Mistake

(a) LLM-as-Judge Score (b) Evidence IoU (%)

InternVL3.5-38B

Qwen3-VL-4B

Precondition

Qwen3-VL-8B

Qwen3-VL-30B

70

Qwen3-VL-4B (Obj-FT)

60

50

40

30

Readiness

Counterfactual

20

10

0

Evolution Mistake

**Figure 4: Performance comparison of MLLMs across five QA types on PROCOBJECT-10K.**

object-centric understanding, as observed by the marginal improvements when scaling from 4B to greater than 30B variants of Qwen3-VL and InternVL3.5 families. 5) “Needle-in-a-Haystack” reasoning is more challenging. Pinpointing very short-term but critical object-state appearance or transitions within long sequences is harder than Multi-hop Reasoning, reflecting the difficulty of identifying sparse dynamics within complex human-object interactions. 6) Object-centric SFT demonstrates superior effectiveness: Our fine-tuned Qwen3-VL-4B model achieves the highest performance across most metrics, with its 4B-parameter architecture surpassing much larger opensource and proprietary baselines, illustrating the effectiveness of our finetuning paradigm. Detailed analysis of performance improvement will be conducted through ablation study in Section 5.4.

5.4 Ablation Study Table 3: Ablation study results.

We conduct ablation study on PROCOBJECT-10K to evaluate the effectiveness of each training objective. Table 3 shows that all losses contribute positively to performance, with the temporal constraint providing more substantial gains. The full loss achieves the best results, validating the effectiveness of combining spatial and temporal supervision to model the dynamic evolution of objects.

Lgen Lspl Ltmp J. IoU

✓ 3.18 42.4 ✓ ✓ 3.21 42.5 ✓ ✓ 3.26 43.4 ✓ ✓ ✓ 3.51 44.5

<!-- Page 9 -->

**Table 4: Evaluation results on MULTIHOP-EGOQA benchmark. The Best and Second-best results are highlighted.**

Methods Input Temporal Grounding Question Answering

mIoP mIoG IoU@0.3 mIoU Sent. Sim. LLM Score

Human – 71.8 81.0 87.0 61.8 74.3 7.5 Zero-shot Inference GPT-4o 60 f 18.9 24.4 12.0 12.2 73.7 5.4 InternVL2-8B 30 f 11.8 24.0 6.3 6.6 71.9 4.5 LLaVA-NeXT-Video-7B 32 f – – – – 62.1 4.2 TimeChat-7B 96 f 10.2 5.6 3.0 3.6 58.9 3.3 VTimeLLM-7B 100 f 12.4 28.2 8.8 9.2 70.5 4.3 Qwen3-VL-4B 60 f 19.0 38.1 15.5 13.1 50.0 3.2 Fine-tuned on MULTIHOP-EGOQA GeLM-7B 180 f 23.7 41.0 18.2 16.7 75.0 4.8 Fine-tuned on PROCOBJECT-10K Qwen3-VL-4B‡ 60 f 31.4 32.1 25.2 19.8 73.6 5.3 Qwen3-VL-4B 60 f 34.8 37.3 30.8 22.6 74.6 5.4

5.5 Generalization Analysis

**Table 5: Evaluation results on ALFRED benchmark [40].**

Methods Test Seen Test Unseen

SR GC SR GC

Zero-shot MLLM GPT-4 37.40 48.30 34.90 44.90 Qwen3-VL-4B 30.50 43.60 29.40 40.50 Flare+Zero-shot MLLM Planner FLARE+LLaMA2 16.96 24.84 17.79 27.40 FLARE+Vicuna 20.61 29.57 22.04 33.57 FLARE+GPT-3.5 32.55 42.02 31.79 43.94 FLARE+GPT-4 40.05 48.84 40.88 51.72 FLARE+Qwen3-VL-4B 31.50 44.20 32.01 44.62 Flare+Fine-tuned MLLM Planner FLARE+Qwen3-VL-4B‡ 36.97 46.74 38.02 50.08 FLARE+Qwen3-VL-4B 40.50 48.20 41.33 51.97

To evaluate the generalization of procedural priors learned from PROCOBJECT-10K, we test the fine-tuned models on (1) zero-shot grounded VideoQA task using MultiHop-EgoQA benchmark [6] , and (2) zero-shot embodied planning task using ALFRED benchmark [40]. Qwen3-VL-4B‡ denotes the model variant fine-tuned only with generative loss Lgen without spatial-temporal constraints.

Grounded VideoQA. The MultiHop-EgoQA dataset [6] also focuses on instructional videos, but contains general procedural questions rather than object-centric reasoning tasks. We follow their LLM-as-Judge protocol to ensure fair comparison, where scores range from 0 to 10 (higher is better). As shown in Table 4, even when trained using only the standard generative objective, Qwen3-VL-4B‡ outperforms the zero-shot baseline and achieves performance competitive with GeLM-7B, which is specifically fine-tuned on the MultiHop-EgoQA training set. This result suggests that PROCOBJECT10K enables models to learn transferable object-centric procedural representations that generalize effectively to related tasks. Furthermore, the model trained with our full object-centric SFT framework (Qwen3-VL-4B) achieves additional gains, obtaining the best performance across both grounding and answering metrics. These results further demonstrate the effectiveness and generalization capability of our proposed fine-tuning paradigm for complex spatial-temporal procedural understanding.

Embodied Planning. ALFRED [40] is a benchmark for embodied agents to perform long horizon household tasks from language instructions. Its test set comprises seen scenarios included in the training data, and the unseen scenarios for evaluation. We evaluate transfer performance by integrating models fine-tuned on PROCOBJECT-10K into the FLARE framework [19], a prior method specifically designed for this task, as the zero-shot planner. Table 5 shows that the fine-tuned Qwen3VL-4B models achieve improvements over the zero-shot baseline. Even when trained only with the standard generative objective, Flare+Qwen3-VL-4B‡ outperforms several zero-shot MLLM planners. Furthermore, the object-centric fine-tuned model Flare+Qwen3-VL-4B achieves the best overall performance. These results demonstrate the generalization of the object-centric representations learned from PROCOBJECT-10K, suggesting its potential to benefit embodied reasoning tasks beyond procedural understanding in instructional videos.

## 6 Conclusion and Discussion

Instruction: Place a cooked slice of apple to the right

of the yellow knife on the counter.

T=55

T=175

(Pickup, Knife)

(Slice, Apple)

T=213

T=216

(Heat, AppleSliced)

(Put, CounterTop)

**Figure 5: Visualization of embodied planning on ALFRED.**

We present PROCOBJECT-10K, a benchmark that reframes procedural video understanding around object state evolution rather than action sequences. Evaluation results on leading MLLMs reveal a persistent answering-grounding gap: models generate plausible answers while failing to localize the supporting evidence, indicating reliance on linguistic priors over fine-grained object dynamics. As a step toward closing this gap, we further provide an object-centric SFT baseline that narrows the gap and yields representations transferable to other grounded VideoQA and embodied planning tasks. We hope PROCOBJECT-10K could catalyze further research on object-centric procedural reasoning with broader domain coverage and tighter integration with embodied agents and world models.

<!-- Page 10 -->

## References

[1] Anthropic. Introducing claude sonnet 4.6. https://www.anthropic.com/news/claude-sonnet-4-6, 2026. Accessed: 2026-05-06.

[2] Kumar Ashutosh, Santhosh Kumar Ramakrishnan, Triantafyllos Afouras, and Kristen Grauman. Videomined task graphs for keystep recognition in instructional videos. Advances in Neural Information Processing Systems, 36:67833–67846, 2023.

[3] Shuai Bai, Yuxuan Cai, Ruizhe Chen, Keqin Chen, Xionghui Chen, Zesen Cheng, Lianghao Deng, Wei Ding, Chang Gao, Chunjiang Ge, et al. Qwen3-vl technical report. arXiv preprint arXiv:2511.21631, 2025.

[4] Wentao Bao, Qi Yu, and Yu Kong. Evidential deep learning for open set action recognition. In Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV), pages 13349–13358, October 2021.

[5] Jr-Jen Chen, Yu-Chien Liao, Hsi-Che Lin, Yu-Chu Yu, Yen-Chun Chen, and Frank Wang. Rextime: A benchmark suite for reasoning-across-time in videos. Advances in Neural Information Processing Systems, 37:28662–28673, 2024.

[6] Qirui Chen, Shangzhe Di, and Weidi Xie. Grounded multi-hop videoqa in long-form egocentric videos. In Proceedings of the AAAI Conference on Artificial Intelligence, volume 39, pages 2159–2167, 2025.

[7] Dima Damen, Hazel Doughty, Giovanni Maria Farinella, Sanja Fidler, Antonino Furnari, Evangelos Kazakos, Davide Moltisanti, Jonathan Munro, Toby Perrett, Will Price, et al. The epic-kitchens dataset: Collection, challenges and baselines. IEEE Transactions on Pattern Analysis and Machine Intelligence, 43(11):4125–4141, 2020.

[8] Shangzhe Di and Weidi Xie. Grounded question-answering in long egocentric videos. In CVPR, 2024.

[9] Abhimanyu Dubey, Abhinav Jauhri, Abhinav Pandey, Abhishek Kadian, Ahmad Al-Dahle, Aiesha Letman, Akhil Mathur, Alan Schelten, Amy Yang, Angela Fan, et al. The llama 3 herd of models. arXiv e-prints, pages arXiv–2407, 2024.

[10] Dayoung Gong, Joonseok Lee, Manjin Kim, Seong Jong Ha, and Minsu Cho. Future transformer for long-term action anticipation. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 3052–3061, 2022.

[11] Google. Gemini 3.1 flash-lite: Built for intelligence at scale. https://blog.google/ innovation-and-ai/models-and-research/gemini-models/gemini-3-1-flash-lite/, 2026. Accessed: 2026-05-06.

[12] Wenliang Guo, Yujiang Pu, and Yu Kong. Procedural mistake detection via action effect modeling. In The Fourteenth International Conference on Learning Representations, 2026.

[13] Ayush Gupta, Anirban Roy, Rama Chellappa, Nathaniel D Bastian, Alvaro Velasquez, and Susmit Jha. Toga: Temporally grounded open-ended video qa with weak supervision. In Proceedings of the IEEE/CVF International Conference on Computer Vision, pages 23593–23603, 2025.

[14] Kimihiro Hasegawa, Wiradee Imrattanatrai, Masaki Asada, Susan Holm, Yuran Wang, Vincent Zhou, Ken Fukuda, and Teruko Mitamura. Promqa-assembly: Multimodal procedural qa dataset on assembly. arXiv preprint arXiv:2509.02949, 2025.

[15] Kimihiro Hasegawa, Wiradee Imrattanatrai, Zhi-Qi Cheng, Masaki Asada, Susan Holm, Yuran Wang, Ken Fukuda, and Teruko Mitamura. Promqa: Question answering dataset for multimodal procedural activity understanding. arXiv preprint arXiv:2410.22211, 2024.

[16] Wei-Jin Huang, Yuan-Ming Li, Zhi-Wei Xia, Yu-Ming Tang, Kun-Yu Lin, Jian-Fang Hu, and Wei-Shi Zheng. Modeling multiple normal action representations for error detection in procedural tasks. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), pages 27794–27804, June 2025.

[17] Yunseok Jang, Sungryull Sohn, Lajanugen Logeswaran, Tiange Luo, Moontae Lee, and Honglak Lee. Multimodal subtask graph generation from instructional videos. arXiv preprint arXiv:2302.08672, 2023.

[18] Yunseok Jang, Yale Song, Youngjae Yu, Youngjin Kim, and Gunhee Kim. Tgif-qa: Toward spatio-temporal reasoning in visual question answering. In Proceedings of the IEEE conference on computer vision and pattern recognition, pages 2758–2766, 2017.

<!-- Page 11 -->

[19] Taewoong Kim, Byeonghwi Kim, and Jonghyun Choi. Multi-modal grounded planning and efficient replanning for learning embodied agents with a few examples. In AAAI, 2025.

[20] Shih-Po Lee and Ehsan Elhamifar. Error recognition in procedural videos using generalized task graph. In Proceedings of the IEEE/CVF International Conference on Computer Vision, pages 10009–10021, 2025.

[21] Shih-Po Lee, Zijia Lu, Zekun Zhang, Minh Hoai, and Ehsan Elhamifar. Error detection in egocentric procedural task videos. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 18655–18666, 2024.

[22] Han Lin, Tushar Nagarajan, Nicolas Ballas, Mido Assran, Mojtaba Komeili, Mohit Bansal, and Koustuv Sinha. Vedit: Latent prediction architecture for procedural video representation learning. arXiv preprint arXiv:2410.03478, 2024.

[23] Bo Liu, Pengfei Qiao, Minhan Ma, Xuange Zhang, Yinan Tang, Peng Xu, Kun Liu, and Tongtong Yuan. Surveillancevqa-589k: A benchmark for comprehensive surveillance video-language understanding with large models. arXiv preprint arXiv:2505.12589, 2025.

[24] Hanchao Liu, Wenyuan Xue, Yifei Chen, Dapeng Chen, Xiutian Zhao, Ke Wang, Liping Hou, Rongjun Li, and Wei Peng. A survey on hallucination in large vision-language models. arXiv preprint arXiv:2402.00253, 2024.

[25] Shilong Liu, Zhaoyang Zeng, Tianhe Ren, Feng Li, Hao Zhang, Jie Yang, Chunyuan Li, Jianwei Yang, Hang Su, Jun Zhu, et al. Grounding dino: Marrying dino with grounded pre-training for open-set object detection. arXiv preprint arXiv:2303.05499, 2023.

[26] Muhammad Maaz, Hanoona Rasheed, Salman Khan, and Fahad Shahbaz Khan. Videogpt+: Integrating image and video encoders for enhanced video understanding. arxiv, 2024.

[27] Karttikeya Mangalam, Raiymbek Akshulakov, and Jitendra Malik. Egoschema: A diagnostic benchmark for very long-form video language understanding. Advances in Neural Information Processing Systems, 36:46212–46244, 2023.

[28] Medhini Narasimhan, Licheng Yu, Sean Bell, Ning Zhang, and Trevor Darrell. Learning and verification of task structure in instructional videos. arXiv preprint arXiv:2303.13519, 2023.

[29] Yulei Niu, Wenliang Guo, Long Chen, Xudong Lin, and Shih-Fu Chang. Schema: State changes matter for procedure planning in instructional videos. arXiv preprint arXiv:2403.01599, 2024.

[30] OpenAI. GPT-4o System Card, 2024.

[31] OpenAI. Gpt-5: Large language model, 2025. Accessed: 2025-11-13.

[32] OpenAI. Introducing gpt-5.4 mini and nano. https://openai.com/index/ introducing-gpt-5-4-mini-and-nano/, 2026. Accessed: 2026-05-06.

[33] Ege Özsoy, Arda Mamur, Felix Tristram, Chantal Pellegrini, Magdalena Wysocki, Benjamin Busam, and Nassir Navab. Egoexor: An ego-exo-centric operating room dataset for surgical activity understanding. arXiv preprint arXiv:2505.24287, 2025.

[34] Constantin Patsch, Yuankai Wu, Marsil Zakour, Driton Salihu, and Eckehard Steinbach. Mistsense: Versatile online detection of procedural and execution mistakes. In Proceedings of the IEEE/CVF International Conference on Computer Vision, pages 14528–14537, 2025.

[35] Rohith Peddi, Shivvrat Arya, Bharath Challa, Likhitha Pallapothula, Akshay Vyas, Bhavya Gouripeddi, Qifan Zhang, Jikai Wang, Vasundhara Komaragiri, Eric Ragan, et al. Captaincook4d: A dataset for understanding errors in procedural activities. Advances in Neural Information Processing Systems, 37:135626–135679, 2024.

[36] Haozhe Qi, Shaokai Ye, Alexander Mathis, and Mackenzie W Mathis. LLaVAction: evaluating and training multi-modal large language models for action understanding. In The Fourteenth International Conference on Learning Representations, 2026.

[37] Qwen Team. Qwen3.5: Towards native multimodal agents, February 2026.

[38] Nils Reimers and Iryna Gurevych. Sentence-bert: Sentence embeddings using siamese bert-networks. In Proceedings of the 2019 conference on empirical methods in natural language processing and the 9th international joint conference on natural language processing (EMNLP-IJCNLP), pages 3982–3992, 2019.

<!-- Page 12 -->

[39] Fadime Sener, Dibyadip Chatterjee, Daniel Shelepov, Kun He, Dipika Singhania, Robert Wang, and Angela Yao. Assembly101: A large-scale multi-view video dataset for understanding procedural activities. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 21096– 21106, 2022.

[40] Mohit Shridhar, Jesse Thomason, Daniel Gordon, Yonatan Bisk, Winson Han, Roozbeh Mottaghi, Luke Zettlemoyer, and Dieter Fox. ALFRED: A Benchmark for Interpreting Grounded Instructions for Everyday Tasks. In The IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2020.

[41] Sungryull Sohn, Hyunjae Woo, Jongwook Choi, and Honglak Lee. Meta reinforcement learning with autonomous inference of subtask dependencies. arXiv preprint arXiv:2001.00248, 2020.

[42] Bilge Soran, Ali Farhadi, and Linda Shapiro. Generating notifications for missing actions: Don’t forget to turn the lights off! In Proceedings of the IEEE International Conference on Computer Vision, pages 4669–4677, 2015.

[43] Tomáš Souˇcek, Jean-Baptiste Alayrac, Antoine Miech, Ivan Laptev, and Josef Sivic. Look for the change: Learning object states and state-modifying actions from untrimmed web videos. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 13956–13966, 2022.

[44] Yansong Tang, Dajun Ding, Yongming Rao, Yu Zheng, Danyang Zhang, Lili Zhao, Jiwen Lu, and Jie Zhou. Coin: A large-scale dataset for comprehensive instructional video analysis. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 1207–1216, 2019.

[45] Haochen Wang, Qirui Chen, Cilin Yan, Jiayin Cai, Xiaolong Jiang, Yao Hu, Weidi Xie, and Stratis Gavves. Object-centric video question answering with visual grounding and referring. In Proceedings of the IEEE/CVF International Conference on Computer Vision, pages 22274–22284, 2025.

[46] Weiyun Wang, Zhangwei Gao, Lixin Gu, Hengjun Pu, Long Cui, Xingguang Wei, Zhaoyang Liu, Linglin Jing, Shenglong Ye, Jie Shao, et al. Internvl3.5: Advancing open-source multimodal models in versatility, reasoning, and efficiency. arXiv preprint arXiv:2508.18265, 2025.

[47] Xin Wang, Taein Kwon, Mahdi Rad, Bowen Pan, Ishani Chakraborty, Sean Andrist, Dan Bohus, Ashley Feniello, Bugra Tekin, Felipe Vieira Frujeri, Neel Joshi, and Marc Pollefeys. Holoassist: an egocentric human interaction dataset for interactive ai assistants in the real world. In Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV), pages 20270–20281, October 2023.

[48] Yibing Wei, Samuel Church, Victor Suciu, Jinhong Lin, Cheng-En Wu, and Pedro Morgado. Trackverse: A large-scale object-centric video dataset for image-level representation learning. In Proceedings of the IEEE/CVF International Conference on Computer Vision, pages 11153–11163, 2025.

[49] Junbin Xiao, Angela Yao, Yicong Li, and Tat-Seng Chua. Can i trust your answer? visually grounded video question answering. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 13204–13214, 2024.

[50] Cilin Yan, Haochen Wang, Shilin Yan, Xiaolong Jiang, Yao Hu, Guoliang Kang, Weidi Xie, and Efstratios Gavves. Visa: Reasoning video object segmentation via large language models. In European Conference on Computer Vision, pages 98–115. Springer, 2024.

[51] An Yang, Anfeng Li, Baosong Yang, Beichen Zhang, Binyuan Hui, Bo Zheng, Bowen Yu, Chang Gao, Chengen Huang, Chenxu Lv, et al. Qwen3 technical report. arXiv preprint arXiv:2505.09388, 2025.

[52] Zhou Yu, Dejing Xu, Jun Yu, Ting Yu, Zhou Zhao, Yueting Zhuang, and Dacheng Tao. Activitynet-qa: A dataset for understanding complex web videos via question answering. In Proceedings of the AAAI Conference on Artificial Intelligence, volume 33, pages 9127–9134, 2019.

[53] Parnian Zameni, Yuhan Shen, and Ehsan Elhamifar. Moscato: Predicting multiple object state change through actions. In Proceedings of the IEEE/CVF International Conference on Computer Vision, pages 11600–11611, 2025.

[54] Chen-Lin Zhang, Jianxin Wu, and Yin Li. Actionformer: Localizing moments of actions with transformers. In European Conference on Computer Vision, pages 492–510. Springer, 2022.

[55] Tianyi Zhang, Varsha Kishore, Felix Wu, Kilian Q Weinberger, and Yoav Artzi. Bertscore: Evaluating text generation with bert. arXiv preprint arXiv:1904.09675, 2019.

[56] Yiwu Zhong, Licheng Yu, Yang Bai, Shangwen Li, Xueting Yan, and Yin Li. Learning procedure-aware video representation from instructional videos and their narrations. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 14825–14835, 2023.
