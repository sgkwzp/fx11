---
source_pdf: "Tang_Spacewalk-18_A_Benchmark_for_Multimodal_and_Long-form_Procedural_Video_Understanding_WACV_2026_paper.pdf"
pages: 11
conversion: "Automatic PDF-to-Markdown text conversion; figures are represented by extracted captions/text rather than embedded images."
---

# Spacewalk-18: A Benchmark for Multimodal and Long-form Procedural Video Understanding in Novel Domains

<!-- Page 1 -->

Zitian Tang* Rohan Myer Krishnan* Zhiqiu Yu Chen Sun Brown University

## Abstract

Learning from (procedural) videos has increasingly served as a pathway for embodied agents to acquire skills from human demonstrations. To do this, video understanding models must be able to obtain structured understandings, such as the temporal segmentation of a demonstration into sequences of actions and skills, and to generalize the understandings to novel environments, tasks, and problem domains. In pursuit of this goal, we introduce Spacewalk-18, a benchmark containing two tasks: (1) step recognition and (2) video question answering, over a dataset of temporally segmented and labeled tasks in International Space Station spacewalk recordings. In tandem, the two tasks quantify a model’s ability to: (1) generalize to novel domains; (2) utilize long temporal context and multimodal (e.g. visual and speech) information. Our extensive experimental analysis highlights the challenges of Spacewalk-18, but also suggests best practices for domain generalization and long-form understanding. Notably, we discover a promising adaptation via summarization technique that leads to significant performance improvement without model fine-tuning. The Spacewalk-18 benchmark is released at https:// brown-palm.github.io/Spacewalk-18.

## 1. Introduction

This is Ground Control to Major Tom You’ve really made the grade And the papers want to know whose shirts you wear Now it’s time to leave the capsule, if you dare

Space Oddity

Procedural videos, such as how-to or cooking, are produced to spread knowledge for human learners. With the recent advances in video understanding and robotic learning [3, 6, 48], these videos have become increasingly valuable learning resources for robots too. However, in order for machines to efficiently learn complex tasks and work alongside humans, they must be able to distill complex procedures into series of steps with only a few examples, often in a previously unseen environment. This challenge inspires

**Figure 1. Key properties of Spacewalk-18: (1) Domain generalization: a side-by-side comparison of a sample frame from Spacewalk-18 and Ego4D [19] illustrates our benchmark’s novel domain. (2) Multimodal: the visual content of the left frame does not align with its audio/speech. Instead, the speech corresponding to the right frame describes the left frame. (3) Long-form: the left frame shows the astronaut working on the solar array and the right frame shows him releasing bolts. These contextualize each other to identify that he is releasing the solar array in both frames.**

the development of procedural video understanding systems that are able to generalize to previously unseen scenarios, and require minimal human supervision.

We introduce a new benchmark, Spacewalk-18, to advance multimodal, long-form, and procedural video understanding in a novel domain. Whereas most of the existing procedural video benchmarks are sourced from daily household scenarios [10, 19, 20, 26, 51, 53], the Spacewalk-18 dataset comprises video clips from 18 recorded extravehic-

<!-- Page 2 -->

ular activities (spacewalks) outside the International Space Station. Videos in this domain are naturally limited by the number of recorded spacewalks. As illustrated in Fig. 1, our dataset is first and foremost a benchmark to test pre-trained video-language models’ (e.g. [2, 61]) capability to leave the capsule and generalize to novel domains. Spacewalk-18 is also inherently multimodal and requires effective incorporation of long-form temporal context. In Spacewalk-18, astronauts go on spacewalks for a variety of reasons, including to perform experiments, test equipment, or carry out maintenance/repairs. The recordings follow a fairly rigid agenda, often illustrated through a short animated sequence giving an overview of the steps planned for the spacewalk. Since our goal is to evaluate a (pre-trained) model’s generalization capability, we follow the design choice of recent benchmarks with similar goals (e.g., [34, 39]) and provide a moderate-sized training data for adaptation rather than pre-training. In order to collect detailed and dense temporal annotations for the training, validation, and test videos, we introduce a new protocol for temporal segmentation and action labeling to efficiently annotate the recordings and categorize content from the mission into the corresponding animated steps. As illustrated in Figure 2, the collected annotations provide a structured representation of each spacewalk recording as a sequence of steps, each of which is in the form of a text description (e.g., “install thermal blanket on degraded antenna”), an animated illustration, and the temporal boundaries within the recording where the step is performed. We define two proxy tasks for evaluation: 1. The step recognition task evaluates the model’s ability to generalize to the spacewalk domain and incorporate video and text content into predictions. 2. The question answering task benchmarks the model’s ability to perform spatiotemporal reasoning. We evaluate state-of-the-art video-language models, including contrastive video-language models (VLMs) [31, 61, 62, 67] and video large language models (VLLMs) [4, 9, 47, 59, 73, 76]. Through our experiments, we identify several best practices for incorporating multimodal information and long-form temporal context. Our human evaluation shows that an average English speaker achieves 67% step recognition performance after viewing 3.5-minute “training” example video, far exceeding any state-of-the-art VLLMs or proprietary APIs (e.g., GPT-5), suggesting a large room for improvement. Surprisingly, we discover that the most effective adaptation technique is to provide a frozen VLLM a summarized video as its context, and the relative gain is significantly larger compared to directly fine-tuning a model on the training split of Spacewalk-18. Our contributions are three-fold: First, we propose a new procedural video benchmark on learning structured video representations and reasoning. Second, we collect the

Spacewalk-18 dataset with a new protocol for efficient annotation. It contains 96 hours of densely annotated videos and spans over 455 animated steps. Finally, we conduct extensive experimental analyses and discover the adaptation via summarization technique for effective domain generalization in Spacewalk-18.

## 2. Related Work

Procedural Video Understanding has important applications in video summarization [38], human machine interaction [37], procedural planning [8], and robotic learning [14, 65]. Common tasks defined for procedural video understanding include temporal action segmentation [13, 15] and detection [42], step localization [53, 77] and prediction [1, 18, 19]. They either use dense segment-level annotations or weak video-level labels [35, 43, 50]. Existing procedural video benchmarks are mainly sourced from two domains, how-to videos from online platforms [35, 49, 53, 77], and ego-centric videos collected from recruited actors [10, 19, 46] in kitchens and other household scenarios. More recently, the Perception Test [39] benchmark, aims to evaluate the zero-shot generalization capabilities of video-language foundation models. Our Spacewalk-18 benchmark is complementary to all of the existing procedural video understanding benchmarks: The spacewalk recordings are scripted and narrated in detail, yet cannot be solved by the visual or language modality alone. They exhibit strong structures and dependencies, and unfold over long temporal horizons. More importantly, unlike existing videos that are captured in kitchens and on earth, the space station is a novel domain, which simulates the real-world scenario of model deployment in unseen environments. Video-language Foundation Models. Inspired by the success with large language models [7, 12], video-language foundation models have been proposed by training with large amounts of image and video data with masked token prediction [17] or video-task matching [74]. The videos are often accompanied by text descriptions such as speech narration [17, 52, 70, 71]. When video and language are encoded separately, a contrastive learning objective can be employed [31, 67] in a similar fashion as its image-based counterpart [40]. The objectives can be combined [69] and the encoders for different modalities can be shared [58]. In addition to joint multimodal pretraining, researchers have demonstrated the effectiveness of adapting visual descriptions into a large language model, such as with gated cross-attention [2], instructional tuning [9, 32, 73], linearly projecting the visual embeddings into the language space [29, 36], or augmenting large language models with video frame captions [59, 72]. Despite their amazing progress, we have shown that the state-of-theart video-language models cannot generalize to Spacewalk18 benchmark, whether zero-shot or with fine-tuning.

