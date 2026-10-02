---
source_pdf: "Ragusa_Ego-EXTRA_video-language_Egocentric_Dataset_for_EXpert-TRAinee_assistance_WACV_2026_paper.pdf"
pages: 13
conversion: "Automatic PDF-to-Markdown text conversion; figures are represented by extracted captions/text rather than embedded images."
---

# Ego-EXTRA: video-language Egocentric Dataset for EXpert-TRAinee assistance

<!-- Page 1 -->

Francesco Ragusa*1,2, Michele Mazzamuto*1,2, Rosario Forte1, Irene D’Ambra1, James Fort3, Jakob Engel3, Antonino Furnari1,2, Giovanni Maria Farinella1,2

1Department of Mathematics and Computer Science - University of Catania, Italy 2Next Vision s.r.l. - Spinoff of the University of Catania, Italy 3Meta Reality Labs Research, USA

Trainee

## Scenarios

How much flour

What is the first

…

should be weighed on the

ingredient to

take?

scale?

…

Now turn and in

You need to weigh 1.5 Kg of

front of you,

there is the flour below

flour

Expert

Where should

How should the

…

the tray with the cookies be

dough be

shaped?

…

placed in the

oven?

We can quietly make this kind of serpentine shape

They go to the

bottom

Time

**Figure 1. We collect egocentric videos of trainees (left) performing procedures while aided by an expert (right) enacting a wearable visual assistant which observes the scene from the trainees’ point-of-view and provides guidance through natural language. We gather transcripts of rich natural language dialogue, plus different multimodal signals including eye gaze of both trainee (T) and expert (E), hand keypoints, SLAM, and IMU. The result is a unique set of videos with temporally-aligned dialogue and multimodal signals gathered by Aria glasses.**

## Abstract

We present Ego-EXTRA, a video-language Egocentric Dataset for EXpert-TRAinee assistance. Ego-EXTRA features 50 hours of unscripted egocentric videos of subjects performing procedural activities (the trainees) while guided by real-world experts who provide guidance and answer

*Equal contribution.

specific questions using natural language. Following a “Wizard of OZ” data collection paradigm, the expert enacts a wearable intelligent assistant, looking at the activities performed by the trainee exclusively from their egocentric point of view, answering questions when asked by the trainee, or proactively interacting with suggestions during the procedures. This unique data collection protocol enables Ego-EXTRA to capture a high-quality dialogue in which expert-level feedback is provided to the trainee. Twoway dialogues between experts and trainees are recorded,

<!-- Page 2 -->

transcribed, and used to create a novel benchmark comprising more than 45k high-quality Visual Question Answer sets, which we use to evaluate Multimodal Large Language Models. The results show that Ego-EXTRA is challenging and highlight the limitations of current models when used to provide expert-level assistance to the user. The Ego-EXTRA dataset is publicly available to support the benchmark of egocentric video-language assistants: https://fpviplab.github.io/Ego-EXTRA/.

## 1. Introduction

Every day, people naturally engage in different activities, such as washing a car, repairing a bike, assembling new furniture or preparing dinner. Mastering most of these activities requires time, dedication and, often, the guidance of an expert. Think of a college student learning to cook their favorite dish from their father or how to fix a running toilet from their mother. Although the web provides plenty of resources to learn such skills autonomously [2, 5, 6], wearable devices equipped with vision and computation abilities, such as smart glasses, have the potential to act as a world expert, providing guidance in a natural way [48]. Thanks to their ability to look at the world from the privileged egocentric point of view of the user, wearable assistants can relate vision to language and contextualize questions like “What is this?” or “What should I do now?”. Furthermore, wearable assistants should be able to provide practical suggestions that guide the user through the steps of repairing a bike chain and checking that each step has been performed safely and correctly. Toward this direction, previous works investigated tasks related to procedural video understanding, such as keystep recognition [29, 56], mistake detection [25, 43, 52], planning [32], procedure understanding [29], and proficiency estimation [21, 29], as well as in tasks related to natural language processing in egocentric vision [12, 14, 42]. Egocentric vision has also seen a significant increase in the availability of large-scale datasets [16, 28, 29, 56]. Although these datasets supported the development of procedural video understanding, they do not capture naturalistic dual-agent conversations paired with realistic egocentric visual observations (see Figure 1). Indeed, they typically include textual information obtained through post-acquisition narrations by the camera wearer [17], third-party annotators [28], or experts’ commentary [29], which makes current video source not directly aligned to the objective of evaluating the performance of wearable procedural assistants. To address this limitation, we present Ego-EXTRA, a new dataset of EXpert-TRAinee interactions aimed at validating video-language models. Ego-EXTRA is composed of 50 hours of egocentric procedural videos with real trainee-expert conversations recorded during the video ac-

quisition process. The dataset has been collected following the “Wizard of OZ” paradigm historically adopted in experimental psychology [37], linguistics and dialogue state tracking [45], where a human simulates a machine interacting with a user. In our setting, as shown in Figure 1, a Trainee wears ARIA glasses [55] to acquire data while performing a given procedural activity, while an Expert observes the scene from the trainee’s point of view through a laptop, providing them with assistance and answering questions. We considered four scenarios (i.e., bike workshop, kitchen, bakery, and assembly) where trainees performed different activities (e.g., replacing bike brake pads or cooking a tart), asking questions to the expert whenever they needed help. Thanks to ARIA’s rich sensor suite, we simultaneously captured different signals, including RGB, SLAM, eye gaze, IMU, magnetometer, barometer, GPS, BLE, Wi-fi, hand keypoints, and audio enriching the dataset and aligning to previous data collection protocols [29]. Conversations between trainees and experts are transcribed to text in order to provide language supervision, resulting in a novel set of egocentric videos associated with dual-agent conversations temporally aligned to videos, a significant departure from current acquisition protocols.

Based on the natural conversations included in EgoEXTRA, we designed a benchmark of 45.198 realistic visually grounded question-answer sets (QA sets) and validate the ability of current Multimodal Large Language Models (MLLMs) in supporting the user with natural language supervision. To build the benchmark we designed a novel protocol based on the extraction, automatic generation and manual validation/refinement of QA sets which is scalable and applicable to future collection efforts. We evaluated 4 state-of-the-art visual-language models on the proposed VQA benchmark and thoroughly examined their limitations. For comparison, we also evaluated a total of 5 LLMs using only textual input, highlighting that QA sets are based on video content. Results show that MLLMs achieve an average accuracy ranging from 29.21% to 41.38%, demonstrating that the proposed VQA benchmark is challenging for current methods. The dataset will be publicly released to support research in this area.

In sum, the contributions of this work are: 1) we present Ego-EXTRA, a new dataset acquired in realistic scenarios that comprises 50 hours of egocentric videos and naturalistic trainee-expert conversations; 2) we build a challenging VQA benchmark with a rigorous human-validation step to ensure the high-quality of QA sets. The benchmark is designed as a test set to validate the assistive ability of models; 3) we evaluate different LLMs and MLLMs on the proposed benchmark to assess their performance when answering trainee’s questions; 4) we will release the dataset to support the research community in evaluating visual-language models aimed at assisting humans in real-world scenarios.

<!-- Page 3 -->

## 2. Related Work

Expert-Level Assistance in Egocentric Vision An appealing feature of wearable egocentric systems is their potential to provide expert-level support to human activities [36, 48]. Previous works investigated a plethora of individual tasks aimed to support the development of such systems, notably including temporal action segmentation [53, 54, 68], action anticipation [27, 44, 70], mistake detection [33, 52, 65], procedure understanding [7, 51, 52, 71], proficiency estimation [29], and skill determination from video [21, 22]. While these tasks provide essential building blocks to enable the development of assistive systems, an holistic benchmark to support the development and evaluation of methods is missing. Towards this direction, we propose Ego-EXTRA, the first dataset of egocentric videos centered around natural vision-language dialogue interactions between experts and trainees aimed to support the evaluation of systems for user assistant in procedural tasks. Language-Based Egocentric Vision Datasets Egocentric vision datasets have often included forms of natural language supervision, usually collected after video acquisition [17, 28, 29]. Datasets of natural conversations of human-object interactions have also been proposed [46]. Other works included natural language data in the form of procedural instructions [34, 47, 50]. While providing natural language data at various levels, these previous works did not explicitly aim to collect the specific language of the expert in natural conversations with the user. Notably, HoloAssist [65] and Ego-Exo4D [29] recently proposed data collection paradigms aimed at including instructor or expert language respectively. In particular, HoloAssist [65] includes videos of trainees following a given procedure, supported by an instructor who gives them guidance in natural language. The natural language data is used to seed labels for a number of tasks, including action recognition, mistake detection, and intervention prediction, but the raw natural language data is not publicly available. EgoExo4D [29] includes videos of subjects with different levels of expertise performing given procedures autonomously. The experts narrate videos after the acquisition, highlighting areas of improvements and good task executions. Similarly to HoloAssist, we collect natural conversations between a trainee and a supervisor. While HoloAssist focuses on simple procedures, we target real-world procedures such as repairing a bike and making a tart, and recruit real-world experts similar to Ego-Exo4D. Differently from Ego-Exo4D, we aim to collect real dialogue between experts and trainees during the execution of the task, with the aim of capturing all nuances of human-assistant dialogue and provide a realistic benchmark for assistive egocentric vision systems. Table 1 compares Ego-EXTRA (bottom row) with other existing state-of-the-art datasets (top rows). Egocentric Vision-Language Benchmarks Owing to the

surge in popularity of language models [23, 61–63], several works proposed benchmarks of egocentric videos based on visual question answering. Typical paradigms for generating high-quality Visual Question Answer (VQA) samples for model evaluation are leveraging synthetic data [26, 57, 66], and augmenting existing datasets of real egocentric videos with human annotations [66], either with automatic or semi-automatic generation [10, 11, 13, 14, 31, 35, 42, 67]. We follow a similar paradigm to generate a curated VQA dataset from the natural language conversations of Ego-EXTRA with the goal of providing a scalable benchmark for vision-based assistive systems communicating with users using natural language. Table 1 compares Ego-EXTRA (bottom row) with other existing state-of-theart VQA benchmarks (middle rows). Multimodal Large Language Models Recent works have focused on enhancing Large Language Models (LLMs) to build Multimodal LLMs capable of processing both vision and language data to tackle complex tasks such as questionanswering [11, 20], open-ended questions [8] and more general tasks [39, 64] like visual question answering, document reading, and mathematical reasoning. Additionally, there is significant interest in evaluating the abilities of MLLMs on zero-shot downstream tasks, without retraining on specific target data [15, 40, 58]. We benchmark a set of recent MLLMs on Ego-EXTRA. Our results highlight the limited performance of current MLLMs in assisting humans in realistic scenarios.

