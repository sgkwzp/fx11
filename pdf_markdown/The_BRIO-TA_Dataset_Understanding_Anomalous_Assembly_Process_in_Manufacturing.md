---
source_pdf: "The_BRIO-TA_Dataset_Understanding_Anomalous_Assembly_Process_in_Manufacturing.pdf"
pages: 5
conversion: "Automatic PDF-to-Markdown text conversion; figures are represented by extracted captions/text rather than embedded images."
---

# THE BRIO-TA DATASET: UNDERSTANDING ANOMALOUS ASSEMBLY PROCESS IN MANUFACTURING

<!-- Page 1 -->

2022 IEEE International Conference on Image Processing (ICIP) | 978-1-6654-9620-9/22/$31.00 ©2022 IEEE | DOI: 10.1109/ICIP46576.2022.9897369

## ABSTRACT

In this paper, we introduce a new video dataset for action segmentation, the BRIO-TA (BRIO Toy Assembly) dataset, which is designed to simulate operations in factory assembly. In contrast with existing datasets, BRIO-TA consists of two types of scenarios: normal work processes and anomalous work processes. Anomalies are further categorized into incorrect processes, omissions, and abnormal durations. The subjects in the videos are asked to perform either normal work or one of the three anomalies, and all video frames are manually annotated into 23 action classes. In addition, we propose a new metric called anomaly section accuracy (ASA) for evaluating the detection accuracy of anomalous segments in a video. With the new dataset and metric, we report that the state-of-the-art methods show a signiﬁcantly low ASA, while they work for normal work segments. Demo videos are available at https://github.com/Tarmo-moriwaki/BRIO-TA sample and the full dataset will be released after publication.

Index Terms— Action segmentation, dataset, assembly, anomaly detection

1. INTRODUCTION

Understanding the behavior of people in videos is important for industrial applications, such as surveillance and robotics. In the ﬁeld of action segmentation, many methods have been proposed [1, 2, 3] in recent years that predict a person’s actions for each frame. Analyzing the results of action segmentation, we can improve productivity by detecting anomalous behavior and working times in industrial ﬁelds such as assembly processes. However, the existing datasets for action segmentation [4, 5, 6] consist only of the same standardized work process. For example, if a video in the datasets shows a scene of a salad being made, the process of making the salad is the same regardless of the workers and the video ID. Also, existing action segmentation methods [1, 2, 3] does not consider anomalous work processes which occur in a real environment because they are evaluated on datasets containing only standardized videos.

Kosuke Moriwaki, Gaku Nakano, Tetsuo Inoshita

NEC Corporation Biometrics Research Laboratories, Japan {k.moriwaki, g-nakano, tetsuo.inoshita}@nec.com

**Fig. 1. BRIO-TA dataset overview. Assembly process in sequence has either standardized process or one of three kinds of anomaly.**

Therefore, in this paper, we propose a new dataset for action segmentation called the BRIO-TA (BRIO Toy Assembly) dataset. As shown in Fig. 1, the BRIO-TA dataset consists of videos of anomalous work processes such as those with incorrect processes, omissions, and abnormal durations in addition to the standardized work processes. In summary, the contributions of this paper are highlighted as follows. Creation of new dataset, BRIO-TA, for action segmentation We propose BRIO-TA, a new dataset consisting of videos with standardized and three types of anomalous work processes in assembly scenes. BRIO-TA differs from existing datasets [4, 5, 6] in that it consists of various work processes for the same type of work. Evaluation of existing methods with BRIO-TA We evaluate existing action segmentation methods using this dataset. Two types of experiments are conducted. The ﬁrst is to train and test the methods by using only assembly videos featuring the standardized assembly process, and the second is to evaluate the accuracy of the existing methods for anomaly assembly processes.

2. RELATED WORK

Datasets for understanding human actions The datasets for action segmentation consist of different kinds of humanaction videos and their frame-wise annotation labels because action segmentation methods predict human-action labels for

1991 978-1-6654-9620-9/22/$31.00 ©2022 IEEE ICIP 2022

Authorized licensed use limited to: Ningbo University. Downloaded on October 02,2026 at 04:44:58 UTC from IEEE Xplore. Restrictions apply.

<!-- Page 2 -->

