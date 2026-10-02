---
source_pdf: "Learning and Verification of Task Structure in Instructional Videos.pdf"
pages: 14
conversion: "Automatic PDF-to-Markdown text conversion; figures are represented by extracted captions/text rather than embedded images."
---

# Learning and Veriﬁcation of Task Structure in Instructional Videos

<!-- Page 1 -->

Medhini Narasimhan1,2, Licheng Yu2, Sean Bell2, Ning Zhang2, Trevor Darrell1

1UC Berkeley, 2Meta AI https://medhini.github.io/task_structure

## Abstract

Given the enormous number of instructional videos available online, learning a diverse array of multi-step task models from videos is an appealing goal. We introduce a new pre-trained video model, VideoTaskformer, focused on representing the semantics and structure of instructional videos. We pre-train VideoTaskformer using a simple and effective objective: predicting weakly supervised textual labels for steps that are randomly masked out from an instructional video (masked step modeling). Compared to prior work which learns step representations locally, our approach involves learning them globally, leveraging video of the entire surrounding task as context. From these learned representations, we can verify if an unseen video correctly executes a given task, as well as forecast which steps are likely to be taken after a given step. We introduce two new benchmarks for detecting mistakes in instructional videos, to verify if there is an anomalous step and if steps are executed in the right order. We also introduce a long-term forecasting benchmark, where the goal is to predict long-range future steps from a given step. Our method outperforms previous baselines on these tasks, and we believe the tasks will be a valuable way for the community to measure the quality of step representations. Additionally, we evaluate VideoTaskformer on 3 existing benchmarks—procedural activity recognition, step classiﬁcation, and step forecasting—and demonstrate on each that our method outperforms existing baselines and achieves new state-of-the-art performance.

## 1. Introduction

Picture this, you’re trying to build a bookshelf by watching a YouTube video with several intricate steps. You’re annoyed by the need to repeatedly hit pause on the video and you’re unsure if you have gotten all the steps right so far. Fortunately, you have an interactive assistant that can guide you through the task at your own pace, verifying each

∗Work done while an intern at Meta AI. Correspondence to medhini@berkeley.edu

“Serve with maple syrup”

“Dip bread

Prediction over

in batter”

Step Classes

T1 T2 T3 T12

“Dip bread

in batter”

VideoTaskformer

EV1

EV1 EV2 EV3 EV12

Mask

Mask

Prior work: Single clip step prediction Ours: Masked step prediction over all clips in video

**Figure 1: Prior work [13, 12] learns step representations from single short video clips, independent of the task, thus lacking knowledge of task structure. Our model, VideoTaskformer, learns step representations for masked video steps through the global context of all surrounding steps in the video, making our learned representations aware of task semantics and structure.**

step as you perform it and interrupting you if you make a mistake. A composite task such as “making a bookshelf” involves multiple ﬁne-grained activities such as “drilling holes” and “adding support blocks.” Accurately categorizing these activities requires not only recognizing the individual steps that compose the task but also understanding the task structure, which includes the temporal ordering of the steps and multiple plausible ways of executing a step (e.g., one can beat eggs with a fork or a whisk). An ideal interactive assistant has both a high-level understanding of a broad range of tasks, as well as a low-level understanding of the intricate steps in the tasks, their temporal ordering, and the multiple ways of performing them. As seen in Fig. 1, prior work [12, 13] models step representations of a single step independent of the overall task context. This might not be the best strategy, given that steps for a task are related, and the way a step is situated in an overall task may contain important information about the step. To address this, we pre-train our model with a masked modeling objective that encourages the step representations to capture the global context of the entire video. Prior work lacks a benchmark for detecting mistakes in videos, which

<!-- Page 2 -->

is a crucial component of verifying the quality of instructional video representations. We introduce a mistake detection task and dataset for verifying if the task in a video is executed correctly—i.e. if each step is executed correctly and in the right order. Our goal is to learn representations for the steps in the instructional video which capture semantics of the task being performed such that each step representation contains information about the surrounding context (other steps in the task). To this end, we train a model VideoTaskformer, using a masked step pre-training approach for learning step representations in instructional videos. We learn step representations jointly for a whole video, by feeding multiple steps to a transformer, and masking out a subset. The network learns to predict labels for the masked steps given just the visual representations of the remaining steps. The learned contextual representations improve performance on downstream tasks such as forecasting steps, classifying steps, and recognizing procedures. Our approach of modeling steps further enables a new method for mistake identiﬁcation. Recall, our original goal was to assist a user following an instructional video. We synthetically generate a mistakes dataset for evaluation using the step annotations in COIN [25]. We consider two mistake types: mistakes in the steps of a task, and mistakes in the ordering of the steps of a task. For the ﬁrst, we randomly replace the steps in a video with steps from a similar video. For the second, we re-order the steps in a task. We show that our network is capable of detecting both mistake types and outperforms prior methods on these tasks. Additionally, we evaluate representations learned by VideoTaskformer on three existing benchmarks: step classiﬁcation, step forecasting, and procedural activity recognition on the COIN dataset. Our experiments show that learning step representation through masking pre-training objectives improves the performance on the downstream tasks. We will release code, models, and the mistake detection dataset and benchmark to the community.

## 2. Related Works

Instructional Video Datasets and Tasks. Large-scale narrated instructional video datasets [6, 17, 25, 30, 31] have paved the way for learning joint video-language representations and task structure from videos. More recently, datasets such as Assembly-101 dataset [21] and Ikea ASM [3] provide videos of people assembling and disassembling toys and furniture. Assembly-101 also contains annotations for detecting mistakes in the video. Some existing benchmarks for evaluating representations learned on instructional video datasets include step localization in videos [6, 25], step classiﬁcation [6, 25, 31], procedural activity recognition [25], and step forecasting [13]. In our work, we focus on a broad range of instructional videos found in HowTo100M [17]

and evaluate the learned representations on the downstream tasks in COIN [25] dataset. We additionally introduce 3 new benchmarks for detecting mistakes in instructional videos and forecasting long-term activities. Procedure Learning from Instructional Videos. Recent works have attempted to learn procedures from instructional videos [2, 5, 13, 19, 27]. Most notably, [5] generates a sequence of actions given a start and a goal image. [2] ﬁnds temporal correspondences between key steps across multiple videos while [19] distinguishes pairs of videos performing the same sequence of actions from negative ones. [13] uses distant supervision from WikiHow to localize steps in instructional videos. Contrary to prior works, our step representations are aware of the task structure as we learn representations globally for all steps in a video jointly, as opposed to locally, as done in past works. Video Representation Learning. There has been signiﬁcant improvement in video action recognition models over the last few years [1, 9, 10, 14]. All of the above methods look at trimmed videos and focus on learning short-range atomic actions. In this work, we build a model that can learn longer and more complex actions, or steps, composed of multiple short-range actions. For example, the ﬁrst step in Fig. 1, “Make batter”, is composed of several atomic actions such as “pour ﬂour” and “whisk”. There have also been works [13, 16, 20, 23, 29] which learn representations for longer video clips containing semantically more complex actions. Our work falls into this line of work.

