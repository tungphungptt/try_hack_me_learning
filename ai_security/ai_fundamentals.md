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

## Neural Networks and Deep Learning
- Simulating intelligence: The main objective of AI is to enable computers to behave like humans. One of the most powerful methods we have for achieving that is through neural networks. To understand how they work, it helps to go back to high school biology for a moment. The human brain processes information using interconnected neurons, cells that transmit signals between the body and brain via connections called synapses. When we encounter something new, the brain adjusts the strength of those synaptic connections based on the patterns it observes. Over time, the brain gets better at recognising and responding to those patterns. Neural networks replicate this exact behaviour.

<img width="486" height="218" alt="image" src="https://github.com/user-attachments/assets/befe4081-7576-4965-9566-a8dac217e214" />

- A neural network is made up of layers of nodes, where each node represents a neuron and each connection between nodes acts as a synapse. The input layer receives raw data. The number of nodes it has depends on the data type: a 4x4 pixel image, for example, has 16 input nodes, one per pixel. The hidden layers in the middle process and refine that input, extracting increasingly complex features as the signal moves deeper. Each connection carries a weight that determines how much influence it has on the next layer. The output layer produces the final prediction.

<img width="998" height="498" alt="image" src="https://github.com/user-attachments/assets/db014846-de60-4273-b3d7-54d57dd15b3b" />

- Consider a neural network tasked with recognising a handwritten digit. Early hidden layers detect simple features like edges and curves. Deeper layers combine those features into more complex patterns. A straight vertical line increases the likelihood of a 1 or 7. Curves push probability toward 3, 8, or 0. The output layer has one node per possible digit, and whichever scores highest wins. When a network has more than three layers, it qualifies as a Deep Learning (DL) algorithm, hence the name
- The key distinction between ML and DL is that DL doesn't require labelled data. Where supervised ML needs a human to attach correct answers to training examples, a DL algorithm can take raw, unstructured input and determine its own features. No human intervention means larger datasets can be processed, which is why DL is sometimes described as scalable ML. The explosion of DL over the last decade is largely down to one thing: the mass digitisation of information suddenly gave these algorithms the volume of data they needed to realise their potential.