## 3. Data Collection

General Setup We collected Ego-EXTRA following the “Wizard of OZ” paradigm [37], historically adopted in the dialogue state tracking literature [45] to collect realistic conversation turns between users and machine-like systems enacted by humans. Each session involves two participants: a trainee performing a procedural activity, such as assembling a chair, and an expert who provides guidance and answers questions to ensure correct execution. The trainee wears a custom rig (see Figure 1-left) consisting of Aria glasses [55], a smartphone positioned to capture a similar viewpoint, and a set of earbuds. The Aria device records multiple signals, including RGB videos, SLAM, eye gaze, IMU, and hand keypoints, while the smartphone and earbuds enable communication with the expert. The expert, located in a separate room, observes the trainee’s actions through a laptop that streams the egocentric video feed and communicates with them via earbuds using natural language. To further capture expert behavior, the laptop is equipped with a Tobii Pro Fusion Bar [60] that records their gaze as they watch the video stream and interact with the trainee. The bidirectional audio conversation is recorded and later synchronized with the collected egocentric videos. Locating trainee and experts in different physical rooms en-

<!-- Page 4 -->

Name Settings/Environment Scenarios Val&Test Hours avg. video duration (min) Expert-Trainee Conversations Modalities QA/Instruction

EPIC-Kitchens-100 [17] Cooking / Real Kitchens 25.30 N/A X RGB X CaptainCook4D [47] Cooking / Real Kitchens 94.5 15.26 X RGB, depth X LEMMA [34] House / Real Kitchens and Living Rooms 10.8 2.00 X RGB, depth X

Ego4D [28] Multi Domain / Real Multiple scenarios 288.70 24.11 X RGB, Audio, 3D environments, stereo, gaze, IMU, multi-view X

Ego-Exo4D [29] Skilled Activities / Real Soccer, Basketball, Dance, Bouldering, Music, Cooking, Bike Repair, Health Care 85.10 15.32 X RGB, 7-channel audio, IMU, eye gaze, SLAM, 3D environment point clouds, multiview X

MECCANO [49] Industrial-like / Lab Toy Assembly 3.15 20.79 X RGB, depth, gaze X Assembly-101 [53] Industrial-like / Lab Toy Assembly 66.80 7.10 X RGB, multi-view, 3D hand-pose X ENIGMA-51 [50] Industrial-like / Lab Electrical Boards Repairing 10.35 26.28 X RGB, 3D models 200 real instructions EMQA [18] Indoor Environment / Synthetic Exploration N/A N/A X RGB 441 synthetic VQA pairs EgoVQA [24] Office / Lab Object Manipulation 0.65 7.5 X RGB 580 human VQA pairs

HoloAssist [65] Assistive Tasks / Lab Object Manipulation 49.80 4.47 X RGB, depth, head pose, 3D hand pose, eye gaze, audio X

MM-Ego [67]*ˆ Multi Domain / Real Multiple scenarios 2 0.2 X RGB 7026 synthetic VQA pairs EAGLE [10]*ˆ Multi Domain / Real Multiple scenarios N/A N/A X RGB 400K synthetic instructions ProMQA° [30] Cooking / Real Kitchens 25 6.47 X RGB 401 synthetic VQA pairs VidEgoThink* [12] Multi Domain / Real Multiple Scenarios 204 2.74 X RGB 600 synthetic VQA pairs EgoPlan-Bench*ˆ [11] Multi Domain / Real Multiple Scenarios N/A N/A X RGB 4,939 synthetic VQA EgoTaskQA# [35] House / Real Kitchens and Living Rooms N/A N/A X RGB 40000 synthetic VQA EnvQA [26] House / Synthetic Kitchens, Living Rooms, Bedrooms, Bathrooms 38.77 0.2 X RGB 85072 synthetic VQA ActPlan-1K [57] House / Synthetic Kitchens, Living Rooms, Bedrooms, Bathrooms N/A N/A X RGB X

D/B

Ego-EXTRA Assistive Procedural Tasks / Real Bike Workshop, Kitchen, Bakery, Assembly 50 22.78 ✓

RGB, SLAM, Trainee eye gaze, Expert eye gaze, IMU, magnetometer, barometer, GPS, BLE, Wi-fi, hand keypoints, and audio

38080 realistic expert-trainee VQA sets

**Table 1. Comparison of Ego-EXTRA (bottom row) with other egocentric datasets (top rows) and benchmarks (middle rows). * indicates a benchmark based on Ego4D [28], ˆ indicates an extension of EPIC-Kitchens [17], ° refers to an extension of CaptainCook4D [47], and #**

indicates an extension of LEMMA [34].

sures that 1) the expert perceives the activity solely from the egocentric point of view of the trainee, and 2) all communication occurs strictly through natural language. Session Acquisition Protocols As the first of its kind, EgoEXTRA aims to capture high-quality interactions between trainees and experts, where questions are related to the procedure at hand, rather than to the location of objects or any other elements peculiar to the environment that an expert unfamiliar with the setting could not answer. To ensure the relevance of collected interactions, we designed two session acquisition protocols, as detailed below. Pro-Active protocol (PA). We instruct the expert to engage in conversations with the trainee in a pro-active way, speaking freely and intervening whenever needed, suggesting next steps, giving instructions, correcting mistakes, and providing any information which is deemed necessary. A typical intervention of the expert could be: E: “Firstly, remove the wheel slowly. You should use the wrench that is in the second chest on your left”. Following this protocol, the trainee gets acquainted with the procedure, the environment, the location of objects or tools, and their functions. This protocol results in dense interactions, with the expert’s commentary being predominant and with several conversation turns related to locations of objects and functional areas. Videos have an average duration of 29.64 minutes, with 82.4% of the words spoken by the expert and an average of 264 conversation turns per video (see Figure 2). On-Demand protocol (OD). We implement this protocol only after the trainee has become familiar with the environment, either by completing a session with the pro-active protocol or by watching a pro-active session conducted by another trainee. The trainee is hence instructed to carry out the procedure autonomously, interacting with the expert whenever they need guidance, while the expert is instructed to only answer the trainee’s questions and to in-

air chambers

Expert

Change

Assemble

1769

4.53h

Lubricate brakes 4.44h

a chair

0.98h

Disassemble

Trainee 1769

Trainee

a chair

2280

1.61h

Bike Workshop 8.97h

Bike Workshop 3538

Disassemble a

chest of drawers

Trainee 1156

3.07h

Make tart

Assembly

Assembly

5.07h

Area

Area

Bakery 2308

4564

10.31h

Expert

Expert 1152

Assemble a chest of drawers

Bakery 7.54h

2284

4.65h

51.75h OD 33.96h

Video Hours

Trainee

Kitchen

21,864 OD 12,358

Conversation Turns

Make biscuits

Make tart

2.47h

Bakery 1.92h

975

1948

Expert

0.49h

Bakery

Make biscuits

742

1485

1.43h

Expert

Trainee

Make vegetarian

Bike Workshop

Change air chambers

Kitchen

973

PA 17.79h

meat balls

743

7.14h

PA 9506

3.64h

4.54h

Kitchen 2169

1.92h

Lubricate brakes 2.62h

Trainee 1084

Bike Workshop

Expert

Kitchen 4.87h

3304

Assembly

Assembly Area 2548

1654

Make lasagna

6.46h

Area

Make lasagna 1.89h

Expert 1085

3.5h

Trainee

Make vegetarian meat balls 2.98h

chest of drawers

Trainee 1273

1650

Assemble a

Disassemble a chest of drawers 1.07h

Expert

3.95h

1275

Assemble

a chair

1.44h

**Figure 2. Left: Breakdown of hours per collection protocol, procedure, and scenario. Right: number of expert/trainee conversation turns. PA: Pro-Active, OD: On-Demand.**

tervene only if a mistake or a potentially dangerous action is about to occur. A typical trainee-expert interaction is: T: “Which of the two wheels should I remove?” - E: “The front wheel”. With this protocol, the dialogue is less dense, but the trainee’s questions are predominant, resulting in a more balanced word distribution between the trainee and the expert, with 61.39% of the words spoken by the expert and an average of 142 conversation turns per video. The average duration of videos is 23.41 minutes (see Figure 2). As we are interested in natural interactions, we reduce the number of pro-active sessions to a minimum, roughly resulting in a 1 : 3 ratio between pro-active and on-demand videos. When constructing our VQA benchmark, we manually filter out all irrelevant conversation turns from proactive videos. Scenarios, Subjects, and Statistics We acquired a total of 123 videos amounting to 50 hours at 15 fps with a resolution of 1408x1408 pixels. The recorded procedures are split across 10 different activities and 4 scenarios, have an average length of 25.24 minutes, and include a mean of 177.76

## Datasets

VQA Benchmarks

<!-- Page 5 -->

EXPERTS

**Figure 3. Participants’ Demographics.**

tools/equipment

ambiguous ref.

ambiguous ref.

quantities

parts/ingredients

comparisons

pronouns

pronouns

ambiguous ref.

parts/ingredients

comparisons

pronouns

quantities

quantities

parts/ingredients

quantities

pronouns

tools/equipment

ambiguous ref.

pronouns

parts/ingredients

ambiguous ref.

have

take

do

parts/ingredients

comparisons

parts/ingredients

tools/equipment

see

tools/equipment

remove

put

quantities

pronouns

pronouns

ambiguous ref.

pronouns

have

Trainee

remove

Expert

parts/ingredients

issues

tools/equipment

comparisons

tools/equipment

parts/ingredients

get

ambiguous ref.

ambiguous ref.

quantities