## 3. Learning Task Structure through Masked Modeling of Steps

Our goal is to learn task-aware step representations from a large corpus of instructional videos. To this end, we develop VideoTaskformer, a video model pre-trained using a BERT [7] style masked modeling loss. In contrast to BERT and VideoBERT [23], we perform masking at the step level, which encourages the network to learn step embeddings that encapsulate the semantics and temporal ordering of steps within the task. Our framework consists of two steps: pre-training and ﬁne-tuning. During pre-training, VideoTaskformer is trained on weakly labeled data on the pre-training task. For ﬁne-tuning, VideoTaskformer is ﬁrst initialized with the pre-trained parameters, and a subset of the parameters is ﬁne-tuned using labeled data from the downstream tasks. Each downstream task yields a separate ﬁne-tuned model. We ﬁrst provide an overview of the pre-training approach before delving into details of the individual components. Overview. Our approach for pre-training VideoTaskformer is outlined in Fig. 2. Consider an instructional video V consisting of K video clips vi, i ∈[1, . . . , K] corresponding to K steps in the video. A step vi ∈RL×H×W ×3 is a sequence of L consecutive frames depicting a step, or semantic com-

<!-- Page 3 -->

Mistake Step Index

Step label prediction

for masked steps

or

Linear Layer ( )

Wrong step

Mistake Step Detection

Step transformer ( )

Step label for

## Mask

Short-Term Step Forecasting

0: incorrect order 1: correct order

Task Label

Wrong order

Procedure Activity

Mistake Step Detection

Recognition

Step labels for

Step Label

Long-Term Step Forecasting

Step Classification

VideoTaskformer Pre-training Downstream Tasks

**Figure 2: VideoTaskformer Pre-training (Left). VideoTaskformer fVT learns step representations for the masked out video clip vi, while attending to the other clips in the video. It consists of a video encoder fvid, a step transformer ftrans, and a linear layer fhead, and is trained using weakly supervised step labels. Downstream Tasks (Right). We evaluate step representations learned from VideoTaskformer on 6 downstream tasks.**

ponent of the task. For example, for the task “Making a french toast”, examples of steps include “Whisk the batter”, and “Dip bread in batter.” We train a video model VideoTaskformer fVT to learn step representations. We mask out a few clips in the input V and feed it to fVT which learns to predict step labels for the masked-out clips. We evaluate the embeddings learned by our pre-training objective on 6 downstream tasks: step classiﬁcation, procedural activity recognition, step forecasting, mistake step detection, mistake ordering detection, and long term forecasting. Below, we provide more details on how we pre-train VideoTaskformer using a masked step modeling loss, followed by ﬁne-tuning details on the downstream tasks.

### 3.1. Pre-training VideoTaskformer with Masked Step Modeling

We extend masked language modeling techniques used in BERT and VideoBERT to learn step representations for instructional videos. While BERT and VideoBERT operate on language and visual tokens respectively, VideoTaskformer operates on clips corresponding to steps in an instructional video. By predicting weakly supervised natural language step labels for masked out clips in the input video, VideoTaskformer learns semantics and long-range temporal interactions between the steps in a task. Unlike prior works wherein step representations are learned from local short video snippets corresponding to the step, our step representations are from the entire video with all the steps as input and capture global context of the video. Masked Step Modeling. Let V = {v1, . . . , vK} denote the visual clips corresponding to K steps in video V . The

goal of our our Masked Step Modeling pre-training setup is to encourage VideoTaskformer to learn representations of clips vi that are aware of the semantics of the corresponding step and the context of the surrounding task. To this end, the task for pre-training is to predict categorical natural language step labels for the masked out steps. While we do not have ground truth step labels, we use the weak supervision procedure proposed by [13] to map each clip vi to a distribution over step labels p(yi | vi) by leveraging the noisy ASR annotations associated with each clip. The distribution p(yi | vi) is a categorical distribution over a ﬁnite set of step labels Y . More details are provided in Sec. 3.3. Let M ⊆[1, . . . , K] denote some subset of clip indices (where each index is included in M with some masking probability r, a hyperparameter). Let V\M denote a partially masked-out sequence of clips: the same sequence as V except with clips vi masked out for all i ∈M. Let fVT represent our VideoTaskformer model with parameters θ. fVT is composed of a video encoder model fvid which encodes each clip vi independently, followed by a step transformer ftrans operating over the sequence of clip representations, and ﬁnally a linear layer fhead (which includes a softmax). The input to the model is an entire video (of size K ×L×H ×W ×3) and the output is of size K ×S (where S is the output dimension of the linear layer). We pre-train fVT by inputting a masked video V\M and predicting step labels yi for each masked-out clip vi, as described below. For the downstream tasks, we extract stepaware representations using fVT by feeding an unmasked video V to the model. We then extract the intermediate outputs of ftrans (which are of size K × D, where D is the

<!-- Page 4 -->

output embedding size). To predict step labels for masked-out steps at pretraining time, we consider two training objectives: (1) step classiﬁcation, and (2) distribution matching. We describe them below in the context of Masked Step Modeling. Step classiﬁcation loss. We use the outputs of fVT to represent an S-dimensional prediction distribution over steps, where S = |Y |. We form the target distribution by placing all probability mass on the best textual step description y∗ i for each clip vi according to the weak supervision process. That is, y∗ i = argmax y∈Y p(y | vi). (1)

We calculate the cross entropy between the predicted and target distributions for each masked out clip, yielding the following expression:

−log([fVT(V\M)]j) (2)

where j is the index of y∗ i in Y , i.e., such that y∗ i = Yj. To get the ﬁnal training objective for a single masked video V\M, we sum over all indices i ∈M, and minimize with respect to θ. Distribution matching loss. For this objective, we treat the distribution of step labels p(yi | vi) from weak supervision as the target distribution for each clip vi. We then compute the KL Divergence between the prediction distribution fVT(V\M) and the target distribution p(yi | vi) as follows:

