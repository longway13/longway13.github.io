---
layout: post
title: Rethinking the Role of Demonstrations: What Makes In-Context Learning Work?
subtitle: Understanding the Mechanisms Behind In-Context Learning
cover-img: /assets/img/icl-cover.jpg
thumbnail-img: /assets/img/icl-thumb.png
share-img: /assets/img/icl-cover.jpg
tags: [NLP, In-Context Learning, AI]
author: Gwanwoo Song
---

# Rethinking the Role of Demonstrations: What Makes In-Context Learning Work?

**Sewon Min, Xinxi Lyu, Ari Holtzman, Mikel Artetxe, Mike Lewis, Hannaneh Hajishirzi, and Luke Zettlemoyer**  
**February 25, 2022**

## Introduction

In this post, I introduce an intriguing NLP paper on large language models.  
Since [Brown et al.](https://arxiv.org/abs/2005.14165) introduced in-context learning—a method where language models are prompted with a few examples (input-output pairs)—numerous studies have explored its potential (e.g., [Wei et al.](https://arxiv.org/abs/2201.11903), [Wang et al.](https://arxiv.org/pdf/2203.11171)).  

In-context learning (ICL) has shown remarkable generalization across various domains. However, we still lack a deep understanding of **why it works**.  
This research investigates different aspects of demonstrations that influence ICL performance through experiments and quantitative analyses.

## In-Context Learning (ICL)

Before diving into the findings, let's briefly review in-context learning.  
ICL enables models to learn tasks **at inference time** using demonstrations, which consist of (input, output) pairs. The model uses these examples to infer the relationship between input and expected output.

### Figure 1: Illustration of In-Context Learning  
![ICL Example](/figures/icl-preview.png)  
*Figure from [Dong et al.](https://arxiv.org/abs/2301.00234)*

In the example above, the task is **sentiment analysis**. The model learns how to classify sentiment using demonstrations. Without these examples, the model may struggle to make accurate predictions.

### Figure 2: Visualization of ICL’s Effect  
![ICL Visualization](/figures/icl-handmade.svg)  

This informal visualization illustrates how demonstrations help the model approximate the **true space** of valid outputs.  
- Without demonstrations, the model’s output space diverges significantly from the desired target space.
- With demonstrations, the model aligns its predictions much closer to the true space.

This suggests that demonstrations **condition the model more precisely to the given task**.

## Key Findings

### **1. Ground-truth labels are not necessary for ICL**  
One of the most surprising findings is that **correct labels are not required for in-context learning to be effective**.

#### Figure 3: ICL with Correct vs. Incorrect Labels  
![ICL Labels](/figures/rethinking.svg)  

Despite conventional wisdom, models perform **almost equally well** with **random labels** as they do with correct ones.  
This contrasts sharply with supervised learning, where ground-truth labels are critical for good performance.

## Experimental Setup

### **Model**
- Six decoder-only language models (sizes: 774M to 175B parameters)
- Two inference methods: **direct** and **channel** inference ([Min et al.](https://arxiv.org/abs/2108.04106))

### **Dataset**
- 26 datasets, including **sentiment analysis, paraphrase detection, and NLI**
- Tasks formatted as **classification or multiple-choice**
- Covers domains like **science, finance, and general NLP benchmarks (GLUE, etc.)**

## Does the Model Understand Input-Label Correspondence?

The study investigates whether the model **actually learns input-label correspondence** or merely leverages other properties of demonstrations.  
To explore this, they compare:

1. **Zero-shot (no demonstrations)**  
   - Prediction: **argmax** over label probabilities given the input.

2. **Gold-label demonstrations**  
   - Standard ICL setup with **correctly paired** input-label examples.

3. **Random-label demonstrations**  
   - Same inputs, but labels are **randomly assigned** from the candidate set.

#### Figure 4: Model Performance Under Different Demonstration Settings  
![ICL Performance](/figures/main_cls_3.png)  
![ICL Performance Multi-Choice](/figures/main_mc_3.png)  

Models perform **almost identically** whether using correct or random labels.  
This implies that the **mere presence of demonstrations is helpful**, but their correctness may not be crucial.

## Other Key Factors in Demonstrations

### **1. The distribution of input text matters**  
The model benefits when the input examples match its **training distribution**.  
Using **out-of-distribution (OOD) inputs** significantly degrades performance.

#### Figure 7: Impact of Input Distribution  
![Input Distribution](/figures/abl_input_total.png)  

- If inputs are sampled from a different domain, **ICL effectiveness drops significantly**.
- The model appears to rely heavily on recognizing familiar **linguistic patterns** rather than true reasoning.

### **2. The label space matters**  
The model benefits when the label space is **well-defined**.

#### Figure 8: Impact of Label Space  
![Label Space](/figures/abl_label_total.png)  

- If labels are replaced with random **English words**, performance **drops significantly** (especially for direct inference models).
- Channel models are **less sensitive** to the label space.

## Conclusion

This study challenges our assumptions about in-context learning.  
- **Correct input-label mapping is less important than expected**.  
- **ICL does not teach the model new tasks**—instead, it **retrieves and aligns pre-existing knowledge**.

### **Limitations**
- The study primarily focuses on **classification tasks** (e.g., sentiment analysis, paraphrase detection).  
  - **More complex tasks** may rely on correct labels to a greater extent.
- Results are **averaged across tasks**, making it hard to determine whether the model is leveraging specific aspects of demonstrations.

## References
- Min, S., Lyu, X., Holtzman, A., Artetxe, M., Lewis, M., Hajishirzi, H., & Zettlemoyer, L. (2022). [Rethinking the role of demonstrations: What makes in-context learning work?](https://arxiv.org/pdf/2202.12837)
- Min, S., Lewis, M., Hajishirzi, H., & Zettlemoyer, L. (2021). [Noisy channel language model prompting for few-shot text classification.](https://arxiv.org/abs/2108.04106)
- Brown, T. B. (2020). [Language models are few-shot learners.](https://arxiv.org/abs/2005.14165)

---