**Table 1. Comparison of BRIO-TA with existing datasets.**

Dataset Setting View Num. of sequences Duration Subjects IDs Ethics

Normal Anomaly Min/seq Total hours guidelines

BRIO-TA Industrial-like Top 30 45 2.3 2.9 15 23 ✓ 50Salads Kitchens Top 50 0 6.4 5.3 25 17 GTEA Kitchens First-person 28 0 4.7 2.2 4 11 Breakfast Kitchens Third-person 1712 0 2.7 77 52 48

every frame. 50Salads [4] dataset contains 50 videos that depict salad-preparation activities from a top-down view. Every frame of the dataset are annotated from among 17 action classes every frame. GTEA [6] dataset has 28 videos corresponding to 7 different activities such as preparing coffee or making a sandwich. Each video in the dataset is recorded egocentrically by a camera mounted on the actor’s head. Breakfast dataset [5] contains 1,712 videos of breakfast preparation activities. The videos are recorded from various angles. These three datasets are usually used for the evaluation of action segmentation, but all of their videos capture only standardized work processes in kitchen scenes. MECCANO dataset [7] provides egocentric videos to study human-object interaction. The videos in the dataset capture industrial-like scenarios in which subjects built a toy model of a motorbike. ActivityNet [8] dataset is composed of videos depicting over 200 activities in human daily life. These datasets have videos of human-action sequences in various scenes, but they are not available for action segmentation because they do not have frame-wise action annotation. Action segmentation methods In recent years, with the development of CNNs using deep learning, action segmentation methods have been typically divided into two phases. The ﬁrst is extracting frame-wise spatio-temporal features using a CNN, and the second is then classifying these feature vectors into action classes using a classiﬁer. CNNs used as a feature extractor in the ﬁrst part of action segmentation include a 2DCNN [9], a 3DCNN [10, 11] which extends convolution in the 3D direction and a two-stream CNN [12, 13] which separately convolutes videos in the temporal and spatial directions and ﬁnally combines them. MS-TCN [1] is the classiﬁer that improves accuracy by connecting multiple TCNs [14] and reﬁning the output results of each TCN. ASRF [15] is also the classiﬁer that takes an approach to avoid oversegmentation, which results in detecting extra steps.

3. BRIO-TA DATASET

BRIO-TA is a dataset in which the scene is of a toy model of a car being assembled as shown in Fig. 1. Its novelty is that it has two types of videos for the same type of assembly work: one is videos of the standardized assembly process, and the other is videos of an anomalous assembly process.

We provide a detailed explanation of the BRIO-TA dataset. First, in section 3.1 and 3.2, we explain general settings regarding data collection and annotation. Next, in section 3.3, we explain standardized and anomalous assembly processes, namely, the novelty of BRIO-TA.

3.1. Data Collection

The BRIO-TA dataset was acquired in an industrial-like scene in which subjects built a toy model of a car. The model is composed of 10 different parts. Subjects were asked to assemble the toy car by using a wrench as shown in Fig. 1, and each subject assembled a toy car ﬁve times. There are 15 different subjects and 75 assembly videos. Videos were recorded at a resolution of 640 × 360 pixels and with a framerate of 30 fps by a camera located on the ceiling. Each video corresponds to a sequence of the assembly process. The average duration of the videos is 2.3 minutes. Table 1 shows a comparison of BRIO-TA and with other datasets for action segmentation. BRIO-TA is the only dataset with an industrial-like setting for action segmentation. Furthermore, no subject’s personal information is included. Therefore, the data in BRIO-TA does not violate ethics guidelines and complies with the publication standards set by CVPR and NeurIPS. It should be noted that the existing datasets were published before the establishment of these guidelines, so it is unclear whether they comply with these guidelines.

3.2. Data Annotation

BRIO-TA has 75 video sequences. We manually annotated the action class label to every frame in the dataset. Fig. 2 shows a list of the action classes in BRIO-TA and their frequency of occurrence. The action labels consist of 23 classes, such as Take a speciﬁc part and Set a speciﬁc part.

3.3. Standardized and Anomalous Assembly Process

Standardized assembly process We deﬁne a certain assembly process as a correct process. Standardized assembly process videos are those of the toy assembly sequence done with the correct and standardized assembly process. They are similar to the videos of the existing action segmentation