S X

j′=1 p(Yj′ | vi) log p(Yj′ | vi) [fVT(V\M)]j′ (3)

We sum over all i ∈M and minimize with respect to θ. Following [13], we use only the top-k steps in p(yi | vi) and set the probability of the remaining steps to 0. Lin et al. [13] show that the distribution matching loss results in a slight improvement over step classiﬁcation loss. For VideoTaskformer, we ﬁnd both objectives to have similar performance and step classiﬁcation outperforms distribution matching on some downstream tasks. We use fVT as a feature extractor (layer before softmax) to extract step representations for new video segments.

### 3.2. Downstream Tasks

To show that the step representations learned by VideoTaskformer capture task structure and semantics, we evaluate the representations on 6 downstream tasks—3 new tasks which we introduce (mistake step detection, mistake ordering detection, and long-term step forecasting) and 3 existing benchmarks (step classiﬁcation, procedural activity recognition, and short-term step forecasting). We describe the dataset creation details for our 3 new benchmarks in Sec. 4. Mistake Detection. A critical aspect of step representations that are successful at capturing the semantics and structure

- of a task is that, from these representations, correctness of task execution can be veriﬁed. We consider two axes of correctness: content (what steps are portrayed in the video) and ordering (how the steps are temporally ordered). We introduce 2 new benchmark tasks to test these aspects of correctness.
- Mistake step detection. The goal of this task is to identify which step in a video is incorrect. More speciﬁcally, each input consists of a video V = {v1, . . . , vK} with K steps. V is identical to some unaltered video V1 that demonstrates a correctly executed task, except that step vj (for some randomly selected j ∈[1, . . . , K]) is replaced with a random step from a different video V2. The goal of the task is to predict the index j of the incorrect step in the video.
- Mistake ordering detection. In this task, the goal is to verify if the steps in a video are in the correct temporal order. The input consists of a video V = {v1, . . . , vK} with K steps. There is a 50% probability that V is identical to some (correctly ordered) video V1 = {v1 1, . . . , v1 K}, and there is a 50% probability that the steps are randomly permuted. That is, vi = v1 πi for some random permutation π of indices [1, . . . , K]. The goal of the task is to predict whether the steps are ordered correctly or are permuted. Step Forecasting. As another way to evaluate how learned step representations capture task structure, we test the capabilities of our model in anticipating future steps given one or more clips of a video.
- Short-term forecasting. Consider a video V = {v1, . . . , vn, vn+1, . . . vK} where vi denotes a step, and V has step labels {y1, . . . , yK}, where yi ∈Y , the ﬁnite set of all step labels in the dataset. Short-term forecasting involves predicting the step label yn+1 given the previous n segments {v1, . . . , vn} [13].
- Long-term step forecasting. We introduce the challenging task of long-term step forecasting. Given a single step vi in a video V = {v1, . . . , vK} with step labels {y1, . . . , yK}, the task is to predict the step labels for the next 5 steps, i.e. {yi+1, yi+2, . . . , yi+5}. This task is particularly challenging since the network receives very little context—just a single step—and needs to leverage task information learned during training from watching multiple different ways of executing the same task. Procedural Activity Recognition. The goal of this task is to recognize the procedural activity (i.e., task label) from a long instructional video. The input to the network is all the K video clips corresponding to the steps in a video, V = {v1, . . . , vK}. The task is to predict the video task label t ∈T where T is the set of all task labels for all the videos in the dataset. Step Classiﬁcation. In this task, the goal is to predict the step label yi ∈Y given the video clip corresponding to step vi from a video V = {v1, . . . , vK}. No context other than the single clip is given. Therefore, this task requires ﬁne-

<!-- Page 5 -->

grained recognition capability, which would beneﬁt from representations that contain information about the context in which a step gets performed. For all of the above tasks, we use the step and task label annotations as supervision. We show the “zero-shot” performance of VideoTaskformer by keeping the video model fvid and the transformer layer ftrans ﬁxed and only ﬁnetuning a linear head fhead on top of the output representations. Additionally, we also show ﬁne-tuning results where we keep the base video model fvid ﬁxed and ﬁne-tune the ﬁnal transformer ftrans and the linear layer fhead on top of it. The network is ﬁne-tuned using cross-entropy loss with supervision from the step labels for all downstream tasks.

### 3.3. Implementation Details

Step labels from Weak Supervision. To train VideoTaskformer, we require step annotations, i.e., step labels with start and end timestamps in the video, for a large corpus of instructional videos. Unfortunately, this is difﬁcult to obtain manually and datasets that provide these annotations, like COIN and CrossTask, are small in size (∼10K videos). To overcome this issue, the video speech transcript can be mapped to steps in WikiHow and used as a weak form of supervision [13]. The intuition behind this is that WikiHow steps are less noisy compared to transcribed speech. The WikiHow dataset contains a diverse array of articles with step-by-step instructions for performing a range of tasks. Denote the steps across all T tasks in WikiHow as s = {s1, . . . , sN}, where sn represents the natural language title of the nth step in s, and N is the number of steps across all tasks in WikiHow. Each step sn contain a lengthy language-based description which we denote as yn. Consider a video with K sentences in the automatic speech transcript denoted as {a1, . . . , aK}. Each sentence is accompanied by a {start, end} timestamp to localize it in the video. This yields K corresponding video segments denoted as {v1, . . . , vK}. Each video segment vi is a sequence of F RGB frames having spatial resolution H × W. To obtain the step label for a segment vi, the corresponding sentence in the transcript ai is used to ﬁnd the distribution of the nearest steps in the WikiHow repository. Following [13], we approximate this distribution using a textual similarity measure sim between yn and ai:

P(yn|vi) ≈ exp (sim(ai, yn)) P n′ exp (sim(ai, yn′)). (4)

The authors of [13] found it best to search over all the steps across all tasks (i.e., all yn), rather than the set of steps for the speciﬁc task referenced in the video. The similarity function sim is formulated as a dot product between language embeddings obtained from a pre-trained language model.

Language model. To compare WikiHow steps to the transcribed speech via the sim function, we follow the same setup as in Lin et al. [13]. For a fair comparison to the baseline, we use MPNet (paraphrase-mpnet-base-v2) to extract sentence embeddings ∈R768. Video model. VideoTaskformer is a TimeSformer model with a two-layer transformer. Following [13], the TimeSformer is initialized with ViT [8] pre-trained on ImageNet21K, and is trained on subsampled clips from HowTo100M (with 8 frames sampled uniformly from each 8-second span). We include additional implementation details in the Supplemental.