<!-- Page 3 -->

**Figure 2. A spacewalk recording can be 7 or 8 hours long. The step recognition task aims to assign each video clip in the recording a step label, which is illustrated by a short animation and a text description. The question answering task targets video reasoning with long-term multimodal context. Both serve as intermediate benchmarks towards the “Goal”, which aims to represent a long procedural video as a sequence of steps and their corresponding video demonstrations for understanding and reasoning.**

Long-form Video Understanding is an important open question for video representation learning. Earlier approaches [54] aim to extract high-level information such as character relationships from movies, where the solution is dominated by language-based approaches. Wu et al. [63] proposed an object-centric self-supervised framework, along with a benchmark for long-form video understanding. To encode long-form temporal context, memory-based approaches [64] have been proposed, along with new architectures that better scale with longer sequences [23, 30]. Our work is inspired by the recent dataset, EgoSchema [34], for question answering from long-form videos. Spacewalk-18 is complementary to EgoSchema, which is based on the same household videos as in Ego4D [19]. We also compute the temporal certificate [34] to measure the context needed for humans to solve a video understanding task, and observe that our task requires 40% more temporal context than EgoSchema.

## 3. The Spacewalk-18 Dataset

As shown in Figure 2, our goal is to segment spacewalk recordings into series of steps. A typical spacewalk video contains an animated preview of the steps to be performed, followed by the recording of astronauts performing the steps over multiple hours. We aim to annotate the spacewalk into a sequence of these steps. Each step corresponds to multiple video clips from recording, denoted by start and end times. Each step is accompanied with a text description annotated by a human expert, and illustrated by a video clip from the animated preview. We obtained 18 spacewalk recordings from YouTube. The total duration is 96 hours. For each video, we chunk the audio files into 10 minute clips and

feed them to Deepgram1 to extract speech transcripts.

### 3.1. Labeling Process

Building our dataset requires temporally segmenting and labeling very long videos. Due to the presence of video clips irrelevant to the spacewalk (e.g., scene of the mission control center in Figure 1), the temporal boundaries of a single step can be fragmented, making annotating the temporal boundaries time-consuming and measuring the inter-annotator agreement challenging. We propose to oversegment the recordings into short clips, each of which corresponds to at most a single step. We then ask the annotators to label each clip from a pre-defined list of steps for a spacewalk, while having access to an unbounded-length context from its neighboring video clips. The human annotators are thus free from having to draw detailed temporal boundaries, or to take multiple passes over the long video for step definition, temporal segmentation, and step labeling. We observe that this design drastically reduced human worker hours. Define the Label Space. Our label space is derived from the animated preview for each spacewalk. We manually segment the animations into steps ourselves, and label each segment with a step caption. On average, this results in around 25 steps per spacewalk video. Create Video Clips. We segment each video into short clips using PySceneDetect’s2 shot boundary detection algorithm. We find that due to the long nature of the steps and tendency of the camera to switch angles often in spacewalk recordings, a given shot-segmented clip rarely spans over one step: we randomly inspect the annotated clips and

1https://deepgram.com 2https://github.com/Breakthrough/PySceneDetect

<!-- Page 4 -->

observe that only 6% of them contain more than one step. Overall, only 4% of the frames are mislabeled due to shot boundaries. While imperfect, we believe the benefit of scaling up annotation significantly outweights the minimal impacts on evaluation metrics. Annotation Interface. To collect annotations, we build the Spacewalk Video Annotation Tool (example interface is shown in Appendix A.1). The interface displays the animated steps and clips for human annotators to label. They watch the annotation clips and select a label for each clip from the set of step labels, “Irrelevant”, or “Unsure”. This interface allows them to view the entire spacewalk recording as context. The “Irrelevant” label categorizes any clip that does not contain footage of one of the steps for the given spacewalk. This includes shots of the mission control center, noisy shots (e.g., blue screen), and shots of “getahead” steps that were not originally planned for the spacewalk. We have at least three human workers annotate each clip and we choose the most commonly selected label as the true label. Our annotation tool is publicly released. Merge Adjacent Clips. Since we intentionally oversegment the long spacewalk recordings before collecting annotations, we include a final step to use the collected annotations to obtain true temporal boundaries by merging all adjacent clips with identical labels. We provide supplementary videos to illustrate the Spacewalk videos and our collected annotations.

### 3.2. Dataset Statistics, Diversity, and Difficulty

The 18 spacewalk videos are segmented into in total 31,812 clips for human annotation. After annotation, adjacent clips with the same step labels are merged as post-processing, resulting in 3,753 merged clips. These clips have an average length of 92 seconds and span over 96 hours in total. There are 455 animated step labels across the dataset (excluding “Irrelevant”). Each step takes on average 5 merged clips and has an average of 9 minutes of video. To analyze the diversity of the dataset, we filter the nouns and verbs from the step captions to obtain a list of objects and actions. Our dataset has 51 diverse objects (e.g. battery) and 47 atomic actions (e.g. install and ingress) (See Appendix A.3). Spacewalk-18 is open-vocabulary, as the list of step labels varies across different videos. Example labels in the training set include “EV1 & EV2 remove the first battery” and “EV2 ingresses foot restraint”. In the test set, there are similar labels, such as “Chris & Bob remove new battery from slot A” and “Chris enters foot restraint”. The visual content in Spacewalk-18 is out-of-domain for most existing models. The shots of astronauts working in space are visually distinct from any worldly actions. There is also a mix of first- and third-person point of view camera angles. Additionally, the two astronauts in each spacewalk often work on different steps in the same time, leading to

steps appearing in multiple non-continuous segments. To reduce the impact of task difficulty on annotation accuracy, we offer training to annotators from online platform, and those who achieve at least 80% accuracy on a held-out set after training are recruited to annotate the dataset.

### 3.3. Task Definition on Spacewalk-18

