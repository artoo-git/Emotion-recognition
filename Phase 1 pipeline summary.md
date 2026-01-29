# PHASE 1 Pipeline: From Audio diaries to a Pain-Emotion Classifier 

## Summary

This pipeline aims to design a **pain-specific fine-tuned emotion classification** model to analyse the emotional content of transcripts of chronic pain diaries. In particular we aim to start from audio input and arrive at a model that can classify the emotional layer of information carried in sentences. In doing this we want to both overcome the limitation of generic emotion classifiers, and build a new classifier that is specific to the experience of chronic pain. We do this by adapting (fine-tuning) a pre-trained multi-label emotion model (roberta-base-go_emotions) to capture pain-specific emotional constructs (catastrophizing, helplessness, etc.).

Audio recordings → Transcripts → Sentence segmentation → Annotation (500(?) sentences) → Fine-tuned classifier → Ready to classify full dataset

## Key Decision Points & Rationale

| Issue | Decision | Why |
|-------|----------|-----|
| **Generic vs. Pain-Specific Emotions** | Pain-specific fine-tuning | Generic 6-emotion classifiers (Hartmann) miss pain psychosocial constructs; distributional embeddings based on pain-specific emotions will have enhanced validity in the context of studying the experience of people living with chronic pain|
| **Base Emotion Classifier** | [SamLowe/roberta-base-go_emotions](https://huggingface.co/SamLowe/roberta-base-go_emotions) (28 emotions) | Pre-trained on [GoEmotions](https://huggingface.co/datasets/google-research-datasets/go_emotions) dataset (58k Reddit comments), multi-label capable, transparent metrics, 570k downloads, widely battle-tested |
| **Fine-tuning** | Fine-tune on (??500??) pain-labeled sentences | Using pre-trained roberta-base-go_emotions alone will miss out on pain semantics (e.g., "catastrophizing" != "fear"); fine-tuning on our naturalistic pain diary data captures pain-specific emotional constructs and their linguistic expressions; transfer learning from GoEmotions prevents overfitting by providing emotion-semantic regularization|
| **Classifier Output** | This will be our only our custom emotion labels. If we have (e.g.) $n=12$ unique pain emotions, these are all the emotions our fine tuned classifier will use) | This is because we are fine-tuning our model setting $num_labels=12$, thus PyTorch will then replace the classification head ( which in roberta-base-go_emotions was num_label = 28) with our number of emotions and output our emotions labes instead of the 28 of the original head |
| **Avoid Data Circularity** | Hold-out validation design | We fine-tune the classifier on 500 annotated sentences (with 70/30 train/validation split for model selection), then apply the trained classifier to the $n=total-500$ held-out sentences that were never used in training|
| **Annotation: how many** | 500(??) sentences| With transfer learning is known to require much less annotation without incurringin overfitting https://aclanthology.org/P18-1031/, therefore as we start from roberta-base-go_emotions we would likely not need to code too many sentences. But surely we will need a sufficient number of examples for each emotion |

## From roberta-base-go_emotions to emoPain-roberta-go_emotion model 

```
What we keep: RoBERTa encoder + hidden layers
├─ Still contains 28-emotion knowledge from GoEmotions
├─ Will be fine-tuned on our 500 sentences
└─ Learns to extract pain-specific emotion features

What we change: Classification head (output layer)
├─ Completely new: 10 neurons (these are trained to classify our emotions only)
├─ Randomly initialized (no pre-trained knowledge)
├─ Trained on our 500 sentences
└─ Learns to MAP hidden features → our 12 labels
```

## Fine Tuning and Classification Head Replacement

We fine-tune the 28-emotion capable classifier (roberta-base-go_emotions) with our (e.g.) 12 pain emotions:

```
BEFORE fine-tuning (SamLowe/roberta-base-go_emotions):
├─ Input: Text
├─ Classification head: 28 output neurons (28 emotions)
└─ Output: Predictions for all 28 emotions

AFTER fine-tuning with (e,g,) 12 labels (UCL/emoPain-roberta-go_emotion model):
├─ Input: Text
├─ Classification head: REPLACED with 12 output neurons (our emotions)
├─ New head: Randomly initialized, then trained on our 500 sentences
└─ Output: ONLY our 12 emotions
```


```