## 4. Datasets and Evaluation Metrics

Pre-training. For pre-training, we use videos and transcripts from the HowTo100M (HT100M) [17] dataset and steps from the WikiHow dataset [4]. HT100M contains 136M video clips from 1.2M long narrated instructional videos, spanning 23k activities such as “gardening” and “personal care.” The WikiHow dataset contains 10,588 steps collected from 1059 WikiHow articles which are sourced from the original dataset [11]. Evaluation. All evaluation benchmarks use videos and step annotations from the COIN dataset [25]. COIN consists of 11,827 videos related to 180 different tasks and provides step labels with start and end timestamps for every video. We use a subset of 11,104 videos that were available to download. As described in Sec. 3.2, we introduce 3 new benchmark tasks in this work: mistake step detection, mistake ordering detection, and long-term step forecasting. Mistake Step Detection. For creating the mistake step detection dataset, for every video in the COIN dataset, we randomly replace one step with a step from a different video. The network predicts the index of the mistake step. We use the same train/validation/test splits as in COIN and report average accuracy of predicting the mistake step index on the test set. Mistake Ordering Detection. We synthetically create the mistake ordering detection dataset by randomly shufﬂing the ordering of the steps in a given video, for 50% of the videos and train the network to predict whether the steps are in the right order or not. While creating the dataset, we repeatedly shufﬂe the input steps until the shufﬂed “mistake” order is different from the original valid order. Additionally, we compare the shufﬂed “mistake” order across all the videos in the same task, to ensure it doesn’t match any other video’s correct ordering of steps. Despite these two pre-processing checks, there might be noise in the dataset. We report average prediction accuracy on the test split. Long-term step forecasting. Given a video clip corresponding to a single step, long-term step forecasting involves pre-

<!-- Page 6 -->

dicting the step class label for the next 5 consecutive steps. If there are fewer than 5 next steps we append NULL tokens to the sequence. We compute classiﬁcation accuracy as the number of correct predictions out of the total number of predictions, ignoring NULL steps. We again use the same splits in the COIN dataset. Additionally, we evaluate on 3 existing benchmarks: step classiﬁcation [25] - predicts the step class label from a single video clip containing one step, procedural activity recognition [25] - predicts the procedure/task label given all the steps in the input video, and short-term step forecasting [13] - predicts the class of the step in the next segment given as input the sequence of observed video segments up to that step (excluded).

## 5. Experiments

We evaluate VideoTaskformer (VideoTF) and compare it with existing baselines on 6 downstream tasks: step classiﬁcation, procedural activity recognition, step forecasting, mistake step detection, mistake ordering detection, and long term forecasting. Results are on the datasets described in Sec. 4.

### 5.1. Baselines

- We compare our method to state-of-the-art video representation learning models for action/step recognition. We ﬁne-tune existing models in a similar fashion to ours on the 6 downstream tasks. We brieﬂy describe the best performing baseline, Learning with Distant Supervision (LwDS) [13].
- TimeSformer (LwDS) [13]. In this baseline model, the TimeSformer backbone is pre-trained on HowTo100M using the Distribution Matching loss (but without any masking of steps as in our model). Next, a single-layer transformer is ﬁne-tuned on top of the pre-trained representations from the base model for each downstream task.
- TimeSformer w/ KB transfer (LwDS) [13]. For procedural activity recognition and step forecasting, the LwDS baseline is modiﬁed to include knowledge base transfer via retrieval of most relevant facts from the knowledge base to assist the downstream task. We also include results by adding the same KB transfer component to our method, referenced as w/ KB Transfer.
- Steps from clustering ASR text. As an alternative to the weak supervision from WikiHow, we introduce an unsupervised baseline that relies only on the transcribed speech (ASR text) to obtain steps. [18] introduced an approach to segment a video into steps by clustering visual features along the time axis. It divides the video into nonoverlapping segments and groups adjacent video segments together based on a similarity threshold. We adopt a similar approach but in the text space. We compute sentence

embeddings for the ASR sentences and group adjacent sentences if their similarity exceeds the average similarity of all sentences across the entire video. We include ablations with different thresholds in the Supplemental.

### 5.2. Ablations

- We evaluate our design choices by ablating different components of our model.
- Base model. We report results for different base video models for pre-training: S3D [16], SlowFast [10], TimeSformer [4] trained on HT100M, and TimeSformer trained on Kinetics. For short-term step forecasting, procedural activity recognition, and step classiﬁcation, the results are from [13].
- Loss function. For pre-training VideoTF, we test both the loss functions, Step Classiﬁcation (SC), and Distribution Matching (DM) described in Sec. 3.
- Modalities. For mistake step detection and long-term forecasting tasks, we tried replacing video features with ASR text during ﬁne-tuning. The base model is a language model for embedding sentences in the ASR text and is kept ﬁxed. The ASR text embeddings for all the segments of the video are fed as input to the downstream model, a basic single-layer transformer, which is ﬁne-tuned to each of the tasks.
- Task label. For mistake detection and long-term forecasting tasks, we include the task name, e.g. “Install a Ceiling Fan”, as input to the downstream model. We compute the sentence embedding of the task label and append it to the list of video tokens fed as input to the model. This domain knowledge provides additional context which boosts the performance on these challenging downstream tasks.
- Linear-probe vs Fine-tuning. In linear-probe evaluation, only the fhead layer is ﬁne-tuned to each downstream task and in the ﬁne-tuning setting, all the layers of the segment transformer ftrans are ﬁne-tuned.

### 5.3. Results

Quantitative Results. We compare our approach to several baselines on all downstream tasks. For all the downstream tasks, the downstream segment transformer is ﬁnetuned, except for linear-probe where we keep our pretrained model ﬁxed and only train a linear head on top of it for each downstream task. On the step classiﬁcation task in Tab. 1, VideoTF with step classiﬁcation loss outperforms LwDS [13] by 2%, indicating that step representations learned with global context also transfer well to a task that only looks at local video clips. In procedural activity recognition (Tab. 2), we see that distribution matching loss works slightly better than step classiﬁcation loss and our ﬁne-tuned model achieves 1% improvement over the best baseline. For short-term forecasting in Tab. 3, we achieve a 3% improvement over

<!-- Page 7 -->

Model Pre-training Supervision Pre-training Dataset Acc (%)