The 18 recordings in Spacewalk-18 contain varying numbers of steps. To ensure each split contains a balanced number of steps, we manually split the dataset into training, validation, and test sets in a ratio of 10:2:6. We introduce two multimodal long-form video understanding tasks on Spacewalk-18, step recognition and question answering: Task 1 - Step Recognition. Our dataset divides the multihour spacewalk recording into several steps. Each step is described by an animated video clip, the transcript of its narration, and an annotated caption. Our step recognition task aims to recognize these steps from the spacewalk recording. Given a timestamp t and the list of K steps in the corresponding spacewalk mission, a model is tasked with determining the step occurring at timestamp t by predicting a label from {0, 1, 2, · · · , K} (label 0 stands for “Irrelevant”). We notice that temporal contexts are crucial for recognizing the steps, from which we can see the astronauts’ actions and know the completed and remaining steps. However, current video-language models are incapable of digesting hours-long spacewalk recordings. So we set a context window length w and offer the video clip [t −w/2, t + w/2] to the model. The clip includes both visual content and textual speech transcripts. We test models with varying w to investigate their ability to understand temporal contexts. We construct 2000 samples from each training, validation, and test video, resulting in an average of 1 sample for every 10 seconds of video. As the visual content of spacewalk videos does not change rapidly, these samples are sufficient to represent an entire video. To balance the categories, 2000/(K + 1) timestamps are uniformly sampled from each step’s corresponding video clips. These timestamps are used as the middle timestamps t in the task definition above across different context lengths. To evaluate model performance, we calculate Accuracy and mean Average Precision (mAP) to measure how accurately each sample is recognized. By merging temporally adjacent samples with the same predictions into intervals, we derive a temporal segmentation of each spacewalk mission. We also adopt the standard intersection-over-union (IoU) metric with the ground truth step boundary annotations to measure the segmentation correctness. We found that sampling more training or validation examples (thus denser in time) has little impact on the IoU metric, which are defined on continuous temporal boundaries. Detailed metric discussions can be found in Appendix B. Task 2 - Question Answering. For ease of evaluating

<!-- Page 5 -->

**Figure 3. Temporal certificate (“long-form-ness”) lengths across commonly adopted datasets with action recognition and question answering annotations. Spacewalk-18 is 1.4x the length of the nearest comparable (EgoSchema). Figure adapted from [34].**

video-language models on temporal understanding and reasoning with Spacewalk videos, we further introduce the question answering task. We first split each Spacewalk video into hour-long consecutive video segments, and then manually collect questions that are either high-level (e.g. what is the goal of this mission) or detailed and requires temporal localization and reasoning (e.g. What type of equipment does the astronaut retrieve first, and how is it utilized during the mission). Question answering is formulated as multiple-choice, where four candidates are provided. The questions and all possible choices are annotated by experienced annotators. To facilitate the question collection, we leverage the step annotations to automatically generate certain types of questions, such as “What did EV1 do while EV2 did [task]”. Overall, we collected 376 questions for testing a pre-trained VLM’s zero-shot performance.

### 3.4. Characteristics of Spacewalk-18

Our benchmark requires semantic understanding and temporal reasoning abilities in a truly unique domain. A comparable benchmark is the recently released Perception Test [39], which is also designed to measure similar skills and generalization capabilities. Spacewalk-18 nicely complements benchmarks like Perception Test as they focus on different domains (daily life vs. space) and has comparable total video durations (74 vs. 96 hours). Long-form-ness: Spacewalk-18 not only contains long videos, but also requires high amounts of context (longform video understanding) in order to annotate. We use the temporal certificate [34] metric to quantify the longform-ness of our video dataset. The temporal certificate measures the amount of video context required for a given dataset/task. Human verifiers are provided with an annotated video clip and are asked to select the amount of the

Benchmarks Duration (h) # Annotations Domain

Step recog. (task 1) Temporal Segments

Ikea-FA [56] 4 2 k Assembling Breakfast [26] 77 8 k Cooking YouCook2 [75] 176 14 k Cooking EPIC-KITCHENS [11] 100 89 k Cooking EgoPER [28] 28 8 k Cooking Assembly101 [46] 513 105 k Assembling EPIC-Tent [24] 5 1 k Assembling Spacewalk-18 (ours) 96 4 k Spacewalk

Question ans. (task 2) Questions

EgoSchema [34] 250 5 k Open-domain Video-MME-L [16] 206 900 Open-domain P. Test 1hr-walk [21] 10 70 Tours Spacewalk-18 (ours) 96 376 Spacewalk

**Table 1. Statistics comparison with single-domain step recognition benchmarks and recent long-form video QA benchmarks.**

video they require to be confident that the provided label is correct. EgoSchema [34] defines benchmarks with certificate length around 1 second as short video tasks, 10 seconds as long-form, and 100 seconds as very long-form. To collect this metric, we split each clip into 5-second chunks and task human workers with selecting the minimum subset of these clips that they need to be confident in the given label. In Figure 3, we benchmark our dataset with eight annotators over 2.5 hours of spacewalk video. Our dataset has an average clip length of 89 seconds and a temporal certificate length of 140 seconds. This is 1.4x the length of the nearest dataset and places it in the category of “very long-form video datasets” [34]. Multi-modality: We also observe that the temporal understanding and reasoning tasks in Spacewalk-18 are inherently multimodal. Table 3 shows the human performance when asked to solve the step recognition task, where they can freely explore the temporal context from the entire spacewalk video when needed. We can see that humans perform the best when both video and audio (speech transcipts) are available, with an accuracy of 67.0%. This multimodal performance is higher than when humans only have access to video (52.2%) or audio (39.1%). Comparison with Relevant Datasets: Table 1 compares Spacewalk-18 with other single-domain step recognition datasets. Our dataset has comparable total duration as relevant datasets, while focusing on a unique domain than cooking or assembling furniture. We also compare with recent long-form video QA benchmarks, which shows that our question size is reasonable as a single-domain benchmark.

## 4. Recognition and Reasoning Models

For step recognition, we benchmark both contrastive videolanguage models (contrastive VLMs) and video large language models (VLLMs). Contrastive VLMs can provide aligned video and text embeddings while VLLMs can process video and language simultaneously and perform video question answering. Both of them are evaluated in zero-

<!-- Page 6 -->

shot scenario to demonstrate how pre-trained models generalize to Spacewalk-18. We also evaluate contrastive VLMs in fine-tuning scenario, which can effectively adapt pretrained models to novel domains [44]. Both last-layer and all-layer fine-tuning are considered here. In Spacewalk-18, the animation of the i-th step is described by an animation video V a i , a transcript of its narration T a i , and a step caption Ca i . A spacewalk recording clip centered at timestamp t with length w contains a video clip Vt,w and a transcript Tt,w. When evaluating contrastive VLMs, we format recognition as retrieval, similar to how contrastive VLMs can be used for zero-shot classification. We employ separate video encoder Fv(·) and text encoder Ft(·) to derive clip features, and match the spacewalk recording clips with step animations. To evaluate VLLMs, we feed the videos and transcripts into the models and treat our tasks as multi-choice video question answering. Unless otherwise mentioned, all the models use Sparse Frame Sampling to process videos, where we uniformly sample k frames from the entire clip Vt,w regardless of the video length and feed them into the video encoder. k is the number of frames used during model pre-training, which may differ between different models. For the question answering task, a model is given the entire hour-long input video, and it needs to search for the relevant evidence from the long-form input, making the task more challenging.

### 4.1. Step Recognition and QA by VLLMs