Authorized licensed use limited to: Ningbo University. Downloaded on October 02,2026 at 04:44:58 UTC from IEEE Xplore. Restrictions apply.

<!-- Page 3 -->

**Fig. 2. Distribution of action class in our dataset**

datasets. BRIO-TA has 30 standardized assembly process videos. Anomalous assembly process Anomalous assembly process videos contain an anomaly in some process of the assembly sequence. In this paper, we deﬁned three types of anomalies in the toy assembly process: incorrect process, omission, and abnormal duration.

Incorrect process: Two parts of the standardized assembly process are swapped. This anomaly causes a disparity in products quality.

Omission: A subject skips a speciﬁc assembly process. If this anomaly were to occur in a real environment, the product’s durability and function would be imperfect.

Abnormal duration: A subject wastes more time on one process of assembly than in the standardized case. In a real environment, this anomaly may occur when a novice worker performs assembly, and detecting it can improve production efﬁciency.

There are 45 anomalous assembly videos (15 subjects × 3 anomalies) in BRIO-TA. We conﬁgured each anomalous process to have ﬁve variations. For example, ﬁve different swap patterns for the incorrect process. Subjects were asked to wrongly build a toy car according to one of the variations when recording an anomalous process video.

4. EXPERIMENTS

We conducted two types of evaluation experiments. One involved both training and testing baseline methods using standardized sequences the same as existing datasets, and the other involved training with standardized sequences and testing with anomalous assembly sequences.

4.1. Baseline Methods

We selected four methods in the experiments as baseline methods: MS-TCN [1], MS-TCN++ (MS-TCN2) [2], ASRF

[15], and BCN [3]. MS-TCN and MS-TCN++ are the most representative of the existing action segmentation methods, and ASRF and BCN recently achieved high accuracy for existing datasets. In addition, we used I3D [10] and X3D [16] as feature extractors for the four classiﬁers. I3D is used in each baseline method originally, and X3D has fewer parameters to achieve faster execution speed than I3D. We selected X3D because the execution speed is an important factor when performing action segmentation in a real environment.

4.2. Evaluation Measures

We used three common metrics for action segmentation [17, 14, 18] : frame-wise accuracy (Acc), segmental edit distance (Edit) [19], and segmental F1 score with overlapping threshold k% (F1@k) [20]. Acc is the fraction of predicted frames that match with the ground truth label. A drawback of Acc is insensitivity to over-segmentation errors. Edit is calculated by the Levenshtein distance between the segment of the prediction result and the ground truth segment. It penalizes errors due to over-segmentation and classes with a small number of frames. F1@k is a measure that classiﬁes a prediction as correct if the temporal IoU for each class is greater than a certain threshold, k%. The F1 score is also sensitive to oversegmentation. In addition to these metrics, we propose two new metrics: anomaly section accuracy (ASA) and standardized section accuracy (SSA). ASA is the frame-wise accuracy for only sections in which anomaly assembly occurred in a sequence, and SSA is the frame-wise accuracy in other sections in the same sequence. We calculate ASA and SSA as follows. First, let {g1, ..., gN} be the ground truth labels in a video with N frames and {p1, ..., pN} be predictions corresponding to each frame. Matching function M(i) can be given by