comparisons

quantities

pronouns

insert

pronouns

see

use

ambiguous ref.

issues

parts/ingredients

parts/ingredients

pronouns

quantities

insert

make

tools/equipment

comparisons

use

tools/equipment

ambiguous ref.

pronouns

quantities

ambiguous ref.

tools/equipment

tools/equipment

parts/ingredients

ambiguous ref.

ambiguous ref.

parts/ingredients

pronouns

ambiguous ref.

pronouns

quantities

parts/ingredients

comparisons

tools/equipment

pronouns

comparisons

quantities

issues

(a) Trainee Language

Detaching a Wheel

Operating the mixer

Hammering wood pegs

Thicken béchamel sauce

Inflating inner tube

Rolling the dough

Fixing the guides

Shape meatballs

Bike Workshop

Bakery

Assembly Area

Kitchen

**Figure 4. Trainees operate in four scenarios, performing varied activities, interacting with objects and tools, assisted by an expert.**

ambiguous ref.

...

quantities

less

tools/equipment

more

larger

same

parts/ingredients

...

others

another

gram

pronouns

anything

kilo

something

quantities

comparisons

spoonful

...

ambiguous ref.

parts/ingredients

a bit

put

it

tools/equipment

quantities

...

which

screw

ambiguous ref.

take

pronouns

that

gear

pronouns

this

eggs

Legend

parts/ingredients

ambiguous ref.

...

tools/equipment

cheese

turn

directions

directions

lift

parts/ingredients

tools/equipment

...

push

pronouns

task-oriented

make

slight rotation

knife

hammer

quantities

comparisons

clockwise

issues

parts/ingredients

thermometer

do

ambiguous ref.

mixer

pronouns

operation

task-oriented

...

work

comparisons

procedure

damage

directions

ambiguous ref.

problem

pronouns

mistake

...

crack

task

...

(b) Expert Language

(c) Legend

**Figure 5. Top verb/noun combinations used by trainees (a) and experts (b). Words reported by category, with examples words in (c).**

conversation turns per video, about 422.5 turns per hour or 7 turns per minute. See Figure 2 for a detailed breakdown of video hours and conversation turns per collection protocol, scenario, and activity. Data collection involved a total of 33 trainees and 4 experts across 19 distinct occupations (see Figure 3), all volunteers who provided their privacy consent and authorization to acquire data in the considered environments using the described protocols, transcribe audio conversations, and publicly release the resulting data for research purposes. Experts were selected as professional figures or individuals with consolidated experience in one of the considered procedures, while we chose trainees with no previous experience in the procedures to ensure realistic interactions. While performing the procedures, trainees interact with scenario-specific objects such as an industrial oven in the bakery or inner tubes in the bike workshop, and engage in skilled specialized activities, for which they will require the guidance of the expert (see Figure 4).

Postprocessing Videos collected through Aria devices are sent to Machine Perception Services [1] for the extraction of multimodal signals. We then synchronize Aria videos

with expert’s videos and bidirectional audio conversations. The expert’s gaze is mapped to Aria’s viewpoint1. All conversations are transcribed using a professional software and translated from the participants’ native language to English using Llama 3.1 405B model [23]. The quality of the translations has been manually verified. Each conversation turn has a timestamp which allows to localize it in the video. Trainee and Expert Language Figure 5 (a-b) summarizes the top-10 verbs and top-5 noun category per verb that trainees and experts use in their conversation turns.2 Figure 5 (c) reports a legend for noun categories with example nouns. We note a large use of pronouns (“it”, “this”, “which”), ambiguous references (“another”, “something”), and comparisons (“more”, “less”, “larger”) which suggests that conversations are naturally grounded in video (also see Figure 7). References to quantities, parts, ingredients, tools, and equipment by both trainees and experts denote a precise language and the need for expert guidance. Language is diversified, with trainees mentioning issues more often

1See the supplementary material for the details. 2We automatically derive them with NLP techniques.

<!-- Page 6 -->

than experts, while the latter give directions and use taskoriented language.

## 4. Ego-EXTRA VQA Benchmark

Based on the rich vision-language interactions between experts and trainees present in Ego-EXTRA, we built a VQA benchmark designed to evaluate models following the multiple-choice visual question-answering paradigm [11, 12, 38, 41]. In the following, we use the term questionanswer pair (QA pair) to denote a question and its correct answer, while the term question-answer set (QA set) denotes a set made of a question, the correct answer3 and four wrong answers, which act as distractors. We automatically extract QA sets from transcript. Sets are manually checked and manually refined to ensure quality and grounding in video. Figure 6 illustrates the pipeline we follow for QA set generation and validation, while individual steps are discussed in the following. Step1: Extraction of Initial Question/Answer Sets While transcripts contain interactions oriented towards a questionanswering scheme, automatically mapping conversation turns to QA sets is not trivial due to the use of filler words, informal exchanges, and unstructured conversations. We hence resorted to language models for automated analysis. Specifically, we prompted the Llama 3.1 405B model [23] to 1) extract trainee’s questions and expert’s answers, 2) automatically correct grammar and transcription errors from the original transcripts, and 3) generate an initial set of 4 negative (incorrect) answers for each QA pair. The result is a high-quality set of QA pairs, with initial sets of negatives which will be further refined. While these are generated from textual input, we observe that they are naturally grounded in video thanks to the nature of conversations. Figure 7 reports examples of question-answer pairs automatically extracted from transcripts, while Figure 6 shows an example of initial QA set generated in step 1. Step 2: Human Validation of QA Sets The initial QA sets were manually reviewed by six human validators to filter out irrelevant, generic, ill-formed, or incorrectly transcribed text. We developed a web-based dashboard to support the manual validation process4. Specifically, we show the question, the correct answer, and a set of negative examples. To facilitate annotators during validation, we also included the conversation turn from which the question was extracted, along with the two preceding and two following turns. As shown in Figure 6-Step 2, for each QA set, human validators could indicate if the current QA set is Acceptable, To be Discarded, Transcription Error or Requires Manual Revision5. Each annotator validated one video for each sce-

3Note that each question has only one correct answer. 4See the supplementary material for the details. 5In this example, annotators flagged the question for revision because all the answer options were written in the first-person singular.

nario in Ego-EXTRA. The validation results from this initial set of QAs were then used to design a scalable human validation process on Amazon Mechanical Turk (AMT) to evaluate the entire dataset. Specifically, high-agreement examples from the previous validation phase were used to create a qualification test for AMT workers. Only workers with a global acceptance rate above 90% and a perfect score (100%) on the qualification test were allowed to participate. Each QA pair was validated by five independent workers. With this process, approximately 25% of the initial QAs were discarded. Step 3: Video Grounding Validation Transcripts of expert–trainee conversations often reference procedural steps and object states that are intrinsically grounded in the video. To ensure this aspect is accurately reflected in the QA sets, we manually reviewed them while also observing the corresponding video clips in which the questions were asked by the trainees (see Figure 6-Step 3). Following the same pipeline adopted for the Step 2, we provided a dashboard to the six annotators to validate the QA sets providing also the video clip associated to the question. For each question, annotators were asked to select one of the following labels: Grounded, Not Grounded, or Video Contains the Answer. As in the previous phase, we extended the validation process to Amazon Mechanical Turk. Only workers with a global acceptance rate above 90% and a perfect score on the qualification test were selected. Each QA pair was validated by five independent workers. Using this pipeline, 28% of the questions were discarded. Statistics We analyze6 the different types of questions and answers in our benchmarks and identify 13 main categories, for which we report statistics and examples in Figure 8. Note that these questions naturally arise from conversations and do not derive from pre-made templates or any bias introduced during prompting. The three most prominent question types are Instructional / Procedural (“What do I do now”), clarifications (“What is the color of the inside?”), and comparisons (“Clockwise or counterclockwise?”). Specific questions about locations (“Does this go here”), removal (“Can I pull this, right?”), and insertion (“How do I insert the pin), and troubleshooting (“What should I do if the tire is not inflating?”) are also frequent. Questions about confirmations, tool selection, purpose, alignment, suitability, measurement are overall less frequent, but with a large enough minimum number of instances (> 800).

## 5. Experiments

Baselines We consider four representative Multimodal Large Language Models (MLLMs) as baselines, namely LLaVA-OneVision [39], MiniGPT4-video [8], LLaVa

6We obtain an initial categorization with language models, then we manually refine.

<!-- Page 7 -->

Step2: Human Validation

Step1: Initial QA set Extraction

E: There are some wooden pegs. T: Yes, what should I do with the wooden pegs? E: You can insert them into the large holes.

Q: What tool should I use to tighten the black bolt?

Accept

Q: What should I do with the wooden pegs?

A: Insert the wooden pegs into the big holes. B: Use the wooden pegs as reference. C: Give the wooden pegs to someone else. D: Put the wooden pegs in a corner. E: Use the hammer to break the wooden pegs.

Q: What should I use to pry open the package?

A: A screwdriver B: A hammer C: Pliers D: A wrench E: A genevile (a type of lever)

Transcription Error

Step3: Video Grounding Validation

AMT

Q: What should I do with the part that is a bit open?

AMT

A: Loosen the bolts B: Tighten it more with the bolts C: Leave it as it is D: Use a different tool E: Remove the part

Q: What is the expert suggesting will help with the current task?

Video Grounded

Discard

Q: Am I correctly holding the pieces in place?

Q: Why shouldn't I apply too much pressure when unscrewing?

A: No, I need to rotate them first B: I'm not sure, I need more guidance C: I'm holding them too loosely D: Yes, I'm holding them tightly E: I'm holding the wrong pieces

A: To avoid stripping the screw B: To avoid breaking the furniture C: To avoid using the hammer D: To avoid touching the camera E: To avoid making a mess

Not Grounded

To Revise

**Figure 6. Our multiple-choice question answers generation pipeline: 1) Extraction of QA pairs from transcripts and generation of initial set of negative answers, 2) Human validation, 3) Video Grounding Validation.**

T: Perfect, is this angle okay or should I raise it? E: Yes, yes, yes. It's fine, it's perfect.

T: How do I know that it's attached? E: Try turning it slightly. You should hear a click.