TSN (RGB+Flow) [26] Supervised: action labels Kinetics 36.5* S3D [16] Unsupervised: MIL-NCE on ASR HT100M 37.5* ClipBERT [12] Supervised: captions COCO + Visual Genome 30.8 VideoCLIP [28] Unsupervised: NCE on ASR HT100M 39.4 SlowFast [10] Supervised: action labels Kinetics 32.9 TimeSformer [4] Supervised: action labels Kinetics 48.3 LwDS: TimeSformer [4] Unsupervised: k-means on ASR HT100M 46.5 LwDS: TimeSformer Distant supervision HT100M 54.1 VideoTF (SC) Unsupervised: NN on ASR HT100M 47.0 VideoTF (DM) Distant supervision HT100M 54.8 VideoTF (SC) Distant supervision HT100M 56.5

**Table 1: Step classiﬁcation. We compare to the accuracy scores for all baselines. VideoTF (SC) pre-trained with step classiﬁcation loss on distant supervision from WikiHow achieves state-of-the-art performance on the downstream step classiﬁcation task. We report baseline results from [13]. * indicates results by ﬁne-tuning on COIN**

Downstream Model Base Model Pre-training Supervision Pre-training Dataset Acc (%)

TSN (RGB+Flow) [26] Inception [24] Supervised: action labels Kinetics 73.4* Transformer S3D [16] Unsupervised: MIL-NCE on ASR HT100M 70.2* Transformer ClipBERT [12] Supervised: captions COCO + Visual Genome 65.4 Transformer VideoCLIP [28] Unsupervised: NCE on ASR HT100M 72.5 Transformer SlowFast [10] Supervised: action labels Kinetics 71.6 Transformer TimeSformer [4] Supervised: action labels Kinetics 83.5 LwDS: Transformer TimeSformer [4] Unsupervised: k-means on ASR HT100M 85.3 LwDS: Transformer TimeSformer Distant supervision HT100M 88.9 LwDS: Transformer w/ KB Transfer TimeSformer Distant supervision HT100M 90.0 VideoTF (SC; ﬁne-tuning) w/ KB Transfer TimeSformer Unsupervised: NN on ASR HT100M 81.2 VideoTF (SC; linear-probe) w/ KB Transfer TimeSformer Distant supervision HT100M 83.1 VideoTF (DM; linear-probe) w/ KB Transfer TimeSformer Distant supervision HT100M 85.7 VideoTF (SC) w/ KB Transfer TimeSformer Distant supervision HT100M 90.5 VideoTF (DM) w/ KB Transfer TimeSformer Distant supervision HT100M 91.0

**Table 2: Accuracy of different methods on the procedural activity recognition dataset.**

Downstream Model Base Model Pre-training Supervision Pre-training Dataset Acc (%)

Transformer S3D [16] Unsupervised: MIL-NCE on ASR HT100M 28.1 Transformer SlowFast [10] Supervised: action labels Kinetics 25.6 Transformer TimeSformer [4] Supervised: action labels Kinetics 34.7 LwDS: Transformer TimeSformer [4] Unsupervised: k-means on ASR HT100M 34.0 LwDS: Transformer w/ KB Transfer TimeSformer Distant supervision HT100M 39.4 VideoTF (SC; ﬁne-tuned) w/ KB Transfer TimeSformer Unsupervised: NN on ASR HT100M 35.1 VideoTF (SC; linear-probe) w/ KB Transfer TimeSformer Distant supervision HT100M 39.2 VideoTF (DM; linear-probe) w/ KB Transfer TimeSformer Distant supervision HT100M 40.1 VideoTF (SC) w/ KB Transfer TimeSformer Distant supervision HT100M 41.5 VideoTF (DM) w/ KB Transfer TimeSformer Distant supervision HT100M 42.4

**Table 3: Accuracy of different methods on the short-term step forecasting dataset.**

Downstream Model Base Model Pre-training Supervision Pre-training Dataset Acc (%)

Transformer (ASR text) w/ Task label MPNet 39.0 Transformer SlowFast [10] Supervised: action labels Kinetics 15.2 Transformer TimeSformer [4] Supervised: action labels HT100M 17.0 Transformer w/ Task label TimeSformer [4] Supervised: action labels HT100M 40.1 LwDS: Transformer w/ Task label TimeSformer Distant supervision HT100M 41.3 VideoTF (DM) TimeSformer Distant supervision HT100M 40.2 VideoTF (DM) w/ Task label TimeSformer Distant supervision HT100M 46.4

**Table 4: Accuracy of different methods on the long-term step forecasting dataset.**

<!-- Page 8 -->

Downstream Model Base Model Pre-training Supervision Pre-training Dataset Mistake Detection Step Order

Transformer (ASR text) w/ Task label MPNet [22] 34.2 33.4 Transformer w/ Task Label SlowFast [10] Supervised: action labels Kinetics 28.6 26.1 Transformer w/ Task label TimeSformer [4] Supervised: action labels HT100M 36.0 34.7 LwDS: Transformer TimeSformer Distant supervision HT100M 17.1 11.2 LwDS: Transformer w/ Task Label TimeSformer Distant supervision HT100M 37.6 31.8 VideoTF (SC) TimeSformer Distant supervision HT100M 20.1 15.4 VideoTF (DM) w/ Task label TimeSformer Distant supervision HT100M 40.8 34.0 VideoTF (SC; ﬁne-tuned) w/ Task label TimeSformer Distant supervision HT100M 41.7 35.4

**Table 5: Accuracy of different methods on the mistake step detection test dataset.**

## 1 2 3 4

Mistake in

step 2

Take out rice Put rice in the cooker Cook the rice Pour soy beans

(A) Mistake Step Detection (Task: Cooking rice)

Wind legs with bandage Wind legs with bandage

Cover injury with bandage

Cut the bandage

(B) Mistake Ordering Detection (Task: Bandage Dog Paw)

clean up the interior of the pumpkin, draw outline, carve outline, remove pumpkin, draw outline

Remove pumpkin pedicle

(C) Long term forecasting (Task: Carve pumpkin)

Incorrect

Add ice

order

Add fruits in

blender Add milk

(D) Short term forecasting (Task: Make smoothie)

**Figure 3: Qualitative results. We show qualitative results of our method on 4 tasks. The step labels are not used during training and are only shown here for illustrative purposes.**

LwDS Ours (VideoTaskformer)

cap Remove air

Install valve

nozzle

nozzle Pump tire Screw off the valve cap and open valve

Install air

Short-Term Forecasting (Task: PumpBiCycle with Tire)

