---
source_pdf: "Are you Struggling Dataset and Baselines for Struggle Determination in Assembly Videos.pdf"
pages: 38
conversion: "Automatic PDF-to-Markdown text conversion; figures are represented by extracted captions/text rather than embedded images."
---

# Are you Struggling? Dataset and Baselines for Struggle Determination in Assembly Videos

<!-- Page 1 -->

International Journal of Computer Vision (2025) 133:7817–7854 https://doi.org/10.1007/s11263-025-02559-4

Shijia Feng1 · Michael Wray1 · Brian Sullivan2 · Youngkyoon Jang5 · Casimir Ludwig3 · Iain Gilchrist3 · Walterio Mayol-Cuevas1,4

Received: 16 February 2024 / Accepted: 30 July 2025 / Published online: 11 August 2025 © The Author(s) 2025

## Abstract

Determining when people are struggling allows for a ﬁner-grained understanding of actions that complements conventional action classiﬁcation and error detection. Struggle detection, as deﬁned in this paper, is a distinct and important task that can be identiﬁed without explicit step or activity knowledge. We introduce the ﬁrst struggle dataset with three real-world problem-solving activities that are labelled by both expert and crowd-source annotators. Video segments were scored w.r.t. their level of struggle using a forced choice 4-point scale. This dataset contains 5.1 hours of video from 73 participants. We conducted a series of experiments to identify the most suitable modelling approaches for struggle determination. Additionally, we compared various deep learning models, establishing baseline results for struggle classiﬁcation, struggle regression, and struggle label distribution learning. Our results indicate that struggle detection in video can achieve up to 88.24% accuracy in binary classiﬁcation, while detecting the level of struggle in a four-way classiﬁcation setting performs lower, with an overall accuracy of 52.45%. Our work is motivated toward a more comprehensive understanding of action in video and potentially the improvement of assistive systems that analyse struggle and can better support users during manual activities.

## Keywords

Struggle Detection · Struggle Dataset · Egocentric Problem-Solving Videos · Deep Learning Models

Communicated by GIOVANNI FARINELLA.

B Walterio Mayol-Cuevas walterio.mayol-cuevas@bristol.ac.uk

Shijia Feng shijia.feng.2019@bristol.ac.uk

Michael Wray michael.wray@bristol.ac.uk

Brian Sullivan brian.sullivan@bristol.ac.uk

Youngkyoon Jang youngkyoonjang@gmail.com

Casimir Ludwig c.ludwig@bristol.ac.uk

Iain Gilchrist i.d.gilchrist@bristol.ac.uk

1 School of Computer Science, University of Bristol, Bristol, UK

2 Bristol Medical School (PHS), University of Bristol, Bristol, UK

## 1 Introduction

The ability to identify when someone is struggling is an important cognitive competence and a key component of skill acquisition (Newell, 1991). Observers can identify instances of struggle even when lacking speciﬁc task knowledge (e.g. a non-rock climber can determine when expert rock climbers are struggling). The ability to detect struggle and subsequently offer help has also been demonstrated in pre-verbal children and even other primates (Warneken & Tomasello, 2006a). These ﬁndings suggest that struggle determination is a rather important and fundamental ability of visual action understanding which we deﬁne as a task within this paper— see examples within Figure 1. However, most research to date in skill assessment has focused on error detection (Ghoddoosian, Dwivedi, Agarwal, and Dariush, 2023; Wang et al., 2023; Sener et al., 2022; Grauman et al., 2024; Flaborea et

3 School of Psychological Science, University of Bristol, Bristol, UK

4 Amazon, Seattle, WA, United States

5 Work done while at the, University of Bristol, Bristol, UK

<!-- Page 2 -->

7818 International Journal of Computer Vision (2025) 133:7817–7854

**Fig. 1 Struggle and non-struggle examples. upper box Three nonstruggle examples from Pipes-Struggle, Tent-Struggle, and TowerStruggle. Three struggle examples from the same activities are shown in the bottom box. Frames are displayed in chronological order from left to right. Non-struggle examples feature ﬂuid motion without errors,**

al., 2024), ranking of skill (Doughty, Damen, and MayolCuevas, 2018; Doughty, Mayol-Cuevas, and Damen, 2019) or task scoring (Parmar & Morris, 2016; Pirsiavash, Vondrick,andTorralba,2014).Struggle,however,isdistinctfrom error detection and these other tasks. It is possible to complete activities without step mistakes while struggling or to conﬁdently complete an activity full of step mistakes without showing signs of struggle. Figure 2 presents examples from our dataset illustrating struggle-error relations. Struggle and error detection, though related, are distinct markers of action assessment. Importantly, the struggle is detectable withoutpriortaskknowledge,canbeusedtoanticipateerrors, can serve as an online scoring predictor before tasks are completed, and can assist an assistive system in identifying common steps that are difﬁcult to perform. Therefore, we argue that visual understanding systems should incorporate both error and struggle detection. Struggle, a subjective behavioural marker, is challenging to deﬁne precisely. Nevertheless, it is easily detectable by non-experts, allowing for the construction of relevant datasets. In this work, we deﬁne struggle through the observations of annotators, where we

whereas videos showcasing struggling show issues (highlighted in orange). For example: repeatedly placing the item back (top row struggle), assembling errors (middle row struggle), and hesitation towards the next step (bottom row struggle) (Color ﬁgure online)

observe that instances of struggle are identiﬁed during periods of motor hesitation (e.g. stopping or showing placement indecision), getting stuck or not knowing how to proceed, actions takinglonger thanexpected; havingnon-smoothhand or body motions; frequent pauses and repeated attempts; or showing signs of frustration (e.g. through hand and or head movements). Motivated by the importance of struggle determination within the larger goal of ﬁner-grained visual understanding and the potential opportunities for eye-wear computing to provide real-time support assistance, we collected a dataset for struggle determination using indoor and outdoor real-world activities, including three domains: assembling plumbing pipes; pitching a camping tent; and solving the ‘Tower of Hanoi’ puzzle. These three domains were chosen speciﬁcally to demonstrate the viability of struggle detection due to their task structures and assembly requirements. The Tower of Hanoi puzzle presented a strict rule-based framework requiring participants to solve a spatial puzzle. Pipe plumbing uses rigid objects and printed instructions for participants to follow, showcasing their ability to per-

<!-- Page 3 -->

International Journal of Computer Vision (2025) 133:7817–7854 7819

**Fig. 2 Struggle vs Errors/Mistakes. The ﬁgure showcases the differences between error detection and struggle determination using video examples from our dataset: (1) participant showing no sign of struggle, but there was an error in the completed activity; (2) participant showing**

form an unknown task with guidance. The tent assembly task introduced a free-form assembly process with deformable objects, demanding greater improvisation and manual skill. Ourchosentasksareexamplesofreal-worldproblem-solving that require structured, sequential assembly with a predeﬁned ﬁnal state. These tasks involve following speciﬁc rules and constraints to achieve a well-deﬁned outcome, such as correctly connecting components or completing a structured puzzle.Compared to other general real daily living tasks (e.g. cooking or crafts), our chosen tasks are examples of real-world problem solving and have well-deﬁned goals (completing the structures). Our tasks allow participants to demonstrate various complex visuomotor behaviours with multiple, albeit constrained, opportunities for struggle. Ourstruggle-determinationapproachisbasedontwomain assumptions. First, we believe there is sufﬁcient information for many activities to determine if a person is struggling from video, based on low-level motion cues of hand-object interaction and without requiring speciﬁc task knowledge. This is supported by behavioural evidence, e.g. Warneken and Tomasello (2006a,b) in people. Second, the same deep learning models and related training and testing strategies can be used for different tasks as they share common visual cues that help to determine struggle, such as the smoothness of hand motion.

signs of struggle but completed the activity successfully without any errors; and (3) participant showing signs of struggle with an error in the completed activity.

**Figure 1 shows sample video frames from our data. We used two levels for annotation: MTurk workers and a domain expert. Annotations used a forced choice, four-level struggle scale so that we can determine if a person is struggling and to what degree. We evaluated several video understanding methods trained on our dataset and provided various baseline results across our proposed struggle determination activities and machine learning tasks. In summary, our contributions are three-fold:**

1. We deﬁne the problem of struggle determination as a ﬁne-grained task from egocentric assembly videos and introduce a public dataset for struggle determination. This includes new data subsets for activities Pipes-Struggle and Tower-Struggle, as well as struggle annotation for activity Tent-Struggle from EPIC-Tent (Jang et al., 2019). All data is annotated using both an expert annotator and crowd-sourcing. 2. We conducted experiments to identify the most suitable modelling approach for struggle determination and to compare the performance of different deep models to determine the most efﬁcient one. These results serve as baseline benchmarks for struggle determination, not only as a classiﬁcation task but also through regression and label distribution learning as alternative approaches.

<!-- Page 4 -->

7820 International Journal of Computer Vision (2025) 133:7817–7854

3. We demonstrate the essential role of motion information in struggle determination and present preliminary results on the generalization across the three activities.

## 2 Related Work

We ﬁrst review related prior video understanding research, highlighting action recognition, action quality assessment, skill ranking and video anomaly detection. We also summarize related datasets and make comparisons to our struggle determination offering, see Table 8 in the Appendix. Action Recognition. Action recognition (AR) aims to classify actions from video. Many mainstream deep architectures are initially designed for AR but have been used for other video tasks such as Action Segmentation/Detection (Kalogeiton, Weinzaepfel, Ferrari, and Schmid, 2017; Chen et al., 2021; Zhao et al., 2022), Action Anticipation (Grauman et al., 2022), and video-text retrieval (Y. Liu, Albanie, Nagrani, and Zisserman, 2019; Gabeur, Sun, Alahari, and Schmid, 2020). Current AR datasets differ in granularity between coarse-grained actions, in which the background context displays large differences between classes (Soomro, Zamir, and Shah, 2012; Kuehne, Jhuang, Garrote, Poggio, and Serre, 2011; Kay et al., 2017; Fabian Caba Heilbron and Niebles, 2015), and ﬁne-grained actions in which models need to discriminate between action-motion information such as hand-object interactions (Damen et al., 2018; C. Zhang, Gupta, and Zisserman, 2021; Goyal et al., 2017; Y. Li, Li, and Vasconcelos, 2018; Shao, Zhao, Dai, and Lin, 2020; Xu et al., 2022). Numerous deep architectures have been developed for action recognition (AR). Initially, 2D-ConvNets were designed to distinguish video actions from 2D spatial features in video frames. These models typically used temporal average pooling and/or optical ﬂow to enhance motion understanding. Subsequently, 3D-ConvNets (Tran, Bourdev, Fergus, Torresani, and Paluri, 2015; Carreira & Zisserman, 2017; Feichtenhofer, 2020; Xie, Sun, Huang, Tu, and Murphy, 2018; Tran, Wang, Torresani, and Feiszli, 2019; Tran et al., 2018; Feichtenhofer et al., 2019) gained popularity due to their ability to extract spatial-temporal features through 3Dconvolutionoperations. Beneﬁtingfromlarge-scalevideo datasets such as Kinetics (Kay et al., 2017), 3D-ConvNets, despite having a larger number of parameters to train, can be trained end-to-end on these extensive datasets and transferred to numerous downstream tasks. Among the commonly used 3D-ConvNets, the I3D (Carreira & Zisserman, 2017) is notable for its use of RGB frames and optical ﬂow as dual inputs to improve the extraction of motion features. On the other hand, the SlowFast Networks (Feichtenhofer et al., 2019) achieve state-of-the-art results within the realm of 3DConvNets across many benchmark datasets. These networks

require only RGB frames as input, with the Fast pathway extracting motion features that are then fused with the Slow pathway, thus saving considerable time by eliminating the need for optical ﬂow calculations. Recently, Transformer-based architectures such as Vision Transformer (ViT) (Dosovitskiy et al., 2020), Swin Transformer (Z. Liu et al., 2021), MLP-Mixer (Tolstikhin et al., 2021), and gMLP (H. Liu, Dai, So, and Le, 2021) have been known to achieve the state-of-the-art results on many visual tasks beneﬁted with the attention layers and MLP layers which are also data-hungry. For AR, those architectures originally designed for handling images are adapted for handling the spatial-temporal dependencies in video data, including hierarchical architecture such as ViViT (Arnab et al., 2021) and Video Transformer Networks (VTN) (Neimark, Bar, Zohar, and Asselmann, 2021), and the integration of spatial and temporal attention modules such as TimeSformer (Bertasius, Wang, and Torresani, 2021), MViTv1 (Fan et al., 2021), and MViTv2 (Y. Li et al., 2022). Finally, the VisualLanguageModel(VLM)hasbroughtattentiontotheresearch ﬁeld where people are trying to train deep models for more general question-answering purposes, including querying the video data, such as Video-LLaVA (Lin et al., 2023). Action Quality Assessment. Action quality assessment (AQA) is a regression task predicting action quality scores. Most datasets, such as AQA-7 (Parmar & Morris, 2019), Olympic Scoring Dataset (Parmar & Morris, 2016; Pirsiavash, Vondrick, and Torralba, 2014), JIGSAW (Gao et al., 2014), and MTL-AQA (Parmar & Tran Morris, 2019), contain sports scenes, where annotations represent the scores given by expert human judges. The typical architecture for AQA consists of 3D convolutional neural networks (C3D) (Tran, Bourdev, Fergus, Torresani, and Paluri, 2015) to extract spatial-temporal features, followed by average pooling (C3D-AVG), LSTM (Hochreiter & Schmidhuber, 1997) (C3D-LSTM), or attention modules to further aggregate information from various video segments and generate regression or multitask predictions (Parmar & Tran Morris, 2019). Some methods tackle this task using Label Distribution Learning (Tang et al., 2020) in which the distribution of assessment scores is predicted. Methods often predict the scores directly using MLPs (Zhang et al., 2021) or the mean/std. devof aGaussianDistributionbasedonVariational Auto-Encoders (VAE) (Tang et al., 2020; Zhou & Huang, 2022). Skill Determination. Similar to AQA, Skill Determination (O˘gul, Gilgien, and ¸Sahin, 2019; O˘gul, Gilgien, and Özdemir, 2022; Doughty, Damen, and Mayol-Cuevas, 2018; Doughty, Mayol-Cuevas, and Damen, 2019; Hipiny, Ujir, Alias, Shanat, and Ishak, 2023; Z. Li, Huang, Cai, and Sato, 2019) aims to predict the skill level of the action being performed but does so via ranking. O˘gul et al. (2019, 2022) mainly focus on ranking surgery skills using Siamese LSTM-

<!-- Page 5 -->

International Journal of Computer Vision (2025) 133:7817–7854 7821

based networks by aggregating kinematic data. Doughty et al. (2018) investigated ranking skills for general daily activities and introduced the EPIC-Skill2018 dataset. Li et al. (2019) proposes an RNN-based spatial attention model and signiﬁcantly improves the results in Doughty et al. (2018). Doughty et al. (2019) employs a combination of contrastive learning and attention to discriminate between high- and lowskill actions that require ﬁne-grained discrimination skills. Most recently, Hipiny et al. (2023) proposes a TikTok dance video dataset for ranking dancing performances. Ego-Exo4D (Grauman et al., 2024) is a large-scale egocentric dataset that has been released concurrent to this research, with a proﬁciency estimation benchmark to infer the skill level of participants from six scenarios. Video Anomaly Action Detection. Video Anomaly Detection aims to detect unintentional (Duka, Kukleva, and Schiele, 2022; Zatsarynna, Farha, and Gall, 2022) or anomalous actions (Zhu, Bao, and Yu, 2022; Sultani, Chen, and Shah, 2018a; Tian et al., 2021; Zhu & Newsam, 2019). Whilst ﬁne-grained discrimination may share some similarities to Struggle Detection, we differ in two major aspects: persons struggling may not contain anomalies, and Anomaly Detection is constructed as a temporal localisation task. Error/Mistakes Detection. Several datasets in error/ mistake detection are also conceptually related to and complementary to struggle. For example, Assembly101 (Sener et al., 2022) is known as a large-scale dataset for procedure video understanding and is also one of the ﬁrst datasets to include the novel task of detecting mistakes in procedural activities. In this dataset, participants are asked to assemble LEGO toy cars while being recorded by eight static cameras capturing the process from surrounding third-person viewpoints and four egocentric cameras capturing from ﬁrstperson viewpoints. The mistake actions primarily involve procedural mistakes, such as putting on a part too early and making it impossible to assemble subsequent parts, which usually require prior knowledge to determine the correct sequence. There are also skill levels of the participants annotated, ranging from 1 (worst) to 5 (best). Besides, Anomalous Toy Assembly (Ghoddoosian, Dwivedi, Agarwal, and Dariush, 2023) contains toy assembly videos recorded from four different viewpoints and is annotated for when the participants display sequential anomalies in the assembly task, which mainly focuses on the procedural error detection. Holoassist (Wang et al., 2023) contains multi-modality video recordings in instructor-performer pairs for training the interactive AI assistant systems. During the task performance, the instructor may capture the mistakes of the performer and intervene in the task completion procedure, including helping the performer correct the mistakes. These datasets for error/mistake detection mainly focus on procedural mistakes, which are mostly related to understanding the right sequence of steps to perform a task and usually require domain prior