Q: Is the inclination okay or should I raise it?

Q: How do I know that it’s attached?

A: It’s fine, no need to raise it.

A: You’ll hear a click.

E: Look inside, there's a cylinder. T: This one below, okay? E: Exactly, turn it with the screwdriver, the flathead one.

E: Add a drop of milk, as they say. T: Is this fine or? E: Add a little more.

Q: This one below, okay?

Q: Is this fine or?

A: Yes, turn it with the flathead screwdriver.

A: Add a little more.

**Figure 7. Conversations between trainees and experts are naturally grounded in video since they often refer to objects or their parts. As a result, the generated QA pairs provide a high-quality, visually grounded test-bed for evaluating visual-language models.**

What should I do with the wooden pegs? What is the color of the inside? Can I wash the seeds?

What is the next step after assembling the drawers?

What is the next step after adding the lard?

Can I assemble the other part now?

Clockwise or counterclockwise? Should I go in front or behind?

Is the egg the next ingredient ?

Can I press the brake now?

Is it better to wait or not?

How do I pierce the dough?

Which one can I carry?

What is the next step?

Can I do that?

How do I release this?

Like this?

What do I do now?

Should I sift more flour on top?

Is the dough going well?

Also down here? Does this go here?

Can I increase the speed a bit?

This one here? Can I put it here on top? Where do these small screws go?

What is the purpose of the plastic clips?

Which pliers should I take?

So, if I do it this way, is it okay?

Can I pull this, right? Do I need to unscrew these guides?

What should I do if the tire is not inflating?

How do I insert the pin?

**Figure 8. VQA Benchmark Question types and examples.**

video [69], and Qwen2.5-VL [9]. We also include five LLMs to assess the ability of current models to infer the correct answer purely from text, without any video context. The considered LLMs are Llama 3.3 Instruct Turbo [4], Llama 3.1 Instruct [3] 8B and 70B, Qwen 2.5 Instruct

72B [59] and DeepSeek-R1 Turbo [19]7. All LLMs are prompted with the QA set and asked to output the correct answer. MLLMs also receive a video clip sampled before the question timestamp (δ = 5s). In the main experiments, we feel video models with 8 frames sampled uniformly from the input clip and ablate on other sampling schemes. We additionally provide a human baseline on a sample of 217 questions (an average of 54.25 per scenario)8. Benchmark Results Table 2 reports the overall and perscenario accuracy of the selected baselines on the VQA benchmark. Language-only models achieve an average accuracy close to a random baseline (20%), ranging from 8.67% with Llama 3.1 Instruct 8B (first row) to 26.65% with Llama 3.1 Instruct 70B (second row). This highlights that Ego-EXTRA is a challenging benchmark for current LLMs. Among Video-Language models, LLaVa-

7More details are in the supplementary materials. 8see the supplementary material for more details.

<!-- Page 8 -->

Model Bike Workshop Bakery Assembly Kitchen Avg.

Llama 3.1 Instruct 8B 07.63 08.62 07.45 10.96 08.67 Llama 3.1 Instruct 70B 27.57 22.54 25.19 31.30 26.65 Llama 3.3 Instruct Turbo 27.14 18.61 24.67 30.42 25.21 Qwen 2.5 Instruct 72B 20.27 15.28 19.01 21.54 19.02 DeepSeek-R1 Turbo 24.22 21.94 21.73 26.15 23.51

Language Only

MiniGPT4-video 06.62 07.09 08.26 15.74 10.68 LLaVa Video 27.01 27.16 26.12 32.09 28.55 LLaVa-OneVision 32.03 33.13 30.88 35.77 33.06 Qwen 2.5-VL 29.99 28.59 27.47 35.87 31.11 Sample Human Baseline 87.50 90.91 100 81.82 89.65

Video-Language

**Table 2. Results on the proposed VQA benchmark. We report the best results in bold and the second-best results in underline.**

Model Input Bike Workshop Bakery Assembly Kitchen Avg.

US 06.62 07.09 08.26 15.74 10.68 QA 06.81 ↑0.19 06.44 ↓0.65 07.95 ↓0.31 16.10 ↑0.36 10.70 ↑0.02 TS 06.00 ↓0.62 07.42 ↑0.33 08.47 ↑0.21 15.21 ↓0.53 10.45 ↓0.23

MiniGPT4-video

US 27.01 27.16 26.12 32.09 28.55 QA 27.62 ↑0.61 27.16 ± 0.00 26.00 ↓0.12 30.07 ↓2.02 27.89 ↓0.66 TS 26.02 ↓0.99 23.30 ↓3.86 24.56 ↓1.56 29.37 ↓2.72 26.51 ↓2.04

LLaVa Video

US 32.03 33.13 30.88 35.77 33.06 QA 26.58 ↓5.45 27.99 ↓5.14 29.41 ↓1.47 34.67 ↓1.10 29.56 ↓3.50 TS 24.05 ↓7.98 25.57 ↓7.56 21.73 ↓9.15 29.46 ↓6.31 25.29 ↓7.77

LLaVa-OneVision

US 29.99 28.59 27.47 35.87 31.11 QA 28.40 ↓1.59 27.23 ↓1.36 25.90 ↓1.57 34.09 ↓1.78 29.48 ↓1.63 TS 26.58 ↓3.41 26.63 ↓1.96 24.51 ↓2.96 32.99 ↓2.88 28.16 ↓2.95

Qwen 2.5-VL

**Table 3. Effect of sampling frames from input video. US: sampling 8 frames uniformly. QA: 8 frames before the timestamp of the question. TS: a single frame at the timestamp. Increments and decrements computed considering as reference the US input.**

OneVision and Qwen 2.5-VL obtain similar performance on average (all around ∼30%). The best performing model is LlaVa-OneVision (third row), which achieves an average accuracy of 33.06%, outperforming other models in each scenario, except for the Kitchen scenario, where Qwen 2.5VL (fourth row) obtains the highest accuracy. Nevertheless, the overall average performance of 33.06% highlights the significant challenge of the proposed VQA benchmark, especially when compared to the human baseline, which achieves an accuracy of 89.65%. See the supp. material for qualitative results. Importance of Video Sampling Following [72], we analyze three different strategies for sampling frames from the video clip given as input to the visual-language models: Uniform Sampling (sampling 8 frames uniformly), QA frames (the last 8 frames of the clip), and TS frame (a single frame at the question timestamp). This last approach also highlights whether image-level understanding is sufficient to solve our VQA benchmark. Results in Table 3 show that US is leads to best results in average, and models generally exhibit performance drops with the QA and TS schemes. Exceptions include the Bike Workshop scenario, where MiniGPT4-video and LlaVa Video achieve gains of 0.19% and 0.61%, respectively, with the QA scheme; the Bakery and Assembly scenarios, where MiniGPT4-video gains 0.33% and 0.21% with TS inputs; and the Kitchen scenario, where MiniGPT4-video improves its performance by 0.36% using QA. In general, these results highlight the importance of observing the video clip compared to a sin-

Model Video Length (s) Bike Workshop Bakery Assembly Kitchen Avg.

5 32.03 33.13 30.88 35.77 33.06 15 26.80 ↓5.23 28.59 ↓4.54 25.15 ↓5.73 33.29 ↓2.48 28.69 ↓4.37 30 26.31 ↓5.72 29.65 ↓3.48 27.52 ↓3.36 34.00 ↓1.77 29.78 ↓3.28

LLaVa-OneVision

**Table 4. Analysis on the effect of input video length uniformly sampled at 8 frames. We selected the best performing model from our VQA benchmark for this analysis. Differences are computed considering as reference the 5 seconds as video length .**

Model Input Bike Workshop Bakery Assembly Kitchen Avg. LLaVa-OneVision 7B* QA 17.45 14.98 13.90 19.20 16.62 LLaVa-OneVision 7B* QA + Transcript 24.71 27.53 33.90 27.53 29.26 LLaVa-OneVision 7B QA + Video 32.03 33.13 30.88 35.77 33.06 LLaVa-OneVision 7B QA + Video + Transcript 31.81 33.74 39.78 35.50 36.17

**Table 5. Comparison of the best performing LLaVa-OneVision 7B with its language-only counterpart and the addition of the transcript as input. * denotes the language-only counterpart, which takes only text as input.**

gle image. To further assess the importance of video length, we evaluate the performance of the best model (LLaVaOneVision), taking as input 8 frames uniformly sampled from video clips of 5, 15 and 30 seconds. Table 4 shows an average drop (last column) of 4.37% and 3.28% when increasing the temporal spans of video clips to 15 and 30 seconds, respectively. Degradation is observed across all scenarios, where accuracy decreases with longer videos. Effect of Textual Context Table 5 also compares the best performing model LLaVa-OneVision 7B with its languageonly counterpart when feeding the models with the textual transcripts related to the 5-second video clip. This allows to assess the ability of language-only models when they are provided with additional context and simulates an assistant with a basic form of memory of previous conversation turns. The comparison shows how the model benefits from video to answer questions (33.06%) compared to using only textual information (16.62%) or only the transcript (29.26%). Using video and transcript improves average performance, leading to 36.17% accuracy.

## 6. Conclusion

In this work, we introduced Ego-EXTRA, a novel egocentric dataset designed to validate intelligent wearable assistants that can provide natural language guidance in realworld scenarios. Through realistic expert-trainee conversations, Ego-EXTRA captures the complexities of procedural tasks across various skill-based domains. Based on real conversations, we designed a VQA benchmark and evaluated a range of LLMs and MLLMs, highlighting the challenging nature of the benchmark, the current limitations of text-based LLMs, and the benefits of contextualized video for MLLMs. Data and benchmark are publicly available to support the community in the validation of wearable assistants able to provide language-based expert-level guidance to users.

<!-- Page 9 -->

## Acknowledgements

This research is supported by Meta Reality Labs, Next Vision s.r.l. and by the project Future Artificial Intelligence Research (FAIR) – PNRR MUR Cod. PE0000013 - CUP: E63C22001940006.

## References