**Figure 4: Qualitative comparison. We compare results from our method VideoTF to the baseline LwDS on the short-term forecasting task. Step labels are not passed to the model as input and are only for reference.**

LwDS and our unsupervised pre-training using NN with ASR outperforms previous unsupervised methods. We also note that linear-probe performance is competitive in Tab. 2 and outperforms baselines in Tab. 3. VideoTF with achieves a strong improvement of 5% over LwDS on the long-term forecasting task, 4% on mistake step detection, and 4% on mistake ordering detection. Adding task labels improves performance on all three tasks. Additionally, we evaluate our approach on the activity recognition task in EPIC Kitchens-100 and include results in the Supplemental. We also report our models performance on the step localization task in COIN.

Qualitative Results. Fig. 3 shows qualitative results of our model VideoTF on the mistake detection tasks. Fig. 3 (A) shows a result on mistake step detection, where our model’s input is the sequence of video clips on the left and it correctly predicts the index of the mistake step “2” as the output. In (B), the order of the ﬁrst two steps is swapped and our model classiﬁes the sequence as incorrectly ordered. In (C), for the long-term forecasting task, the next 5 steps predicted by our model match the ground truth and in (D), for the short-term forecasting task, the model predicts the next step correctly given the past 2 steps. In Fig. 4 we show an example result of our method compared to the baseline LwDS on the short-term forecasting task. Our method correctly predicts the next step as “remove air nozzle” since it has acquired knowledge of task structure whereas the baseline predicts the next step incorrectly as “install valve cap.”

## 6. Conclusion

In this work, we introduce a new video model, VideoTaskformer, for learning contextualized step representations through masked modeling of steps in instructional videos. We also introduce 3 new benchmarks: mistake step detection, mistake order detection, and long term forecasting. We demonstrate that VideoTaskformer improves performance on 6 downstream tasks, with particularly strong improve-

<!-- Page 9 -->

ments in detecting mistakes in videos and long-term forecasting. Our method opens the possibility of learning to execute a variety of tasks by watching instructional videos; imagine learning to cook a complicated meal by watching a cooking show. Acknowledgements. We would like to thank Suvir Mirchandani for his help with experiments and paper writing. This work was supported in part by DoD including DARPA’s LwLL, PTG and/or SemaFor programs, as well as BAIR’s industrial alliance programs.

## References

[1] Anurag Arnab, Mostafa Dehghani, Georg Heigold, Chen Sun, Mario Luˇci´c, and Cordelia Schmid. Vivit: A video vision transformer. In IEEE International Conference on Computer Vision (ICCV), 2021. 2 [2] Siddhant Bansal, Chetan Arora, and CV Jawahar. My view is the best view: Procedure learning from egocentric videos. In European Conference on Computer Vision (ECCV), 2022. 2 [3] Yizhak Ben-Shabat, Xin Yu, Fatemeh Saleh, Dylan Campbell, Cristian Rodriguez-Opazo, Hongdong Li, and Stephen Gould. The ikea asm dataset: Understanding people assembling furniture through actions, objects and pose. In Winter Conference on Applications of Computer Vision (WACV), 2021. 2 [4] Gedas Bertasius, Heng Wang, and Lorenzo Torresani. Is space-time attention all you need for video understanding? In International Conference on Machine Learning (ICML), 2021. 5, 6, 7, 8, 11 [5] Chien-Yi Chang, De-An Huang, Danfei Xu, Ehsan Adeli, Li Fei-Fei, and Juan Carlos Niebles. Procedure planning in instructional videos. In European Conference on Computer Vision (ECCV), 2020. 2 [6] Dima Damen, Hazel Doughty, Giovanni Maria Farinella, Sanja Fidler, Antonino Furnari, Evangelos Kazakos, Davide Moltisanti, Jonathan Munro, Toby Perrett, Will Price, et al. Scaling egocentric vision: The EPIC-KITCHENS dataset. In European Conference on Computer Vision (ECCV), 2018. 2 [7] Jacob Devlin, Ming-Wei Chang, Kenton Lee, and Kristina Toutanova. BERT: Pre-training of deep bidirectional transformers for language understanding. In Conference of the North American Chapter of the Association for Computational Linguistics, 2019. 2 [8] Alexey Dosovitskiy, Lucas Beyer, Alexander Kolesnikov, Dirk Weissenborn, Xiaohua Zhai, Thomas Unterthiner, Mostafa Dehghani, Matthias Minderer, Georg Heigold, Sylvain Gelly, et al. An image is worth 16x16 words: Transformers for image recognition at scale. In International Conference on Learning Representations (ICLR), 2020. 5, 11 [9] Haoqi Fan, Bo Xiong, Karttikeya Mangalam, Yanghao Li, Zhicheng Yan, Jitendra Malik, and Christoph Feichtenhofer. Multiscale vision transformers. In IEEE International Conference on Computer Vision (ICCV), 2021. 2 [10] Christoph Feichtenhofer, Haoqi Fan, Jitendra Malik, and Kaiming He. SlowFast networks for video recognition. In

