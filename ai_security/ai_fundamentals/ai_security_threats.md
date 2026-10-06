# Learning Objectives
  - Understand the key vulnerabilities that AI models introduce and how attackers exploit them
  - Understand how AI is being used to enhance existing attacks like phishing, malware generation, and social engineering
  - Understand how AI can be used defensively across analysis, prediction, summarisation, and investigation
  - Understand what it means to adopt AI securely and the frameworks that guide that process.

    
# Vulnerabilities in AI Models
## Learn the New Threats
- Now that AI is embedded in business operations across every industry, it's introduced a new category of security concern: vulnerabilities that are specific to AI models themselves. These aren't the same as traditional software vulnerabilities. They emerge from the nature of how these models are built, trained, and deployed
- MITRE have built something similar with a focus specifically on AI threats, called the ATLAS framework. It maps out the tactics, techniques, and procedures attackers use against AI systems, and it's a useful reference as you work through this room (https://atlas.mitre.org/matrices/ATLAS)

  <img width="442" height="542" alt="image" src="https://github.com/user-attachments/assets/50e52f24-c9e8-468e-997a-4ac3e20be116" />

## Vulnerability Breakdown
The five key vulnerabilities in AI models that every security practitioner should know : 
- **Prompt Injection** occurs when an attackers overrides the original instructions provided to a model. Every AI model operates under a system prompt, a set of instructions that define how it should behave. An RPG chatbot might be told to stay in character and never discuss its underlying infrastructure. Prompt injection is when user input is crafted in a way that overrides or bypasses those instructions, causing the model to behave in ways it wasn't supposed to, whether that's revealing sensitive information, generating harmful content, or acting outside its defined scope.
- **Data Poisoning** is when an attacker manipulates the training data used to build an AI model, causing its outputs to be incorrect or biased. Take a spam filter trained on email data. If an attacker can tamper with that training data before the model is trained, they can cause the model to misclassify spam as legitimate mail, effectively blinding it to the very emails it was built to catch.

  <img width="548" height="434" alt="image" src="https://github.com/user-attachments/assets/3377f530-454d-4776-8565-401d5aed1f82" />

- **Model Theft** occurs when an attacker gains unauthorised access to an AI model, either to steal the intellectual property it represents or to use it for malicious purposes. One method is to repeatedly query a model's API and use the outputs to train a clone that replicates its behaviour, without ever needing direct access to the original weights.
- **Privacy Leakage** refers to the possibility of an AI model inadvertently revealing sensitive information from its training data. A model trained on private medical records, for example, could under the right prompting conditions surface details about real patients that were never intended to be accessible. The information doesn't disappear when training ends; it gets baked into the model's weights.
- **Model Drift** is when a model's performance degrades over time as the world it was trained on changes. A model trained on last year's network traffic patterns may start performing poorly as attack techniques evolve. This is why monitoring deployed models isn't optional; it's a security requirement. Drift can go undetected until the model is already failing in production.

# AI - Enhanced Attacks
## Ai - Generated Malware
- Generative AI can produce functional code in seconds from a natural language prompt. That's an enormous productivity boost for developers, and it's an equally enormous productivity boost for attackers. Writing malware has historically required technical skill and time. With generative AI, that barrier drops considerably. Attackers can generate, iterate, and customise malicious code faster than ever, and the models doing the generating have no way to verify the intent behind the request.
  <img width="930" height="514" alt="image" src="https://github.com/user-attachments/assets/5cc41705-6fe8-46de-9b67-26da9b3f341a" />

## Deepfakes
- Authentication, at its core, is about answering one question: are you who you say you are? For most of human history, seeing and hearing someone was enough to answer it. Generative AI has broken that assumption. Given enough training data, an AI can now generate a convincing likeness of a real person, whether that's their voice, their face, or both, to a degree of accuracy that fools even technically aware individuals.
- The attack scenario practically writes itself. A finance employee receives a voice message from what sounds exactly like their CEO, requesting an urgent wire transfer. The voice is a deepfake. Examples of this already being used in the wild include deepfaked video interviews that led to fraudulent job offers being extended to candidates who didn't exist. The technology is advancing faster than our ability to detect it.
  <img width="652" height="472" alt="image" src="https://github.com/user-attachments/assets/3bef3560-7acc-4b32-980a-b7a32b7945ab" />

## AI - Enhanced Phishing 
- Phishing is one of the most common initial access methods in use today. For years, security awareness training gave defenders a fighting chance by teaching people to spot the telltale signs: suspicious links, urgency, and, perhaps most reliably, broken or unnatural language. That last indicator is becoming obsolete. Generative AI can produce fluent, contextually appropriate, highly targeted phishing emails at scale and with minimal effort, regardless of the attacker's own writing ability.
- Most LLMs have guardrails designed to prevent them from generating obviously malicious content. But as covered in the previous task, prompt injection techniques can sometimes be used to bypass those guardrails, making the same models that power productivity tools available to attackers as phishing content generators.

# Defensive AI
## Harness the Power