[1] Aria mps. https://facebookresearch.github. io/projectaria_tools/docs/ARK/mps. 5 [2] ehow. https://www.ehow.com/. 2 [3] Llama 3.1 instruct, . https://huggingface.co/ meta-llama/Llama-3.1-8B-Instruct. 7 [4] Llama 3.3 instruct turbo, . https://huggingface. co/meta-llama/Llama-3.3-70B-Instruct. 7 [5] wikihow. https://www.wikihow.com/Main-Page.

2 [6] youtube. https://www.youtube.com/. 2 [7] Kumar Ashutosh, Santhosh Kumar Ramakrishnan, Triantafyllos Afouras, and Kristen Grauman. Video-mined task graphs for keystep recognition in instructional videos. Advances in Neural Information Processing Systems, 36, 2024. 3 [8] Kirolos Ataallah, Xiaoqian Shen, Eslam Abdelrahman, Essam Sleiman, Deyao Zhu, Jian Ding, and Mohamed Elhoseiny. Minigpt4-video: Advancing multimodal llms for video understanding with interleaved visual-textual tokens, 2024. 3, 6 [9] Shuai Bai, Keqin Chen, Xuejing Liu, Jialin Wang, Wenbin Ge, Sibo Song, Kai Dang, Peng Wang, Shijie Wang, Jun Tang, Humen Zhong, Yuanzhi Zhu, Mingkun Yang, Zhaohai Li, Jianqiang Wan, Pengfei Wang, Wei Ding, Zheren Fu, Yiheng Xu, Jiabo Ye, Xi Zhang, Tianbao Xie, Zesen Cheng, Hang Zhang, Zhibo Yang, Haiyang Xu, and Junyang Lin. Qwen2.5-vl technical report. arXiv preprint arXiv:2502.13923, 2025. 7 [10] Jing Bi, Yunlong Tang, Luchuan Song, Ali Vosoughi, Nguyen Nguyen, and Chenliang Xu. EAGLE: egocentric AGgregated language-video engine. arXiv, 2409.17523, 2024. 3, 4 [11] Yi Chen, Yuying Ge, Yixiao Ge, Mingyu Ding, Bohao Li, Rui Wang, Ruifeng Xu, Ying Shan, and Xihui Liu. Egoplanbench: Benchmarking multimodal large language models for human-level planning. arXiv preprint arXiv:2312.06722, 2023. 3, 4, 6 [12] Yi Chen, Yuying Ge, Yixiao Ge, Mingyu Ding, Bohao Li, Rui Wang, Ruifeng Xu, Ying Shan, and Xihui Liu. EgoPlanBench: benchmarking multimodal large language models for human-level planning. arXiv, 2312.06722, 2024. 2, 4, 6 [13] Sijie Cheng, Kechen Fang, Yangyang Yu, Sicheng Zhou, Bohao Li, Ye Tian, Tingguang Li, Lei Han, and Yang Liu. VidEgoThink: assessing egocentric video understanding capabilities for embodied AI. arXiv, 2410.11623, 2024. 3 [14] Sijie Cheng, Zhicheng Guo, Jingwen Wu, Kechen Fang, Peng Li, Huaping Liu, and Yang Liu. EgoThink: evaluating first-person perspective thinking capability of vision-

language models. In Computer Vision and Pattern Recognition (CVPR), 2024. 2, 3 [15] Wenliang Dai, Junnan Li, Dongxu Li, Anthony Meng Huat Tiong, Junqi Zhao, Weisheng Wang, Boyang Li, Pascale Fung, and Steven Hoi. Instructblip: towards general-purpose vision-language models with instruction tuning. In Proceedings of the 37th International Conference on Neural Information Processing Systems, Red Hook, NY, USA, 2024. Curran Associates Inc. 3 [16] Dima Damen, Hazel Doughty, Giovanni Maria Farinella, Sanja Fidler, Antonino Furnari, Evangelos Kazakos, Davide Moltisanti, Jonathan Munro, Toby Perrett, Will Price, et al. Scaling egocentric vision: The epic-kitchens dataset. In Proceedings of the European conference on computer vision (ECCV), pages 720–736, 2018. 2 [17] Dima Damen, Hazel Doughty, Giovanni Maria Farinella, Antonino Furnari, Jian Ma, Evangelos Kazakos, Davide Moltisanti, Jonathan Munro, Toby Perrett, Will Price, and Michael Wray. Rescaling egocentric vision: Collection, pipeline and challenges for epic-kitchens-100. International Journal of Computer Vision (IJCV), 2022. 2, 3, 4 [18] Samyak Datta, Sameer Dharur, Vincent Cartillier, Ruta Desai, Mukul Khanna, Dhruv Batra, and Devi Parikh. Episodic memory question answering. 2022. 4 [19] DeepSeek-AI, Daya Guo, Dejian Yang, Haowei Zhang, Junxiao Song, Ruoyu Zhang, Runxin Xu, Qihao Zhu, Shirong Ma, Peiyi Wang, Xiao Bi, Xiaokang Zhang, Xingkai Yu, Yu Wu, Z. F. Wu, Zhibin Gou, Zhihong Shao, Zhuoshu Li, Ziyi Gao, Aixin Liu, Bing Xue, Bingxuan Wang, Bochao Wu, Bei Feng, Chengda Lu, Chenggang Zhao, Chengqi Deng, Chenyu Zhang, Chong Ruan, Damai Dai, Deli Chen, Dongjie Ji, Erhang Li, Fangyun Lin, Fucong Dai, Fuli Luo, Guangbo Hao, Guanting Chen, Guowei Li, H. Zhang, Han Bao, Hanwei Xu, Haocheng Wang, Honghui Ding, Huajian Xin, Huazuo Gao, Hui Qu, Hui Li, Jianzhong Guo, Jiashi Li, Jiawei Wang, Jingchang Chen, Jingyang Yuan, Junjie Qiu, Junlong Li, J. L. Cai, Jiaqi Ni, Jian Liang, Jin Chen, Kai Dong, Kai Hu, Kaige Gao, Kang Guan, Kexin Huang, Kuai Yu, Lean Wang, Lecong Zhang, Liang Zhao, Litong Wang, Liyue Zhang, Lei Xu, Leyi Xia, Mingchuan Zhang, Minghua Zhang, Minghui Tang, Meng Li, Miaojun Wang, Mingming Li, Ning Tian, Panpan Huang, Peng Zhang, Qiancheng Wang, Qinyu Chen, Qiushi Du, Ruiqi Ge, Ruisong Zhang, Ruizhe Pan, Runji Wang, R. J. Chen, R. L. Jin, Ruyi Chen, Shanghao Lu, Shangyan Zhou, Shanhuang Chen, Shengfeng Ye, Shiyu Wang, Shuiping Yu, Shunfeng Zhou, Shuting Pan, S. S. Li, Shuang Zhou, Shaoqing Wu, Shengfeng Ye, Tao Yun, Tian Pei, Tianyu Sun, T. Wang, Wangding Zeng, Wanjia Zhao, Wen Liu, Wenfeng Liang, Wenjun Gao, Wenqin Yu, Wentao Zhang, W. L. Xiao, Wei An, Xiaodong Liu, Xiaohan Wang, Xiaokang Chen, Xiaotao Nie, Xin Cheng, Xin Liu, Xin Xie, Xingchao Liu, Xinyu Yang, Xinyuan Li, Xuecheng Su, Xuheng Lin, X. Q. Li, Xiangyue Jin, Xiaojin Shen, Xiaosha Chen, Xiaowen Sun, Xiaoxiang Wang, Xinnan Song, Xinyi Zhou, Xianzu Wang, Xinxia Shan, Y. K. Li, Y. Q. Wang, Y. X. Wei, Yang Zhang, Yanhong Xu, Yao Li, Yao Zhao, Yaofeng Sun, Yaohui Wang, Yi Yu, Yichao Zhang, Yifan Shi, Yiliang Xiong, Ying He, Yishi Piao, Yisong Wang,

<!-- Page 10 -->

Yixuan Tan, Yiyang Ma, Yiyuan Liu, Yongqiang Guo, Yuan Ou, Yuduan Wang, Yue Gong, Yuheng Zou, Yujia He, Yunfan Xiong, Yuxiang Luo, Yuxiang You, Yuxuan Liu, Yuyang Zhou, Y. X. Zhu, Yanhong Xu, Yanping Huang, Yaohui Li, Yi Zheng, Yuchen Zhu, Yunxian Ma, Ying Tang, Yukun Zha, Yuting Yan, Z. Z. Ren, Zehui Ren, Zhangli Sha, Zhe Fu, Zhean Xu, Zhenda Xie, Zhengyan Zhang, Zhewen Hao, Zhicheng Ma, Zhigang Yan, Zhiyu Wu, Zihui Gu, Zijia Zhu, Zijun Liu, Zilin Li, Ziwei Xie, Ziyang Song, Zizheng Pan, Zhen Huang, Zhipeng Xu, Zhongyu Zhang, and Zhen Zhang. Deepseek-r1: Incentivizing reasoning capability in llms via reinforcement learning, 2025. 7 [20] Zhikang Dong, Apoorva Beedu, Jason Sheinkopf, and Irfan Essa. Mamba fusion: Learning actions through questioning. arXiv, 2409.11513, 2024. 3 [21] Hazel Doughty, Dima Damen, and Walterio Mayol-Cuevas. Who’s better? who’s best? pairwise deep ranking for skill determination. In Proceedings of the IEEE conference on computer vision and pattern recognition, pages 6057–6066, 2018. 2, 3 [22] Hazel Doughty, Walterio Mayol-Cuevas, and Dima Damen. The pros and cons: Rank-aware temporal attention for skill determination in long videos. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), 2019. 3 [23] Abhimanyu Dubey, Abhinav Jauhri, Abhinav Pandey, Abhishek Kadian, Ahmad Al-Dahle, Aiesha Letman, Akhil Mathur, Alan Schelten, Amy Yang, Angela Fan, Anirudh Goyal, Anthony Hartshorn, Aobo Yang, Archi Mitra, Archie Sravankumar, Artem Korenev, Arthur Hinsvark, Arun Rao, Aston Zhang, Aurelien Rodriguez, Austen Gregerson, Ava Spataru, Baptiste Roziere, Bethany Biron, Binh Tang, Bobbie Chern, Charlotte Caucheteux, Chaya Nayak, Chloe Bi, Chris Marra, Chris McConnell, Christian Keller, Christophe Touret, Chunyang Wu, Corinne Wong, Cristian Canton Ferrer, Cyrus Nikolaidis, Damien Allonsius, Daniel Song, Danielle Pintz, Danny Livshits, David Esiobu, Dhruv Choudhary, Dhruv Mahajan, Diego Garcia-Olano, Diego Perino, Dieuwke Hupkes, Egor Lakomkin, Ehab AlBadawy, Elina Lobanova, Emily Dinan, Eric Michael Smith, Filip Radenovic, Frank Zhang, Gabriel Synnaeve, Gabrielle Lee, Georgia Lewis Anderson, Graeme Nail, Gregoire Mialon, Guan Pang, Guillem Cucurell, Hailey Nguyen, Hannah Korevaar, Hu Xu, Hugo Touvron, Iliyan Zarov, Imanol Arrieta Ibarra, Isabel Kloumann, Ishan Misra, Ivan Evtimov, Jade Copet, Jaewon Lee, Jan Geffert, Jana Vranes, Jason Park, Jay Mahadeokar, Jeet Shah, Jelmer van der Linde, Jennifer Billock, Jenny Hong, Jenya Lee, Jeremy Fu, Jianfeng Chi, Jianyu Huang, Jiawen Liu, Jie Wang, Jiecao Yu, Joanna Bitton, Joe Spisak, Jongsoo Park, Joseph Rocca, Joshua Johnstun, Joshua Saxe, Junteng Jia, Kalyan Vasuden Alwala, Kartikeya Upasani, Kate Plawiak, Ke Li, Kenneth Heafield, Kevin Stone, Khalid El-Arini, Krithika Iyer, Kshitiz Malik, Kuenley Chiu, Kunal Bhalla, Lauren Rantala-Yeary, Laurens van der Maaten, Lawrence Chen, Liang Tan, Liz Jenkins, Louis Martin, Lovish Madaan, Lubo Malo, Lukas Blecher, Lukas Landzaat, Luke de Oliveira, Madeline Muzzi, Mahesh Pasupuleti, Mannat Singh, Manohar Paluri, Marcin Kar-