Zero-shot. We format the step recognition task as multichoice video question answering as the following. Given a spacewalk clip with K steps in the mission, we provide the video Vt,w and transcript Tt,w to the model and ask “Which step does the frame in the middle of this video belong to?” We also provide the caption Ca i and transcript T a i of each step and require the model to choose a step index between 0 and K. See our prompt in Appendix C.2. We follow the same strategy for the question answering task, with the only difference that the choice candidates are provided by each question. We focus on the zero-shot setup and report performance not only on state-of-the-art VLLMs, but also proprietary APIs, such as GPT-4o.

### 4.2. Step Recognition by Contrastive VLMs

Zero-shot. To derive a feature for a step animation, we first extract the video, transcript, and caption features respectively, and then concatenate them. Formally, the feature of the i-th step is f a i = [Fv(V a i ), Ft(T a i ), Ft(Ca i )]. To construct a feature for the “Irrelevant” category, we write a textual description of it and extract its text feature. The description is DES=“The mission control center, noisy shots (e.g. blue screen), or tasks not planned for the spacewalk.” Hence the “Irrelevant” category feature is f a 0 = [Ft(DES), Ft(DES), Ft(DES)]. For a record-

ing clip, we concatenate the video and transcript features: f s = [Fv(Vt,w), Ft(Tt,w), Ft(Tt,w)]. Here, the transcript feature is repeated so that the feature dimensionality is the same as those of the animation features. To form a prediction, we compute the similarities f s ·f a i between a recording clip and an animation step, and pick the step with the highest similarity. Last-layer Fine-tuning. We fine-tune a linear layer upon the pre-trained models using the training set. Specifically, we freeze the models and train a linear layer Gθ(·) mapping the spacewalk clip features to the animation features. We minimize the cross entropy loss during training, which is

L = −log exp   Gθ(f s) · f a y 

1≤i≤K exp (Gθ(f s) · f a i ). (1)

P

After training, instead of constructing or learning a feature for the “Irrelevant” category, we find a threshold τ and recognize all clips whose similarities to task steps are all below τ as “Irrelevant”. This is more reasonable than an “Irrelevant” feature because this category contains a subspace formed by various concepts rather than a single concept. With the threshold τ, we make predictions by

