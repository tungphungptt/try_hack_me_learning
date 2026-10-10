# Learning Objectives 
- Understand where AI training data comes from and the security risks introduced by poor data provenance
- Recognise how PII and sensitive credentials can become permanently embedded in model weights through large-scale web scraping
- Understand how key model-building decisions (overfitting, quantisation, and federated learning) each introduce distinct security risks
- Understand the inheritance problem and what organisations unknowingly take on when fine-tuning pre-trained models
- Recognise why trained models are opaque black boxes, and what model cards do (and fail to do) to address this

# Training Data

<img width="742" height="528" alt="image" src="https://github.com/user-attachments/assets/d63af567-7712-4c0b-a32f-bd8cfb502bc0" />

## Where does the Data Come From?

Training a large language model requires a staggering amount of text. GPT-3 was trained on roughly 570GB of filtered text, and that's considered relatively modest by "modern" standards. To hit numbers like that, developers can't carefully hand-pick sources. The pipeline typically draws from four buckets:

| Source | What it is | Trust profile |
|---------|------------|---------------|
| Web scraping | Automated crawls of public internet content (news, forums, blogs, social media, etc.) | Low: no curator, no version control, content changes after collection |
| Licensed datasets | Data purchased or agreed with platforms (e.g., OpenAI + Reddit, Meta's own social posts) | Medium: terms often unclear; original users rarely consented to AI training use |
| Synthetic data | AI-generated content used to train further AI systems | Variable: growing fast; ~12% of fine-tuning datasets now contain LLM-generated content |
| Internal corpora | Company knowledge bases, support transcripts, clinical notes used for fine-tuning | Higher: organisation has direct control, but also direct liability if mishandled |

The most widely used training dataset is Common Crawl, a free, publicly available archive of web crawl data that has underpinned essentially every major model family. DeepSeek-V2 was pretrained on it; DeepSeek-V3 trained on 14.8 trillion tokens with Common Crawl as a core source; and LLaMA 4 was scaled to 40 trillion tokens across 200 languages using a similar pipeline. GPT-3 is one of the few models whose breakdown is publicly documented: 60% of its tokens came from a filtered version of Common Crawl, and more recent models lean even more heavily on it. The keyword is "filtered", and how that filtering was done, by whom, and what slipped through is where the security story begins.

## The Problem of Provenance
Data provenance is the ability to answer three questions about any piece of training data:
1. Where did it come from?
2. When was it collected?
3. Has it been modified since?

- In most AI supply chains today, the honest answer to all three is we don't fully know. Most major models are essentially trained on datasets of datasets, huge composites assembled from hundreds of upstream sources, where the original attribution has been lost, simplified, or never recorded in the first place. The Data Provenance Initiative audited over 1,800 datasets and found that more than 70% of licenses on popular hosting platforms were listed as "Unspecified", and of those that were labelled, 66% were miscategorised, usually listed as more permissive than they actually were. Organisations fine-tuning on these datasets often don't know what they legally have, let alone what's actually inside it.

<img width="700" height="432" alt="image" src="https://github.com/user-attachments/assets/20cf07fa-03a9-485e-8a2d-98b5f26374e2" />

- The software security world has been here before. SolarWinds taught the industry that you can't trust a compiled binary if you don't know what went into it, which is exactly why software bills of materials (SBOMs) became standard practice. The AI equivalent is the ML-BOM: a documented inventory of dataset sources, licenses, PII categories, and filtering decisions. Adoption is still early, and most organisations deploying third-party models today have nothing close to one

## PII (Personally Identifiable Information) in the Pipeline
- One of the most direct consequences of undocumented, large-scale web scraping is that personally identifiable information ends up baked into model weights. Once it's there, it's very difficult to remove. Medical records, personal email threads, forum posts about health conditions or political views: all of it gets swept up if it was publicly accessible at crawl time. The EU's GDPR explicitly requires data minimisation (collect only what's necessary). This sits in direct tension with the "more data is always better" logic driving pre-training.