das, Mathew Oldham, Mathieu Rita, Maya Pavlova, Melanie Kambadur, Mike Lewis, Min Si, Mitesh Kumar Singh, Mona Hassan, Naman Goyal, Narjes Torabi, Nikolay Bashlykov, Nikolay Bogoychev, Niladri Chatterji, Olivier Duchenne, Onur C¸ elebi, Patrick Alrassy, Pengchuan Zhang, Pengwei Li, Petar Vasic, Peter Weng, Prajjwal Bhargava, Pratik Dubal, Praveen Krishnan, Punit Singh Koura, Puxin Xu, Qing He, Qingxiao Dong, Ragavan Srinivasan, Raj Ganapathy, Ramon Calderer, Ricardo Silveira Cabral, Robert Stojnic, Roberta Raileanu, Rohit Girdhar, Rohit Patel, Romain Sauvestre, Ronnie Polidoro, Roshan Sumbaly, Ross Taylor, Ruan Silva, Rui Hou, Rui Wang, Saghar Hosseini, Sahana Chennabasappa, Sanjay Singh, Sean Bell, Seohyun Sonia Kim, Sergey Edunov, Shaoliang Nie, Sharan Narang, Sharath Raparthy, Sheng Shen, Shengye Wan, Shruti Bhosale, Shun Zhang, Simon Vandenhende, Soumya Batra, Spencer Whitman, Sten Sootla, Stephane Collot, Suchin Gururangan, Sydney Borodinsky, Tamar Herman, Tara Fowler, Tarek Sheasha, Thomas Georgiou, Thomas Scialom, Tobias Speckbacher, Todor Mihaylov, Tong Xiao, Ujjwal Karn, Vedanuj Goswami, Vibhor Gupta, Vignesh Ramanathan, Viktor Kerkez, Vincent Gonguet, Virginie Do, Vish Vogeti, Vladan Petrovic, Weiwei Chu, Wenhan Xiong, Wenyin Fu, Whitney Meers, Xavier Martinet, Xiaodong Wang, Xiaoqing Ellen Tan, Xinfeng Xie, Xuchao Jia, Xuewei Wang, Yaelle Goldschlag, Yashesh Gaur, Yasmine Babaei, Yi Wen, Yiwen Song, Yuchen Zhang, Yue Li, Yuning Mao, Zacharie Delpierre Coudert, Zheng Yan, Zhengxing Chen, Zoe Papakipos, Aaditya Singh, Aaron Grattafiori, Abha Jain, Adam Kelsey, Adam Shajnfeld, Adithya Gangidi, Adolfo Victoria, Ahuva Goldstand, Ajay Menon, Ajay Sharma, Alex Boesenberg, Alex Vaughan, Alexei Baevski, Allie Feinstein, Amanda Kallet, Amit Sangani, Anam Yunus, Andrei Lupu, Andres Alvarado, Andrew Caples, Andrew Gu, Andrew Ho, Andrew Poulton, Andrew Ryan, Ankit Ramchandani, Annie Franco, Aparajita Saraf, Arkabandhu Chowdhury, Ashley Gabriel, Ashwin Bharambe, Assaf Eisenman, Azadeh Yazdan, Beau James, Ben Maurer, Benjamin Leonhardi, Bernie Huang, Beth Loyd, Beto De Paola, Bhargavi Paranjape, Bing Liu, Bo Wu, Boyu Ni, Braden Hancock, Bram Wasti, Brandon Spence, Brani Stojkovic, Brian Gamido, Britt Montalvo, Carl Parker, Carly Burton, Catalina Mejia, Changhan Wang, Changkyu Kim, Chao Zhou, Chester Hu, ChingHsiang Chu, Chris Cai, Chris Tindal, Christoph Feichtenhofer, Damon Civin, Dana Beaty, Daniel Kreymer, Daniel Li, Danny Wyatt, David Adkins, David Xu, Davide Testuggine, Delia David, Devi Parikh, Diana Liskovich, Didem Foss, Dingkang Wang, Duc Le, Dustin Holland, Edward Dowling, Eissa Jamil, Elaine Montgomery, Eleonora Presani, Emily Hahn, Emily Wood, Erik Brinkman, Esteban Arcaute, Evan Dunbar, Evan Smothers, Fei Sun, Felix Kreuk, Feng Tian, Firat Ozgenel, Francesco Caggioni, Francisco Guzm´an, Frank Kanayet, Frank Seide, Gabriela Medina Florez, Gabriella Schwarz, Gada Badeer, Georgia Swee, Gil Halpern, Govind Thattai, Grant Herman, Grigory Sizov, Guangyi, Zhang, Guna Lakshminarayanan, Hamid Shojanazeri, Han Zou, Hannah Wang, Hanwen Zha, Haroun Habeeb, Harrison Rudolph, Helen Suk, Henry Aspegren,

<!-- Page 11 -->

Hunter Goldman, Ibrahim Damlaj, Igor Molybog, Igor Tufanov, Irina-Elena Veliche, Itai Gat, Jake Weissman, James Geboski, James Kohli, Japhet Asher, Jean-Baptiste Gaya, Jeff Marcus, Jeff Tang, Jennifer Chan, Jenny Zhen, Jeremy Reizenstein, Jeremy Teboul, Jessica Zhong, Jian Jin, Jingyi Yang, Joe Cummings, Jon Carvill, Jon Shepard, Jonathan McPhie, Jonathan Torres, Josh Ginsburg, Junjie Wang, Kai Wu, Kam Hou U, Karan Saxena, Karthik Prasad, Kartikay Khandelwal, Katayoun Zand, Kathy Matosich, Kaushik Veeraraghavan, Kelly Michelena, Keqian Li, Kun Huang, Kunal Chawla, Kushal Lakhotia, Kyle Huang, Lailin Chen, Lakshya Garg, Lavender A, Leandro Silva, Lee Bell, Lei Zhang, Liangpeng Guo, Licheng Yu, Liron Moshkovich, Luca Wehrstedt, Madian Khabsa, Manav Avalani, Manish Bhatt, Maria Tsimpoukelli, Martynas Mankus, Matan Hasson, Matthew Lennie, Matthias Reso, Maxim Groshev, Maxim Naumov, Maya Lathi, Meghan Keneally, Michael L. Seltzer, Michal Valko, Michelle Restrepo, Mihir Patel, Mik Vyatskov, Mikayel Samvelyan, Mike Clark, Mike Macey, Mike Wang, Miquel Jubert Hermoso, Mo Metanat, Mohammad Rastegari, Munish Bansal, Nandhini Santhanam, Natascha Parks, Natasha White, Navyata Bawa, Nayan Singhal, Nick Egebo, Nicolas Usunier, Nikolay Pavlovich Laptev, Ning Dong, Ning Zhang, Norman Cheng, Oleg Chernoguz, Olivia Hart, Omkar Salpekar, Ozlem Kalinli, Parkin Kent, Parth Parekh, Paul Saab, Pavan Balaji, Pedro Rittner, Philip Bontrager, Pierre Roux, Piotr Dollar, Polina Zvyagina, Prashant Ratanchandani, Pritish Yuvraj, Qian Liang, Rachad Alao, Rachel Rodriguez, Rafi Ayub, Raghotham Murthy, Raghu Nayani, Rahul Mitra, Raymond Li, Rebekkah Hogan, Robin Battey, Rocky Wang, Rohan Maheswari, Russ Howes, Ruty Rinott, Sai Jayesh Bondu, Samyak Datta, Sara Chugh, Sara Hunt, Sargun Dhillon, Sasha Sidorov, Satadru Pan, Saurabh Verma, Seiji Yamamoto, Sharadh Ramaswamy, Shaun Lindsay, Shaun Lindsay, Sheng Feng, Shenghao Lin, Shengxin Cindy Zha, Shiva Shankar, Shuqiang Zhang, Shuqiang Zhang, Sinong Wang, Sneha Agarwal, Soji Sajuyigbe, Soumith Chintala, Stephanie Max, Stephen Chen, Steve Kehoe, Steve Satterfield, Sudarshan Govindaprasad, Sumit Gupta, Sungmin Cho, Sunny Virk, Suraj Subramanian, Sy Choudhury, Sydney Goldman, Tal Remez, Tamar Glaser, Tamara Best, Thilo Kohler, Thomas Robinson, Tianhe Li, Tianjun Zhang, Tim Matthews, Timothy Chou, Tzook Shaked, Varun Vontimitta, Victoria Ajayi, Victoria Montanez, Vijai Mohan, Vinay Satish Kumar, Vishal Mangla, V´ıtor Albiero, Vlad Ionescu, Vlad Poenaru, Vlad Tiberiu Mihailescu, Vladimir Ivanov, Wei Li, Wenchen Wang, Wenwen Jiang, Wes Bouaziz, Will Constable, Xiaocheng Tang, Xiaofang Wang, Xiaojian Wu, Xiaolan Wang, Xide Xia, Xilun Wu, Xinbo Gao, Yanjun Chen, Ye Hu, Ye Jia, Ye Qi, Yenda Li, Yilin Zhang, Ying Zhang, Yossi Adi, Youngjin Nam, Yu, Wang, Yuchen Hao, Yundi Qian, Yuzi He, Zach Rait, Zachary DeVito, Zef Rosnbrick, Zhaoduo Wen, Zhenyu Yang, and Zhiwei Zhao. The llama 3 herd of models, 2024. 3, 5, 6

