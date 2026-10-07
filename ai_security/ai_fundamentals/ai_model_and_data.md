<img width="538" height="418" alt="image" src="https://github.com/user-attachments/assets/c2474534-42be-44c5-b686-bf0d84517161" /># Learning Objectives 
- Understand where AI training data comes from and the security risks introduced by poor data provenance
- Recognise how PII and sensitive credentials can become permanently embedded in model weights through large-scale web scraping
- Understand how key model-building decisions (overfitting, quantisation, and federated learning) each introduce distinct security risks
- Understand the inheritance problem and what organisations unknowingly take on when fine-tuning pre-trained models
- Recognise why trained models are opaque black boxes, and what model cards do (and fail to do) to address this

# Training Data

<img width="742" height="528" alt="image" src="https://github.com/user-attachments/assets/d63af567-7712-4c0b-a32f-bd8cfb502bc0" />

##Where does the Data Come From?

Training a large language model requires a staggering amount of text. GPT-3 was trained on roughly 570GB of filtered text, and that's considered relatively modest by "modern" standards. To hit numbers like that, developers can't carefully hand-pick sources. The pipeline typically draws from four buckets:

| Source | What it is | Trust profile |
|---------|------------|---------------|
| Web scraping | Automated crawls of public internet content (news, forums, blogs, social media, etc.) | Low: no curator, no version control, content changes after collection |
| Licensed datasets | Data purchased or agreed with platforms (e.g., OpenAI + Reddit, Meta's own social posts) | Medium: terms often unclear; original users rarely consented to AI training use |
| Synthetic data | AI-generated content used to train further AI systems | Variable: growing fast; ~12% of fine-tuning datasets now contain LLM-generated content |
| Internal corpora | Company knowledge bases, support transcripts, clinical notes used for fine-tuning | Higher: organisation has direct control, but also direct liability if mishandled |

The most widely used training dataset is Common Crawl, a free, publicly available archive of web crawl data that has underpinned essentially every major model family. DeepSeek-V2 was pretrained on it; DeepSeek-V3 trained on 14.8 trillion tokens with Common Crawl as a core source; and LLaMA 4 was scaled to 40 trillion tokens across 200 languages using a similar pipeline. GPT-3 is one of the few models whose breakdown is publicly documented: 60% of its tokens came from a filtered version of Common Crawl, and more recent models lean even more heavily on it. The keyword is "filtered", and how that filtering was done, by whom, and what slipped through is where the security story begins.

##The Problem of Provenance
Data provenance is the ability to answer three questions about any piece of training data:
1. Where did it come from?
2. When was it collected?
3. Has it been modified since?

- In most AI supply chains today, the honest answer to all three is we don't fully know. Most major models are essentially trained on datasets of datasets, huge composites assembled from hundreds of upstream sources, where the original attribution has been lost, simplified, or never recorded in the first place. The Data Provenance Initiative audited over 1,800 datasets and found that more than 70% of licenses on popular hosting platforms were listed as "Unspecified", and of those that were labelled, 66% were miscategorised, usually listed as more permissive than they actually were. Organisations fine-tuning on these datasets often don't know what they legally have, let alone what's actually inside it.

<img width="700" height="432" alt="image" src="https://github.com/user-attachments/assets/20cf07fa-03a9-485e-8a2d-98b5f26374e2" />

- The software security world has been here before. SolarWinds taught the industry that you can't trust a compiled binary if you don't know what went into it, which is exactly why software bills of materials (SBOMs) became standard practice. The AI equivalent is the ML-BOM: a documented inventory of dataset sources, licenses, PII categories, and filtering decisions. Adoption is still early, and most organisations deploying third-party models today have nothing close to one

##PII in the Pipeline
- One of the most direct consequences of undocumented, large-scale web scraping is that personally identifiable information ends up baked into model weights. Once it's there, it's very difficult to remove. Medical records, personal email threads, forum posts about health conditions or political views: all of it gets swept up if it was publicly accessible at crawl time. The EU's GDPR explicitly requires data minimisation (collect only what's necessary). This sits in direct tension with the "more data is always better" logic driving pre-training.

<img width="538" height="418" alt="image" src="https://github.com/user-attachments/assets/f2af81e5-94b4-4499-8f01-73282f9e9fe2" />

- The security implication is measurable and concrete. Truffle Security scanned the December 2024 Common Crawl archive (400TB of data from 2.67 billion web pages) and found nearly 12,000 live, verified API keys and passwords. With the right prompt, a model trained on that data can sometimes be coaxed into surfacing training content near-verbatim, including credentials. This isn't a bug introduced by an attacker. It's a consequence of what went in during training, and no patch fixes it once the model is deployed.

##A Model Engineer
- A model's behaviour is a direct product of what it was trained on. If that data was scraped without audit, contaminated with PII, or manipulated upstream, those characteristics become part of the model, and there's no reliable way for the organisation deploying it to know. The data supply chain is as real and as exploitable as a software supply chain. For organisations right now, it's almost entirely invisible.