knowledge to be annotated. Most of the methods developed to detect procedural mistakes are similar to anomaly detection pipelines. For example, Flaborea et al. (2024) proposed PREGO, in which the model is trained to anticipate the next steps from normal videos without mistakes and detect mistakes by calculating the distance from the feature of the actual videos during the test stage to see how much difference there are. This PREGO model is recognized as the ﬁrst online one-class classiﬁcation model for mistake detection in procedural egocentric videos and comes with two datasets adapted from Assembly101 (Sener et al., 2022) and EPIC-Tent (Jang et al., 2019). In contrast, our struggle determination, as introduced in Section 1, though correlated with some motor errors, is more general and can be determined without prior knowledge, and in many cases, is a complement to the mistakes/errors. We ﬁnd that ﬁne-grained video understanding has been explored in a variety of different datasets and tasks (see Table 8 in the Appendix). However, despite being crucial for ﬁne-grained activity and skill understanding, determining the participant’s level of struggle has not been attempted. Previous research efforts motivate us to explore three modelling approaches for our dataset: struggle classiﬁcation, struggle level regression, and struggle label distribution learning. As labels for these tasks do not exist, we annotate struggle for one existing dataset (Jang et al., 2019; Sullivan, Ludwig, Damen, Mayol-Cuevas, and Gilchrist, 2021) and collect and annotate two new activities. Our three activities were chosen to require complex visuomotor coordination and problemsolving while remaining goal-driven. This promotes similar yet varied occurrences of struggle behaviours and provides enough diversity within and across activities.

## 3 Struggle Determination Dataset

Here we detail the methodology to record our egocentric videos and collect annotations for the struggle determination dataset. Our struggle dataset is presented under the Non-Commercial Government Licence for public sector information The National Archives (2023). We initially recorded the participants’ behaviour in three scenarios that require complex visuomotor coordination and problem-solving and in which participants may occasionally struggle to complete the tasks. These scenarios were intended to be similar to what one might encounter in daily life (both indoors and outdoors) and where one might want to receive supporting advice. Finding suitable scenarios for struggle detection presents a balancing act. The scenarios shouldn’t be too easy, as participants might not exhibit struggle behaviours, but they also shouldn’t be overly complex or open-ended to prevent unclear goals and excessive task com-

<!-- Page 6 -->

7822 International Journal of Computer Vision (2025) 133:7817–7854

pletion times (we favour activities contained within a few minutes). To effectively study struggle, an ideal video dataset should meetthefollowingcriteria:(1)Thevideosshouldberecorded with unobstructed views (e.g., egocentric) of a person performing a skill; (2) The dataset should contain videos of people performing a skill repeatedly; (3) The skills depicted in the video recordings should be such that they are likely to show instances of struggle. We reviewed several existing datasets and found that most do not fully meet these criteria, particularly the second and third. Table 8 in the Appendix compares these datasets in detail. Forexample,EPIC-Kitchens(Damenetal.,2018)consists of egocentric videos, satisfying the ﬁrst criterion. However, the dataset primarily features daily cooking activities, which are not consistently repeated across participants, making it less suitable for studying struggle. Similarly, Ego4D GoalStep (Song et al., 2023) mainly captures cooking tasks, but the videos vary by recipe, making it difﬁcult to ﬁnd repeated instances of the same task with both struggling and nonstruggling cases. Some prior datasets do contain repeated activities that could be suitable for struggle annotation. For example, Jang et al. (2019); Sullivan et al. (2021) features multiple subjects repeatedly assembling a camping tent-a complex visuomotor task where struggle is more evident. We annotate this data in our work. Other datasets, such as MECCANO (Ragusa, Furnari, Livatino, and Farinella, 2021; Ragusa, Furnari, and Farinella, 2023) and Assembly101 (Sener et al., 2022), contain videos of participants assembling LEGO toys. Assembly101 also includes annotations for procedural mistakes and skill levels. However, neither dataset explicitly annotates struggle, and their focus on LEGO assembly makes them narrower in scope for our study. Therefore, although many existing datasets exist, we decided to collect our own because our research focuses speciﬁcally on user struggle, which is not well-represented in those datasets. More importantly, we aimed for scenarios that, while diverse, would be ordinary enough for naive observers to estimate whether a participant was struggling without prior knowledge. To achieve this, we recorded ﬁrstperson videos of two new activities, assembling plumbing pipes and game of Hanoi tower, and collected the struggle annotations of these videos from human observers. Additionally, we included the existing EPIC-Tent Dataset (Jang et al., 2019; Sullivan, Ludwig, Damen, Mayol-Cuevas, and Gilchrist, 2021), which also captures a relevant struggleprone task of pitching a camping tent. Our dataset includes three scenarios: assembling plastic plumbing pipes by following one of two diagrams (easy or difﬁcult) while seated indoors (Pipes), solving a version of the Tower of Hanoi puz-

zle while seated indoors (Tower), and assembling a camping tent while moving freely outdoors (Tent). The corresponding subsets for struggle determination are named Pipes-Struggle, Tower-Struggle, and Tent-Struggle. Each scenario consists of ﬁne-grained actions that people perform routinely (e.g., arranging, picking up, and placing objects, attaching them, etc.). However, they differ in required equipment, difﬁculty, and goals. Participant performance also varies, with some struggling more than others when completing a scenario.

### 3.1 Dataset Construction

#### 3.1.1 Data Collection

For all three scenarios, university undergraduate and graduate student populations were recruited. All participants gave informed consent and approval for data sharing. We recruited 24 individuals to perform the plumbing task (80% female, mean age 22.6 years), and 20 participants from this group also performed the Tower of Hanoi task. For Struggle-Tent, as stated in Jang et al. (2019), 29 individuals (71% female, mean age 23.3 years) performed the tent-pitching scenario. In all activities, participants wore a head-mounted GoPro Hero 5 (1080p at 60Hz). Videos were captured at 1920 × 1080 resolution, at 30Hz for Tent-Struggle, and 60Hz for PipesStruggle and Tower-Struggle. In the plumbing task, participants assembled home plumbing pipes following a diagram of a double sink layout (see supplementary material). Participants were evenly divided betweenthosegivenaneasierdiagramandthosegivenamore difﬁcult one. To simplify dexterity requirements, the activity was reduced to assembling the layout of the pipes on a table rather than in a real sink. For Struggle-Tent, we focused on video segments from ﬁve actions in EPIC-Tent (Jang et al., 2019): assemble support, insert stake, insert support, insert support tab, and place guyline. Our expert annotator identiﬁed these actions as having the most obvious visual cues of struggle based on a series of pilot studies. The video data was segmented into 10-second clips, and the struggle determination annotation was accomplished by one expert and MTurkers, both of whom rated these short video clips from the three activities. We choose to annotate struggle using video-level weak labels based on the 10-second windows for the following reasons: (1) Videolevel weak labels simplify the annotation process, making it easier for annotators and easier to scale. During early annotation experiments, we observed that the annotators expressed difﬁculty locating the beginning and end of struggle segments accurately. (2) Annotating video-level weak labels has been found to be much more time-efﬁcient than annotating the start and end frames in long, untrimmed videos (e.g. six times more time efﬁcient Ma et al. (2020). When annotators mark the start and end frames of temporal segments, they

<!-- Page 7 -->

International Journal of Computer Vision (2025) 133:7817–7854 7823

**Table 1 Struggle level descriptions**

Deﬁnitely non-struggling The actions are executed with complete ease and conﬁdence, with no indication of any challenge or difﬁculty encountered throughout the task.

Slightly non-struggling The actions are generally executed smoothly and with conﬁdence, with only occasional minor indications for some level of challenge or difﬁculty.

Slightly struggling There are some signs of challenge or difﬁculty, including occasional errors, pauses, dropping off items, or wobbly hands that indicate the participants may not be very experienced in the task at hand but are still making an effort to complete it.

Deﬁnitely struggling There are clear, consistent signs of difﬁculty, such as repeated attempts, unsmooth motions, and signiﬁcant pauses during the task. These actions indicate that the participants don’t really know what to do and may lack the knowledge or skill necessary to complete the task successfully.

often need to replay videos multiple times to verify boundaries. In contrast, annotating small video segments with a single segment-level label for struggle reduces the need for repeated viewings. (3) Video-level weak labels help mitigate issues with the disagreement of the temporal action boundaries among the annotators, especially for more conceptual annotations, such as struggle. This was found in previous work Moltisanti et al. (2017), in which researchers uncovered inconsistencies in the ground truth temporal boundaries across annotators. By focusing on whether a segment contains struggle, ambiguity is reduced to determining the presence of struggle within the segment. (4) We choose 10 seconds for the size of the annotation window following evidence from Experimental Psychology research that shows that manipulation struggle actions can be identiﬁed well within 10 seconds (Warneken & Tomasello, 2006a,b). Based on the arguments above, our annotation clips thus were created by uniformly splitting the videos into 10-second segments from beginning to end (e.g. 0-9s, 10-19s, etc.). We also resized videos to 456 × 256 at 30Hz. This ensured segments had a sufﬁcient resolution for both human observers and the deep learning models. Our annotation process thus focuses on evaluating struggle within isolated 10-second clips. This means that we assess and label the presence of struggle solely within each clip without considering the broader context of the full video recording.

#### 3.1.2 Annotating Struggle

Since the original videos are uniformly trimmed into 10second segments, we focus on ‘low-level’ struggling cues, which we believe are the most transferable between participants and scenarios. Signs of struggle may include shaky hands, pausing actions, dropping items, or action repetition, however, we note that these are often task dependent. While there is a variety of ways one might ask an observer to estimate struggle, we chose to ask observers to rate video segments using a four-point scale varying from deﬁnitely non-struggle to deﬁnitely struggle. Table 1 lists the descriptions for each struggle level. The value of a ﬁner, four-point

gradation of struggle supports the longer-term goal of assistive systems. For example, the support provided to a person whois‘slightly’strugglingmayconsistofafewbasicinstructions or reminders. However, a person determined to be deﬁnitely struggling may require more detailed guidance or even recommendations to stop the task to prevent injury. This scoring approach allows non-expert observers to indicate the degree of struggle/non-struggle. Note that we forced annotators to choose between struggle and nonstruggle instead of allowing a neutral option. This avoided centring strategies on ambiguous judgements and allowed easybinarisationofannotationsduringearlypilotapproaches that classed only struggle presence/absence. Throughout our research, we found that indicating the degree of struggle is necessary to address signiﬁcant challenges in classifying ﬁne-grained features. As self-reporting levels of struggle can be unreliable and provide only a single judgement, we focus on a balance between expert and naive observers. We collected two distincttypesofstruggledeterminationannotations:naive‘voter labels’ obtained from Amazon Mechanical Turk (MTurk) participants and ‘expert annotations’ (or ‘golden annotation’ (GA)) obtained from the author who collected the video dataset, who repeatedly viewed them during analysis, and annotated action and error labels in the EPIC-Tent dataset. For each 10s video clip, there is only one expert annotation label, and there are multiple voters’ labels that form a frequency distribution. This distribution consists of 15 or 20 voter labels. To extract a single struggle label from this distribution, we take the mode. Since voter labels and expert annotation come from distinct groups of individuals with differing prior knowledge, label distributions can vary across all video segments for each dataset. Consistency between the voters’ modes and expert annotations indicates that the struggle-level annotations of the videos are more reliable. In addition, the standard deviation of the voters’ distributions provides a measure of the voter consistency or label ambiguity for a segment (smaller values indicate greater consistency, i.e. less label ambiguity). We explore these annotations in Section 3.2.

<!-- Page 8 -->

7824 International Journal of Computer Vision (2025) 133:7817–7854

#### 3.1.3 Expert Annotator Qualiﬁcations and Training

Unlike the MTurk workers who only viewed 10-second clips for each task, the expert, as a paper author, had previously reviewed the full videos (multiple times) during data annotation and quality control. For expert annotation, this broader context helped ensure consistency, as the expert worked independently. In contrast, crowd annotators did not require prior task knowledge, but to achieve a relatively consistent struggle annotation, we relied on multiple crowd workers per clip and aggregated their inputs by averaging struggle levels or taking the mode after ﬁltering out outliers. Additionally, the expert had prior experience coding actions, objects, and errors within the ‘tent’ dataset, further honing their observation and annotation skills relevant to the 10-second clips. We acknowledge the inherent ambiguity in struggle determination, similar to skill assessment. Much like Olympic judges offering slightly different scores for a performance of exceptional skill, we anticipate slight disagreement in struggle determination. In our dataset of short clips, struggle typically manifests as motor errors, such as abrupt stops, repetitive attempts without success, object drops, or awkward manipulation. We believe that basic motor struggle is readily detectable by most human observers based on the ﬂuidity of movement. However, the expert’s advantage lies in their extensive experience making such judgments, coupled with their unique knowledge of the full videos and speciﬁc task goals.

#### 3.1.4 MTurk Annotator Validation

We recognize that struggle determination and its annotation is an ambiguous task, which is part of the challenge and research value for this problem. However, we still want to ensure annotation by crowd-sourcing is as valid as possible and that the votes and MTurkers can be trusted. We use the following assumptions in this ambiguous undertaking: 1) we assume the expert annotator is trustworthy, 2) ambiguity between sides of struggle (e.g. deﬁnitely struggling and slightly struggling and similarly for non-struggling) is acceptable and expected, 3) If an MTurker is untrustworthy in one of the assignments, all results by that MTurker is questionable and best to remove. Based on these assumptions, we implemented the following validation checks:

- If all votes for all videos assigned to the MTurker on an assignment are the same, all results from all assignments by that MTurker are rejected.
- We also identiﬁed 6 validation videos using three definitely struggling and three deﬁnitely non-struggling videos as labelled by the expert annotator. These videos were further checked for unanimous agreement by simple inspection by the authors.

- These 6 validation videos were included as part of the MTurk assignments, and for every MTurker, we calculate the level of disagreement w.r.t. the validation videos and the expert’s votes. If the MTurker’s votes are on the same side of struggle w.r.t the expert’s vote, i.e. (struggling (3,4) or non-struggling (1,2)), this does not count as a disagreement. The MTurker is rejected if the ratio of disagreement w.r.t. the validation videos is > 0.33, i.e. three or more out of 6 disagreements for the validation videos.

### 3.2 Struggle Statistics

The three subsets of Our struggle determination dataset, Tent-Struggle, Pipes-Struggle, and Tower-Struggle, contain 585, 1011, and 236 annotated 10s videos, respectively, totalling 5.09 hours recorded with 725,100 frames. We further divide each of the subsets into four splits for four-fold cross-validation such that the video samples from the same participants are within the same split and the struggle label distributions are similar across the four splits (see Figure 3 for more details). The ﬁnal split of participants and the corresponding number of video segments they contributed are shown in Figure 4. We note that some participants have contributed a signiﬁcantly higher number of video segments than the other participants. When considering struggle labels for video samples, there are three types of labels to take into account: a) Expert annotation—the struggling label given by the expert annotator; b) Voters’ mode—the statistical mode’s struggle value the voters give; and c) Voters’ label distribution—composed of the frequencies of voters’ labels on each of the struggle levels for each video sample. Table 2 shows the statistics for the three subsets, including the total number of videos, the number of videos per split and the proportion of videos whose voters’ mode label is consistent with expert annotations. Figure 5 shows both the overall frequencies for each struggle label and the histograms of standard deviations that indicate the label ambiguity for each dataset. The histograms of standard deviations tell us that the Tent-Struggle dataset has the most ambiguous voter labels with a standard deviation between 0.9 and 1. This is followed by the PipesStruggle dataset and then the Tower-Struggle dataset, which has the least ambiguous labels. These ambiguities reﬂect the degree of variation innate to each task, with a well-deﬁned task, such as Towers of Hanoi, having less ambiguity and assembling the non-rigid tent outdoors having the highest. The variation of ambiguity is a further element of interest for struggle-aware systems. Furthermore, Figure 13 in the Appendix shows the intra-group voter disagreement in the dataset. This ﬁgure illustrates additional analysis on the frequency of annotators’ disagreements regarding the binary

<!-- Page 9 -->

International Journal of Computer Vision (2025) 133:7817–7854 7825

**Fig. 3 Frequency distributions of four-level struggle labels across dataset splits. The three graphs illustrate the distribution of struggle labels (ranging from 1 to 4 on the x-axis) for the activities Pipes-Struggle (left), Tent-Struggle (middle), and Tower-Struggle (right).**

**Fig. 4 A summary of the number of video samples corresponding to participants for each split. The three graphs display participant IDs and the number of video segments for the activities Pipes-Struggle (left), Tent-Struggle (middle), and Tower-Struggle (right), respectively.**

**Table 2 Statistics for the three activities: the number of videos altogether and in each split, percentage of consistent videos in binary labels, percentage of consistent videos in four-class labels.**

Tasks #Videos (Split 1/2/3/4) %Binary Consistent %Four-way Consistent

Pipes 1011 (275/229/244/263) 80.61% 45.40%

Tent 585 (138/147/146/154) 72.48% 42.91%

Tower 236 (65/54/67/50) 84.32% 52.12%

**Fig. 5 Comparison of the distributions. The ﬁrst three graphs on the left display the frequency distribution of the struggle labels, including expert annotation (EA), voters mode (Mode), and voters labels distributions (Voters), for the three activities. The three graphs on the right**

display the histogram with smoothed kernel density estimate (KDE) of the standard deviation (StdDev) of the voters’ labels for the three activities (Color ﬁgure online)

<!-- Page 10 -->

7826 International Journal of Computer Vision (2025) 133:7817–7854

**Table 3 Comparison of Classiﬁcation and Regression-to-Classiﬁcation Accuracy based on MViTv2 (Y. Li et al., 2022)**

Activity Classiﬁcation Regression-to-Classiﬁcation

Binary Accuracy (%) Four-Way Accuracy (%) Binary Accuracy (%) Four-Way Accuracy (%)

Pipes-Struggle 76.68 48.43 75.03 44.71

Tent-Struggle 64.42 37.06 59.86 35.60

Tower-Struggle 84.29 57.42 81.29 33.41

struggle/non-struggle labels, focusing on the minority group, which has fewer instances of disagreement, ranging from 0 to 0.5. As shown in the ﬁgure, video samples with a minority group disagreement frequency of 0.5 constitute only a small proportion of the entire dataset.

## 4 Experiments