[24] Chenyou Fan. Egovqa - an egocentric video question answering benchmark dataset. In 2019 IEEE/CVF International Conference on Computer Vision Workshop (ICCVW), page

4359–4366, Seoul, Korea (South), 2019. IEEE. 4 [25] Alessandro Flaborea, Guido Maria D’Amely di Melendugno, Leonardo Plini, Luca Scofano, Edoardo De Matteis, Antonino Furnari, Giovanni Maria Farinella, and Fabio Galasso. Prego: Online mistake detection in procedural egocentric videos. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), pages 18483–18492, 2024. 2 [26] Difei Gao, Ruiping Wang, Ziyi Bai, and Xilin Chen. Env-qa: A video question answering benchmark for comprehensive understanding of dynamic environments. In 2021 IEEE/CVF International Conference on Computer Vision (ICCV), page 1655–1665, Montreal, QC, Canada, 2021. IEEE. 3, 4 [27] Rohit Girdhar and Kristen Grauman. Anticipative video transformer. In Proceedings of the IEEE/CVF international conference on computer vision, pages 13505–13515, 2021. 3 [28] Kristen Grauman, Andrew Westbury, Eugene Byrne, Zachary Chavis, Antonino Furnari, Rohit Girdhar, Jackson Hamburger, Hao Jiang, Miao Liu, Xingyu Liu, Miguel Martin, Tushar Nagarajan, Ilija Radosavovic, Santhosh Kumar Ramakrishnan, Fiona Ryan, Jayant Sharma, Michael Wray, Mengmeng Xu, Eric Zhongcong Xu, Chen Zhao, Siddhant Bansal, Dhruv Batra, Vincent Cartillier, Sean Crane, Tien Do, Morrie Doulaty, Akshay Erapalli, Christoph Feichtenhofer, Adriano Fragomeni, Qichen Fu, Abrham Gebreselasie, Cristina Gonz´alez, James Hillis, Xuhua Huang, Yifei Huang, Wenqi Jia, Weslie Khoo, J´achym Kol´aˇr, Satwik Kottur, Anurag Kumar, Federico Landini, Chao Li, Yanghao Li, Zhenqiang Li, Karttikeya Mangalam, Raghava Modhugu, Jonathan Munro, Tullie Murrell, Takumi Nishiyasu, Will Price, Paola Ruiz, Merey Ramazanova, Leda Sari, Kiran Somasundaram, Audrey Southerland, Yusuke Sugano, Ruijie Tao, Minh Vo, Yuchen Wang, Xindi Wu, Takuma Yagi, Ziwei Zhao, Yunyi Zhu, Pablo Arbel´aez, David Crandall, Dima Damen, Giovanni Maria Farinella, Christian Fuegen, Bernard Ghanem, Vamsi Krishna Ithapu, C. V. Jawahar, Hanbyul Joo, Kris Kitani, Haizhou Li, Richard Newcombe, Aude Oliva, Hyun Soo Park, James M. Rehg, Yoichi Sato, Jianbo Shi, Mike Zheng Shou, Antonio Torralba, Lorenzo Torresani, Mingfei Yan, and Jitendra Malik. Ego4d: Around the world in 3,000 hours of egocentric video. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), pages 18995–19012, 2022. 2, 3, 4 [29] Kristen Grauman, Andrew Westbury, Lorenzo Torresani, Kris Kitani, Jitendra Malik, Triantafyllos Afouras, Kumar Ashutosh, Vijay Baiyya, Siddhant Bansal, Bikram Boote, Eugene Byrne, Zach Chavis, Joya Chen, Feng Cheng, FuJen Chu, Sean Crane, Avijit Dasgupta, Jing Dong, Maria Escobar, Cristhian Forigua, Abrham Gebreselasie, Sanjay Haresh, Jing Huang, Md Mohaiminul Islam, Suyog Jain, Rawal Khirodkar, Devansh Kukreja, Kevin J Liang, JiaWei Liu, Sagnik Majumder, Yongsen Mao, Miguel Martin, Effrosyni Mavroudi, Tushar Nagarajan, Francesco Ragusa, Santhosh Kumar Ramakrishnan, Luigi Seminara, Arjun Somayazulu, Yale Song, Shan Su, Zihui Xue, Edward Zhang, Jinxu Zhang, Angela Castillo, Changan Chen, Xinzhu Fu, Ryosuke Furuta, Cristina Gonzalez, Prince Gupta, Jiabo Hu, Yifei Huang, Yiming Huang, Weslie Khoo, Anush Ku-

<!-- Page 12 -->

mar, Robert Kuo, Sach Lakhavani, Miao Liu, Mi Luo, Zhengyi Luo, Brighid Meredith, Austin Miller, Oluwatumininu Oguntola, Xiaqing Pan, Penny Peng, Shraman Pramanick, Merey Ramazanova, Fiona Ryan, Wei Shan, Kiran Somasundaram, Chenan Song, Audrey Southerland, Masatoshi Tateno, Huiyu Wang, Yuchen Wang, Takuma Yagi, Mingfei Yan, Xitong Yang, Zecheng Yu, Shengxin Cindy Zha, Chen Zhao, Ziwei Zhao, Zhifan Zhu, Jeff Zhuo, Pablo Arbelaez, Gedas Bertasius, Dima Damen, Jakob Engel, Giovanni Maria Farinella, Antonino Furnari, Bernard Ghanem, Judy Hoffman, C.V. Jawahar, Richard Newcombe, Hyun Soo Park, James M. Rehg, Yoichi Sato, Manolis Savva, Jianbo Shi, Mike Zheng Shou, and Michael Wray. Ego-exo4d: Understanding skilled human activity from first- and thirdperson perspectives. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), pages 19383–19400, 2024. 2, 3, 4 [30] Kimihiro Hasegawa, Wiradee Imrattanatrai, Zhi-Qi Cheng, Masaki Asada, Susan Holm, Yuran Wang, Ken Fukuda, and Teruko Mitamura. ProMQA: question answering dataset for multimodal procedural activity understanding. arXiv, 2410.22211, 2024. 4 [31] Kimihiro Hasegawa, Wiradee Imrattanatrai, Zhi-Qi Cheng, Masaki Asada, Susan Holm, Yuran Wang, Ken Fukuda, and Teruko Mitamura. Promqa: Question answering dataset for multimodal procedural activity understanding, 2024. 3 [32] Md Mohaiminul Islam, Tushar Nagarajan, Huiyu Wang, FuJen Chu, Kris Kitani, Gedas Bertasius, and Xitong Yang. Propose, assess, search: Harnessing llms for goal-oriented planning in instructional videos. In European Conference on Computer Vision, 2024. 2 [33] Youngkyoon Jang, Brian Sullivan, Casimir Ludwig, Iain Gilchrist, Dima Damen, and Walterio Mayol-Cuevas. Epictent: An egocentric video dataset for camping tent assembly. In Proceedings of the IEEE/CVF International Conference on Computer Vision Workshops, pages 0–0, 2019. 3 [34] Baoxiong Jia, Yixin Chen, Siyuan Huang, Yixin Zhu, and Song-Chun Zhu. Lemma: A multi-view dataset for learning multi-agent multi-task activities. page 767–786, Berlin, Heidelberg, 2020. Springer-Verlag. 3, 4 [35] Baoxiong Jia, Ting Lei, Song-Chun Zhu, and Siyuan Huang. Egotaskqa: Understanding human tasks in egocentric videos. (arXiv:2210.03929), 2022. arXiv:2210.03929 [cs]. 3, 4 [36] Takeo Kanade and Martial Hebert. First-person vision. Proceedings of the IEEE, 100(8):2442–2453, 2012. 3 [37] John F. (”Jeff”) Kelley. Wizard of oz (woz): a yellow brick journey. J. Usability Studies, 13(3):119–124, 2018. 2, 3 [38] Bohao Li, Yuying Ge, Yixiao Ge, Guangzhi Wang, Rui Wang, Ruimao Zhang, and Ying Shan. Seed-bench: Benchmarking multimodal large language models. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), pages 13299–13308, 2024. 6 [39] Bo Li, Yuanhan Zhang, Dong Guo, Renrui Zhang, Feng Li, Hao Zhang, Kaichen Zhang, Peiyuan Zhang, Yanwei Li, Ziwei Liu, and Chunyuan Li. Llava-onevision: Easy visual task transfer, 2024. 3, 6 [40] Bin Lin, Bin Zhu, Yang Ye, Munan Ning, Peng Jin, and Li Yuan. Video-llava: Learning united visual represen-