( arg max1≤i≤K Gθ(f s) · f a i , max Gθ(f s) · f a i ≥τ, 0, otherwise. (2) We find the τ with the best F1 score on the the validation set and use it in the test phase. In practice, this method performs better than constructing an “Irrelevant” feature through text descriptions. All-layer Fine-tuning. We fine-tune the entire backbone of a pre-trained model to encode spacewalk video clips. We freeze the animation features and use the same cross entropy loss to fine-tune the model. The threshold τ for the “Irrelevant” category is also set using the validation set. Incorporating Longer Temporal Context. Our Sparse Frame Sampling approach naturally incorporates long temporal context with the same computational cost, but may lose important details due to sampling. We therefore explore several alternatives to incorporate temporal context: Dense Frame Sampling at 1 FPS, which are directly encoded by the video encoder to obtain a single video embedding. This approach is bounded by GPU memory since the number of frames scales linearly with the context duration. Long-term Feature Bank (LFB) [64], which first divides the long context into seconds-long segments and leverages a frozen video encoder to extract one video embedding for each segment with a frame sampling rate of 1 FPS. We explore the following LFB variants following [64]: Average pooling over the query and all of the context features to form a single embedding (LFB Avg); Average pooling the history context and future context separately, and concatenate them together with the query embedding (LFB

ˆy =

<!-- Page 7 -->

Cat); Learning to aggregate temporal context via non-local blocks [60] (LFB NL) or a two-layer Transformer encoder (LFB TF). More details can be found in Appendix C.5.

## 5. Experiments

Our experiments show that current video-language models perform significantly worse than humans on Spacewalk-18. We also demonstrate the importance of both vision and language on our tasks. Furthermore, the capability of LFB in incorporating temporal contexts is verified.

### 5.1. Evaluated Models

We evaluate contrastive VLMs and VLLMs on Spacewalk18. For contrastive VLMs, we test EgoVLP [31], VideoCLIP [67], InternVideo [61], and InternVideo2 [62]. For VLLMs, we test open-source LLaVA-Next-Video [73], VideoLLaMA2 [9], LongVU [47], Qwen2.5-VL [4], and InternVL3 [76], and proprietary GPT-4o and GPT-5. Moreover, recent works [59, 72] solve VideoQA tasks effectively by feeding generated video frame captions into LLMs. Hence, we develop a caption-enhanced LLM with LLaVA-1.5-13B [33] as the video frame captioner and GPT-4o as the LLM reasoner. See the selected checkpoints and number of sampled frames in Appendix C.1.

### 5.2. Human Performance Evaluation

On the step recognition task, we evaluate human performance with all our annotators. Each of them is assigned part of the video clips and has access to an unlimited video context. We measure the accuracy of each human performer and take their average as the human performance. Overall, they achieve an accuracy of 67%. This can be viewed as the “upperbound” performance on step recognition.

### 5.3. Main Results

**Table 2 shows the model performances on Spacewalk-18. Due to the high computational cost of all-layer fine-tuning, it is only employed on InternVideo. On the step recognition task, we report the results under a few context lengths and conduct a thorough exploration of it in Section 5.5. The best accuracy on this task is 25.75% (InternVL3-8B) for open-source models and 36.15% (GPT5) for proprietary models. Their performances are far less than the human performance of 67%. This verifies that our task is reasonable for humans but challenging to models. VLLMs outperform contrastive VLMs, and significantly when the context window is long. Among the contrastive VLMs, InternVideo performs the best. Both last-layer and all-layer fine-tuning can boost constrastive VLMs, and alllayer fine-tuning yields higher performance than last-layer fine-tuning. However, the improvements are marginal, indicating that adapting models to extremely rare domains remains challenging.**

Step Recognition QA w = 1 min w = 3 min w = 5 min

## Method

Acc. mAP IoU Acc. mAP IoU Acc. mAP IoU Acc.

Random 4.22 -1.51 4.22 -1.51 4.22 -1.51 25.00 Human∗ -67.0 --

Zero-shot

EgoVLP 7.18 8.28 1.90 7.71 9.08 1.67 7.59 9.83 1.59 -VideoCLIP 8.00 10.24 2.12 6.58 11.11 2.65 7.85 10.68 2.38 -InternVideo 9.35 10.78 2.96 9.48 11.01 2.73 9.07 11.56 2.97 -InternVideo2 6.33 9.38 1.61 5.64 9.05 1.53 7.08 9.72 1.63 -LLaVA-Next-Video-34B 10.35 -3.77 13.71 -5.07 13.82 -4.92 29.52 VideoLLaMA2-7B 9.34 -2.88 14.37 -5.45 17.32 -6.28 31.65 LongVU-7B 8.58 -3.04 10.49 -3.81 12.01 -4.41 36.44 Qwen2.5-VL-7B 13.33 -4.54 19.79 -7.62 22.99 -8.52 33.78 InternVL3-8B 16.07 -6.19 21.72 -8.83 25.75 -10.79 31.65 Caption-enhanced LLM 18.56 -9.33 26.32 -12.89 28.49 -13.16 30.37 GPT-4o 15.47 -6.36 21.65 -9.41 26.40 -11.55 32.45 GPT-5 -36.16 -18.42 46.54

Last-layer Fine-tuning

EgoVLP 6.34 8.92 2.17 9.68 10.66 3.03 10.21 10.86 3.18 -VideoCLIP 8.40 9.80 3.21 9.98 11.14 3.71 8.87 10.39 3.46 -InternVideo 10.12 12.17 4.02 11.13 12.68 4.04 10.08 12.53 4.04 -InternVideo2 9.41 10.36 3.53 8.96 10.00 2.98 8.39 9.97 2.97 -

All-layer Fine-tuning

InternVideo 13.21 11.77 4.60 13.34 12.93 4.54 12.93 13.09 4.63 -

**Table 2. Model performances on Spacewalk-18. GPT-5 performs the best among all the models on both step recognition and question answering tasks. Both last-layer and all-layer fine-tuning improve the performances of contrastive VLMs. ∗: Humans have access to unlimited context.**

Besides VLMs, we evaluate a video-only model, VideoMAE [55], on the step recognition task. Since the task is open-vocabulary, we train an MLP to project the VideoMAE embeddings to the SentenceBert [41] text embeddings of the step captions. It achieves 10.34% accuracy, 12.91% mAP, and 2.97% IoU with 5-minute context window length. On the question answering task, GPT-5 shows strong performance (46.54%). The most performant open-source model is LongVU (36.44%), which has token compression mechanism designed for long-form video understanding. In contrast, all other models outperform the random baseline marginally. For both tasks, we provide qualitative results including success and failure cases in Appendix D.6.

### 5.4. Effect of Modality

In Table 3, we ablate the input modality on the step recognition task using zero-shot video-language models. And we also measure the human performances in multimodal and unimodal cases. In most cases, human and open-source video-language models perform the best when both modalities are provided, demonstrating the inherently multimodal nature of our task. However, the proprietary VLLMs achieve the highest accuracy when only text is given, probably due to their strong language prior. Besides, we notice that humans can better use the visual inputs, while our models benefit more from the texts. We hypothesize that humans excel in extracting abstract concepts from videos and match them with other modalities, while it is difficult for videolanguage models. Moreover, while the overall performance of InternVideo is worse than VLLMs, it outperforms most

<!-- Page 8 -->

Accuracy

## Method

w = 1 min w = 5 min

V T V+T V T V+T

Human∗ -52.2 39.1 67.0 InternVideo 7.84 9.15 9.35 6.26 9.68 9.07 LLaVA-Next-Video 5.22 9.38 10.35 4.93 14.65 13.82 VideoLLaMA2 4.39 8.03 9.34 4.56 13.03 17.33 Qwen2.5-VL 6.59 13.71 13.33 6.98 22.78 22.99 Caption-enhanced LLM 5.48 18.92 18.56 5.13 31.03 28.49 GPT-4o 6.86 18.92 15.48 6.68 31.03 26.40 GPT-5 -13.00 39.18 36.16

**Table 3. Ablation about input modality on step recognition task. The models are evaluated in zero-shot scenario. V: video; T: text (captions and transcripts). *: Human has unlimited context and accesses transcripts in the form of audio.**

0.08

0.125

0.07

0.120

EgoVLP VideoCLIP InternVideo LLaVA-Next-Video VideoLLaMA2

0.115

0.06

0.110

mAP

IoU

0.05

0.105

0.04

0.100

0.095

0.03

0.090

0.02

1min 2min 3min 5min 10min 15min 20min Context length

1min 2min 3min 5min 10min 15min 20min Context length

**Figure 4. Ablation on context length. We test the models under various context lengths. Contrastive VLMs are last-layer finetuned while MLLMs are zero-shot. When the temporal context is extremely long, the models can no longer benefit from it.**

0.050

0.18

Sparse Frame Sampling Dense Frame Sampling LFB Avg LFB Cat LFB NL LFB TF

0.045

0.16

0.040

mAP

IoU

0.14

0.035

0.12

0.030

1min 2min 3min 5min 10min 15min 20min Context length

1min 2min 3min 5min 10min 15min 20min Context length

**Figure 5. Performances of different temporal context incorporation methods built upon frozen InternVideo features. While LFB methods yield increasing mAP when the temporal context extends, one-time feed-forward models with either sparse or dense frame sampling cannot benefit from the context.**

of the models in the video-only setting, demonstrating its better visual generalization to rare visual domains.

### 5.5. Leveraging Temporal Context

As shown in Figure 4, unlike humans that have temporal certificate of 2.3 minutes, the open-source video-language models cannot benefit from very long context on both tasks. As context window length increases, their performances initially improve, but start to decline after reaching their peaks. To address this issue, we explore the temporal context incorporation approaches for contrastive VLMs introduced in Section 4.2 on the step recognition task. As Table 3 shows that contrastive VLMs have the most robust visual capability to rare domains, it is more effective to explore video context incorporation using them than VLLMs. The performance curves with respect to context lengths are shown in Figure 5. The two one-time feedforward methods – Sparse/Dense Frame Sampling – have similar performances, and they are not improved given expanded temporal context windows as well. This indicates that naively sampling more frames cannot help pre-trained

Task Method Summary Accuracy (%)

InternVideo -9.48 Step InternVideo (fine-tuned) -13.34 Recog. Caption-enhanced LLM -23.50 Caption-enhanced LLM Step Oracle 28.49

GPT-4o -32.45 QA GPT-4o Animation 55.59 GPT-4o Step Oracle 81.12

**Table 4. Adaptation via summarization significantly improves Spacewalk-18 performance on both tasks.**

video-language models to understand long-form videos. In contrast, the mAP curves of all the LFB methods show significant upward trends, verifying their ability to incorporate temporal contexts in long videos. Among them, LFB Cat, which concatenates past, present, and future video features, outperforms other methods on the accuracy and IoU metrics, demonstrating its effectiveness.

### 5.6. Adaptation via Summarization

We finally investigate how to effectively adapt a pre-trained model to solve Spacewalk-18. Our key inspiration is that for long-form, novel-domain videos, a model may adapt to and understand the videos by watching their summarizations. In the step recognition task, the full list of steps is available to the VLLMs. This list in fact serves as an oracle summarizing the overall spacewalk mission, which might contribute to their better performance than contrastive VLMs. To ablate its influence, we give the caption-enhanced LLM only one step each time and ask it to rate each step from 1 to 10. In Table 4, the step oracle indeed improves its performance from 23.50% to 28.49%, which is an even higher gain than fine-tuning the InternVideo. For the QA task, we simulate the summarization by either re-purposing the 5-minute animation video of each spacewalk, or by assuming a known list of steps that occur within the question video. These steps are represented by step captions and middle frames. In Table 4, the animation and step oracle provide significant improvements over GPT4o. While both animation and step oracle should be considered as privileged information, they highlight the direction to adapt pre-trained models via condensed knowledge.

## 6. Conclusion

We introduce the Spacewalk-18 benchmark to evaluate video-language models’ capability to generalize to unseen domains, and their comprehension of multimodal information and long-term temporal context. We demonstrate that while average human annotators achieve competitive performance on our benchmark, existing video-language models still struggle with domain generalization and longform video understanding. Meanwhile, we discover that a promising direction for model adaptation is to provide video summary as its context, even without model fine-tuning.

<!-- Page 9 -->

Acknowledgements: This work is supported by NASA, Samsung Advanced Institute of Technology, and a Richard B. Salomon award for Chen Sun. We thank Karttikeya Mangalam and Raiymbek Akshulakov for their kind help with EgoSchema temporal certificate evaluation, and Professor Stefanie Tellex for her inspiration on looking into Spacewalk videos. Our research was conducted using computational resources at the Center for Computation and Visualization at Brown University.

## References

[1] Yazan Abu Farha, Alexander Richard, and Juergen Gall. When will you do what?-anticipating temporal occurrences of activities. In CVPR, 2018. 2 [2] Jean-Baptiste Alayrac, Jeff Donahue, Pauline Luc, Antoine Miech, Iain Barr, Yana Hasson, Karel Lenc, Arthur Mensch, Katherine Millican, Malcolm Reynolds, et al. Flamingo: a visual language model for few-shot learning. In NeurIPS, 2022. 2 [3] Shikhar Bahl, Russell Mendonca, Lili Chen, Unnat Jain, and Deepak Pathak. Affordances from human videos as a versatile representation for robotics. In CVPR, 2023. 1 [4] Shuai Bai, Keqin Chen, Xuejing Liu, Jialin Wang, Wenbin Ge, Sibo Song, Kai Dang, Peng Wang, Shijie Wang, Jun Tang, Humen Zhong, Yuanzhi Zhu, Mingkun Yang, Zhaohai Li, Jianqiang Wan, Pengfei Wang, Wei Ding, Zheren Fu, Yiheng Xu, Jiabo Ye, Xi Zhang, Tianbao Xie, Zesen Cheng, Hang Zhang, Zhibo Yang, Haiyang Xu, and Junyang Lin. Qwen2.5-VL technical report. arXiv preprint arXiv:2502.13923, 2025. 2, 7, 10 [5] Max Bain, Arsha Nagrani, Gül Varol, and Andrew Zisserman. Frozen in time: A joint video and image encoder for end-to-end retrieval. In ICCV, 2021. 9 [6] Homanga Bharadhwaj, Abhinav Gupta, Shubham Tulsiani, and Vikash Kumar. Zero-shot robot manipulation from passive human videos. arXiv preprint arXiv:2302.02011, 2023. 1 [7] Tom Brown, Benjamin Mann, Nick Ryder, Melanie Subbiah, Jared D Kaplan, Prafulla Dhariwal, Arvind Neelakantan, Pranav Shyam, Girish Sastry, Amanda Askell, et al. Language models are few-shot learners. In NeurIPS, 2020. 2 [8] Chien-Yi Chang, De-An Huang, Danfei Xu, Ehsan Adeli, Li Fei-Fei, and Juan Carlos Niebles. Procedure planning in instructional videos. In ECCV, 2020. 2 [9] Zesen Cheng, Sicong Leng, Hang Zhang, Yifei Xin, Xin Li, Guanzheng Chen, Yongxin Zhu, Wenqi Zhang, Ziyang Luo, Deli Zhao, and Lidong Bing. Videollama 2: Advancing spatial-temporal modeling and audio understanding in videollms. arXiv preprint arXiv:2406.07476, 2024. 2, 7, 9 [10] Dima Damen, Hazel Doughty, Giovanni Maria Farinella, Sanja Fidler, Antonino Furnari, Evangelos Kazakos, Davide Moltisanti, Jonathan Munro, Toby Perrett, Will Price, and Michael Wray. Scaling egocentric vision: The EPICKITCHENS dataset. In ECCV, 2018. 1, 2 [11] Dima Damen, Hazel Doughty, Giovanni Maria Farinella, Antonino Furnari, Evangelos Kazakos, Jian Ma, Davide

Moltisanti, Jonathan Munro, Toby Perrett, Will Price, et al. Rescaling egocentric vision: Collection, pipeline and challenges for epic-kitchens-100. IJCV, 2022. 5 [12] Jacob Devlin, Ming-Wei Chang, Kenton Lee, and Kristina Toutanova. Bert: Pre-training of deep bidirectional transformers for language understanding. In NAACL, 2019. 2 [13] Guodong Ding, Fadime Sener, and Angela Yao. Temporal action segmentation: An analysis of modern techniques. IEEE TPAMI, 2023. 2 [14] Yilun Du, Mengjiao Yang, Bo Dai, Hanjun Dai, Ofir Nachum, Josh Tenenbaum, Dale Schuurmans, and Pieter Abbeel. Learning universal policies via text-guided video generation. In NeurIPS, 2023. 2 [15] Debidatta Dwibedi, Yusuf Aytar, Jonathan Tompson, Pierre Sermanet, and Andrew Zisserman. Counting out time: Class agnostic video repetition counting in the wild. In CVPR, 2020. 2, 14 [16] Chaoyou Fu, Yuhan Dai, Yongdong Luo, Lei Li, Shuhuai Ren, Renrui Zhang, Zihan Wang, Chenyu Zhou, Yunhang Shen, Mengdan Zhang, et al. Video-MME: The first-ever comprehensive evaluation benchmark of multi-modal llms in video analysis. In CVPR, 2025. 5, 14 [17] Tsu-Jui Fu, Linjie Li, Zhe Gan, Kevin Lin, William Yang Wang, Lijuan Wang, and Zicheng Liu. VIOLET: End-toend video-language transformers with masked visual-token modeling. arXiv preprint arXiv:2111.1268, 2021. 2 [18] Rohit Girdhar and Kristen Grauman. Anticipative video transformer. In ICCV, 2021. 2 [19] Kristen Grauman, Andrew Westbury, Eugene Byrne, Zachary Chavis, Antonino Furnari, Rohit Girdhar, Jackson Hamburger, Hao Jiang, Miao Liu, Xingyu Liu, et al. Ego4D: Around the world in 3,000 hours of egocentric video. In CVPR, 2022. 1, 2, 3 [20] Fabian Caba Heilbron, Victor Escorcia, Bernard Ghanem, and Juan Carlos Niebles. ActivityNet: A large-scale video benchmark for human activity understanding. In CVPR, 2015. 1, 2 [21] Joseph Heyward, João Carreira, Dima Damen, Andrew Zisserman, and Viorica P˘atr˘aucean. Perception test 2024: Challenge summary and a novel hour-long videoqa benchmark. arXiv preprint arXiv:2411.19941, 2024. 5 [22] Frank Hutter Ilya Loshchilov. Decoupled weight decay regularization. In ICLR, 2019. 22 [23] Md Mohaiminul Islam and Gedas Bertasius. Long movie clip classification with state-space video models. In ECCV, 2022. 3 [24] Youngkyoon Jang, Brian Sullivan, Casimir Ludwig, Iain D Gilchrist, Dima Damen, and Walterio Mayol-Cuevas. Epictent: An egocentric video dataset for camping tent assembly. In ICCV Workshop, 2019. 5 [25] Diederik P Kingma and Jimmy Ba. Adam: A method for stochastic optimization. arXiv preprint arXiv:1412.6980, 2014. 10 [26] H. Kuehne, A. B. Arslan, and T. Serre. The language of actions: Recovering the syntax and semantics of goal-directed human activities. In CVPR, 2014. 1, 5

<!-- Page 10 -->

[27] Sateesh Kumar, Sanjay Haresh, Awais Ahmed, Andrey Konin, M Zeeshan Zia, and Quoc-Huy Tran. Unsupervised action segmentation by joint representation learning and online clustering. In CVPR, 2022. 14 [28] Shih-Po Lee, Zijia Lu, Zekun Zhang, Minh Hoai, and Ehsan Elhamifar. Error detection in egocentric procedural task videos. In CVPR, 2024. 5 [29] Junnan Li, Dongxu Li, Silvio Savarese, and Steven Hoi. BLIP-2: bootstrapping language-image pre-training with frozen image encoders and large language models. In ICML, 2023. 2 [30] Yanghao Li, Chao-Yuan Wu, Haoqi Fan, Karttikeya Mangalam, Bo Xiong, Jitendra Malik, and Christoph Feichtenhofer. Mvitv2: Improved multiscale vision transformers for classification and detection. In CVPR, 2022. 3 [31] Kevin Qinghong Lin, Alex Jinpeng Wang, Mattia Soldan, Michael Wray, Rui Yan, Eric Zhongcong Xu, Difei Gao, Rongcheng Tu, Wenzhe Zhao, Weijie Kong, et al. Egocentric video-language pretraining. In NeurIPS, 2022. 2, 7, 9 [32] Haotian Liu, Chunyuan Li, Qingyang Wu, and Yong Jae Lee. Visual instruction tuning. In NeurIPS, 2023. 2 [33] Haotian Liu, Chunyuan Li, Yuheng Li, and Yong Jae Lee. Improved baselines with visual instruction tuning. In CVPR, 2024. 7, 10 [34] Karttikeya Mangalam, Raiymbek Akshulakov, and Jitendra Malik. EgoSchema: A diagnostic benchmark for very longform video language understanding. In NeurIPS, 2023. 2, 3, 5, 8, 9, 14 [35] Antoine Miech, Dimitri Zhukov, Jean-Baptiste Alayrac, Makarand Tapaswi, Ivan Laptev, and Josef Sivic. Howto100m: Learning a text-video embedding by watching hundred million narrated video clips. In ICCV, 2019. 2, 9 [36] Seungwhan Moon, Andrea Madotto, Zhaojiang Lin, Tushar Nagarajan, Matt Smith, Shashank Jain, Chun-Fu Yeh, Prakash Murugesan, Peyman Heidari, Yue Liu, et al. Anymal: An efficient and scalable any-modality augmented language model. In EMNLP, 2024. 2 [37] Tushar Nagarajan, Yanghao Li, Christoph Feichtenhofer, and Kristen Grauman. Ego-topo: Environment affordances from egocentric video. In CVPR, 2020. 2 [38] Medhini Narasimhan, Arsha Nagrani, Chen Sun, Michael Rubinstein, Trevor Darrell, Anna Rohrbach, and Cordelia Schmid. Tl; dw? summarizing instructional videos with task relevance and cross-modal saliency. In ECCV, 2022. 2 [39] Viorica P˘atr˘aucean, Lucas Smaira, Ankush Gupta, Adrià Recasens Continente, Larisa Markeeva, Dylan Banarse, Skanda Koppula, Joseph Heyward, Mateusz Malinowski, Yi Yang, et al. Perception test: A diagnostic benchmark for multimodal video models. In NeurIPS, 2023. 2, 5 [40] Alec Radford, Jong Wook Kim, Chris Hallacy, Aditya Ramesh, Gabriel Goh, Sandhini Agarwal, Girish Sastry, Amanda Askell, Pamela Mishkin, Jack Clark, Gretchen Krueger, and Ilya Sutskever. Learning transferable visual models from natural language supervision. In ICML, 2021. 2 [41] Nils Reimers and Iryna Gurevych. Sentence-bert: Sentence embeddings using siamese bert-networks. In EMNLP, 2019. 7

[42] Alexander Richard and Juergen Gall. Temporal action detection using a statistical language model. In CVPR, 2016.

2 [43] Alexander Richard, Hilde Kuehne, and Juergen Gall. Weakly supervised action learning with rnn based fine-to-coarse modeling. In CVPR, 2017. 2 [44] Elan Rosenfeld, Pradeep Ravikumar, and Andrej Risteski. Domain-adjusted regression or: Erm may already learn features sufficient for out-of-distribution generalization. arXiv preprint arXiv:2202.06856, 2022. 6 [45] Saquib Sarfraz, Naila Murray, Vivek Sharma, Ali Diba, Luc Van Gool, and Rainer Stiefelhagen. Temporally-weighted hierarchical clustering for unsupervised action segmentation. In CVPR, 2021. 22 [46] Fadime Sener, Dibyadip Chatterjee, Daniel Shelepov, Kun He, Dipika Singhania, Robert Wang, and Angela Yao. Assembly101: A large-scale multi-view video dataset for understanding procedural activities. In CVPR, 2022. 2, 5 [47] Xiaoqian Shen, Yunyang Xiong, Changsheng Zhao, Lemeng Wu, Jun Chen, Chenchen Zhu, Zechun Liu, Fanyi Xiao, Balakrishnan Varadarajan, Florian Bordes, Zhuang Liu, Hu Xu, Hyunwoo J. Kim, Bilge Soran, Raghuraman Krishnamoorthi, Mohamed Elhoseiny, and Vikas Chandra. LongVU: Spatiotemporal adaptive compression for long video-language understanding. In ICML, 2025. 2, 7, 10 [48] Sumedh A Sontakke, Jesse Zhang, Sébastien MR Arnold, Karl Pertsch, Erdem Bıyık, Dorsa Sadigh, Chelsea Finn, and Laurent Itti. Roboclip: one demonstration is enough to learn robot policies. In NeurIPS, 2023. 1 [49] Tomáš Souˇcek, Jean-Baptiste Alayrac, Antoine Miech, Ivan Laptev, and Josef Sivic. Look for the change: Learning object states and state-modifying actions from untrimmed web videos. In CVPR, 2022. 2 [50] Tomáš Souˇcek, Jean-Baptiste Alayrac, Antoine Miech, Ivan Laptev, and Josef Sivic. Multi-task learning of object states and state-modifying actions from web videos. In CVPR, 2022. 2 [51] Sebastian Stein and Stephen J. McKenna. Combining embedded accelerometers with computer vision for recognizing food preparation activities. In UBICOMP, 2013. 1 [52] Chen Sun, Austin Myers, Carl Vondrick, Kevin Murphy, and Cordelia Schmid. VideoBert: A joint model for video and language representation learning. In ICCV, 2019. 2 [53] Yansong Tang, Dajun Ding, Yongming Rao, Yu Zheng, Danyang Zhang, Lili Zhao, Jiwen Lu, and Jie Zhou. COIN: A large-scale dataset for comprehensive instructional video analysis. In CVPR, 2019. 1, 2 [54] Makarand Tapaswi, Yukun Zhu, Rainer Stiefelhagen, Antonio Torralba, Raquel Urtasun, and Sanja Fidler. MovieQA: Understanding stories in movies through question-answering. In CVPR, 2016. 3 [55] Zhan Tong, Yibing Song, Jue Wang, and Limin Wang. VideoMAE: Masked autoencoders are data-efficient learners for self-supervised video pre-training. In NeurIPS, 2022. 7, 10 [56] Sam Toyer, Anoop Cherian, Tengda Han, and Stephen Gould. Human pose forecasting via deep markov models. In DICTA, 2017. 5

<!-- Page 11 -->

[57] Carl Vondrick, Deva Ramanan, and Donald Patterson. Efficiently scaling up video annotation with crowdsourced marketplaces. In ECCV, 2010. 2 [58] Alex Jinpeng Wang, Yixiao Ge, Rui Yan, Ge Yuying, Xudong Lin, Guanyu Cai, Jianping Wu, Ying Shan, Xiaohu Qie, and Mike Zheng Shou. All in one: Exploring unified video-language pre-training. In CVPR, 2023. 2 [59] Shijie Wang, Qi Zhao, Minh Quan Do, Nakul Agarwal, Kwonjoon Lee, and Chen Sun. Vamos: Versatile action models for video understanding. In ECCV, 2024. 2, 7 [60] Xiaolong Wang, Ross Girshick, Abhinav Gupta, and Kaiming He. Non-local neural networks. In CVPR, 2018. 7, 11 [61] Yi Wang, Kunchang Li, Yizhuo Li, Yinan He, Bingkun Huang, Zhiyu Zhao, Hongjie Zhang, Jilan Xu, Yi Liu, Zun Wang, Sen Xing, Guo Chen, Junting Pan, Jiashuo Yu, Yali Wang, Limin Wang, and Yu Qiao. InternVideo: General video foundation models via generative and discriminative learning. arXiv preprint arXiv:2212.03191, 2022. 2, 7, 9, 10 [62] Yi Wang, Kunchang Li, Xinhao Li, Jiashuo Yu, Yinan He, Chenting Wang, Guo Chen, Baoqi Pei, Rongkun Zheng, Jilan Xu, Zun Wang, et al. InternVideo2: Scaling video foundation models for multimodal video understanding. In ECCV, 2024. 2, 7, 9 [63] Chao-Yuan Wu and Philipp Krahenbuhl. Towards long-form video understanding. In CVPR, 2021. 3 [64] Chao-Yuan Wu, Christoph Feichtenhofer, Haoqi Fan, Kaiming He, Philipp Krahenbuhl, and Ross Girshick. Long-term feature banks for detailed video understanding. In CVPR, 2019. 3, 6, 11 [65] Philipp Wu, Alejandro Escontrela, Danijar Hafner, Pieter Abbeel, and Ken Goldberg. Daydreamer: World models for physical robot learning. In CoRL, 2023. 2 [66] Saining Xie, Chen Sun, Jonathan Huang, Zhuowen Tu, and Kevin Murphy. Rethinking spatiotemporal feature learning: Speed-accuracy trade-offs in video classification. In ECCV, 2018. 9 [67] Hu Xu, Gargi Ghosh, Po-Yao Huang, Dmytro Okhonko, Armen Aghajanyan, Florian Metze, Luke Zettlemoyer, and Christoph Feichtenhofer. VideoCLIP: Contrastive pretraining for zero-shot video-text understanding. In EMNLP, 2021. 2, 7, 9 [68] Jun Xu, Tao Mei, Ting Yao, and Yong Rui. MSR-VTT: A large video description dataset for bridging video and language. In CVPR, 2016. 9 [69] Jiahui Yu, Zirui Wang, Vijay Vasudevan, Legg Yeung, Mojtaba Seyedhosseini, and Yonghui Wu. Coca: Contrastive captioners are image-text foundation models. TMLR, 2024. 2 [70] Rowan Zellers, Ximing Lu, Jack Hessel, Youngjae Yu, Jae Sung Park, Jize Cao, Ali Farhadi, and Yejin Choi. Merlot: Multimodal neural script knowledge models. In NeurIPS, 2021. 2 [71] Rowan Zellers, Jiasen Lu, Ximing Lu, Youngjae Yu, Yanpeng Zhao, Mohammadreza Salehi, Aditya Kusupati, Jack Hessel, Ali Farhadi, and Yejin Choi. Merlot reserve: Neural script knowledge through vision and language and sound. In CVPR, 2022. 2

[72] Ce Zhang, Taixi Lu, Md Mohaiminul Islam, Ziyang Wang, Shoubin Yu, Mohit Bansal, and Gedas Bertasius. A simple llm framework for long-range video question-answering. In EMNLP, 2024. 2, 7, 10 [73] Yuanhan Zhang, Bo Li, haotian Liu, Yong jae Lee, Liangke Gui, Di Fu, Jiashi Feng, Ziwei Liu, and Chunyuan Li. Llava-next: A strong zero-shot video understanding model. https://llava- vl.github.io/blog/202404-30-llava-next-video, 2024. 2, 7, 9 [74] Honglu Zhou, Roberto Martín-Martín, Mubbasir Kapadia, Silvio Savarese, and Juan Carlos Niebles. Procedure-aware pretraining for instructional video understanding. In CVPR, 2023. 2 [75] Luowei Zhou, Chenliang Xu, and Jason Corso. Towards automatic learning of procedures from web instructional videos. In AAAI, 2018. 5 [76] Jinguo Zhu, Weiyun Wang, Zhe Chen, Zhaoyang Liu, Shenglong Ye, Lixin Gu, Hao Tian, Yuchen Duan, Weijie Su, Jie Shao, Zhangwei Gao, Erfei Cui, Xuehui Wang, Yue Cao, Yangzhou Liu, Xingguang Wei, Hongjie Zhang, Haomin Wang, Weiye Xu, Hao Li, Jiahao Wang, Nianchen Deng, Songze Li, Yinan He, Tan Jiang, Jiapeng Luo, Yi Wang, Conghui He, Botian Shi, Xingcheng Zhang, Wenqi Shao, Junjun He, Yingtong Xiong, Wenwen Qu, Peng Sun, Penglong Jiao, Han Lv, Lijun Wu, Kaipeng Zhang, Huipeng Deng, Jiaye Ge, Kai Chen, Limin Wang, Min Dou, Lewei Lu, Xizhou Zhu, Tong Lu, Dahua Lin, Yu Qiao, Jifeng Dai, and Wenhai Wang. InternVL3: Exploring advanced training and test-time recipes for open-source multimodal models. arXiv preprint arXiv:2504.10479, 2025. 2, 7, 10 [77] Dimitri Zhukov, Jean-Baptiste Alayrac, Ramazan Gokberk Cinbis, David Fouhey, Ivan Laptev, and Josef Sivic. Crosstask weakly supervised learning from instructional videos. In CVPR, 2019. 2