<img width="538" height="418" alt="image" src="https://github.com/user-attachments/assets/f2af81e5-94b4-4499-8f01-73282f9e9fe2" />

- The security implication is measurable and concrete. Truffle Security scanned the December 2024 Common Crawl archive (400TB of data from 2.67 billion web pages) and found nearly 12,000 live, verified API keys and passwords. With the right prompt, a model trained on that data can sometimes be coaxed into surfacing training content near-verbatim, including credentials. This isn't a bug introduced by an attacker. It's a consequence of what went in during training, and no patch fixes it once the model is deployed.

## A Model Engineer
- A model's behaviour is a direct product of what it was trained on. If that data was scraped without audit, contaminated with PII, or manipulated upstream, those characteristics become part of the model, and there's no reliable way for the organisation deploying it to know. The data supply chain is as real and as exploitable as a software supply chain. For organisations right now, it's almost entirely invisible.


## Key Takeaway (Note)
### AI Data Supply Chain Risks

- LLMs are trained on massive datasets from web scraping, licensed datasets, synthetic data, and internal corpora.
- Common Crawl is one of the most widely used training data sources.
- Data provenance answers:
  1. Where did the data come from?
  2. When was it collected?
  3. Has it been modified?
- Many datasets lack proper licensing and provenance information.
- ML-BOM (Machine Learning Bill of Materials) helps track dataset sources, licenses, PII, and filtering decisions.
- Public web data may contain sensitive information and PII.
- Truffle Security found ~12,000 live API keys and passwords in Common Crawl.
- Risks include data poisoning, privacy leakage, compliance violations, and model memorization.

# Building the Model
## Epochs and Overfitting
- An epoch is one complete pass of the training algorithm through the entire dataset. In practice, models are trained over many epochs. The algorithm repeatedly sees the same data, adjusting its parameters each time until it converges on accurate predictions.
- The catch is that more epochs don't always mean a better model. Train for too long and the model stops learning general patterns and starts memorising training data specifically, a problem called overfitting. An overfit model performs well on its training data but poorly on other data. This matters for security because overfitting is one mechanism by which a model can "memorise" specific details from its training data, including sensitive ones, making it more likely to reproduce them when prompted.
## Model Validation
- To catch overfitting early, a portion of the training data is held back and never used for training; this is the validation set. At regular intervals during training, the model is tested on unseen data to check whether its performance is actually generalising or just improving on the training examples it's seen before. If training accuracy keeps climbing but validation accuracy plateaus or drops, that's overfitting in real time.

<img width="1122" height="686" alt="image" src="https://github.com/user-attachments/assets/0a115c5a-bd0d-489b-8a24-b79c0de03348" />

- From a security perspective, validation is the quality gate in the ML lifecycle. A model that skips thorough validation is one whose real-world behaviour is unknown, and such unknown behaviour is a security risk. It also means any biases or anomalies introduced through compromised training data may go undetected until the model is already deployed.

## Post-Training Optimisation: Pruning and Quantisation
Once a model is trained, it often goes through compression steps before deployment (particularly if it needs to run efficiently on limited hardware). Two of the most common are pruning and quantisation:

| Technique | What it does | Security consideration |
|---|---|---|
| **Pruning** | Removes parameters that contribute little to predictions, shrinking model size. | Changes model behaviour post-training; rarely documented in detail. |
| **Quantisation** | Reduces numerical precision of weights (e.g., from 32-bit to 8-bit) to cut memory and compute requirements. | Can degrade safety-aligned behaviour; backdoor defences tested on full-precision models may fail to detect threats in quantised versions. |

--> Both steps are applied after the training is complete, often by a different third-party team packaging the model for distribution. Research has shown that quantisation can silently degrade the safety mechanisms built into a model; defences that worked on the full-precision version may fail to detect backdoors once the model is compressed. When an organisation downloads a quantised model without documentation of what changed during compression, they're inheriting unknown behaviour modifications alongside efficiency gains.

