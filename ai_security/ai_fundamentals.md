# The building blocks of AI 
## AI and Machine Learning
Machine Learning:
- ML is a subfield of AI that refers to a computer's ability to learn from data without being explicitly given instructions. Think of it like how the human brain learns: over time, with enough exposure to patterns, it gets better at recognising and predicting them. ML algorithms work the same way. Feed them enough data and they'll improve their accuracy over time without being hand-coded to do so.
- ML follows a structured lifecycle. It starts with defining the problem, for example, determining whether an email is spam. Data is then collected, cleaned, and prepared. The model is trained on that data, then evaluated and tuned until it performs well. Once it's ready, it gets deployed into a real-world environment. But the lifecycle doesn't stop at deployment. Models require ongoing monitoring and periodic retraining as the world changes around them, which is what makes it an iterative process rather than a one-and-done job.

<img width="582" height="342" alt="image" src="https://github.com/user-attachments/assets/a9258070-d92b-4987-bace-e24418cebd67" />

- One term worth flagging here is overfitting. This is when a model becomes so familiar with its training data that it fails to generalise to new, unseen data. Rather than learning the underlying pattern, it essentially memorises the examples it was trained on. You'll hear this term come up again later in the path, particularly in a security context.


## Machine Learning Algorithms
- The Brains of the Operation: ML algorithms are the mathematical methods used to learn patterns from data. The trained outputs they produce are what we call ML models. Every algorithm follows the same basic structure: a decision process that makes predictions based on input data, an error function that evaluates how far off those predictions were, and a model optimisation process that adjusts the algorithm to do better next time. This loop repeats until the model reaches a satisfactory level of performance

<img width="488" height="238" alt="image" src="https://github.com/user-attachments/assets/0df4bbbc-d02e-4124-8847-ea1d50b1e748" />

- ML algorithms fall into four main categories depending on how they learn and what kind of data they work with.
    - Supervised learning trains on labelled data, meaning every training example comes with the correct answer already attached. The model learns to map inputs to outputs based on those examples. Predicting house prices or classifying whether an email is spam are both supervised learning problems.
    - Unsupervised learning works with unlabelled data and has to find its own structure. Rather than being told what the answer is, the algorithm identifies patterns, clusters, and relationships in the data on its own. It's useful for things like grouping customers by behaviour or detecting anomalies in network traffic.
    - Semi-supervised learning sits between the two. It uses a small portion of labelled data to guide the learning process across a much larger pool of unlabelled data. This is practical in situations where labelling data is expensive or time-consuming.
    - Reinforcement learning works differently from all three. Rather than learning from a fixed dataset, an agent learns by taking actions in an environment and receiving rewards or penalties based on the outcomes. Over time, it refines its behaviour to maximise reward. It's the approach behind things like game-playing AI and autonomous systems.