IEEE International Conference on Computer Vision (ICCV), 2019. 2, 6, 7, 8 [11] Mahnaz Koupaee and William Yang Wang. Wikihow: A large scale text summarization dataset. arXiv:1810.09305, 2018. 5 [12] Jie Lei, Linjie Li, Luowei Zhou, Zhe Gan, Tamara L Berg, Mohit Bansal, and Jingjing Liu. Less is more: ClipBERT for video-and-language learning via sparse sampling. In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2021. 1, 7 [13] Xudong Lin, Fabio Petroni, Gedas Bertasius, Marcus Rohrbach, Shih-Fu Chang, and Lorenzo Torresani. Learning to recognize procedural activities with distant supervision. In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2022. 1, 2, 3, 4, 5, 6, 7, 11 [14] Ze Liu, Jia Ning, Yue Cao, Yixuan Wei, Zheng Zhang, Stephen Lin, and Han Hu. Video swin transformer. In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2022. 2 [15] Ilya Loshchilov and Frank Hutter. Decoupled weight decay regularization. In International Conference on Learning Representations (ICLR), 2018. 11 [16] Antoine Miech, Jean-Baptiste Alayrac, Lucas Smaira, Ivan Laptev, Josef Sivic, and Andrew Zisserman. End-to-end learning of visual representations from uncurated instructional videos. In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2020. 2, 6, 7 [17] Antoine Miech, Dimitri Zhukov, Jean-Baptiste Alayrac, Makarand Tapaswi, Ivan Laptev, and Josef Sivic. HowTo100M: Learning a text-video embedding by watching hundred million narrated video clips. In IEEE International Conference on Computer Vision (ICCV), 2019. 2, 5 [18] Medhini Narasimhan, Arsha Nagrani, Chen Sun, Michael Rubinstein, Trevor Darrell, Anna Rohrbach, and Cordelia Schmid. TL; DW? summarizing instructional videos with task relevance and cross-modal saliency. In European Conference on Computer Vision (ECCV), 2022. 6 [19] Yicheng Qian, Weixin Luo, Dongze Lian, Xu Tang, Peilin Zhao, and Shenghua Gao. SVIP: Sequence veriﬁcation for procedures in videos. In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2022. 2 [20] Zhaofan Qiu, Ting Yao, and Tao Mei. Learning spatiotemporal representation with pseudo-3d residual networks. In IEEE International Conference on Computer Vision (ICCV), 2017. 2 [21] Fadime Sener, Dibyadip Chatterjee, Daniel Shelepov, Kun He, Dipika Singhania, Robert Wang, and Angela Yao. Assembly101: A large-scale multi-view video dataset for understanding procedural activities. In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2022. 2 [22] Kaitao Song, Xu Tan, Tao Qin, Jianfeng Lu, and Tie-Yan Liu. MPNet: Masked and permuted pre-training for language understanding. Advances in Neural Information Processing Systems (NeurIPS), 2020. 8 [23] Chen Sun, Austin Myers, Carl Vondrick, Kevin Murphy, and Cordelia Schmid. VideoBERT: A joint model for video and language representation learning. In IEEE International Conference on Computer Vision (ICCV), 2019. 2

<!-- Page 10 -->

[24] Christian Szegedy, Vincent Vanhoucke, Sergey Ioffe, Jon Shlens, and Zbigniew Wojna. Rethinking the inception architecture for computer vision. In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2016. 7 [25] Yansong Tang, Dajun Ding, Yongming Rao, Yu Zheng, Danyang Zhang, Lili Zhao, Jiwen Lu, and Jie Zhou. COIN: A large-scale dataset for comprehensive instructional video analysis. In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2019. 2, 5, 6 [26] Yansong Tang, Jiwen Lu, and Jie Zhou. Comprehensive instructional video analysis: The COIN dataset and performance evaluation. IEEE transactions on pattern analysis and machine intelligence, 2020. 7 [27] Qingyun Wang, Manling Li, Hou Pong Chan, Lifu Huang, Julia Hockenmaier, Girish Chowdhary, and Heng Ji. Multimedia generative script learning for task planning. arXiv:2208.12306, 2022. 2 [28] Hu Xu, Gargi Ghosh, Po-Yao Huang, Dmytro Okhonko, Armen Aghajanyan, Florian Metze, Luke Zettlemoyer, and Christoph Feichtenhofer. VideoCLIP: Contrastive pretraining for zero-shot video-text understanding. In Conference on Empirical Methods in Natural Language Processing, 2021. 7 [29] Zhongwen Xu, Yi Yang, and Alex G Hauptmann. A discriminative CNN video representation for event detection. In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2015. 2 [30] Luowei Zhou, Chenliang Xu, and Jason J Corso. Towards automatic learning of procedures from web instructional videos. In The Association for the Advancement of Artiﬁcial Intelligence Conference (AAAI), 2018. 2 [31] Dimitri Zhukov, Jean-Baptiste Alayrac, Ramazan Gokberk Cinbis, David Fouhey, Ivan Laptev, and Josef Sivic. Crosstask weakly supervised learning from instructional videos. In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2019. 2

<!-- Page 11 -->

Supplementary Materials

In this section, we describe additional implementation details of our method and provide more qualitative results and comparisons on all the 6 downstream tasks.

S1. Implementation Details

Pre-training. The base video model is a Timesformer [4] model with a ViT backbone initialized with ImageNet-21K ViT pretraining [8]. We pre-train our model on 64 A100 GPUs for 20 epochs which takes 120 hours for all the videos in the HowTo100M dataset. We use a batch size of 64 videos (1 video per GPU), each consisting of 12 segments. To train the model, we use SGD optimizer with momentum and weight decay. The learning rate is set to 0.01 and is decayed using a step learning rate policy of 10% decay at steps 15 and 19. We perform a second round of pretraining for 15 epochs using AdamW [15] with a learning rate of 0.00005. We use a 15% masking ratio during pre-training. Segment transformer ftrans is a two layer transformer with 12 video segments as input. Each segment consists of 8 embedding vectors extracted from a series of 8 adjacent 8-second clips from the input video (spanning a total of 64 seconds). It has a 768 embedding dimnesion and 12 heads, along with learnable positional encodings at the beginning. The WikiHow knowledgebase has 10588 step classes all of which are used for training the network with step classiﬁcation loss. For obtaining the distant supervision from WikiHow and mapping ASR text to step labels in the WikiHow knowledge base, we follow the setup described in [13].

Fine-tuning. For mistake step detection, mistake ordering detection, long term and short term step forecasting, and procedural activity recognition the input consists of 12 segments from the video. We ﬁne-tune only the segment transformer ftrans and the linear head fhead using cross entropy loss, while the keeping the base TimeSformer video model fvid as a ﬁxed feature extractor. We use a learning rate of 0.005 with a step decay of 10% and train the network for 50 epochs using sgd optimizer. For the step classiﬁcation task, we only ﬁne-tune the linear head, while keeping both the base video model and the 2 layer segment transformer ﬁxed. We use a learning rate of 0.005 with a step decay of 10% and train the network for 50 epochs using sgd optimizer.

S2. Additional Quantitative Results

Activity Recognition. In Tab. T2, we include results for activity recognition on EPIC-KITCHENS-100 by ﬁne-tuning our pre-trained model for noun, verb, and action recognition tasks. We outperform all baselines on noun recognition, and are on par with MoViNet (Kondratyuk et al., CVPR 2021)

on action recognition.

Model Action (%) Verb (%) Noun (%)

MoViNet 47.7 72.2 57.3 LwDS: TimeSformer 44.4 67.1 58.1 VideoTaskformer (SC) 47.6 70.4 59.8

Table T1: Activity Recognition on EPIC-KITCHENS-100.