## Federated Learning
- All the training approaches covered so far assume that data flows into a single central location for model training. Federated learning flips this: the model is trained across many decentralised devices or organisations, with each participant training locally on their own data and only sending weight updates (not the raw data itself) back to a central server for aggregation.
- This was designed with privacy in mind. A hospital sharing patient records to train a model is a data protection problem; a hospital contributing model updates without ever sending the records is a much easier conversation. In that sense, federated learning genuinely does reduce privacy risk at the data level.

<img width="1108" height="732" alt="image" src="https://github.com/user-attachments/assets/18bafde2-30c9-4eda-8597-f4fec12a432c" />

- The security trade-off, however, is that the integrity of the training process becomes much harder to verify. In a centralised setup, the organisation that trains the model controls the data. In a federated setup, participants can submit poisoned local updates (subtly manipulated gradients designed to skew the global model's behaviour), and these can be very difficult to detect at the aggregation stage. The question shifts from "who controls the data?" to "who controls the aggregation, and can any participant corrupt it?"

--> Federated learning is therefore an interesting case study in security trade-offs: it solves one trust problem by distributing control, but in doing so creates a different one.

# The Inheritance Problem 
Training an LLM from scratch means assembling trillions of tokens of data, acquiring the compute infrastructure to process it, and running a training job that can cost tens of millions of dollars. For most organisations, that's not a realistic option. Instead, the dominant model for AI development today is to start with someone else's work.

## Pre-Trained Models & Fine-Tuning
- A pre-trained model is one that has already been trained on a large, general-purpose dataset (the kind of web-scale corpus discussed in Task 2). These base models learn broad language understanding: grammar, facts, reasoning patterns, and world knowledge. They're produced by a small number of well-resourced organisations and then made available for others to build on, either through open weights (like Meta's LLaMA family) or through API access (like OpenAI's GPT series).
- Fine-tuning is the process of continuing to train one of these pre-trained models on a smaller, task-specific dataset. A healthcare company might fine-tune a base model trained on clinical documentation to improve its understanding of medical terminology. A law firm might fine-tune on case law. The result is a model with the broad capabilities of the base model, now specialised for a particular domain or use case.

<img width="784" height="426" alt="image" src="https://github.com/user-attachments/assets/dba578b1-618e-41f6-b2ae-2da10f6196eb" />

**What fine-tuning changes**: the model's task-specific behaviour, tone, and domain knowledge.
**What fine-tuning does not change**: the base model weights (the billions of parameters shaped by pre-training on data the fine-tuning organisation never saw and almost certainly never audited).

## The Inheritance Problem 
<img width="640" height="436" alt="image" src="https://github.com/user-attachments/assets/03899de3-4d46-47f0-acbb-d719bf251de7" />

This shows up in three concrete ways:
1. Safety alignment erodes, not breaks
- Stanford and Princeton found that the defence mechanisms of aligned LLMs can be compromised by fine-tuning on as few as 10 adversarially crafted examples (at a cost of under $0.20). Even benign fine-tuning on legitimate data degraded safety as a side effect.

- Think of safety alignment like a well-worn path through a forest. The model has been trained to follow this safe path when generating responses. Fine-tuning is like adding new paths through the same forest. Even if those new paths are legitimate (like teaching medical terminology), they can gradually obscure the original safe path. The model hasn't forgotten how to be unsafe; the probability weights have just shifted, making unsafe responses more likely again. The defence mechanisms don't snap; they wear down.

2. Specialisation increases attack surface
- Cisco found that fine-tuned models are measurably more susceptible to prompt injection than the base models they were fine-tuned on. The reason is structural: fine-tuning narrows the focus, reducing resilience to unexpected tokens. Think of it like this: a model fine-tuned on financial records gets better at financial reasoning, but also becomes more responsive to an attacker who frames their prompt in financial terms.

3.  Version matters, and it's rarely tracked
- Fine-tuning always targets a specific checkpoint of a base model. If that checkpoint later turned out to contain a backdoor or problematic training data, every derivative inherits it, regardless of whether anyone downstream was told. Without knowing exactly which version a model was fine-tuned from, there's no way to assess that exposure after the fact.

## Inheritance Tax
When your organisation deploys a fine-tuned model, you're not deploying the fine-tuning work your team did; you're deploying the entire pre-trained base beneath it. That base was shaped by a training process you didn't control, on data you didn't audit, by an organisation whose supply chain you almost certainly haven't reviewed. Fine-tuning is powerful, but it doesn't sanitise what came before it.


# The Black Box Problem
- When a security team wants to audit a piece of software, they have options. Source code is readable. Even compiled binaries can be disassembled, stepped through, and reasoned about. A trained model's weights are neither. They're billions of floating-point numbers (the cumulative result of a training process), and they carry no human-readable record of how they were shaped, what data influenced them, or what behaviours they encode. You cannot open a model and find the decision that makes it behave a certain way. This is a "black box" in the truest sense.

<img width="480" height="404" alt="image" src="https://github.com/user-attachments/assets/7047de4e-c532-4a3f-b3dd-1cea51bba435" />

## Model Cards
- The documentation artefact designed to address this is the model card: a structured document that accompanies a model and describes what it is, how it was built, and where it falls short. The concept was introduced by Google researchers in 2019 and has since become the closest thing the industry has to a standard transparency format.

--> A well-formed model card should give you the answers to the questions you can't get by inspecting the weights themselves:
| Section | What it should tell you |
|---|---|
| **Training data** | What sources were used, how they were filtered, known gaps or biases |
| **Intended use** | What the model was designed for (and explicitly what it wasn't) |
| **Evaluation results** | Performance metrics across different conditions and demographics |
| **Known limitations** | Conditions under which the model is known to underperform or behave unexpectedly |
| **Bias assessment** | Where training data or evaluation may have introduced skew |
| **Licence** | What you're legally permitted to do with the model |


## The Gaps
- Model cards are often incomplete, vague, or entirely missing because creating them is voluntary for most AI use cases.
- Lack of incentives: Organisations may avoid documenting limitations because transparency could reduce model adoption.
- Documentation gaps: The Data Provenance Initiative audited over 1,800 datasets and found widespread documentation gaps across the AI supply chain.
- Security risk: A missing or incomplete model card may indicate insufficient model evaluation or a decision not to disclose findings.
- Impact on users: Without reliable documentation, downstream users cannot properly assess a model's limitations, biases, or potential security risks.

--> A missing or incomplete model card is a potential security warning sign because it makes evaluating an AI model's trustworthiness and risks more difficult

--> The model card is your audit trail. In the absence of one, there is no audit trail, just a blackbox anđ the hope that whoeever built it was thorough. In security, hope is not a control

## Practical 
THM{A_m0del_Stud3nt}

## Conclusion
- AI training data is drawn from poorly documented, unaudited sources, meaning most organisations have no reliable answer to where their training data came from, what it contained, or whether it was tampered with

- PII and live credentials routinely end up baked into model weights through large-scale web scraping and cannot be patched out once the model is deployed

- Model-building decisions such as quantisation and federated learning introduce security trade-offs that are rarely documented, meaning organisations inherit unknown behaviour modifications alongside efficiency gains

- Fine-tuning a pre-trained model inherits everything beneath it: safety alignment erodes with as few as 10 adversarial examples, and fine-tuned models are measurably more susceptible to prompt injection than their base counterparts

- Trained model weights are fundamentally opaque; security testing can only sample behaviour rather than audit it, and model cards (the primary transparency mechanism) remain voluntary, frequently incomplete, and sometimes absent entirely
