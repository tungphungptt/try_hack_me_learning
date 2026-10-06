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

<img width="540" height="450" alt="image" src="https://github.com/user-attachments/assets/7df06b9c-3430-4653-af64-5a9eb43bd24e" />

## Large Language Models 
- Packaged Intelligence: 
    - So far in this room we've traced AI from its origins as a field of research, through the emergence of ML, into the development of neural networks and DL. All of that brings us to the technology that kicked off the AI boom we're currently living through: Large Language Models.
    - If you've heard of ChatGPT, you already know what the moment felt like. Its ability to generate fluent, human-like text in response to a natural language query triggered discussions across news, politics, education, and industry simultaneously. Something had shifted. We were in a new era, and LLMs were the reason why. But how do they actually work?
  
    <img width="504" height="282" alt="image" src="https://github.com/user-attachments/assets/fc721cbf-d747-417e-b9fb-f0b8afaec491" />

- What are LLMs and how do they work?
    - Large Language Models are deep learning-based AI models that process and generate text by predicting the next word in a sequence. When you send a message to a chatbot, what's happening in the background is a rapid series of predictions about what word should come next in the response, repeated until the reply is complete. The key question is: how does the model get good enough at those predictions to be useful?
    - LLMs are first trained in a pre-training phase, where they process enormous volumes of text. GPT-3 alone was trained on data that would take a human 2,600 years to read nonstop. Instead of labelled data, LLMs rely on billions of parameters, numerical values that function like puzzle pieces, collectively encoding the model's understanding of language. During pre-training, the model is fed a piece of text with the final word removed and asked to predict it. Its initial guess is random. That guess is then compared against the correct answer, and the parameters are adjusted via an algorithm called backpropagation to make the right answer more likely next time. Repeat this process trillions of times across a vast dataset and the model develops a remarkably accurate sense of how language works.


    <img width="2044" height="724" alt="image" src="https://github.com/user-attachments/assets/5d88617e-7e08-4e2e-8999-4a013b48622c" />

     - The scale of pre-training is only possible because of advances in hardware (specifically GPUs enabling parallel processing) and a specific type of neural network called transformer neural networks. Introduced in Google's 2017 paper Attention is All You Need, transformers enabled parallel text processing instead of sequential word-by-word analysis. The key innovation was attention: the ability to assign different levels of importance to different words depending on context. Take this sentence: "The bank approved the loan because it was financially stable."
    - A model without attention might struggle to resolve what "it" refers to. Transformers calculate attention scores across the whole sentence, correctly linking "it" back to "the bank" rather than "the loan."


    <img width="822" height="800" alt="image" src="https://github.com/user-attachments/assets/eddb7cba-90fa-42e5-87a6-4df64c878434" />

    - After pre-training, humans come back into the loop in a process called RLHF (Reinforcement Learning from Human Feedback). Reviewers evaluate model outputs, flag anything unhelpful or problematic, and the parameters are adjusted accordingly. This is the step that shapes a raw language model into something usable as a chatbot or assistant.
    - LLMs power generative AI products like ChatGPT, LLaMA, and DeepSeek, which can create original text-based content in response to user prompts. Generative AI as a whole extends further still, enabling the creation of images, audio, video, and more. The AI boom didn't happen overnight. It's the product of decades of incremental research finally converging at the right moment. The diagram below shows how everything we've covered connects:

    <img width="814" height="1472" alt="image" src="https://github.com/user-attachments/assets/8f7043cc-057f-4370-8846-6be3ff3ae4fb" />

  --> Artificial Intelligence is the overarching field. Machine Learning is a subfield of AI that enables learning from data. Deep Learning is a subfield of ML that uses neural networks to process data at scale without human intervention. Large Language Models are advanced DL models built on transformer neural networks, designed to understand and generate human-like text.

## NEURON-1
Input layer --> Hidden layer --> Output layer 

<img width="822" height="618" alt="image" src="https://github.com/user-attachments/assets/3a050bfa-d0d1-459b-8f17-993f003e4a78" />

- Hidden node 1 : First hidden node — processes raw edge information from the input pixels --> Edge Detection

- Hidden node 2 : Second hidden node — looks at the overall arrangement and structure --> Pattern Matching

- Hidden node 3 : Third hidden node — checks for rounded or curved components --> Curve Detection

## Practice : 
Flag : THM{y0u_tr41n3d_th3_n3tw0rk}