Evaluating on step localization: We evaluate our pretrained embeddings on the action segmentation task in COIN. Following previous work, we train a linear head on top of our ﬁxed features and predict action labels for nonoverlapping 1-second input video segments. LwDS attains 67.6% on this task, and our method achieves 69.1%. Step labels as input: Our method uses visual features since step labels are not always available during inference. Nevertheless, for the purpose of comparison, we assume we have access to ground-truth step labels during inference and include results for all tasks. The results shown in Tab. T2 are from training a single layer transformer on the COIN train set and evaluating on the test set, i.e. there is no pre-training. As expected, using step labels makes the task much simpler and it outperforms using visual features. However, adding task label information to visual features improves performance signiﬁcantly for all the tasks.

Task Step Labels(%) Visual Features (%) -w/ Task label -w/ Task label

Short term forecasting 65 68 20 49 Long term forecasting 50 53 14 40 Mistake Ordering 80 82 60 65 Mistake Step 64 68 28 33

Table T2: Step labels vs Visual features.

S3. Additional Qualitative Results

Step Classiﬁcation. We compare results from our method VideoTaskformer, to the baseline LwDS [13] in Fig. F1. Since our model was trained on the entire video by masking out segments, it has a better understanding of the relationship between different steps in the same task, i.e. learned representations are “context-aware”. As a result, it is better at distinguishing the steps within a task and correctly classiﬁes all the steps in the four examples shown here. LwDS on the other hand incorrectly classiﬁes all of the steps. For reference, we show a keyframe from the correct video step clip corresponding to the incorrect step class chosen by LwDS. The input image clips and the correct clips for the LwDS predictions are closely related and contain similar objects of interest, they correspond to different stages of the task and contain different actions. Since our model learns step representations “globally” from the whole video, it is able to capture these subtle differences.

<!-- Page 12 -->

Correct Clip for LwDS prediction Input

Take out toner

cartridge

Shake the

mixture

Apply detergent

Pour noodles in

water and stir

Close door of

printer

Pour in after

mixing

Clean the

floor

Pour cooked

noodles

Figure F1: Step classiﬁcation. We qualitatively compare results from our method (VideoTaskeformer) to the baseline LwDS on the step classiﬁcation task. While the inputs are video clips, we only show a keyframe from the clip for visualization purposes. Correct predictions (VideoTaskformer) are shown in green and incorrect predictions (LwDS) are in red. We also show a frame from the clip corresponding to the incorrect prediction made by LwDS.

Input LwDS Ground Truth VideoTaskformer

Apply lubricant Wipe off excessive lubricant Insert key repeatedly

Task: Lubricate A Lock

Fix the new string on the lower part of the guitar Fix the new string on the head of the guitar Adjust tightness of the string

Task: Change Guitar Strings

(ours)

Incorrect

Correct

Incorrect

order

order

order

Incorrect

Correct

Incorrect

order

order

order

Figure F2: Mistake Order Detection. Qualitative comparison of results from VideoTaskformer to LwDS. Step and task labels shown along with the input are for visualization purpose only. Correct answers are shown in green and incorrect answers in red.

LwDS VideoTaskformer

(Ours)

<!-- Page 13 -->

Input

In hole Step labels (for visualization only)

Check type of

back cover

Replace battery Install back cover Insert paper clip

Step Indices 0 1 2 3

Ground Truth

1

LwDS

3

Open the back cover

VideoTaskformer

1

Correct step for visualization

(ours)

Figure F3: Mistake Step Detection. Qualitative comparison of results from VideoTaskformer to LwDS. Step and task labels shown along with the input are for visualization purpose only. Correct answers are shown in green and incorrect answers in red.

Input LwDS Ground Truth VideoTaskformer

Procedural Activity

(ours)

Paste car sticker Remove scratches

from windshield Clean window

surface

of sticker

Task label: Paste car sticker

Short-Term Step Forecasting

1. Insert paper clip into lock

2. Twist paper clip by hand

Task label: Open lock with paper clips

Long-Term Step Forecasting

1. Unscrew the screws used to fix the screen

Task label: Replace laptop screen

side of sticker

into lock 3. Install the new

3. Insert paper clip

3. Insert paper clip

doorknob

into lock

2. Pull out screen

2. Pull out screen

connector, 3. Remove the screen,

connector, 3. Remove the screen,

2. Unscrew the

screws, 3. Reset and screw on screw

- 4. Install new screen,
- 5. Reset and screw on

- 4. Install new screen,
- 5. Reset and screw on

screw

screw

Figure F4: Qualitative results for procedural activity recognition, short term step forecasting, and long term step forecasting. Step and task labels shown along with the input are for visualization purpose only. Correct answers are shown in green and incorrect answers in red.

Mistake Ordering Detection. Fig. F2 compares results of our method VideoTaskformer to the baseline LwDS on the mistake ordering detection task. We show two examples, “lubricate a lock” and “change guitar string”, where the steps in the input are swapped as shown by red arrows. Our method correctly detects that the input steps are in the incorrect order whereas the baseline predicts the ordering to be correct. As seen, detecting the order requires a high level understanding of the task structure, which our model learns through masking.

Mistake Step Detection. Qualitative comparison on the mistake step detection task is shown in Fig. F3. The input consists of video clip steps for the task “change battery of watch”. The second step is swapped with an incorrect step from a different task. Our method correctly identiﬁes the index of the mistake step 1, whereas the baseline predicts 3 which is incorrect. We show the correct step for visualization purposes.

Procedural Activity Recognition. A result is shown in

Task

Paste car sticker

Recognition

Put on sticker

Press sticker

Take off front

Tear off other

<!-- Page 14 -->

Fig. F4. VideoTaskformer’s representations are contextaware and can identify the right task given the sequence of clips, “paste car sticker”. The baseline misidentiﬁes the task as an incorrect similar task, “remove scratches from windshield”.

Short-term Step Forecasting. Fig. F4 shows an input consisting of two clips corresponding to the ﬁrst two steps for the task “open lock with paper clips”. The clips are far apart temporally, so the model needs to understand broader context of the task to predict what the next step is. Our method VideoTaskformer correctly identiﬁes the next step as “insert paper clip into lock” whereas the baseline incorrectly predicts a step “install the new doorknob” from another task.

Long-term Step Forecasting. In Fig. F4 we compare the future steps predicted by our model and the baseline LwDS on the long-term step forecasting task. Both models only receive a single clip as input, corresponding to the ﬁrst step “unscrew the screws used to ﬁx the screen” of the task “replace laptop screen”. Our model predicts all the next 4 ground-truth steps correctly, and in the right order. The baseline on the other hand predicts steps from the same task but in the incorrect order. All of the above qualitative results further support the effectiveness of learning step representations through masking, and show that our learned step representations are “context-aware” and possess “global” knowledge of taskstructure.