In this section, we provide baseline results for our struggle determination dataset and structure experiments to answer thefollowingquestions:(i)Whatarethemostimportantmodelling approaches for struggle determination? (ii) To what extent can a struggle determination model being trained on one subset generalize to other struggle subsets? (iii) How do different backbone deep architectures impact struggle determination performance? and (iv) What are the most appropriate baseline models for struggle determination?

### 4.1 Comparison of Struggle Modelling Approaches

Weinitiallythoughtaboutthreedifferentmodellingapproaches to assess struggle determination comprehensively. First, struggle classiﬁcation includes binary and four-way classiﬁcations using expert labels. In the four-way classiﬁcation, we use the original struggle labels ranging from 1 to 4, corresponding to deﬁnitely non-struggle to deﬁnitely struggle. In binary classiﬁcation, we binarize the label of struggling versus non-struggling by grouping 1&2 votes and 3&4 votes. Secondly, regression ﬁts the degree of struggle. Here we treat the struggle level labels from the experts as continuous real numbers ranging from 1 to 4. The third approach is label distribution learning. Given the video samples, we train a deep model that can predict the frequency distribution over the four struggle levels while using the frequency distributions of the voters’ annotations for each video segment as the target. This modelling approach is a supplementary way of making full use of the voters’ annotations, and thus, we will include it in our ablation study (Section 5.3.2). Compared to struggle classiﬁcation, struggle regression provides the advantage of modelling struggle levels as continuous real numbers from 1 to 4, capturing subtle variations more effectively. To determine the most suitable modelling

approach, we ﬁrst train a deep model for struggle classiﬁcation and then train the same model for struggle regression. The regression outputs are then quantized into either binary or four-way struggle classiﬁcations, and we evaluate the accuracy rate for comparison. We adopt this regression-toclassiﬁcation approach because the primary goal in struggle determination is to conclude whether a person is struggling or not, rather than merely predicting a numerical score. The experimental results are presented in Table 3. We selected MViTv2 (Y. Li et al., 2022) for this comparison due to its efﬁciency in capturing spatiotemporal features as a recent representative transformer-based model. The results show that both binary and four-way struggle classiﬁcation achieve higher accuracy when the model is trained as a classiﬁcation task rather than a regression task. This trend holds consistently across all three struggle datasets, suggesting a signiﬁcant advantage of training the deep model as a classiﬁcation task. One possible reason is that the regression model predicts a continuous struggle level, and when these scores are quantized into binary or four-way classes, some instances may be misclassiﬁed due to threshold selection. We also conducted extensive experiments comparing classiﬁcation and regression results across different deep models to assess the consistency of this trend (see Appendix Table 9 and 10). The ﬁndings consistently indicate that models trained for classiﬁcation outperform those trained for regression.

### 4.2 Generalisation Experiments

After identifying struggle classiﬁcation as a priority modelling approach for struggle determination, we conduct experiments to show the generalisation capability among the three different scenarios: Pipes-Struggle, Tent-Struggle, and Tower-Struggle. We used the same MViTv2 (Y. Li et al., 2022) model to conduct the generalisation experiment. We ﬁrst trained and evaluated the model on the same struggle subset and then conducted zero-shot evaluations on the other two struggle subsets. See Table 4 for the results. Although the struggle classiﬁcation accuracy dropped when directly evaluated on unseen struggle subsets, some training-testing combinations demonstrate potential generalization. In general, models trained on Pipes-Struggle and Tent-Struggle tend to transfer better to Tower-Struggle, achieving rela-

<!-- Page 11 -->

International Journal of Computer Vision (2025) 133:7817–7854 7827

**Table 4 Results show a comparison between training the model on a combined training set versus the training on each of the struggle subsets separately. ‘Best Epoch Over Three’ refers to selecting a single epoch that performs relatively best based on the average accuracy across all three validation sets from all three struggle subsets, which is then eval-**

uated individually on each subset. ‘Best Epochs Separately’ refers to choosing the best epochs with the highest accuracy on each subset’s validation set. The results are reported using the mean accuracy across all cross-validation splits.

Dataset Test Sets - Binary Cls. Accuracy (%) Test Sets - Fourway Cls. Accuracy (%)

Training Sets Pipes-Struggle Tent-Struggle Tower-Struggle Pipes-Struggle Tent-Struggle Tower-Struggle

Pipes-Struggle 76.68 49.22 71.56 48.43 21.58 40.79

Tent-Struggle 53.99 64.42 75.54 30.97 37.06 29.04

Tower-Struggle 62.31 47.90 84.29 24.31 25.42 57.42

Combined (Best epoch over three) 77.29 66.71 90.22 48.79 40.26 66.55

Combined (Best epochs Separately) 79.83 69.48 91.74 51.89 41.81 68.81

tively high accuracy. This is followed by models trained on Tent-Struggle and Tower-Struggle transferring to PipesStruggle, though the generalization is not as strong. The struggle in the Tent-Struggle subset appears to be the hardest to generalize to, as models trained on Pipes-Struggle and Tower-Struggle achieve the lowest accuracy when tested on Tent-Struggle. This may be due to the struggle patterns in Tent-Struggle being more complex and occurring in an outdoor setting, compared to the other two indoor environments. Additionally, models trained on Tower-Struggle struggle to generalize to the other subsets, especially in four-way classiﬁcation, where accuracy drops signiﬁcantly. This may be because the struggle patterns in the Tower-Struggle subset are relatively simple, the task complexity is low, and the number of data samples is the smallest among the three subsets. These ﬁndings suggest that certain struggle features are more transferable across subsets, while others remain more dataset-speciﬁc. We also conducted multi-activity learning experiments to train the model using a combination of the training splits of the three struggle subsets and evaluating the model on the validation splits individually. We concatenate the training data to train the MViTv2 (Y. Li et al., 2022) model and run three evaluation stages separately to evaluate the accuracy rate on the three subsets. We present results in Table 4, where we report both the binary classiﬁcation accuracy and the four-way classiﬁcation accuracy by comparing the combined training results with the baseline results of training separately. The results are reported in two ways: (1) selecting the epochs with the highest accuracy on each validation set individually, which may be different for each set, and (2) selecting a single epoch that provides the best average accuracy across all three validation sets. As shown in Table 4, training the MViTv2 (Y. Li et al., 2022) model on combined struggle subsets demonstrates a signiﬁcant trend of improvement in both binary and four-way classiﬁcation accuracy, with increases generally ranging from 3% to 11% compared with training on each of the struggle subsets separately. This

is not only because the MViTv2 (Y. Li et al., 2022) model beneﬁts from having more training data for struggle classiﬁcation but also because it may indicate that struggle patterns are potentially transferable across different task-performing activities. We show that the generalisation across datasets is challenging for MViTv2 (Y. Li et al., 2022), particularly in a zero-shot setting. Our results indicate that struggle classiﬁcation accuracy drops signiﬁcantly when the model is tested on a different struggle subset. Moreover, MViTv2 (Y. Li et al., 2022) requires a large amount of data to generalise well or, more speciﬁcally, to surpass the accuracy achieved when trained and tested on individual struggle subsets. This is evident from the bottom two rows of our results, where training on combined subsets leads to better performance on the evaluation test sets.

### 4.3 Model Comparison on Struggle Classification

#### 4.3.1 Baseline Methods

We compare four different types of deep architectures in our struggle classiﬁcation experiments: (1) 2D-ConvNet, represented by Temporal Segment Networks (TSN) (L. Wang et al., 2016), is implemented using yjxiong and Line290 (2019). (2) 3D-ConvNet, represented by SlowFast Networks (Feichtenhofer et al., 2019), is based on ResNet50 (SlowFast-R50) (Fan, Li, Xiong, Lo, and Feichtenhofer, 2020). (3) Hybrid architectures, which combine 3D-ConvNets and Vision Transformers (Dosovitskiy et al., 2020), include VTNSlowFast-ViTandSlowFast-gMLP.(4)ATransformer-based model, MViTv2 (Y. Li et al., 2022), which has been adopted in the previous experiments. Besides, we included the VideoLLaVA (Lin et al., 2023) VLM model, evaluated in a zero-shot setting without further ﬁne-tuning, as shown in Appendix 3. The two-hybrid architectures, VTN-SlowFast-ViT and SlowFast-gMLP, utilize the SlowFast Networks as the backbone to extract spatial-temporal features from video

<!-- Page 12 -->

7828 International Journal of Computer Vision (2025) 133:7817–7854

sequences. These features are then processed by different attention mechanisms: VTN-SlowFast-ViT incorporates a Vision Transformer encoder block (Dosovitskiy et al., 2020), implemented using lucidrains (2023), while SlowFast-gMLP applies a gMLP layer (H. Liu, Dai, So, and Le, 2021), implemented using lucidrains (2021). Speciﬁcally, given a dataset containing a set of K videos P = {pi, 1 ≤pi ≤K}. We sample N frames from each video sample pi and feed them into the SlowFast backbone (·). This process result in N/8 feature vectors, which are then passed through either the Vision Transformer encoder or a gMLP layer. Finally, an MLP head is applied for struggle classiﬁcation. We employ four-fold cross-validation to train and evaluate these deep models across all tasks and report the averaged top-1 classiﬁcation accuracy over all four validation splits. To mitigateoverﬁtting,wefreezetheparametersinthebackbone layers during training, effectively performing linear probing for TSN (L. Wang et al., 2016) and SlowFast-R50 (Feichtenhofer et al., 2019). For the hybrid architectures, only the Vision Transformer-based layers are trained while keeping the backbone frozen.

#### 4.3.2 Results and Model Comparison

The struggle classiﬁcation results are shown in Table 5. The ﬁrst three rows represent naive baselines: (1) the random performance baseline, which uses the frequency of classes as the probability for random predictions; (2) the majority class baseline, which predicts the majority class and reports its percentage; and (3) the voter’s baseline, where the mode of the voters’ annotations is used to compare with the expert annotations and calculate the averaged accuracy. The experimental results indicate a performance gap between the voter’s baseline accuracy and the best-performing deep models. Among the evaluated models, TSN (L. Wang et al., 2016) and MViTv2 (Y. Li et al., 2022) exhibit relatively low accuracy in both binary and four-way classiﬁcations. The poor performance of the TSN (L. Wang et al., 2016) in struggle classiﬁcation is probably attributed to the 2DConvNet design, which lacks explicit temporal modelling and relies solely on global average pooling (GAP) to aggregate frame-level features. This approach disregards frame order, negatively impacting performance. On the other hand, MViTv2 (Y. Li et al., 2022), despite leveraging spatialtemporal dependencies through multi-head self-attention, still underperforms. This may be due to the fact that transformer-based models typically require large amounts of training data to effectively capture useful patterns. This hypothesis is supported by the signiﬁcant improvement in struggle classiﬁcation performance when MViTv2 (Y. Li et al., 2022) is trained on combined datasets, as shown in

**Table 4, which also outperforms the struggle classiﬁcation accuracy of all other deep models shown in Table 5. In contrast, 3D-ConvNets, represented by the SlowFast(Feichtenhoferetal.,2019)anditsenhancedvariantslike VTN-SlowFast-ViT and SlowFast-gMLP, which incorporate more advanced temporal modelling, generally achieve higher accuracy. This suggests that the SlowFast model, as a feature extraction backbone, is more robust for struggle classiﬁcation, particularly when training data is limited. The struggle classiﬁcation accuracy is similar across the three models that use SlowFast (Feichtenhofer et al., 2019) as the backbone. This suggests that incorporating MLP- or Transformer-based architectures for additional temporal modelling does not signiﬁcantly improve performance. A likely explanation is that our video-level weak labels for video segments do not require extensive temporal processing beyond the initial spatialtemporal feature extraction. As a result, the potential of the MLP/Transformer layers to enhance accuracy remains limited in this context.**

### 4.4 Implementation Details

We conducted two classiﬁcation experiments: binary classiﬁcation, which distinguishes between struggle and nonstruggle, and four-class classiﬁcation, which predicts the level of struggle. In both cases, the model is supervised using the Cross-Entropy (CE) loss. Top-1 accuracy is used as the evaluation metric. During training, we uniformly sample 128 frames from each video and randomly crop the input frames from 256 × 456 to 256 × 256. Additionally, a horizontal ﬂip is applied with a probability of 0.5 during training. For the evaluation stage, we perform 5 random crops and ensemble the predictions to improve robustness. For TSN (L. Wang et al., 2016), we implement the BNInception base architecture with the average consensus for the output from each segment. We use ImageNet (Russakovsky et al., 2015) pre-trained features and train only the fully connected layer for classiﬁcation. To address overﬁtting, we set the dropout rate to 0.5. The model is trained for 65 epochs using mini-batch stochastic gradient descent with a momentum of 0.9 and a weight decay of 5 × 10−4. The learning rate starts at 0.0001 and decreases by a factor of 10× at epochs 30 and 60. For the SlowFast (Feichtenhofer et al., 2019) model and its variants, we use the architecture that is based upon 3D-ResNet50 and load parameters pre-trained on Kinetics400 (Kay et al., 2017). During training, we freeze the backbone parameters and train the remaining MLP layers. The same optimizer is used, but the weight decay is set to 1 × 10−5. We train these models for 70 epochs with a cyclic cosine decay learning rate scheduler (Athiwaratkun, Finzi, Izmailov, and Wilson, 2019), where the learning rate starts at 0.001, decreases by a factor of 100 over 50 epochs, and

<!-- Page 13 -->

International Journal of Computer Vision (2025) 133:7817–7854 7829

**Table 5 Struggle classiﬁcation results. Classiﬁcation accuracy for binary classiﬁcation, four-way classiﬁcation and the four-way to binary classiﬁcation.**

Dataset Pipes-Struggle Tent-Struggle Tower-Struggle Top-1 Accuracy (%) Top-1 Accuracy (%) Top-1 Accuracy (%)

Models Binary Four-Way Four-to-Bin Binary Four-Way Four-to-Bin Binary Four-Way Four-to-Bin

Random 53.38 28.62 53.38 50.59 27.12 50.59 67.86 40.45 67.86

Majority Class 62.60 39.60 62.60 54.91 35.79 54.91 79.74 57.56 79.74

Voters’ Baseline 80.56 45.31 80.56 72.63 43.12 72.63 84.41 51.86 84.41

TSN (L. Wang et al., 2016) 74.54 48.59 71.43 68.05 38.23 61.24 82.59 59.49 78.68

### 78.01 50.77 75.73 69.56 39.48 65.19 88.24 63.92 80.27

SlowFast-R50 (Feichtenhofer et al., 2019)

SlowFast-gMLP 78.59 49.70 74.49 68.00 39.52 64.68 87.86 66.40 83.17

VTN-SlowFast-ViT 77.80 50.83 72.48 68.53 40.11 60.86 88.11 64.03 82.54

MViTv2 (Y. Li et al., 2022) 76.68 48.43 72.25 64.42 37.06 58.15 84.29 57.42 82.59

includes two restart intervals of 10 epochs each. During these intervals, the learning rate increases by a factor of 10 at the start and decreases back to the minimum value. For MViTv2 (Y. Li et al., 2022), we implement its small variant to accommodate limited GPU memory. We load parameters pre-trained on Kinetics-400 (Kay et al., 2017) and ﬁne-tune the model using linear probing to mitigate overﬁtting. The AdamW optimizer is used with a weight decay of 5 × 10−4 and a cyclic cosine decay learning rate scheduler. The ﬁrst 10 epochs serve as a warm-up stage, starting at 0.1 times the base learning rate of 0.001. The learning rate is then gradually decreased by a factor of 0.01 from epoch 10 to epoch 50, followed by two ﬁxed restart intervals where the learning rate decreases from 0.1 times the base learning rate to 0.01 times the base learning rate. The total training duration is 70 epochs. In the generalization experiments, we maintain the same settings but employ a multi-step learning rate scheduler. The base learning rate starts at 1 × 10−6 and increases by a factor of 10 at epochs 30 and 60, with the models trained for 70 epochs for full parameter ﬁne-tuning. All experiments are conducted using two Titan X (Pascal) 12GB GPUs or two GTX 1080Ti 11GB GPUs.

## 5 Ablation Study

In this section, we investigate the importance of temporal modelling, motion understanding, and alternative training strategies in struggle determination. Speciﬁcally, we conduct two series of experiments: (1) validating the use of multiple frames for struggle determination by comparing it with single-frame input and (2) evaluating the importance of temporal ordering in frame sequences for struggle determination. Additionally, we explore alternative training strategies by presenting baseline results for struggle regression and strug-

gle label distribution learning. We also compare different deep learning models to identify the most effective architecture for struggle determination under these two training approaches.

### 5.1 Multiple Frames

To demonstrate the importance of using multiple frames, we conducted experiments comparing a single-frame input to the original multi-frame setting. First, we initialized the model with parameters trained on 128 frames, which is the standard setting for our experiments. We then tested the model using only one frame from each video sequence. The selected frame was taken from three different positions within each video segment-at 25%, 50%, and 75% of its duration. Since 3D-ConvNets require a minimum number of input frames, we duplicated the selected frame to meet these requirements: 8 frames for SlowFast (Feichtenhofer et al., 2019) and 128 frames for SlowFast-gMLP and VTN-SlowFast-ViT. To ensure consistency between training and testing, we conducted an additional experiment where we trained the model using randomly selected single frames from each video. During testing, we applied the same single-frame selection strategy as before, choosing frames from 25%, 50%, and 75% of each video segment. The results of these two approaches are shown in Figure 6. We report the average test accuracy for single-frame inputs across the three proportions. Across all tested models and activities, we observed a signiﬁcant drop in accuracy when using only one frame. Since our dataset consists of randomly trimmed 10-second video segments, accuracy values for frames sampled from 25%, 50%, and 75% are expected to be similar, leading to a small standard deviation (StdDev) in accuracy, as reﬂected in the bar plots.