{ 1 if pi = gi, 0 else. (1)

M(i) =

Next, let [fs, fe] denote the list of the start and end frame numbers of the section in which an anomaly assembly occurred in a video; ASA and SSA are written by

∑fe i=fs M(i)

fe −fs + 1 × 100 [%], (2)

ASA =

∑fs i=1 M(i) + ∑N i=(fe+1) M(i)

N −(fe −fs + 1) × 100 [%]. (3)

SSA =

4.3. Evaluation of standaridized assembly sequences

Experimental Protocol In action segmentation, crossvalidation experiment is the standard protocol [1, 2, 15, 3]. We divided all 30 standardized sequences into ﬁve groups and conducted 5-fold cross-validation. Therefore, we used

Authorized licensed use limited to: Ningbo University. Downloaded on October 02,2026 at 04:44:58 UTC from IEEE Xplore. Restrictions apply.

<!-- Page 4 -->

**Table 2. Quantitative results for standardized sequences**

Feature Classiﬁer F1@10,25,50 Edit Acc

MS-TCN 88.2 91.9 94.7 93.7 86.6 MS-TCN++ 88.3 89.5 92.6 91.3 85.3 BCN 73.2 58.7 69.4 66.9 78.7 ASRF 88.0 88.9 92.8 91.7 86.4

I3D

MS-TCN 85.8 88.3 92.1 91.1 82.0 MS-TCN++ 86.8 88.6 91.3 89.8 82.1 BCN 74.1 60.2 70.8 68.7 72.6 ASRF 86.5 88.9 92.1 90.6 83.2

X3D

24 sequences and six sequences as training data and test data, respectively. Result Table 2 shows the quantitative result of each baseline method. All methods achieved an Acc in a range of about 72 to 86% regardless the feature extractor. The results of all three evaluation metrics are similar to the result obtained by evaluating the baseline methods with the existing dataset. Therefore, BRIO-TA is as difﬁcult as the existing dataset in standardized sequences prediction.

4.4. Evaluation of anomalous assembly process

Experimental Setting We trained the baseline methods using 30 standardized sequences and tested using 45 anomalous sequences. Since anomalous assembly processes are rarely observed compared with normal processes in real industrial situations, we excluded anomalous sequences in the training data. Result Table 3 shows the Acc, ASA, and SSA scores for each method. The scores for edit and F1@k are omitted because our aim in this experiment was to evaluate the accuracy difference between ASA and SSA. As per Table 3, while SSA was 70–80% similar to the standardized sequence test, ASA decreased to 50–60%. The results highlighted that the current action segmentation methods have difﬁculty in correctly predicting assembly anomalies. Moreover, these results indicate that ASA is a more appropriate metric than Acc to evaluate the detection accuracy of anomaly sections because anomaly frames are few which do not have a signiﬁcant impact on Acc. Fig. 3 visualizes a prediction result for an anomalous sequence with abnormal duration at the beginning. We can observe that none of the methods can detect the abnormal duration, causing over-segmentation errors.

4.5. Discussion and future work

Since BRIO-TA has standardized and anomalous assembly process sequences, we evaluate the prediction capabilities of the baseline methods with the kind of sequences which is not used in training data. As shown in Fig. 3, we revealed

**Table 3. Quantitative results for anomalous sequences.**

Feature Classiﬁer Acc ASA SSA

MS-TCN 80.8 61.4 86.3 MS-TCN++ 80.2 57.6 86.7 BCN 65.9 50.4 70.6 ASRF 63.3 63.3 86.9

I3D

MS-TCN 79.2 53.9 86.0 MS-TCN++ 78.7 50.6 86.2 BCN 66.7 48.2 71.4 ASRF 80.8 54.8 84.6

X3D

**Fig. 3. Qualitative evaluation result for sequence with abnormal duration**

that the all prediction result of baseline methods had oversegmentation errors in the anomaly section by using BRIOTA. This tendency of predictions is thought to be caused by training data bias. Since the sequences in the training data have no anomalous assembly process, their duration for each assembly process in the training data is standardized. Therefore, the classiﬁers trained with such sequences fail to predict anomalous assembly sequences, which have the abnormal duration anomaly and are not included in training data. To apply action segmentation in industrial scenes, further investigation needs to be explored to develop a method that correctly predicts anomalous events, even if the events are rarely observed.

5. CONCLUSION

In this paper, we proposed BRIO-TA, a new dataset for action segmentation, and raises a new challenging issue in this resaerch ﬁeld, i.e. anomaly detection in assembly scenarios. BRIO-TA contains the videos which captured an assembly process for industrial use of action segmentation and focuses on anomalous assembly processes, which are rarely observed and an issue in industrial domains. Experiments on BRIO-TA revealed that the prediction results of the baseline methods have lower accuracy for anomalous assembly process videos than standardized ones. We hope that BRIO-TA contributes to the research for industrial applications of action segmentation.

Authorized licensed use limited to: Ningbo University. Downloaded on October 02,2026 at 04:44:58 UTC from IEEE Xplore. Restrictions apply.

<!-- Page 5 -->

6. REFERENCES

[1] Y. Abu and J. Gall, “MS-TCN: Multi-stage temporal convolutional network for action segmentation,” In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), pp. 3575–3584, 2019.