tation by alignment before projection. arXiv preprint arXiv:2311.10122, 2023. 3 [41] Yuan Liu, Haodong Duan, Yuanhan Zhang, Bo Li, Songyang Zhang, Wangbo Zhao, Yike Yuan, Jiaqi Wang, Conghui He, Ziwei Liu, Kai Chen, and Dahua Lin. Mmbench: Is your multi-modal model an all-around player? In Computer Vision – ECCV 2024, pages 216–233, Cham, 2025. Springer Nature Switzerland. 6 [42] Karttikeya Mangalam, Raiymbek Akshulakov, and Jitendra Malik. Egoschema: A diagnostic benchmark for very longform video language understanding. ArXiv, abs/2308.09126, 2023. 2, 3 [43] Michele Mazzamuto, Antonino Furnari, Yoichi Sato, and Giovanni Maria Farinella. Gazing into missteps: Leveraging eye-gaze for unsupervised mistake detection in egocentric videos of skilled human activities. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), pages 8310–8320, 2025. 2 [44] Himangi Mittal, Nakul Agarwal, Shao-Yuan Lo, and Kwonjoon Lee. Can’t make an omelette without breaking some eggs: Plausible action anticipation using large videolanguage models. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 18580–18590, 2024. 3 [45] Nikola Mrkˇsi´c, Diarmuid ´O S´eaghdha, Tsung-Hsien Wen, Blaise Thomson, and Steve Young. Neural belief tracker: Data-driven dialogue state tracking. In Proceedings of the 55th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), pages 1777–1788, Vancouver, Canada, 2017. Association for Computational Linguistics. 2, 3 [46] Curtis Northcutt, Shengxin Zha, Steven Lovegrove, and Richard Newcombe. Egocom: A multi-person multi-modal egocentric communications dataset. IEEE Transactions on Pattern Analysis and Machine Intelligence, 2020. 3 [47] Rohith Peddi, Shivvrat Arya, Bharath Challa, Likhitha Pallapothula, Akshay Vyas, Bhavya Gouripeddi, Jikai Wang, Qifan Zhang, Vasundhara Komaragiri, Eric Ragan, Nicholas Ruozzi, Yu Xiang, and Vibhav Gogate. CaptainCook4D: A Dataset for Understanding Errors in Procedural Activities, 2024. 3, 4 [48] Chiara Plizzari, Gabriele Goletto, Antonino Furnari, Siddhant Bansal, Francesco Ragusa, Giovanni Maria Farinella, Dima Damen, and Tatiana Tommasi. An outlook into the future of egocentric vision. International Journal of Computer Vision, pages 1–57, 2024. 2, 3 [49] Francesco Ragusa, Antonino Furnari, and Giovanni Maria Farinella. Meccano: A multimodal egocentric dataset for humans behavior understanding in the industrial-like domain. Computer Vision and Image Understanding (CVIU), 2023. 4 [50] Francesco Ragusa, Rosario Leonardi, Michele Mazzamuto, Claudia Bonanno, Rosario Scavo, Antonino Furnari, and Giovanni Maria Farinella. Enigma-51: Towards a finegrained understanding of human-object interactions in industrial scenarios. IEEE Winter Conference on Application of Computer Vision (WACV), 2024. 3, 4 [51] Tim J. Schoonbeek, Tim Houben, Hans Onvlee, Peter H.N. de With, and Fons van der Sommen. Industreal: A dataset

<!-- Page 13 -->

for procedure step recognition handling execution errors in egocentric videos in an industrial-like setting. In Proceedings of the IEEE/CVF Winter Conference on Applications of Computer Vision (WACV), pages 4365–4374, 2024. 3 [52] Luigi Seminara, Giovanni Maria Farinella, and Antonino Furnari. Differentiable task graph learning: Procedural activity representation and online mistake detection from egocentric videos, 2024. 2, 3 [53] F. Sener, D. Chatterjee, D. Shelepov, K. He, D. Singhania, R. Wang, and A. Yao. Assembly101: A large-scale multi-view video dataset for understanding procedural activities. CVPR 2022. 3, 4 [54] Fadime Sener, Dipika Singhania, and Angela Yao. Temporal aggregate representations for long-range video understanding. In Computer Vision–ECCV 2020: 16th European Conference, Glasgow, UK, August 23–28, 2020, Proceedings, Part XVI 16, pages 154–171. Springer, 2020. 3 [55] Kiran K. Somasundaram, Jing Dong, Huixuan Tang, Julian Straub, Mingfei Yan, Michael Goesele, Jakob J. Engel, Renzo De Nardi, and Richard A. Newcombe. Project aria: A new tool for egocentric multi-modal ai research. ArXiv, abs/2308.13561, 2023. 2, 3 [56] Yale Song, Eugene Byrne, Tushar Nagarajan, Huiyu Wang, Miguel Martin, and Lorenzo Torresani. Ego4d goal-step: Toward hierarchical understanding of procedural activities. In Advances in Neural Information Processing Systems, pages 38863–38886. Curran Associates, Inc., 2023. 2 [57] Ying Su, Zhan Ling, Haochen Shi, Jiayang Cheng, Yauwai Yim, and Yangqiu Song. ActPlan-1K: benchmarking the procedural planning ability of visual language models in household activities. arXiv, 2410.03907, 2024. 3, 4 [58] Chameleon Team. Chameleon: Mixed-modal early-fusion foundation models. arXiv preprint arXiv:2405.09818, 2024. 3 [59] Qwen Team. Qwen2.5: A party of foundation models, 2024.

7 [60] Tobii. Tobii pro fusion bar. https://www.tobii. com/products/eye-trackers/screen-based/ tobii-pro-fusion. 3 [61] Hugo Touvron, Thibaut Lavril, Gautier Izacard, Xavier Martinet, Marie-Anne Lachaux, Timoth´ee Lacroix, Baptiste Rozi`ere, Naman Goyal, Eric Hambro, Faisal Azhar, et al. Llama: Open and efficient foundation language models. arXiv preprint arXiv:2302.13971, 2023. 3 [62] Hugo Touvron, Louis Martin, Kevin Stone, Peter Albert, Amjad Almahairi, Yasmine Babaei, Nikolay Bashlykov, Soumya Batra, Prajjwal Bhargava, Shruti Bhosale, Dan Bikel, Lukas Blecher, Cristian Canton Ferrer, Moya Chen, Guillem Cucurull, David Esiobu, Jude Fernandes, Jeremy Fu, Wenyin Fu, Brian Fuller, Cynthia Gao, Vedanuj Goswami, Naman Goyal, Anthony Hartshorn, Saghar Hosseini, Rui Hou, Hakan Inan, Marcin Kardas, Viktor Kerkez, Madian Khabsa, Isabel Kloumann, Artem Korenev, Punit Singh Koura, Marie-Anne Lachaux, Thibaut Lavril, Jenya Lee, Diana Liskovich, Yinghai Lu, Yuning Mao, Xavier Martinet, Todor Mihaylov, Pushkar Mishra, Igor Molybog, Yixin Nie, Andrew Poulton, Jeremy Reizenstein, Rashi Rungta, Kalyan Saladi, Alan Schelten, Ruan

Silva, Eric Michael Smith, Ranjan Subramanian, Xiaoqing Ellen Tan, Binh Tang, Ross Taylor, Adina Williams, Jian Xiang Kuan, Puxin Xu, Zheng Yan, Iliyan Zarov, Yuchen Zhang, Angela Fan, Melanie Kambadur, Sharan Narang, Aurelien Rodriguez, Robert Stojnic, Sergey Edunov, and Thomas Scialom. Llama 2: Open foundation and finetuned chat models, 2023. [63] A Vaswani. Attention is all you need. Advances in Neural Information Processing Systems, 2017. 3 [64] P. Wang, S. Bai, S. Tan, S. Wang, Z. Fan, J. Bai, K. Chen, X. Liu, J. Wang, W. Ge, Y. Fan, K. Dang, M. Du, X. Ren, R. Men, D. Liu, C. Zhou, J. Zhou, and J. Lin. Qwen2-VL: enhancing vision-language models perception of the world at any resolution. arXiv, 2409.12191, 2024. 3 [65] Xin Wang, Taein Kwon, Mahdi Rad, Bowen Pan, Ishani Chakraborty, Sean Andrist, Dan Bohus, Ashley Feniello, Bugra Tekin, Felipe Vieira Frujeri, Neel Joshi, and Marc Pollefeys. Holoassist: an egocentric human interaction dataset for interactive ai assistants in the real world. In Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV), pages 20270–20281, 2023. 3, 4 [66] Benita Wong, Joya Chen, You Wu, Stan Weixian Lei, Dongxing Mao, Difei Gao, and Mike Zheng Shou. Assistq: Affordance-centric question-driven task completion for egocentric assistant. (arXiv:2203.04203), 2022. arXiv:2203.04203 [cs]. 3 [67] H. Ye, H. Zhang, E. Daxberger, L. Chen, Z. Lin, Y. Li, B. Zhang, H. You, D. Xu, Z. Gan, J. Lu, and Y. Yang. MMEgo: towards building egocentric multimodal LLMs. arXiv, 2410.07177, 2024. 3, 4 [68] Chen-Lin Zhang, Jianxin Wu, and Yin Li. Actionformer: Localizing moments of actions with transformers. In European Conference on Computer Vision. Springer, 2022. 3 [69] Yuanhan Zhang, Jinming Wu, Wei Li, Bo Li, Zejun Ma, Ziwei Liu, and Chunyuan Li. Video instruction tuning with synthetic data, 2024. 7 [70] Qi Zhao, Shijie Wang, Ce Zhang, Changcheng Fu, Minh Quan Do, Nakul Agarwal, Kwonjoon Lee, and Chen Sun. Antgpt: Can large language models help longterm action anticipation from videos? arXiv preprint arXiv:2307.16368, 2023. 3 [71] Honglu Zhou, Roberto Mart´ın-Mart´ın, Mubbasir Kapadia, Silvio Savarese, and Juan Carlos Niebles. Procedure-aware pretraining for instructional video understanding. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 10727–10738, 2023. 3 [72] Sheng Zhou, Junbin Xiao, Qingyun Li, Yicong Li, Xun Yang, Dan Guo, Meng Wang, Tat-Seng Chua, and Angela Yao. Egotextvqa: Towards egocentric scene-text aware video question answering, 2025. 8