<!-- Page 14 -->

7830 International Journal of Computer Vision (2025) 133:7817–7854

**Fig. 6 Importance of Multiple Frames. The left three bar charts show binary classiﬁcation results for Pipes-Struggle, Tent-Struggle, and Tower-Struggle, while the right three show four-way classiﬁcation for the same activities. Each group of bars represents the base model (blue), evaluation with a single frame (orange), and training/evaluation with a**

**Fig. 7 Importance of Temporal Ordering. The left three bar charts show binary classiﬁcation results for Pipes-Struggle, Tent-Struggle, and Tower-Struggle, while the right three show four-way classiﬁcation for the same activities. Each group of bars represents input frames in the**

Even when the models were trained using a single frame, the test accuracy remained notably lower compared to the multi-frame setting, though slightly higher than testing on a single frame without training adjustments. This suggests that a single frame does not provide sufﬁcient visual information for deep learning models to reliably determine whether a person is struggling. Therefore, effectively capturing the useful temporal motion features is crucial for our struggle determination task. A sequence of frames can typically reveal anomalous hand motions such as ‘unsmooth’ motions, stopping, and repeating that indicate the person did not have things under control. A single frame cannot represent these dynamic features.

### 5.2 Temporal Ordering

To further examine the role of temporal motion features in determining struggle, we conducted a shufﬂe-frame test to assess the importance of frame ordering. In this experiment, we excluded TSN (L. Wang et al., 2016), as it only considers 2D spatial features and averages the output logits of all input frames, making it inherently insensitive to frame order.

single frame (green). Error bars indicate the standard deviation (StdDev) across sampled frames from 25%, 50%, and 75% of the video. See Appendix Table 25 for detailed result numbers. Model abbreviations: TSN (L. Wang et al., 2016), SF: SlowFast (Feichtenhofer et al., 2019), SFG: SlowFast-gMLP, VSV: VTN-SlowFast-ViT.

correct order (blue), evaluation with shufﬂed frames (orange), and training/evaluation with shufﬂed frames (green). See Appendix Table 26 for detailed result numbers. Model abbreviations: SF: SlowFast (Feichtenhofer et al., 2019), SFG: SlowFast-gMLP, VSV: VTN-SlowFast-ViT.

We began by loading model parameters pre-trained on 128 frames in their correct sequential order. Then, for each video sample, we randomly shufﬂed the same number of 128 frames, disrupting their natural temporal sequence. The model was run in inference mode ten times, and we reported the average accuracy across these runs. As shown in Figure 7, the accuracy dropped signiﬁcantly to a near-random level, indicating that the models for determining struggle are highly dependent on the correct frame order. To ensure a fair comparison, we conducted an additional experiment in which we trained the models using shufﬂed frames and then evaluated them under the same conditions. While training on shufﬂed frames led to improved performance compared to testing on shufﬂed frames alone, the accuracy remained lower than when the models were trained and tested on correctly ordered frames. These results suggest that deep learning models learn crucial temporal dependencies when trained on properly ordered sequences to determine struggle. When this temporal structureisdisrupted,modelperformancedeteriorates,conﬁrming that the models rely on motion continuity to infer strugglerelated cues.

<!-- Page 15 -->

International Journal of Computer Vision (2025) 133:7817–7854 7831

**Table 6 Mean Squared Error (MSE) and Mean Absolute Error (MAE), along with converted binary and four-way classiﬁcation accuracy.**

Activities Models Regression & Classiﬁcation

MSE↓ MAE↓ Binary Acc.↑ Four-way Acc.↑

Pipes-Struggle Voters’ Baseline 0.890 0.654 80.56% 45.31%

TSN (L. Wang et al., 2016) 0.798 0.725 72.42% 45.02%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.645 0.655 76.89% 45.28%

VTN-SlowFast-ViT 0.648 0.650 77.30% 48.56%

MViTv2 (Y. Li et al., 2022) 0.745 0.708 75.03% 44.71%

Tent-Struggle Voters’ Baseline 1.031 0.713 72.63% 43.12%

TSN (L. Wang et al., 2016) 0.888 0.768 68.27% 40.95%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.850 0.760 69.06% 41.63%

VTN-SlowFast-ViT 0.868 0.770 66.39% 40.69%

MViTv2 (Y. Li et al., 2022) 1.055 0.858 59.86% 35.60%

Tower-Struggle Voters’ Baseline 0.726 0.561 84.41% 51.86%

TSN (L. Wang et al., 2016) 0.935 0.748 85.35% 58.01%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.793 0.690 86.89% 56.00%

VTN-SlowFast-ViT 0.723 0.673 87.86% 61.73%

MViTv2 (Y. Li et al., 2022) 0.955 0.828 81.29% 33.41%

### 5.3 Alternate Training Strategies

This section explores alternative training strategies for struggle determination, focusing on struggle regression and struggle label distribution learning. Struggle regression provides a full regression baseline, complementing the regression-toclassiﬁcation approach. Struggle label distribution learning trains models to predict the distribution of voters’ annotations, making better use of crowd-sourced struggle labels. We detail the implementation of these strategies and evaluate their effectiveness across different deep models.

#### 5.3.1 Struggle Regression

Implementation Details In the regression task, struggle labels are treated as continuous scores ranging from 1 (definitely non-struggle) to 4 (deﬁnitely struggle), representing the degree of struggle. A fully connected layer is added for regression, which takes the feature vectors as input and outputs a regression score. The predicted score is supervised using Mean Squared Error (MSE) with the target regression labels. Evaluation metrics include MSE and Mean Absolute Error (MAE) for regression, as well as top-1 accuracy for binary and four-way classiﬁcation. To ensure a solid conclusion on whether a person is struggling and to what extent, the continuous regression scores are quantized into strugglelevel categories by thresholds when computing classiﬁcation accuracy for the struggle regression model. The hyperparameter settings for optimization remain the same as in the struggle classiﬁcation task.

Regression Results Regression results are shown in Table 6. Overall, VTN-SlowFast-ViT achieves the lowest MSE and MAE and the highest binary and four-way classiﬁcation accuracy for the Pipes-Struggle and Tower-Struggle activities. SlowFast-R50 (Feichtenhofer et al., 2019) follows closely, showing competitive regression performance and classiﬁcation accuracy, particularly for Tent-Struggle. This suggests that incorporating a vision transformer layer on top of the frame-level features from the SlowFast (Feichtenhofer et al., 2019) backbone as a temporal model improves the ﬁt to the struggle regression labels. In contrast, TSN (L. Wang et al., 2016) and MViTv2 (Y. Li et al., 2022) generally underperform compared to these two models.

#### 5.3.2 Struggle Label Distribution Learning

Implementation Details In label distribution learning, we train a deep model to predict the frequency distribution of voters’ struggle labels given video samples. In our implementation, the ﬁnal layer output of the model has four nodes correspondingtoeachofthefourstrugglelevelsrangingfrom 1 (deﬁnitely non-struggle) to 4 (deﬁnitely struggle), followed by a SoftMax layer so that each node’s output value ranges from 0 to 1 with the summation equalling to one. We optimize the Kullback-Leibler (KL) divergence loss function to align the predicted distribution with the frequency distribution of the voters’ struggle labels for each video sample. For evaluation, we calculate Spearmans’ Rank Correlation (Spearmans’ Rho) between the predicted frequency distribution and the voters’ struggle label frequency distri-

<!-- Page 16 -->

7832 International Journal of Computer Vision (2025) 133:7817–7854

**Table 7 Struggle Label distribution learning results. Mean Absolute Error (MAE) and Spearman’s Rank Correlation (Spearman’s Rho), along with the converted binary classiﬁcation accuracy and four-way classiﬁcation accuracy.**

Activities Models Label Distribution Learning

MAE↓ Spearman’s Rho↑ Binary Cls.↑ Four-way Cls.↑

Pipes-Struggle TSN (L. Wang et al., 2016) 0.13 0.5120 79.72% 46.01%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.12 0.5614 79.84% 48.79%

VTN-SlowFast-ViT 0.12 0.5689 80.30% 46.47%

MViTv2 (Y. Li et al., 2022) 0.12 0.5331 78.88% 47.72%

Tent-Struggle TSN (L. Wang et al., 2016) 0.17 0.2307 54.04% 28.41%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.14 0.2887 57.07% 31.09%

VTN-SlowFast-ViT 0.13 0.3318 63.29% 34.95%

MViTv2 (Y. Li et al., 2022) 0.13 0.2730 64.58% 37.16%

Tower-Struggle TSN (L. Wang et al., 2016) 0.24 0.4168 71.06% 37.06%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.15 0.4936 74.86% 40.44%

VTN-SlowFast-ViT 0.13 0.5689 77.58% 50.20%

MViTv2 (Y. Li et al., 2022) 0.14 0.5264 80.27% 50.81%

bution so that the value ranges from -1 to 1, with a higher score indicating that the predicted distribution better aligns with its target. We also calculate the Mean Absolute Error (MAE) between the predicted and the target distribution. The converted classiﬁcation accuracy is derived from the predicted voters’ label frequency distribution. To convert the frequency distribution into class labels, we select the top-1 frequency label for four-way classiﬁcation. For binary classiﬁcation, we merge the predicted frequencies of labels 1 and 2 into the “non-struggle" category and labels 3 and 4 into the “struggle" category. We determine the accuracy by comparing the predicted highest-frequency label with the voters’ mode label. The hyperparameter settings remain consistent with those used in previous tasks. Distribution Results The results for label distribution learning are shown in Table 7. Due to the continuous nature of MAE and Spearman’s Rho evaluation metrics, discrepancies can be observed among the implemented baseline models. These discrepancies follow the same trend when converted to accuracy rates. TheMViTv2(Y.Lietal.,2022)modelachievesthehighest binary and four-way classiﬁcation accuracy rates on TentStruggle and Tower-Struggle. However, it does not perform as well in struggle label distribution regression, as indicated by its lower Spearman’s Rho and MAE scores. In contrast, VTN-SlowFast-ViT consistently achieves the best label distribution regression performance, with the lowest MAE and highest Spearman’s Rho across all activities. The converted classiﬁcation accuracy is also among the highest although slightly lower than that of MViTv2 (Y. Li et al., 2022) on Tent-Struggle and Tower-Struggle. Additionally, SlowFastR50 (Feichtenhofer et al., 2019) outperforms TSN (L. Wang

et al., 2016) in both struggle label distribution regression and classiﬁcation, showcasing that 3D-ConvNets is more advanced in extracting spatial-temporal features than the 2DConvNets. VTN-SlowFast-ViT further surpasses SlowFastR50 (Feichtenhofer et al., 2019) in general, suggesting that its additional ViT layer helps better align predictions with the voters’ label distributions.

## 6 Attention Visualization

In this section, we visualize what the models focus on using attention weights and activation maps (Chattopadhyay, Sarkar, Howlader, and Balasubramanian, 2017), ensuring they effectively detect participants’ struggles. Speciﬁcally, we examine activation maps from the backbone network and attention scores from the self-attention layer of the VTNSlowFast-ViT model. Figure 8 presents heatmaps from the ﬁnal convolutional layer of the backbone network, generated using GradCAM++ (Chattopadhyay, Sarkar, Howlader, and Balasubramanian, 2017), along with attention scores visualized as green bars. We provide examples from three activities: PipesStruggle, Tent-Struggle, and Tower-Struggle. The heatmaps reveal that the model predominantly focuses on hands and active objects, though occasionally not robust enough causing attention to drift toward the background. For example, in the Pipes-Struggle visualised frames, the heatmaps highlight the hand of the person holding the pipes as well as the pipes that are lying in the background. Similarly, in the TentStruggle examples, the model focuses on the tent’s poles and the hands of the person attempting to put it up, as shown by the heatmaps. In the Tower-Struggle examples, the heatmaps

<!-- Page 17 -->

International Journal of Computer Vision (2025) 133:7817–7854 7833

**Fig. 8 Visualization results of the activation heatmaps and temporal attention scores. Visualization results for both non-struggle (upper box) and struggle (bottom box) are shown from activities: PipesStruggle (top), Tent-Struggle (middle), and Tower-Struggle (bottom) respectively with each row corresponding to the frames sampled from a video sample that is fed into the slow pathway and down-sampling**

mainly lie on the hands and the Hanoi puzzle blocks. The corresponding green bars below show that the model is giving each frame varying amounts of attention. Since the analysed video clips are limited to 10 seconds and do not capture the full action sequence, the green bars may not align perfectly with explicit actions. However, they still indicate the model’s preference for speciﬁc frames within each sequence.

## 7 Limitations

We acknowledge several limitations and areas for improvement as follows: (1) Our dataset primarily consists of two indoor environments for pipe assembly and the Tower of Hanoi game, along with one outdoor setting for tent assem-

into 8 frames. The heatmaps are the Grad-CAM++ Chattopadhyay et al. (2017) visualization and the green bars below are the colourmap visualizations for the temporal attention scores of the corresponding video segments. The intensity of the green indicates the value of the attention score range from 0 to 1. The closer to 1, the darker the green colour (Color ﬁgure online)

bly. This may limit its generalizability to more diverse backgrounds and is also constrained by the amount of available data. (2) Struggle in our dataset is annotated based on 10-second trimmed video segments, which may limit the abilitytoachieveﬁner-grainedtemporallocalizationandcapture struggle over longer temporal dependencies.

## 8 Discussion and Conclusions

In this paper, we introduced the struggle determination task as an important component in video understanding. We collected a new dataset for struggle determination, comprising three daily activities–Pipes-Struggle, Tent-Struggle, and Tower-Struggle–with a total of 5.09 hours of video record-

<!-- Page 18 -->

7834 International Journal of Computer Vision (2025) 133:7817–7854

ings. Struggle annotations were provided at the video level by both human experts and crowd voters. Our experiments evaluated various deep learning models as baselines for struggle determination. Among the different modelling approaches, struggle classiﬁcation proved to be the most effective. The struggle classiﬁcation results reveal opportunities for improving four-way struggle classiﬁcation, which particularly focuses on ﬁner-grained struggle assessment. The similar accuracy between four-way to binary conversion and binary classiﬁcation suggests that the main challenge lies in distinguishing different degrees of strugglean essential factor for developing reliable assistive systems. We further conducted struggle generalization experiments across the activities showcasing the challenges, and we also compareddifferentmodelstoidentifythemostefﬁcientbackbone model architectures for struggle determination. Our ﬁndings in the ablation study highlight the importance of temporal motion information and the correct frame sequence for struggle determination. Additionally, we report baseline results for alternative training strategies, treating struggle determination as regression and label distribution learning tasks. In conclusion, our research work in this paper lays the foundation for further research in struggle determination from videos. We hope this research provides valuable insights for additional work on ﬁne-grained egocentric video understanding and supports new methods and directions to benchmark assistive systems.

Dataset Access: Our struggle determination dataset can be found via this URL: https://github.com/FELIXFENG2019/ Struggle-Determination.

## Appendix A: Related Datasets

**Table 8 contains a comparison of datasets in video understanding previously used for coarse-grained Action Recognition (AR), ﬁne-grained AR, Action Quality Assessment (AQA), Skill Determination, Video Anomaly Detection (VAD) and Struggle Determination. These datasets are relevanttoourstruggledeterminationtaskasdiscussedinSection 2. Action recognition datasets encompass both coarsegrainedandﬁne-grainedhumanactions.Deepmodelstrained on large-scale action recognition datasets can be transferred to various downstream tasks due to the diverse scenarios and learned features. However, these datasets do not focus on scenes depicting struggling, particularly in the case of coarsegrained AR, which mainly includes common human actions and lacks scenarios for skills. Action quality assessment datasets primarily annotate action quality scores for videos showcasing sports or surgical**

skills. However, lower action quality scores do not necessarily indicate struggling, and well-performed actions can still receive relatively low scores. Skill determination datasets are more relevant to our struggle determination task as they often showcase different skills at different levels with hand-object interactions. However, the annotations in these datasets are in the form of pairwise rankings, indicating which skill is better than the other. Lower-ranked videos in these pairs may not guarantee that a person is struggling. Video anomaly detection datasets mainly consist of surveillance videos capturing unusual events, such as ﬁghts or unintentional falls. These datasets are typically used for binary classiﬁcation tasks to distinguish between anomaly and normal videos. While struggle determination also involves binary classiﬁcation, it falls within the context of determining skills. Moreover, our goal is not only to differentiate struggle from non-struggle but also to determine the degree of struggle, which is crucial for developing a wearable intelligent system to provide appropriate assistance to the user. In comparison, our struggle determination datasets encompass three tasks that demonstrate different types of skills in assembling pipes, pitching tents, and playing the Tower of Hanoi game. These datasets cover indoor and outdoor scenes and include both rigid and non-rigid objects. Additionally, we speciﬁcally use a struggle scoring scale. Thus, the annotations in our dataset are not limited to binary labels but are further ﬁne-grained to indicate the degree of struggle. Aside from the focus on struggle, this distinguishes them from the class labels in AR, action quality scores in AQA, ranking labels in skill determination, and even the binary contrastive labels in VAD. Hence, it is essential to create a dataset with a focus on struggle annotations to train deep models for determining struggle and quantifying its degree.

## Appendix B: Dataset Collection Details

Instructions for Participants

During our data collection process, instructions were given to our in-person participants both verbally and written on paper. For the plumbing pipes task, there are two instruction diagrams illustrating how to assemble the pipes correctly. One version is easy to follow, shown in Figure 9, and another more difﬁcult version, shown in Figure 10 so that the participants using a hard version of instructions are more likely to show signs of struggle. For the tent-pitching task, we used the paper instructions from the manufacturer, as shown in Figure 11. For the Tower of Hanoi game task, the researchers only provided the instructions verbally introducing the basic rules of the game, such as only one disk can be moved at a