[2] Y. Liu M. M. Cheng S. J. Li, Y. A. Farha and J. Gall, “MS-TCN++: Multi-stage temporal convolutional network for action segmentation,” IEEE Trans. Pattern Analysis and Machine Intelligence, pp. 1–1, 2020.

[3] L. Wang Z. Li Z. Wang, Z. Gao and G. Wu, “Boundaryaware cascade networks for temporal action segmentation,” In European Conference on Computer Vision (ECCV), pp. 34–51, 2020.

[4] S. Stein and S. J McKenna, “Combining embedded accelerometers with computer vision for recognizing food preparation activities,” In ACM International Joint Conference on Pervasive and Ubiquitous Computing, pp. 729–738, 2013.

[5] A. Arslan H. Kuehne and T. Serre., “An end-to-end generative framework for video segmentation and recognition.,” In IEEE Winter Conference on Applications of Computer Vision (WACV), pp. 1–8, 2016.

[6] X. Ren A. Fathi and J. M. Rehg, “Learning to recognize objects in egocentric activities,” In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), pp. 3281–3288, 2011.

[7] S. Livatino F. Ragusa, A. Furnari and G. M. Farinella, “The MECCANO dataset: Understanding human-object interactions from egocentric videos in an industrial-like domain,” IEEE Winter Conference on Application of Computer Vision (WACV), 2020.

[8] V. Escorcia B. G. F. C. Heilbron and J. C. Niebles, “Activitynet: A large-scale video benchmark for human activity understanding,” In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), pp. 961–970, 2015.

[9] C. Gan J. Lin and S. Han., “TSM: Temporal shift module for efﬁcient video understanding,” IEEE International Conference on Computer Vision (ICCV), pp. 7083–7093, 2019.

[10] J. Carreira and A. Zisserman, “Quo vadis, action recognition? a new model and the kinetics dataset,” In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), pp. 4724–4733, 2017.

[11] J. Huang Z. Tu S. Xie, C. Sun and K. Murphy, “Rethinking spatiotemporal feature learning: Speed-accuracy trade-offs in video classiﬁcation,” European Conference on Computer Vision (ECCV), 2018.

[12] K. Simonyan and A. Zisserman, “Two-stream convolutional networks for action recognition in videos.,” In Proceedings of the Advances in Neural Information Processing Systems (NIPS), pp. 568–576, 2014.

[13] A. Pinz C. Feichtenhofer and A. Zis-serman, “Convolutional two-stream network fusionfor video action recognition,” In IEEE/CVF Inter-national Conference on Computer Vision and PatternRecognition (CVPR), pp. 1933–1941, 2016.

[14] R. Vidal A. Reiter C. Lea, M. D. Flynn and G. D. Hager., “Temporal convolutional networks for action segmentation and detection,” In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), pp. 1012–1017, 2017.

[15] Y. Aoki Y. Ishikawa, S. Kasai and H. Kataoka, “Alleviating over-segmentation errors by detecting action boundaries,” In IEEE Winter Conf. Applications of Computer Vision (WACV), pp. 2322–2331, 2021.

[16] C. Feichtenhofer, “X3D: Expanding architectures for efﬁcient video recognition,” In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), pp. 203– 213, 2020.

[17] R. Vidal C. Lea and G. D. Hager, “Segmental spatiotemporal cnns for ﬁne-grained action segmentation,” In European Conference on Computer Vision (ECCV), pp. 36–52, 2016.

[18] R. Vidal C. Lea and G. D. Hager, “Learning convolutinal action primitives for ﬁne-grained action recognition,” In IEEE International Conference on Robotics and Automation (ICRA), pp. 1642–1649, 2016.

[19] V. I. Levenshtein, “Binary codes capable of correcting deletions, insertions, and reversals,” Soviet PhysicsDoklady, pp. 707–710, 1996.

[20] R. Vidal A. Reiter C. Lea, M. D. Flynn and G. D. Hager, “Temporal convolutional networks for action segmentation and detection,” In IEEE Conference on Computer Vision and Pattern Recognition (CVPR), pp. 1003–1012, 2017.

Authorized licensed use limited to: Ningbo University. Downloaded on October 02,2026 at 04:44:58 UTC from IEEE Xplore. Restrictions apply.