<!-- Page 19 -->

International Journal of Computer Vision (2025) 133:7817–7854 7835

Kinetics (Kay et al., 2017) 2017-2020 YouTube Third-person Action recognition 400/600/700 human action classes Up to 650,000 video clips

2015 YouTube Third-person Human activity understanding 200 activities 19,994 untrimmed videos

303 competition records

Diving48 (Y. Li, Li, and Vasconcelos, 2018) 2018 Web videos Third-person Fine-grained diving action recognition 48 dive classes About 18k videos

2011 Movies and online Third-person Human motion recognition 51 action categories 6,766 video clips

108,499 videos

UCF101 (Soomro, Zamir, and Shah, 2012) 2012 YouTube Third-person Action recognition 101 action classes Over 13k clips

Dataset Year Source Viewpoint Tasks Available Annotations Size

FineGym (Shao, Zhao, Dai, and Lin, 2020) 2020 YouTube Third-person Fine-grained action recognition 15 set classes, 530 element classes, 4,883 instances, 32,697 sub-instances

**Table 8 Comparison of related datasets. Note that AR is short for Action Recognition. AQA: Action Quality Assessment. VAD: Video Anomaly Detection.**

174 labels for different types of hand-object interactions

SSV2 (Goyal et al., 2017) 2017 In-person recording First-person Fine-grained understanding of human-object interaction

HMDB51 (Kuehne, Jhuang, Garrote, Poggio, and Serre, 2011)

ActivityNet (Fabian Caba Heilbron and Niebles, 2015)

Coarse-grained AR

Fine-grained AR

3,000 video samples

1,412 video samples

1189 videos

JIGSAW (Gao et al., 2014) 2017 JHU and Sunnyvale, CA. ISI First-person Surgery skill assessment 15 gestures and 5-point Likert scale rating score 206 videos

Olympic Scoring (Parmar & Morris, 2016) 2017 Existing datasets and online Third-person Olympics sports score assessment score ranges from 0 or 20 to 100 696 videos

16 classes of events, 5,748 commentaries, and AQA scores range from 0 to 100

52 action types and 29 sub-action types with temporal boundaries, along with judges’ scores, and 23 difﬁculty degree types.

2019) 2019 YouTube Third-person Action quality assessment across seven sports AQA scores range from 6.72 to 104.88 altogether across seven sports

FineDiving (Xu et al., 2022) 2022 YouTube Third-person Coarse- to ﬁne-grained diving action procedures and AQA

MTL-AQA (Parmar & Tran Morris, 2019) 2019 Online Third-person Multi-task learning including ﬁne-grained action recognition, commentary generation, AQA score estimation

AQA-7 (Parmar & Morris,

AQA

<!-- Page 20 -->

7836 International Journal of Computer Vision (2025) 133:7817–7854

70 videos in Ped 1 and 28 videos in Ped 2

UCF-Crime (Sultani, Chen, and Shah, 2018b) 2018 Surveillance videos Third-person Determine videos with abnormal actions Video-level weak labels for anomalies 1,900 untrimmed videos

196 video samples

437 videos

500 videos

Video pairs with the winning (better) video. 240 videos

Dataset Year Source Viewpoint Tasks Available Annotations Size

2019 In-person recording First-person Pairwise skill ranking including ﬁve tasks Lists of video 1, video 2, and the better video in the pair.

2018 Existing dataset and in-person recording First-person Pairwise skill ranking including four tasks Lists of video 1, video 2, and the better video in the pair.

2013 Surveillance camera Third-person Determine videos with abnormal actions Binary per-frame ﬂag indicator and pixel-level binary masks for abnormal regions

Frame-level masks and pixel-level masks for abnormal events

2023 Participants on TikTok Third-person Pairwise dance skill ranking tasks including 12 dance challenges

ShanghaiTech (W. Liu, W. Luo, and Gao, 2018) 2018 Surveillance camera Third-person Predict future frames and determine abnormal events

TikTok Dance (Hipiny, Ujir, Alias, Shanat, and Ishak,

EPIC-Skill2018 (Doughty, Damen, and Mayol-Cuevas, 2018)

UCSD-Peds (Mahadevan, Li, Bhalodia, and Vasconcelos, 2010)

BEST (Doughty, Mayol-Cuevas, and Damen, 2019)

Skill Determination

**Table 8 continued**

2023)

VAD

1,832 trimmed video segments.

Video-level binary labels for struggle/non-struggle or four-scale struggle levels from 1 to 4.

Ours 2023 Existing dataset and in-person recording First-person Determine struggle and degrees of struggle under three sub-tasks: struggle classiﬁcation, struggle level regression, and struggle label distribution learning

Struggle Determination

<!-- Page 21 -->

International Journal of Computer Vision (2025) 133:7817–7854 7837

**Fig. 9 The diagram of the easy double sink layout.**

**Fig. 10 The diagram of the difﬁcult double sink layout.**

time, and a larger disk cannot be placed on top of a smaller one.

Additional Dataset Statistics

We add Figure 12 to display the struggle labels frequency distributions in the binary case (non-struggle vs struggle). To analyse intra-group voter disagreements, we converted the four-way struggle labels into binary labels and examined thedistributionofdisagreementlevelsamongvoters.Thedisagreement frequency for each video sample was computed by determining the proportion of voters who selected either struggle or non-struggle, relative to the total number of voters. The smaller of these two proportions was designated as

the minority group frequency, representing the level of disagreement among voters. We show the intra-group disagreement in Figure 13 using three histograms to illustrate the distribution of these minority group frequencies across the three activities. The x-axis represents the frequency of the minority group of voters, ranging from 0 to 0.5, while the y-axis indicates the number of video samples. Additionally, a smoothed KDE curve is overlaid to highlight the density of disagreements within the dataset.

<!-- Page 22 -->

7838 International Journal of Computer Vision (2025) 133:7817–7854

**Fig. 11 An OCR photocopy of the assembly instructions for the tent we use.**

**Fig. 12 Comparison of the distributions using binary struggle labels. The three graphs display the frequency distribution of the binary struggle labels (0 for non-struggle, 1 for struggle) including expert**

annotation (EA), voters mode (Mode), and voters labels distributions (Voters), for the three activities: Pipes-Struggle, Tent-Struggle, and Tower-Struggle, respectively (Color ﬁgure online)

<!-- Page 23 -->

International Journal of Computer Vision (2025) 133:7817–7854 7839

**Fig. 13 Intra-group voter disagreements based on the binary struggle labels. The three graphs illustrate the frequency distribution of disagreements after converting the four-way struggle labels into binary**

## Appendix C: Additional Experiment Results

Struggle Modelling Approach Comparison

We also conducted extensive experiments comparing classiﬁcation and regression results across different deep models to assess the consistency of this trend (see Table 9 and 10). The results show that for the TSN (L. Wang et al., 2016) model, there are three instances where classiﬁcation achieves higher four-way accuracy (Pipes-Struggle and Tower-Struggle) and binary accuracy (Pipes-Struggle), while regression-to-classiﬁcation slightly outperforms classiﬁcation in some cases (e.g., binary accuracy for Tower-Struggle and four-way accuracy for Tent-Struggle). However, classiﬁcationremainsthemorestableandeffectiveapproachoverall. Similarly, for the SlowFast-R50 (Feichtenhofer et al., 2019) model, classiﬁcation consistently outperforms regression-toclassiﬁcation across all activities in binary accuracy and, in most cases, for four-way accuracy. These ﬁndings consistently indicate that models trained for classiﬁcation achieve superior performance compared to those trained for regression, reinforcing the advantage of direct classiﬁcation training for struggle detection.

Struggle Classification

We show more details of our struggle classiﬁcation modelling approach about the top-1 accuracy rate for each of the four splits together with the mean and standard deviation (StdDev) over the accuracy of the four splits. Table 12, 13, and 14 show the additional results for binary classiﬁcation, four-way classiﬁcation, and four-way to binary classiﬁcation, respectively. We ﬁrst give the full deﬁnitions of the three naive baselines: random, majority class, and voters’ baseline. Given a dataset D (Pipes-Struggle, Tent-Struggle, or Tower-

labels. Each graph presents a histogram of video sample counts (y-axis) against the frequency of the minority group of voters (x-axis), with a smoothed kernel density estimate (KDE) curve.

Struggle), N is the number of videos. Let Vi denote the ith video. The expert annotation for video Vi is ye i and the frequency distribution of the struggle labels given by the 15 or 20 voters Disti is denoted as Disti = { f1, f2, f3, f4} where f1 is the frequency of the voters label for struggle level 1 (deﬁnitely non-struggle), noted as Disti( f1); f2 is the frequency of the voters label for struggle level 2 (slightly non-struggle), noted as Disti( f2); f3 is the frequency of the voters label for struggle level 3 (slightly struggle), noted as Disti( f3); f4 is the frequency of the voters label for struggle level 4 (deﬁnitely struggle), noted as Disti( f4), while f1 + f2 + f3 + f4 = 1. Let the mode of the voters’ label for video Vi be yl i . yl i is the struggle label index (from 1 to 4 ) of Mode(Disti). Note that in the case of binary classiﬁcation, we combine 1 and 2 to 0 (non-struggle), and 3 and 4 to 1 (struggle) as discussed in the main paper. Moreover, the label distributions over a certain split are calculated by averaging the expert annotation ye i or the voters’ label distributions Disti = { f1, f2, f3, f4} across all the videos Vi within the split, denoted as Dist(y0) and Dist(y1) respectively. In this way, for the random baseline, we assume the random model only has the ability to predict each of the struggle levels in a probability that corresponds to the proportions of theexpertannotationsinthetestsplit Dist(y0),sothatwecan calculate the top-1 accuracy rate each time we run the random model. When the number of runs tends to be a large number close to inﬁnity, the averaged top-1 accuracy rate over the number of runs is approaching a set percentage which is the random baseline accuracy. The majority class baseline is the percentage of the maximum entity in Dist(y0) for a split. Finally, we illustrate the voter’s baseline. Given a video Vi, we deﬁne the Kronecker delta function:

 1 if ye i = yl i 0 if ye i ̸= yl i (1)

δye i yl i =

<!-- Page 24 -->

7840 International Journal of Computer Vision (2025) 133:7817–7854

**Table 9 Comparison of Classiﬁcation and Regression-to-Classiﬁcation Accuracy for TSN (L. Wang et al., 2016).**

Activity Classiﬁcation Regression-to-Classiﬁcation

Binary Accuracy Four-Way Accuracy Binary Accuracy Four-Way Accuracy

Pipes-Struggle 74.54 48.59 72.42 45.02

Tent-Struggle 68.05 38.23 68.27 40.95

Tower-Struggle 82.59 59.49 85.35 58.01

**Table 10 Comparison of Classiﬁcation and Regression-to-Classiﬁcation Accuracy for SlowFast-R50 (Feichtenhofer et al., 2019).**

Activity Classiﬁcation Regression-to-Classiﬁcation

Binary Accuracy Four-Way Accuracy Binary Accuracy Four-Way Accuracy

Pipes-Struggle 78.01 50.77 76.89 45.28

Tent-Struggle 69.56 39.48 69.06 41.63

Tower-Struggle 88.24 63.92 86.89 56.00

**Table 11 Struggle classiﬁcation results with VLM baseline. Classiﬁcation accuracy for binary classiﬁcation, four-way classiﬁcation and the four-way to binary classiﬁcation.**

Dataset Pipes-Struggle Tent-Struggle Tower-Struggle Top-1 Accuracy (%) Top-1 Accuracy (%) Top-1 Accuracy (%)

Models Binary Four-Way Four-to-Bin Binary Four-Way Four-to-Bin Binary Four-Way Four-to-Bin

Random 53.38 28.62 53.38 50.59 27.12 50.59 67.86 40.45 67.86

Majority Class 62.60 39.60 62.60 54.91 35.79 54.91 79.74 57.56 79.74

Voters’ Baseline 80.56 45.31 80.56 72.63 43.12 72.63 84.41 51.86 84.41

Video-LLaVA (Lin et al., 2023) 37.40 14.45 37.40 48.03 24.10 48.03 20.26 12.12 20.26

where ye i is the expert annotation and yl i is the mode of the voters’ labels. So the voters’ baseline accuracy is calculated as:

N j 

1 N j

i=1 δye i yl i × 100% (2)

where the N j is the number of videos in a split j.

Struggle VLM Baselines

We used pre-trained Video-LLaVA (Lin et al., 2023) and evaluated the model’s struggle classiﬁcation performance on our datasets in a zero-shot setting without further ﬁne-tuning. The evaluation results are shown in Table 11. We can see that the Video-LLaVA (Lin et al., 2023) VLM model does not perform very well in both binary and fourway struggle classiﬁcation. The highest binary classiﬁcation accuracy is still lower than the random level (48.03 vs 53.38), and the situation remains the same on four-way struggle classiﬁcation, indicating that the Video-LLaVA (Lin et al., 2023) model struggles to capture struggle-speciﬁc visual cues given text prompts and video input. This is reasonable because the VLM model is not trained or ﬁne-tuned speciﬁcally on

struggle determination tasks, and the struggle is distinct from many video-understanding tasks on which the VLM model has been trained. We designed structured input prompts to guide the VLM in identifying struggles. Below is an example of the input prompt used for the Tent-Struggle dataset: “USER: This video shows a person pitching a camping tent in a random 10-second temporal window. Please focus on the hand’s movements and object status and pay attention to the visual signs that may indicate a struggle, such as motor hesitation (e.g., stopping or placement indecision), getting stuck, prolonged actions, non-smooth movements, frequent pauses, repeated attempts, or visible signs of frustration (e.g., hand or head movements). Describe what you observe before deciding if the person is struggling or not. ASSISTANT: " To further prompt the VLM to classify struggle and struggle level, we add the following text input: “If no struggle indicators are present, state ‘None observed.’ Then, classify the struggle level on a scale from 1 to 4: (1) deﬁnitely non-struggle - smooth, conﬁdent movements; (2) slightly non-struggle - minor hesitation but controlled actions; (3) slightly struggle - noticeable pauses, reattempts, or mild frustration; (4) deﬁnitely struggle - frequent failed attempts, prolonged hesitation, or strong frustration cues. Provide the

<!-- Page 25 -->

International Journal of Computer Vision (2025) 133:7817–7854 7841

**Table 12 Experiment results for struggle binary classiﬁcation over all splits.**

Activities Models Top1 Accuracy Rate (%) - Binary Classiﬁcation

Split 1 Split 2 Split 3 Split 4 Mean± StdDev

EPIC-Pipes Random 55.00% 51.60% 51.93% 54.98% 53.38±1.87%

Majority Class 65.82% 58.95% 59.84% 65.78% 62.60±3.72%

Voters’ Baseline 83.27% 79.91% 80.33% 78.71% 80.56±1.68%

TSN (L. Wang et al., 2016) 75.27% 76.86% 74.18% 71.86% 74.54±2.10%

SlowFast-R50 (Feichtenhofer et al., 2019) 77.09% 75.11% 80.74% 79.09% 78.01±2.44%

SlowFast-gMLP 78.91% 76.42% 80.33% 78.71% 78.59±1.62%

VTN-SlowFast-ViT 77.82% 75.55% 79.51% 78.33% 77.80±1.66%

MViTv2 (Y. Li et al., 2022) 76.73% 73.80% 77.46% 78.71% 76.68±2.08%

EPIC-Tent Random 50.37% 51.02% 50.94% 50.03% 50.59±0.47%

Majority Class 54.35% 57.14% 56.85% 51.30% 54.91±2.71%

Voters’ Baseline 79.71% 73.47% 68.49% 68.83% 72.63±4.54%

TSN (L. Wang et al., 2016) 74.64% 70.07% 66.44% 61.04% 68.05±5.75%

SlowFast-R50 (Feichtenhofer et al., 2019) 76.81% 72.11% 64.38% 64.94% 69.56±5.98%

SlowFast-gMLP 73.19% 73.47% 63.01% 62.34% 68.00±6.16%

VTN-SlowFast-ViT 73.91% 72.79% 65.07% 62.34% 68.53±5.70%

MViTv2 (Y. Li et al., 2022) 68.12% 68.03% 64.38% 57.14% 64.42±5.16%

EPIC-Tower Random 71.88% 65.43% 70.59% 63.53% 67.86±4.01%

Majority Class 83.08% 77.78% 82.09% 76.00% 79.74±3.39%

Voters’ Baseline 81.54% 87.04% 85.07% 84.00% 84.41±1.98%

TSN (L. Wang et al., 2016) 86.15% 79.63% 86.57% 78.00% 82.59±4.41%

SlowFast-R50 (Feichtenhofer et al., 2019) 89.23% 85.18% 92.54% 86.00% 88.24±3.36%

SlowFast-gMLP 89.23% 85.18% 91.04% 86.00% 87.86±2.75%

VTN-SlowFast-ViT 89.23% 85.18% 94.03% 84.00% 88.11±4.54%

MViTv2 (Y. Li et al., 2022) 84.62% 94.44% 82.09% 76.00% 84.29±7.67%

output in this format: Observation: [description] Struggle Indicators: [if any] Struggle Level: [1, 2, 3, or 4].” The text output from the Video-LLaVA (Lin et al., 2023) VLM model reveals that it suffers from hallucination when handling unseen struggle-related data. In many cases, the Assistant’s responses describe events that are not present in the video. This issue likely arises from the model’s tendency to rely more on textual priors than actual visual input. Furthermore, because struggle-related tasks are not explicitly represented in the VLM’s training data and differ from conventional visual tasks, the model may struggle to extract the necessary temporal features for accurately detecting struggle.

Struggle Regression

We provide more experimental results on our struggle level regression modelling approach with the evaluation metrics of Mean Squared Error (MSE), Mean Absolute Error (MAE),

and the top-1 accuracy rate of binary and four-way classiﬁcation converted from the predicted struggle level regression scores. These evaluation metrics of various deep models on the three activities for split 1 to 4 are shown in Tables 15, 16, 17, and 18, respectively. The mean and standard deviation (StdDev) for these evaluation metrics for the four splits are shown in Table 19.

Struggle Label Distribution Learning

We provide more experimental results about voters’ struggle label distribution learning modelling approach with the evaluation metrics of Mean Absolute Error (MAE), Spearman’s Rank Correlation (Spearman’s Rho), and the top-1 accuracy rate of binary and four-way classiﬁcation by selecting the struggle level classes with the highest frequency in the label

<!-- Page 26 -->

7842 International Journal of Computer Vision (2025) 133:7817–7854

**Table 13 Experiment results for struggle four-way classiﬁcation over all splits.**

Activities Models Top1 Accuracy Rate (%) - Four-way Classiﬁcation

Split 1 Split 2 Split 3 Split 4 Mean± StdDev

EPIC-Pipes Random 28.82% 29.65% 27.88% 28.12% 28.62±0.69%

Majority Class 40.36% 41.92% 38.11% 38.02% 39.60±1.63%

Voters’ Baseline 47.27% 46.29% 39.75% 47.91% 45.31±3.26%

TSN (L. Wang et al., 2016) 48.00% 53.71% 45.49% 47.15% 48.59±3.09%

SlowFast-R50 (Feichtenhofer et al., 2019) 53.46% 50.66% 48.77% 50.19% 50.77±1.70%

SlowFast-gMLP 53.46% 48.04% 46.72% 50.57% 49.70±2.58%

VTN-SlowFast-ViT 50.54% 51.53% 48.77% 52.47% 50.83±1.37%

MViTv2 (Y. Li et al., 2022) 48.00% 47.60% 52.87% 45.25% 48.43±3.20%

EPIC-Tent Random 26.71% 27.15% 28.87% 25.73% 27.12±1.14%

Majority Class 35.51% 36.05% 40.41% 31.17% 35.79±3.27%

Voters’ Baseline 50.72% 38.10% 47.95% 35.71% 43.12±6.35%

TSN (L. Wang et al., 2016) 40.58% 39.46% 40.41% 32.47% 38.23±3.35%

SlowFast-R50 (Feichtenhofer et al., 2019) 45.65% 38.10% 39.73% 34.42% 39.48±4.05%

SlowFast-gMLP 41.30% 40.82% 36.99% 38.96% 39.52±1.70%

VTN-SlowFast-ViT 42.75% 40.14% 42.47% 35.06% 40.11±3.08%

MViTv2 (Y. Li et al., 2022) 38.41% 38.78% 42.47% 28.57% 37.06±5.95%

EPIC-Tower Random 42.77% 38.54% 44.71% 35.76% 40.45±3.51%

Majority Class 60.00% 55.56% 62.69% 52.00% 57.56±4.10%

Voters’ Baseline 56.92% 59.26% 49.25% 42.00% 51.86±6.79%

TSN (L. Wang et al., 2016) 63.08% 53.70% 67.16% 54.00% 59.49±5.82%

SlowFast-R50 (Feichtenhofer et al., 2019) 66.15% 64.82% 62.69% 62.00% 63.92±1.66%

SlowFast-gMLP 64.62% 64.82% 70.15% 66.00% 66.40±2.23%

VTN-SlowFast-ViT 67.69% 59.26% 67.16% 62.00% 64.03±3.54%

MViTv2 (Y. Li et al., 2022) 63.08% 64.82% 47.76% 54.00% 57.42±8.00%

distributions and using the voters’ mode label as ground truth. These evaluation metrics of various deep models on the three activities for split 1 to 4 are shown in Tables 20, 21, 22, and 23, respectively. The mean and standard deviation (StdDev) for these evaluation metrics for the four splits are shown in Table 24.

Multiple Frames and Temporal Ordering

We provide more experiment results of the ablation study we conducted using one frame as input and shufﬂed frame orders, see Table 25 and Table 26.

## Appendix D: igures and Tables

<!-- Page 27 -->

International Journal of Computer Vision (2025) 133:7817–7854 7843

**Table 14 Experiment results of four-way classiﬁcation convert into binary classiﬁcation over all splits.**

Activities Models Top1 Accuracy Rate (%) - Four-way to Binary

Split 1 Split 2 Split 3 Split 4 Mean± StdDev

EPIC-Pipes Random 55.00% 51.60% 51.93% 54.98% 53.38±1.62%

Majority Class 65.82% 58.95% 59.84% 65.78% 62.60±3.22%

Voters’ Baseline 83.27% 79.91% 80.33% 78.71% 80.56±1.68%

TSN (L. Wang et al., 2016) 73.82% 71.52% 69.67% 70.72% 71.43±1.53%

SlowFast-R50 (Feichtenhofer et al., 2019) 78.18% 73.36% 74.18% 77.19% 75.73±2.01%

SlowFast-gMLP 78.18% 72.93% 74.59% 72.24% 74.49±2.30%

VTN-SlowFast-ViT 72.73% 71.18% 73.77% 72.24% 72.48±0.93%

MViTv2 (Y. Li et al., 2022) 70.91% 68.12% 75.82% 74.14% 72.25±3.42%

EPIC-Tent Random 50.37% 51.02% 50.94% 50.03% 50.59±0.41%

Majority Class 54.35% 57.14% 56.85% 51.30% 54.91±2.35%

Voters’ Baseline 79.71% 73.47% 68.49% 68.83% 72.63±4.54%

TSN (L. Wang et al., 2016) 67.39% 63.95% 62.33% 51.30% 61.24±6.02%

SlowFast-R50 (Feichtenhofer et al., 2019) 72.46% 70.75% 63.01% 54.55% 65.19±7.10%

SlowFast-gMLP 67.39% 72.11% 57.53% 61.69% 64.68±5.54%

VTN-SlowFast-ViT 68.84% 63.27% 55.48% 55.84% 60.86±5.56%

MViTv2 (Y. Li et al., 2022) 56.52% 61.90% 60.27% 53.90% 58.15±3.62%

EPIC-Tower Random 71.88% 65.43% 70.59% 63.53% 67.86±3.47%

Majority Class 83.08% 77.78% 82.09% 76.00% 79.74±2.94%

Voters’ Baseline 81.54% 87.04% 85.07% 84.00% 84.41±1.98%

TSN (L. Wang et al., 2016) 83.08% 74.07% 83.58% 74.00% 78.68±4.65%

SlowFast-R50 (Feichtenhofer et al., 2019) 84.62% 70.37% 82.09% 84.00% 80.27±5.79%

SlowFast-gMLP 87.69% 75.93% 85.07% 84.00% 83.17±4.39%

VTN-SlowFast-ViT 86.15% 75.93% 88.06% 80.00% 82.54±4.84%

MViTv2 (Y. Li et al., 2022) 81.54% 90.74% 82.09% 76.00% 82.59±6.09%

**Table 15 Experiment results for struggle regression—split 1.**

Activities Models Regression & Classiﬁcation - Split 1

MSE↓ MAE↓ Binary Acc.↑ Four-way Acc.↑

EPIC-Pipes TSN (L. Wang et al., 2016) 0.83 0.72 72.36% 45.82%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.64 0.67 77.82% 43.27%

VTN-SlowFast-ViT 0.68 0.66 77.45% 48.36%

MViTv2 (Y. Li et al., 2022) 0.75 0.73 75.27% 42.54%

EPIC-Tent TSN (L. Wang et al., 2016) 0.82 0.74 69.57% 42.03%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.75 0.72 73.91% 42.03%

VTN-SlowFast-ViT 0.78 0.74 68.84% 41.30%

MViTv2 (Y. Li et al., 2022) 0.99 0.85 61.59% 35.51%

EPIC-Tower TSN (L. Wang et al., 2016) 0.75 0.68 86.15% 58.46%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.77 0.70 87.69% 58.46%

VTN-SlowFast-ViT 0.70 0.67 87.69% 60.00%

MViTv2 (Y. Li et al., 2022) 0.82 0.78 86.15% 30.77%

<!-- Page 28 -->

7844 International Journal of Computer Vision (2025) 133:7817–7854

**Table 16 Experiment results for struggle regression—split 2.**

Activities Models Regression & Classiﬁcation - Split 2

MSE↓ MAE↓ Binary Acc.↑ Four-way Acc.↑

EPIC-Pipes TSN (L. Wang et al., 2016) 0.64 0.65 76.42% 50.22%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.60 0.62 73.46% 48.47%

VTN-SlowFast-ViT 0.67 0.65 74.24% 49.78%

MViTv2 (Y. Li et al., 2022) 0.68 0.67 73.80% 48.47%

EPIC-Tent TSN (L. Wang et al., 2016) 0.94 0.78 73.47% 42.86%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.79 0.72 72.79% 45.58%

VTN-SlowFast-ViT 0.85 0.76 68.71% 39.46%

MViTv2 (Y. Li et al., 2022) 0.94 0.81 62.58% 37.42%

EPIC-Tower TSN (L. Wang et al., 2016) 0.93 0.76 85.19% 57.41%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.78 0.69 83.33% 51.85%

VTN-SlowFast-ViT 0.88 0.72 85.19% 59.26%

MViTv2 (Y. Li et al., 2022) 0.76 0.73 88.89% 37.04%

**Table 17 Experiment results for struggle regression—split 3.**

Activities Models Regression & Classiﬁcation - Split 3

MSE↓ MAE↓ Binary Acc.↑ Four-way Acc.↑

EPIC-Pipes TSN (L. Wang et al., 2016) 0.83 0.74 71.31% 42.21%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.61 0.63 78.69% 47.95%

VTN-SlowFast-ViT 0.56 0.61 80.33% 51.23%

MViTv2 (Y. Li et al., 2022) 0.73 0.68 75.00% 47.54%

EPIC-Tent TSN (L. Wang et al., 2016) 0.87 0.76 65.75% 43.84%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.82 0.75 68.49% 43.84%

VTN-SlowFast-ViT 0.87 0.77 63.70% 41.10%

MViTv2 (Y. Li et al., 2022) 0.96 0.81 56.16% 36.99%

EPIC-Tower TSN (L. Wang et al., 2016) 0.87 0.71 88.06% 64.18%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.49 0.54 92.54% 65.67%

VTN-SlowFast-ViT 0.58 0.62 92.54% 71.64%

MViTv2 (Y. Li et al., 2022) 1.19 0.91 76.12% 35.82%

<!-- Page 29 -->

International Journal of Computer Vision (2025) 133:7817–7854 7845

**Table 18 Experiment results for struggle regression—split 4.**

Activities Models Regression & Classiﬁcation - Split 4

MSE↓ MAE↓ Binary Acc.↑ Four-way Acc.↑

EPIC-Pipes TSN (L. Wang et al., 2016) 0.89 0.79 69.58% 41.83%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.73 0.70 77.57% 41.44%

VTN-SlowFast-ViT 0.68 0.68 77.19% 44.87%

MViTv2 (Y. Li et al., 2022) 0.82 0.75 76.05% 40.30%

EPIC-Tent TSN (L. Wang et al., 2016) 0.92 0.79 64.29% 35.06%

SlowFast-R50 (Feichtenhofer et al., 2019) 1.04 0.85 61.04% 35.06%

VTN-SlowFast-ViT 0.97 0.81 64.29% 40.91%

MViTv2 (Y. Li et al., 2022) 1.33 0.96 59.09% 32.47%

EPIC-Tower TSN (L. Wang et al., 2016) 1.19 0.84 82.00% 52.00%

SlowFast-R50 (Feichtenhofer et al., 2019) 1.13 0.83 84.00% 48.00%

VTN-SlowFast-ViT 0.73 0.68 86.00% 56.00%

MViTv2 (Y. Li et al., 2022) 1.05 0.89 74.00% 30.00%

**Table 19 Experiment results for struggle regression—results across all four splits including mean values and standard deviation (StdDev).**

Activities Models Regression & Classiﬁcation - Statistics (Mean± StdDev)

MSE↓ MAE↓ Binary Acc.↑ Four-way Acc.↑

EPIC-Pipes TSN (L. Wang et al., 2016) 0.798±0.094 0.725±0.050 72.42±2.52% 45.02±3.38%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.645±0.051 0.655±0.032 76.89±2.02% 45.28±3.00%

VTN-SlowFast-ViT 0.648±0.051 0.650±0.025 77.30±2.16% 48.56±2.36%

MViTv2 (Y. Li et al., 2022) 0.745±0.058 0.708±0.039 75.03±0.93% 44.71±3.93%

EPIC-Tent TSN (L. Wang et al., 2016) 0.888±0.047 0.768±0.019 68.27±3.57% 40.95±3.46%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.850±0.112 0.760±0.053 69.06±5.05% 41.63±3.99%

VTN-SlowFast-ViT 0.868±0.068 0.770±0.025 66.39±2.40% 40.69±0.72%

MViTv2 (Y. Li et al., 2022) 1.055±0.184 0.858±0.071 59.86±2.87% 35.60±2.24%

EPIC-Tower TSN (L. Wang et al., 2016) 0.935±0.161 0.748±0.061 85.35±2.19% 58.01±4.32%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.793±0.227 0.690±0.103 86.89±3.66% 56.00±6.72%

VTN-SlowFast-ViT 0.723±0.107 0.673±0.036 87.86±2.85% 61.73±5.92%

MViTv2 (Y. Li et al., 2022) 0.955±0.200 0.828±0.087 81.29±7.33% 33.41±3.54%

<!-- Page 30 -->

7846 International Journal of Computer Vision (2025) 133:7817–7854

**Table 20 Struggle label distribution learning experiment results on split 1.**

Activities Models Label Distribution Learning - Split 1

MAE↓ Spearman’s Rho↑ Binary Cls.↑ Four-way Cls.↑

EPIC-Pipes TSN (L. Wang et al., 2016) 0.12 0.4922 80.00% 39.27%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.11 0.5484 80.00% 44.73%

VTN-SlowFast-ViT 0.11 0.5316 78.55% 38.55%

MViTv2 (Y. Li et al., 2022) 0.12 0.4876 78.18% 39.64%

EPIC-Tent TSN (L. Wang et al., 2016) 0.16 0.2743 52.17% 29.71%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.13 0.3658 71.74% 37.68%

VTN-SlowFast-ViT 0.13 0.3785 64.49% 36.96%

MViTv2 (Y. Li et al., 2022) 0.13 0.3302 69.56% 40.58%

EPIC-Tower TSN (L. Wang et al., 2016) 0.30 0.4881 73.85% 35.38%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.14 0.4938 73.85% 30.77%

VTN-SlowFast-ViT 0.13 0.5709 76.92% 44.62%

MViTv2 (Y. Li et al., 2022) 0.12 0.5535 76.92% 47.69%

**Table 21 Struggle label distribution learning experiment results on split 2.**

Activities Models Label Distribution Learning - Split 2

MAE↓ Spearman’s Rho↑ Binary Cls.↑ Four-way Cls.↑

EPIC-Pipes TSN (L. Wang et al., 2016) 0.13 0.4771 78.17% 41.92%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.12 0.5012 77.73% 45.41%

VTN-SlowFast-ViT 0.12 0.5113 78.17% 42.79%

MViTv2 (Y. Li et al., 2022) 0.12 0.5047 77.73% 44.98%

EPIC-Tent TSN (L. Wang et al., 2016) 0.18 0.1983 50.34% 21.09%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.14 0.2048 53.74% 27.21%

VTN-SlowFast-ViT 0.13 0.2941 63.95% 38.10%

MViTv2 (Y. Li et al., 2022) 0.14 0.2294 61.22% 34.69%

EPIC-Tower TSN (L. Wang et al., 2016) 0.17 0.4137 72.22% 37.04%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.14 0.5532 75.93% 48.15%

VTN-SlowFast-ViT 0.12 0.5879 77.78% 57.41%

MViTv2 (Y. Li et al., 2022) 0.12 0.6738 87.04% 59.26%

<!-- Page 31 -->

International Journal of Computer Vision (2025) 133:7817–7854 7847

**Table 22 Struggle label distribution learning experiment results on split 3.**

Activities Models Label Distribution Learning - Split 3

MAE↓ Spearman’s Rho↑ Binary Cls.↑ Four-way Cls.↑

EPIC-Pipes TSN (L. Wang et al., 2016) 0.12 0.5605 82.38% 55.33%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.13 0.5940 79.51% 53.69%

VTN-SlowFast-ViT 0.12 0.6202 81.97% 52.05%

MViTv2 (Y. Li et al., 2022) 0.13 0.5533 77.46% 54.92%

EPIC-Tent TSN (L. Wang et al., 2016) 0.17 0.2733 63.01% 34.93%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.13 0.3282 54.11% 32.19%

VTN-SlowFast-ViT 0.13 0.3536 63.01% 33.56%

MViTv2 (Y. Li et al., 2022) 0.13 0.3449 67.81% 36.99%

EPIC-Tower TSN (L. Wang et al., 2016) 0.24 0.4273 70.15% 35.82%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.15 0.4739 71.64% 32.84%

VTN-SlowFast-ViT 0.13 0.4985 77.61% 44.78%

MViTv2 (Y. Li et al., 2022) 0.16 0.3775 73.13% 40.30%

**Table 23 Struggle label distribution learning experiment results on split 4.**

Activities Models Label Distribution Learning - Split 4

MAE↓ Spearman’s Rho↑ Binary Cls.↑ Four-way Cls.↑

EPIC-Pipes TSN (L. Wang et al., 2016) 0.13 0.5181 78.33% 47.53%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.12 0.6020 82.13% 51.33%

VTN-SlowFast-ViT 0.12 0.6125 82.51% 52.47%

MViTv2 (Y. Li et al., 2022) 0.12 0.5866 82.13% 51.33%

EPIC-Tent TSN (L. Wang et al., 2016) 0.17 0.1769 50.65% 27.92%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.14 0.2560 48.70% 27.27%

VTN-SlowFast-ViT 0.14 0.3011 61.69% 31.17%

MViTv2 (Y. Li et al., 2022) 0.14 0.1873 59.74% 36.36%

EPIC-Tower TSN (L. Wang et al., 2016) 0.24 0.3382 68.00% 40.00%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.15 0.4534 78.00% 50.00%

VTN-SlowFast-ViT 0.12 0.6183 78.00% 54.00%

MViTv2 (Y. Li et al., 2022) 0.13 0.5008 84.00% 56.00%

<!-- Page 32 -->

7848 International Journal of Computer Vision (2025) 133:7817–7854

**Table 24 Struggle label distribution learning experiment results—results across all the four splits including mean values and standard deviation (StdDev).**

Activities Models Label Distribution Learning - Statistics (Mean± StdDev)

MAE↓ Spearman’s Rho↑ Binary Cls.↑ Four-way Cls.↑

EPIC-Pipes TSN (L. Wang et al., 2016) 0.13±0.01 0.5120±0.0365 79.72±1.96% 46.01±7.10%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.12±0.01 0.5614±0.0466 79.84±1.81% 48.79±4.41%

VTN-SlowFast-ViT 0.12±0.01 0.5689±0.0555 80.30±2.26% 46.47±6.91%

MViTv2 (Y. Li et al., 2022) 0.12±0.00 0.5331±0.0453 78.88±2.19% 47.72±6.77%

EPIC-Tent TSN (L. Wang et al., 2016) 0.17±0.01 0.2307±0.0505 54.04±6.03% 28.41±5.72%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.14±0.01 0.2887±0.0721 57.07±10.08% 31.09±4.98%

VTN-SlowFast-ViT 0.13±0.01 0.3318±0.0409 63.29±1.23% 34.95±3.17%

MViTv2 (Y. Li et al., 2022) 0.13±0.00 0.2730±0.0768 64.58±4.83% 37.16±2.48%

EPIC-Tower TSN (L. Wang et al., 2016) 0.24±0.05 0.4168±0.0616 71.06±2.54% 37.06±2.08%

SlowFast-R50 (Feichtenhofer et al., 2019) 0.15±0.01 0.4936±0.0430 74.86±2.73% 40.44±10.04%

VTN-SlowFast-ViT 0.13±0.01 0.5689±0.0509 77.58±0.47% 50.20±6.50%

MViTv2 (Y. Li et al., 2022) 0.14±0.02 0.5264±0.1229 80.27±6.38% 50.81±8.53%

<!-- Page 33 -->

International Journal of Computer Vision (2025) 133:7817–7854 7849

Only Test Train & Test  Mean Only Test Train & Test  Mean

**Table 25 Experiment results using one frame as input. The original experiment results for the classiﬁcation task compared with the one-frame experiment results of training and testing the models using only one frame. We report the results by calculating the mean and standard deviation (StdDev) over the four splits.  Mean = Mean(train&test) - Mean(test only)**

One-frame test 50% 3.72% 62.60% 3.68% 67.74% 5.14% 7.46% 31.73% 2.69% 41.87% 10.15%

One-frame test 75% 3.72% 62.60% 2.40% 65.24% 2.64% 6.40% 29.62% 1.99% 40.74% 11.12%

One-frame test 75% 7.45% 62.31% 2.95% 63.36% 1.05% 3.55% 40.36% 1.89% 39.60% -0.75%

One-frame test 25% 5.38% 63.24% 3.03% 63.25% 0.01% 3.90% 38.58% 1.85% 39.51% 0.94%

One-frame test 50% 4.39% 62.25% 3.11% 63.66% 1.41% 3.23% 38.67% 2.02% 39.79% 1.11%

One-frame test 25% 3.72% 62.60% 3.24% 64.92% 2.32% 6.99% 31.34% 2.14% 38.84% 7.50%

One-frame test 25% 2.99% 64.60% 2.65% 67.18% 2.58% 3.87% 38.55% 3.00% 42.10% 3.55%

One-frame test 50% 2.80% 67.10% 3.35% 69.07% 1.97% 2.95% 40.85% 3.57% 42.31% 1.46%

One-frame test 75% 3.79% 67.39% 4.63% 70.19% 2.81% 3.49% 41.73% 3.58% 42.33% 0.61%

SlowFast-R50 (Feichtenhofer et al., 2019) 2.44% 78.01% 2.44% 78.01% -% 1.97% 50.77% 1.97% 50.77% -%

Pipes-Struggle TSN (L. Wang et al., 2016) 2.10% 74.54% 2.10% 74.54% -% 3.57% 48.59% 3.57% 48.59% -%

SlowFast-gMLP 1.62% 78.59% 1.62% 78.59% -% 2.97% 49.70% 2.97% 49.70% -%

VTN-SlowFast-ViT 1.66% 77.80% 1.66% 77.80% -% 1.58% 50.83% 1.58% 50.83% -%

StdDev Mean StdDev Mean StdDev Mean StdDev Mean

Binary Classiﬁcation Four-way Classiﬁcation

Activities Models Top1 Accuracy Rate (%)

One-frame test 50% 2.69% 55.79% 4.03% 58.22% 2.43% 5.64% 29.20% 3.82% 36.32% 7.12%

One-frame test 75% 2.43% 55.07% 4.00% 56.82% 1.75% 4.24% 29.33% 4.59% 35.27% 5.93%

One-frame test 25% 3.37% 52.38% 2.99% 55.64% 3.26% 8.21% 31.66% 3.07% 39.04% 7.38%

One-frame test 50% 7.84% 52.97% 4.63% 57.53% 4.57% 5.59% 33.11% 4.21% 34.48% 1.37%

One-frame test 25% 2.84% 54.76% 3.73% 56.53% 1.77% 6.31% 33.06% 4.51% 37.16% 4.10%

One-frame test 25% 6.09% 58.49% 3.06% 59.58% 1.10% 5.46% 31.77% 4.48% 37.86% 6.09%

One-frame test 75% 3.33% 58.72% 5.32% 61.84% 3.12% 5.50% 33.59% 4.07% 35.79% 2.20%

One-frame test 25% 3.83% 62.89% 3.27% 63.98% 1.09% 4.26% 34.10% 1.97% 39.82% 5.72%

One-frame test 50% 3.73% 62.79% 3.04% 63.77% 0.98% 4.58% 35.36% 2.42% 39.69% 4.33%

One-frame test 75% 3.73% 62.21% 2.75% 63.66% 1.45% 4.87% 34.14% 1.84% 40.24% 6.10%

One-frame test 25% 3.08% 55.82% 2.64% 55.24% -0.57% 3.28% 29.67% 4.73% 38.02% 8.35%

One-frame test 75% 2.99% 53.36% 5.69% 52.94% -0.42% 5.87% 32.47% 2.89% 33.00% 0.53%

One-frame test 50% 3.03% 55.84% 2.71% 55.27% -0.56% 7.26% 33.45% 4.07% 35.47% 2.02%

One-frame test 75% 3.07% 55.13% 3.50% 54.34% -0.79% 3.32% 35.69% 3.79% 35.95% 0.25%

One-frame test 50% 3.74% 61.88% 6.34% 61.07% -0.81% 5.07% 31.66% 4.38% 36.81% 5.15%

SlowFast-R50 (Feichtenhofer et al., 2019) 5.98% 69.56% 5.98% 69.56% 0.00% 4.68% 39.48% 4.68% 39.48% -%

Tent-Struggle TSN (L. Wang et al., 2016) 5.75% 68.05% 5.75% 68.05% -% 3.87% 38.23% 3.87% 38.23% -%

SlowFast-gMLP 6.16% 68.00% 6.16% 68.00% -% 1.96% 39.52% 1.96% 39.52% -%

VTN-SlowFast-ViT 5.70% 68.53% 5.70% 68.53% -% 3.56% 40.11% 3.56% 40.11% -%

<!-- Page 34 -->

7850 International Journal of Computer Vision (2025) 133:7817–7854

Only Test Train & Test  Mean Only Test Train & Test  Mean

One-frame test 25% 3.45% 77.61% 4.28% 81.89% 4.28% 6.29% 55.34% 4.73% 57.56% 2.23%

One-frame test 50% 4.29% 80.48% 4.21% 81.98% 1.49% 4.43% 55.54% 4.73% 57.56% 2.02%

One-frame test 75% 3.01% 78.99% 3.25% 80.74% 1.75% 2.73% 55.78% 4.73% 57.56% 1.79%

One-frame test 25% 3.39% 79.74% 3.39% 79.74% 0.00% 4.73% 57.56% 4.60% 58.44% 0.87%

One-frame test 50% 3.39% 79.74% 3.39% 79.74% 0.00% 4.73% 57.56% 4.00% 58.06% 0.50%

One-frame test 75% 3.39% 79.74% 3.39% 79.74% 0.00% 4.73% 57.56% 5.23% 58.81% 1.25%

One-frame test 25% 2.94% 79.35% 3.39% 79.74% 0.38% 4.73% 57.56% 4.73% 57.56% 0.00%

One-frame test 25% 6.40% 77.05% 3.39% 79.74% 2.69% 4.85% 54.50% 4.73% 57.56% 3.07%

One-frame test 50% 5.85% 73.66% 3.39% 79.74% 6.07% 7.02% 51.95% 4.73% 57.56% 5.61%

One-frame test 75% 3.65% 72.64% 3.39% 79.74% 7.10% 6.31% 51.50% 4.73% 57.56% 6.07%

SlowFast-R50 (Feichtenhofer et al., 2019) 3.36% 88.24% 3.36% 88.24% -% 1.91% 63.92% 1.91% 63.92% -%

Tower-Struggle TSN (L. Wang et al., 2016) 4.41% 82.59% 4.41% 82.59% -% 6.72% 59.49% 6.72% 59.49% -%

SlowFast-gMLP 2.75% 87.86% 2.75% 87.86% -% 2.57% 66.40% 2.57% 66.40% -%

VTN-SlowFast-ViT 4.54% 88.11% 4.54% 88.11% -% 4.09% 64.03% 4.09% 64.03% -%

StdDev Mean StdDev Mean StdDev Mean StdDev Mean

Binary Classiﬁcation Four-way Classiﬁcation

Activities Models Top1 Accuracy Rate (%)

**Table 25 continued**

One-frame test 50% 2.94% 79.35% 3.39% 79.74% 0.38% 4.73% 57.56% 4.73% 57.56% 0.00%

One-frame test 75% 3.39% 79.74% 3.39% 79.74% 0.00% 4.73% 57.56% 4.73% 57.56% 0.00%

<!-- Page 35 -->

International Journal of Computer Vision (2025) 133:7817–7854 7851

**Table 26 Experiment results using shufﬂed frames. Mean Absolute Error (MAE) and Spearman’s Rank Correlation (Spearman’s Rho) with the corresponding converted classiﬁcation accuracy for binary classiﬁcation and four-way classiﬁcation**

Shufﬂe Train&Test 75.64% 74.67% 76.23% 78.33% 1.55% 76.22% 48.36% 42.36% 42.62% 49.05% 3.60% 45.60%

Shufﬂe Train&Test 74.91% 74.24% 75.92% 74.90% 0.69% 74.99% 47.27% 44.98% 46.72% 50.95% 2.51% 47.48%

Shufﬂe Train&Test 73.46% 70.74% 73.77% 78.33% 3.15% 74.08% 46.18% 33.19% 40.98% 44.49% 5.77% 41.21%

SlowFast-gMLP 78.91% 76.42% 80.33% 78.71% 1.62% 78.59% 53.46% 48.04% 46.72% 50.57% 2.97% 49.70%

Shufﬂe frames test 64.22% 46.46% 60.25% 47.53% 8.96% 54.62% 24.36% 27.60% 30.08% 28.52% 2.41% 27.64%

Shufﬂe frames test 41.24% 45.15% 62.62% 47.30% 9.37% 49.08% 28.51% 22.97% 29.67% 22.66% 3.66% 25.95%

Shufﬂe frames test 45.09% 53.97% 63.11% 43.42% 9.08% 51.40% 23.27% 32.40% 32.30% 24.18% 4.99% 28.04%

Shufﬂe frames test 50.00% 42.99% 43.56% 48.70% 3.55% 46.31% 22.32% 19.86% 22.60% 26.23% 2.62% 22.75%

VTN-SlowFast-ViT 77.82% 75.55% 79.51% 78.33% 1.66% 77.80% 50.54% 51.53% 48.77% 52.47% 1.58% 50.83%

Pipes-Struggle SlowFast-R50 (Feichtenhofer et al., 2019) 77.09% 75.11% 80.74% 79.09% 2.44% 78.01% 53.46% 50.66% 48.77% 50.19% 1.97% 50.77%

Tent-Struggle SlowFast-R50 (Feichtenhofer et al., 2019) 76.81% 72.11% 64.38% 64.94% 5.98% 69.56% 45.65% 38.10% 39.73% 34.42% 4.68% 39.48%

Split 1 Split 2 Split 3 Split 4 StdDev Mean Split 1 Split 2 Split 3 Split 4 StdDev Mean

Binary Classiﬁcation Four-way Classiﬁcation

Activities Models Top1 Accuracy Rate (%)

Shufﬂe Train&Test 81.54% 75.93% 82.09% 66.00% 7.46% 76.39% 63.08% 48.15% 65.67% 44.00% 10.75% 55.23%

Shufﬂe Train&Test 73.19% 75.51% 65.75% 57.14% 8.29% 67.90% 39.13% 39.46% 36.30% 31.17% 3.84% 36.52%

Shufﬂe Train&Test 68.84% 74.15% 65.07% 61.69% 5.34% 67.44% 36.96% 36.74% 31.51% 33.12% 2.70% 34.58%

Shufﬂe Train&Test 68.84% 72.79% 63.01% 59.09% 6.08% 65.93% 41.30% 38.10% 30.14% 28.57% 6.15% 34.53%

Shufﬂe Train&Test 81.54% 77.78% 83.58% 82.00% 2.46% 81.23% 63.08% 50.00% 55.22% 62.00% 6.13% 57.58%

Shufﬂe Train&Test 87.69% 77.78% 86.57% 84.00% 4.43% 84.01% 60.00% 55.56% 62.69% 58.00% 3.02% 59.06%

SlowFast-gMLP 73.19% 73.47% 63.01% 62.34% 6.16% 68.00% 41.30% 40.82% 36.99% 38.96% 1.96% 39.52%

SlowFast-gMLP 89.23% 85.18% 91.04% 86.00% 2.75% 87.86% 64.62% 64.82% 70.15% 66.00% 2.57% 66.40%

Shufﬂe frames test 32.00% 39.63% 49.25% 72.40% 17.54% 48.32% 21.54% 21.48% 42.99% 27.20% 10.15% 28.30%

Shufﬂe frames test 47.08% 42.96% 64.18% 64.00% 11.14% 54.56% 28.92% 22.59% 46.57% 36.40% 10.31% 33.62%

Shufﬂe frames test 49.54% 42.22% 62.69% 66.40% 11.29% 55.21% 26.46% 19.63% 40.60% 38.00% 9.85% 31.17%

Shufﬂe frames test 51.16% 45.03% 45.48% 49.74% 3.06% 47.85% 27.97% 29.25% 21.92% 24.42% 3.34% 25.89%

Shufﬂe frames test 51.88% 42.99% 44.52% 48.70% 4.04% 47.02% 28.70% 29.93% 25.89% 24.68% 2.43% 27.30%

VTN-SlowFast-ViT 73.91% 72.79% 65.07% 62.34% 5.70% 68.53% 42.75% 40.14% 42.47% 35.06% 3.56% 40.11%

VTN-SlowFast-ViT 89.23% 85.18% 94.03% 84.00% 4.54% 88.11% 67.69% 59.26% 67.16% 62.00% 4.09% 64.03%

Tower-Struggle SlowFast-R50 (Feichtenhofer et al., 2019) 89.23% 85.18% 92.54% 86.00% 3.36% 88.24% 66.15% 64.82% 62.69% 62.00% 1.91% 63.92%

<!-- Page 36 -->

7852 International Journal of Computer Vision (2025) 133:7817–7854

Acknowledgements We are grateful to Prof. Dima Damen’s contributions to early discussions and involvement in the initial investigation of this work. Shijia Feng is supported by a scholarship from the China Scholarship Council (No.202109210007). Initial research, data capture and annotation supported by EPSRC grant GLANCE (EP/N013964/1).

Open Access This article is licensed under a Creative Commons Attribution 4.0 International License, which permits use, sharing, adaptation, distribution and reproduction in any medium or format, as long as you give appropriate credit to the original author(s) and the source, provide a link to the Creative Commons licence, and indicate if changes were made. The images or other third party material in this article are included in the article’s Creative Commons licence, unless indicated otherwise in a credit line to the material. If material is not included in the article’s Creative Commons licence and your intended use is not permitted by statutory regulation or exceeds the permitteduse,youwillneedtoobtainpermissiondirectlyfromthecopyright holder. To view a copy of this licence, visit http://creativecomm ons.org/licenses/by/4.0/.

## References

Arnab, A., Dehghani, M., Heigold, G., Sun, C., Lucic, M., & Schmid, C. (2021). Vivit: A video vision transformer. arXiv. https://arxiv. org/abs/2103.15691 Athiwaratkun, B., Finzi, M., Izmailov, P., & Wilson, A.G. (2019). There are many consistent explanations of unlabeled data: Why you should average. Bertasius, G., Wang, H., & Torresani, L. (2021). Is space-time attention all you need for video understanding? CoRR, abs/2102.05095,

https://arxiv.org/abs/2102.05095 Carreira, J., & Zisserman, A. (2017). Quo vadis, action recognition? a new model and the kinetics dataset. proceedings of the ieee conference on computer vision and pattern recognition (pp. 6299–6308). Chattopadhyay, A., Sarkar, A., Howlader, P., & Balasubramanian, V.N. (2017). Grad-cam++: Generalized gradient-based visual explanations for deep convolutional networks. CoRR, abs/1710.11063, , http://arxiv.org/abs/1710.11063 Chen, S., Sun, P., Xie, E., Ge, C., Wu, J., Ma, L., & Luo, P. (2021). Watchonlyonce:Anend-to-endvideoactiondetectionframework. Proceedings of the ieee/cvf international conference on computer vision (iccv) (p.8178-8187). Damen, D., Doughty, H., Farinella, G.M., Fidler, S., Furnari, A., Kazakos, E., & Wray, M. (2018). Scaling egocentric vision: The epic-kitchens dataset. European conference on computer vision (eccv). Dosovitskiy, A., Beyer, L., Kolesnikov, A., Weissenborn, D., Zhai, X., Unterthiner, T., & Houlsby, N. (2020). An image is worth 16x16 words: Transformers for image recognition at scale. arXiv. https:// arxiv.org/abs/2010.11929 Doughty, H., Damen, D., & Mayol-Cuevas, W. (2018). Who’s Better? Who’s Bst? The ieee conference on computer vision and pattern recognition (cvpr): Pairwise Deep Ranking for Skill Determination. Doughty, H., Mayol-Cuevas, W., & Damen, D. (2019). The Pros and Cons: Rank-aware Temporal Attention for Skill Determination in Long Videos. The ieee conference on computer vision and pattern recognition (cvpr). Duka, E., Kukleva, A., & Schiele, B. (2022). Leveraging self-supervised training for unintentional action recognition. arXiv. https://arxiv. org/abs/2209.11870 Fabian Caba Heilbron, B.G., Victor Escorcia, & Niebles, J.C. (2015). Activitynet: A large-scale video benchmark for human activity

understanding. Proceedings of the ieee conference on computer vision and pattern recognition (pp. 961–970). Fan, H., Li, Y., Xiong, B., Lo, W.Y., & Feichtenhofer, C. (2020). Pyslowfast. (https://github.com/facebookresearch/slowfast. Last accessed on 24-05-2023) Fan, H., Xiong, B., Mangalam, K., Li, Y., Yan, Z., Malik, J., & Feichtenhofer, C. (2021). Multiscale vision transformers. Proceedings of the ieee/cvf international conference on computer vision (iccv) (p.6824-6835). Feichtenhofer, C. (2020). X3D: expanding architectures for efﬁcient video recognition. CoRR, abs/2004.04730, , https://arxiv.org/abs/ 2004.04730 Feichtenhofer, C., Fan, H., Malik, J., & He, K. (2019). Slowfast networks for video recognition. Proceedings of the ieee/cvf international conference on computer vision (pp. 6202–6211). Flaborea,A.,diMelendugno,G.M.D.,Plini,L.,Scofano,L.,DeMatteis, E., Furnari, A., & Galasso, F. (2024). Prego: Online mistake detection in procedural egocentric videos. Proceedings of the ieee/cvf conference on computer vision and pattern recognition (cvpr) (p.18483-18492). Gabeur, V., Sun, C., Alahari, K., & Schmid, C. (2020). Multi-modal transformer for video retrieval. Computer vision–eccv 2020: 16th european conference, glasgow, uk, august 23–28, 2020, proceedings, part iv 16 (pp. 214–229). Gao, Y., Vedula, S.S., Reiley, C.E., Ahmidi, N., Varadarajan, B., Lin, H.C., & others (2014). Jhu-isi gesture and skill assessment working set (jigsaws): A surgical activity dataset for human motion modeling. Miccai workshop: M2cai (Vol. 3, p.3). Ghoddoosian, R., Dwivedi, I., Agarwal, N., & Dariush, B. (2023). Weakly-supervised action segmentation and unseen error detection in anomalous instructional videos. Proceedings of the ieee/cvf international conference on computer vision (iccv) (p.1012810138). Goyal, R., Kahou, S.E., Michalski, V., Materzynska, J., Westphal, S., Kim, H., & Memisevic, R. (2017). The "something something" video database for learning and evaluating visual common sense. arXiv. https://arxiv.org/abs/1706.04261 Grauman, K., Westbury, A., Byrne, E., Chavis, Z., Furnari, A., Girdhar, R., & Malik, J. (2022). Ego4d: Around the World in 3,000 Hours of Egocentric Video. Ieee/cvf computer vision and pattern recognition (cvpr). Grauman, K., Westbury, A., Torresani, L., Kitani, K., Malik, J., Afouras, T., & Wray, M. (2024). Ego-exo4d: Understanding skilled human activity from ﬁrst- and third-person perspectives.https://arxiv.org/ abs/2311.18259 Hipiny, I., Ujir, H., Alias, A.A., Shanat, M., & Ishak, M.K. (2023). Who danced better? ranked tiktok dance video dataset and pairwise action quality assessment method. International Journal of Advances in Intelligent Informatics, 9(1), 96-107, (Name - TikTok Inc; Copyright - © 2023. This article is published under https://creativecommons.org/licenses/by/4.0/ (the “License”). Notwithstanding the ProQuest Terms and Conditions, you may use this content in accordance with the terms of the License; Last updated - 2023-04-28) Hochreiter, S., & Schmidhuber, J. (1997). Long short-term memory. Neural Computation, 9(8), 1735–1780. Jang, Y., Sullivan, B., Ludwig, C., Gilchrist, I.D., Damen, D., & MayolCuevas, W. (2019). Epic-tent: An egocentric video dataset for camping tent assembly. 2019 ieee/cvf international conference on computer vision workshop (iccvw) (p.4461-4469). Los Alamitos, CA, USA: IEEE Computer Society. Kalogeiton,V.,Weinzaepfel,P.,Ferrari,V.,&Schmid,C.(2017).Action tubelet detector for spatio-temporal action localization. Proceedings of the ieee international conference on computer vision (pp. 4405–4413).

<!-- Page 37 -->

International Journal of Computer Vision (2025) 133:7817–7854 7853

Kay, W., Carreira, J., Simonyan, K., Zhang, B., Hillier, C., Vijayanarasimhan,S.,&Zisserman,A.(2017).Thekineticshumanaction video dataset. arXiv. https://arxiv.org/abs/1705.06950 Kuehne, H., Jhuang, H., Garrote, E., Poggio, T., & Serre, T. (2011). HMDB: a large video database for human motion recognition. Proceedings of the international conference on computer vision (iccv). Li,Y.,Li,Y.,&Vasconcelos,N.(2018).Resound:Towardsactionrecognition without representation bias. Proceedings of the european conference on computer vision (eccv) (pp. 513–528). Li, Y., Wu, C.Y., Fan, H., Mangalam, K., Xiong, B., Malik, J., & Feichtenhofer, C. (2022). Mvitv2: Improved multiscale vision transformers for classiﬁcation and detection. Li, Z., Huang, Y., Cai, M., & Sato, Y. (2019). Manipulation-skill assessment from videos with spatial attention network. Proceedings of the ieee/cvf international conference on computer vision workshops (pp. 0–0). Lin, B., Zhu, B., Ye, Y., Ning, M., Jin, P., & Yuan, L. (2023). Videollava: Learning united visual representation by alignment before projection. arXiv preprint arXiv:2311.10122. Liu, H., Dai, Z., So, D.R., & Le, Q.V. (2021). Pay attention to mlps. CoRR, abs/2105.08050, https://arxiv.org/abs/2105.08050 Liu, W., W. Luo, D.L., & Gao, S. (2018). Future frame prediction for anomaly detection – a new baseline. 2018 ieee conference on computer vision and pattern recognition (cvpr). Liu, Y., Albanie, S., Nagrani, A., & Zisserman, A. (2019). Use what you have: Video retrieval using representations from collaborative experts. British machine vision conference. Liu, Z., Lin, Y., Cao, Y., Hu, H., Wei, Y., Zhang, Z., & Guo, B. (2021). Swin transformer: Hierarchical vision transformer using shifted windows. lucidrains (2021). gmlp - pytorch. (https://github.com/lucidrains/gmlp-pytorch/. Last accessed on 24-05-2023) lucidrains (2023). Vision transformer - pytorch. (https://github.com/ lucidrains/vit-pytorch. Last accessed on 24-05-2023) Ma, F., Zhu, L., Yang, Y., Zha, S., Kundu, G., Feiszli, M., & Shou, Z. (2020). Sf-net: Single-frame supervision for temporal action localization.https://arxiv.org/abs/2003.06845 Mahadevan, V., Li, W., Bhalodia, V., & Vasconcelos, N. (2010). Anomaly detection in crowded scenes. 2010 ieee computer society conference on computer vision and pattern recognition (pp. 1975–1981). Moltisanti, D., Wray, M., Mayol-Cuevas, W., & Damen, D. (2017). Trespassing the boundaries: Labeling temporal bounds for object interactions in egocentric video. Proceedings of the ieee international conference on computer vision (pp. 2886–2894). Neimark, D., Bar, O., Zohar, M., & Asselmann, D. (2021). Video transformer network. Proceedings of the ieee/cvf international conference on computer vision (iccv) workshops (p.3163-3172). Newell,K.(1991).Motorskillacquisition.Annualreviewofpsychology, 42(1), 213–237. O˘gul, B. B., Gilgien, M., & Özdemir, S. (2022). Ranking surgical skills using an attention-enhanced siamese network with piecewise aggregated kinematic data. International Journal of Computer Assisted Radiology and Surgery, 17(6), 1039–1048. https://doi. org/10.1007/s11548-022-02581-8 O˘gul, B.B., Gilgien, M.F., & ¸Sahin, P.D. (2019). Ranking robotassisted surgery skills using kinematic sensors. I. Chatzigiannakis, B. De Ruyter, and I. Mavrommati (Eds.), Ambient intelligence (pp. 330–336). Cham: Springer International Publishing. Parmar, P., & Morris, B. (2019). Action quality assessment across multiple actions. 2019 ieee winter conference on applications of computer vision (wacv) (pp. 1468–1476). Parmar, P., & Morris, B.T. (2016). Learning to score olympic events.https://arxiv.org/abs/1611.05125

Parmar, P., & Tran Morris, B. (2019). What and how well you performed? a multitask learning approach to action quality assessment. Proceedings of the ieee conference on computer vision and pattern recognition (pp. 304–313). Pirsiavash, H., Vondrick, C., & Torralba, A. (2014). Assessing the quality of actions. European conference on computer vision (pp. 556–571). Ragusa, F., Furnari, A., & Farinella, G. M. (2023). Meccano: A multimodal egocentric dataset for humans behavior understanding in the industrial-like domain. Computer Vision and Image Understanding (CVIU). https://doi.org/10.1016/j.cviu.2023.103764 https:// iplab.dmi.unict.it/MECCANO/. Ragusa, F., Furnari, A., Livatino, S., & Farinella, G.M. (2021). The meccano dataset: Understanding human-object interactions from egocentric videos in an industrial-like domain. Proceedings of the ieee/cvf winter conference on applications of computer vision (wacv) (p.1569-1578). Russakovsky, O., Deng, J., Su, H., Krause, J., Satheesh, S., Ma, S., & Fei-Fei, L. (2015). ImageNet Large Scale Visual Recognition Challenge. International Journal of Computer Vision (IJCV), 115(3), 211–25. https://doi.org/10.1007/s11263-015-0816-y Sener, F., Chatterjee, D., Shelepov, D., He, K., Singhania, D., Wang, R., & Yao, A. (2022). Assembly101: A large-scale multi-view video dataset for understanding procedural activities.https://arxiv.org/ abs/2203.14712 Shao, D., Zhao, Y., Dai, B., & Lin, D. (2020). Finegym: A hierarchical video dataset for ﬁne-grained action understanding. Ieee conference on computer vision and pattern recognition (cvpr). Song, Y., Byrne, E., Nagarajan, T., Wang, H., Martin, M., & Torresani, L. (2023). Ego4d goal-step: Toward hierarchical understanding of procedural activities. A. Oh, T. Naumann, A. Globerson, K. Saenko, M. Hardt, and S. Levine (Eds.), Advances in neural information processing systems (Vol. 36, pp. 38863–38886). Curran Associates, Inc. Soomro, K., Zamir, A.R., & Shah, M. (2012). Ucf101: A dataset of 101 human actions classes from videos in the wild. arXiv preprint arXiv:1212.0402. Sullivan, B., Ludwig, C. J., Damen, D., Mayol-Cuevas, W., & Gilchrist, I. D. (2021). Look-ahead ﬁxations during visuomotor behavior: Evidence from assembling a camping tent. Journal of vision, 21(3), 13–13. Sultani, W., Chen, C., & Shah, M. (2018a). Real-world anomaly detection in surveillance videos. Proceedings of the ieee conference on computer vision and pattern recognition (pp. 6479–6488). Sultani, W., Chen, C., & Shah, M. (2018b). Real-world anomaly detection in surveillance videos. Proceedings of the ieee conference on computer vision and pattern recognition (cvpr). Tang, Y., Ni, Z., Zhou, J., Zhang, D., Lu, J., Wu, Y., & Zhou, J. (2020). Uncertainty-aware score distribution learning for action quality assessment. Proceedings of the ieee/cvf conference on computer vision and pattern recognition (pp. 9839–9848). The National Archives. (2023). Non Commercial Government Licence — nationalarchives.gov.uk. (https://www.nationalarchives.gov. uk/doc/non-commercial-government-licence/version/2/. Last accessed on 06-Jun-2023) Tian, Y., Pang, G., Chen, Y., Singh, R., Verjans, J.W., & Carneiro, G. (2021). Weakly-supervised video anomaly detection with robust temporal feature magnitude learning. Tolstikhin, I.O., Houlsby, N., Kolesnikov, A., Beyer, L., Zhai, X., Unterthiner, T., & Dosovitskiy, A. (2021). Mlp-mixer: An all-mlp architecture for vision. CoRR, abs/2105.01601. https://arxiv.org/ abs/2105.01601 Tran, D., Bourdev, L., Fergus, R., Torresani, L., & Paluri, M. (2015). Learning spatiotemporal features with 3d convolutional networks. Proceedings of the ieee international conference on computer vision (pp. 4489–4497).

<!-- Page 38 -->

7854 International Journal of Computer Vision (2025) 133:7817–7854

Tran, D., Wang, H., Torresani, L., & Feiszli, M. (2019). Video classiﬁcation with channel-separated convolutional networks. Proceedings of the ieee/cvf international conference on computer vision (pp. 5552–5561). Tran, D., Wang, H., Torresani, L., Ray, J., LeCun, Y., & Paluri, M. (2018). A closer look at spatiotemporal convolutions for action recognition. Proceedings of the ieee conference on computer vision and pattern recognition (pp. 6450–6459). Wang, L., Xiong, Y., Wang, Z., Qiao, Y., Lin, D., Tang, X., & Gool, L.V. (2016). Temporal segment networks: Towards good practices for deep action recognition. European conference on computer vision (pp. 20–36). Wang, X., Kwon, T., Rad, M., Pan, B., Chakraborty, I., Andrist, S., & Pollefeys, M. (2023). Holoassist: an egocentric human interaction dataset for interactive ai assistants in the real world. Proceedings of the ieee/cvf international conference on computer vision (iccv) (p.20270-20281). Warneken, F., & Tomasello, M. (2006). Altruistic helping in human infants and young chimpanzees. Science, 311(5765), 1301–1303. Warneken, F., & Tomasello, M. (2006b). Helping in infants and chimpanzees. (https://sites.lsa.umich.edu/warneken/studyvideos/. Last accessed on 01/10/2024) Xie, S., Sun, C., Huang, J., Tu, Z., & Murphy, K. (2018). Rethinking spatiotemporal feature learning: Speed-accuracy trade-offs in video classiﬁcation. Proceedings of the european conference on computer vision (eccv) (pp. 305–321). Xu, J., Rao, Y., Yu, X., Chen, G., Zhou, J., & Lu, J. (2022). Finediving: A ﬁne-grained dataset for procedure-aware action quality assessment.https://arxiv.org/abs/2204.03646 yjxiong, & Line290. (2019). Tsn-pytorch. (https://github.com/yjxiong/ tsn-pytorch. Last accessed on 24-05-2023) Zatsarynna, O., Farha, Y.A., & Gall, J. (2022). Self-supervised learning for unintentional action prediction. Zhang, B., Chen, J., Xu, Y., Zhang, H., Yang, X., & Geng, X. (2021). Auto-encoding score distribution regression for action quality assessment. arXiv preprintarXiv:2111.11029 Zhang, C., Gupta, A., & Zisserman, A. (2021). Temporal query networks for ﬁne-grained video understanding. Proceedings of the ieee/cvf conference on computer vision and pattern recognition (cvpr) (p.4486-4496).

Zhao, J., Zhang, Y., Li, X., Chen, H., Shuai, B., Xu, M., & Tighe, J. (2022). Tuber: Tubelet transformer for video action detection. Proceedings of the ieee/cvf conference on computer vision and pattern recognition (cvpr) (p.13598-13607). Zhou, C., & Huang, Y. (2022). Uncertainty-driven action quality assessment.https://arxiv.org/abs/2207.14513 Zhu, Y., Bao, W., & Yu, Q. (2022). Towards open set video anomaly detection.https://arxiv.org/abs/2208.11113 Zhu, Y., & Newsam, S.D. (2019). Motion-aware feature for improved video anomaly detection. CoRR, abs/1907.10211, http://arxiv.org/ abs/1907.10211

Publisher’s Note Springer Nature remains neutral with regard to jurisdictional claims in published maps and institutional afﬁliations.
