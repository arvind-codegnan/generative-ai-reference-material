# Generative AI for Java Professionals

> # Part 1: Artificial Intelligence Foundations

[← Home](index.md) · [Next: Part 1: Generative AI with Java →](generative-ai-with-java.md)

## Table of Contents

- [1. What Is Artificial Intelligence?](#1-what-is-artificial-intelligence)
- [2. Why Should Java Developers Learn AI?](#2-why-should-java-developers-learn-ai)
- [3. What Does Intelligence Mean for a Machine?](#3-what-does-intelligence-mean-for-a-machine)
- [4. AI vs Automation vs Traditional Programming](#4-ai-vs-automation-vs-traditional-programming)
- [5. A Brief History of Artificial Intelligence](#5-a-brief-history-of-artificial-intelligence)
- [6. Major Goals of AI](#6-major-goals-of-ai)
- [7. Types of AI by Capability](#7-types-of-ai-by-capability)
- [8. Types of AI by Functionality](#8-types-of-ai-by-functionality)
- [9. Major Branches of AI](#9-major-branches-of-ai)
- [10. Data: The Foundation of Modern AI](#10-data-the-foundation-of-modern-ai)
- [11. Features, Labels, and Examples](#11-features-labels-and-examples)
- [12. What Is Machine Learning?](#12-what-is-machine-learning)
- [13. Supervised Learning](#13-supervised-learning)
- [14. Unsupervised Learning](#14-unsupervised-learning)
- [15. Semi-Supervised and Self-Supervised Learning](#15-semi-supervised-and-self-supervised-learning)
- [16. Reinforcement Learning](#16-reinforcement-learning)
- [17. The Machine Learning Workflow](#17-the-machine-learning-workflow)
- [18. Training, Validation, and Test Data](#18-training-validation-and-test-data)
- [19. Classification and Regression](#19-classification-and-regression)
- [20. Common Machine Learning Algorithms](#20-common-machine-learning-algorithms)
- [21. Model, Training, and Inference](#21-model-training-and-inference)
- [22. Underfitting and Overfitting](#22-underfitting-and-overfitting)
- [23. Evaluating AI Models](#23-evaluating-ai-models)
- [24. Neural Networks](#24-neural-networks)
- [25. Deep Learning](#25-deep-learning)
- [26. Natural Language Processing](#26-natural-language-processing)
- [27. Computer Vision and Speech AI](#27-computer-vision-and-speech-ai)
- [28. Expert Systems, Robotics, and Recommendation Systems](#28-expert-systems-robotics-and-recommendation-systems)
- [29. AI Agents](#29-ai-agents)
- [30. Knowledge Representation, Reasoning, Search, and Planning](#30-knowledge-representation-reasoning-search-and-planning)
- [31. The AI Application Lifecycle](#31-the-ai-application-lifecycle)
- [32. Responsible AI](#32-responsible-ai)
- [33. Limitations and Risks of AI](#33-limitations-and-risks-of-ai)
- [34. The Role of Java in AI Applications](#34-the-role-of-java-in-ai-applications)
- [35. Predictive AI vs Generative AI](#35-predictive-ai-vs-generative-ai)
- [36. From AI Foundations to Generative AI](#36-from-ai-foundations-to-generative-ai)
- [37. Common Misconceptions](#37-common-misconceptions)
- [38. Practice Exercises](#38-practice-exercises)
- [39. Frequently Asked Interview Questions](#39-frequently-asked-interview-questions)
- [40. Quick Revision](#40-quick-revision)
- [41. Glossary](#41-glossary)

## 1. What Is Artificial Intelligence?

**Artificial Intelligence**, commonly called **AI**, is the field of creating computer systems that perform tasks normally associated with human intelligence.

Such tasks include:

- Recognizing patterns
- Understanding language
- Classifying information
- Making predictions
- Solving problems
- Planning actions
- Learning from data
- Recommending choices
- Generating new content

A simple definition is:

> AI enables a computer system to observe information, process it, and produce a useful decision or output.

```text
Input or Environment
        |
        v
AI System
        |
        v
Prediction, Decision, Recommendation, or Generated Content
```

Examples:

| Input | AI task | Output |
| --- | --- | --- |
| Email text | Spam classification | Spam or not spam |
| Customer data | Risk prediction | Risk score |
| Product history | Recommendation | Suggested products |
| Photograph | Image recognition | Objects in the image |
| Spoken audio | Speech recognition | Text transcript |
| User instruction | Content generation | Text, code, image, or audio |

AI is an umbrella term. Machine Learning, Deep Learning, Natural Language Processing, Computer Vision, and Generative AI are areas within or closely connected to AI.

[↑ Go to Table of Contents](#table-of-contents)

## 2. Why Should Java Developers Learn AI?

AI systems need much more than a trained model. They also need applications, APIs, databases, authentication, validation, monitoring, business rules, and reliable production infrastructure.

These are familiar responsibilities for Java developers.

Java developers may build:

- Fraud-detection services
- Document-processing systems
- Recommendation APIs
- Intelligent search applications
- Customer-support assistants
- Predictive-maintenance systems
- Image-processing backends
- AI-enabled enterprise applications
- Model-integration services
- Retrieval-Augmented Generation applications

The role of a Java developer usually includes:

- Collecting or receiving application data
- Calling a trained model
- Validating inputs and model outputs
- Applying deterministic business rules
- Connecting AI results with databases and services
- Protecting sensitive data
- Monitoring reliability, quality, latency, and cost
- Exposing the result through an API or user interface

```text
User or Business System
          |
          v
Java Application
  ├── Validation
  ├── Authentication
  ├── Business Rules
  ├── Database Access
  └── AI Integration
          |
          v
AI Model or AI Service
```

A Java fresher does not need to become a data scientist before learning AI integration. However, understanding the vocabulary and lifecycle prevents incorrect assumptions when working with models.

[↑ Go to Table of Contents](#table-of-contents)

## 3. What Does Intelligence Mean for a Machine?

Human intelligence includes awareness, emotion, experience, judgment, common sense, and many other abilities. An AI system normally performs a narrower, measurable task.

Machine intelligence may involve:

- **Perception:** Identifying objects in an image or sounds in audio
- **Learning:** Finding useful patterns in examples
- **Reasoning:** Applying rules or relationships
- **Prediction:** Estimating a likely future or unknown value
- **Planning:** Choosing a sequence of actions
- **Language processing:** Analysing or producing human language
- **Decision support:** Ranking possible actions or outcomes

An AI system that is excellent at one task may be unable to perform another task.

For example, a model trained to classify loan risk cannot automatically identify objects in photographs. Intelligence in AI is usually task-specific.

[↑ Go to Table of Contents](#table-of-contents)

## 4. AI vs Automation vs Traditional Programming

These ideas overlap, but they are not identical.

### Traditional Programming

A programmer writes explicit rules.

```text
Data + Programmer-Written Rules → Output
```

Java example:

```java
public String classifyAge(int age) {
    if (age < 13) {
        return "Child";
    }
    if (age < 20) {
        return "Teenager";
    }
    return "Adult";
}
```

The rule is visible, deterministic, and easy to explain.

### Automation

Automation makes a process run with reduced human effort. It may use fixed rules and may not use AI.

Examples:

- Sending a scheduled email
- Copying a file every night
- Calculating payroll using fixed rules
- Generating an invoice after payment

### Machine Learning

The system learns a model from examples rather than relying only on manually written rules.

```text
Training Data + Learning Algorithm → Trained Model

New Data + Trained Model → Prediction
```

### Artificial Intelligence

AI is the broader goal of creating systems that perform intelligent tasks. An AI solution may use fixed rules, search, optimization, machine learning, neural networks, or combinations of these techniques.

| Approach | Rules come from | Output behavior |
| --- | --- | --- |
| Traditional programming | Programmer | Usually deterministic |
| Rule-based automation | Programmer or process designer | Deterministic workflow |
| Machine learning | Patterns learned from data | Probabilistic prediction |
| Generative AI | Patterns learned by a generative model plus current context | Probabilistic new content |

AI should not replace ordinary Java logic when a simple, stable rule solves the problem correctly.

## 5. A Brief History of Artificial Intelligence

AI developed through several waves rather than appearing suddenly.

```text
1950s          1970s–1980s       1990s–2000s       2010s          2020s
Early AI  →    Expert Systems →  Machine Learning → Deep Learning → Generative AI
```

Important stages:

- **1950s:** Researchers began asking whether machines could demonstrate intelligent behaviour.
- **1956:** The term Artificial Intelligence became associated with an academic field.
- **1960s–1970s:** Programs solved puzzles and applied symbolic rules in limited environments.
- **1980s:** Expert systems represented specialist knowledge as rules.
- **1990s–2000s:** Statistical machine learning grew as digital data and computing power increased.
- **2010s:** Deep learning achieved major progress in vision, speech, and language tasks.
- **2020s:** Foundation models and Generative AI made natural-language and multimodal interaction widely accessible.

Progress in AI has been influenced by:

- More data
- Faster processors and specialized hardware
- Better algorithms
- Cloud computing
- Improved storage and networking
- Open research and software ecosystems

AI history also contains periods of reduced funding and expectations, sometimes called **AI winters**. This reminds developers to separate proven capabilities from marketing claims.

## 6. Major Goals of AI

AI systems are designed to achieve one or more goals.

| Goal | Meaning | Example |
| --- | --- | --- |
| Perception | Interpret sensory data | Detect a pedestrian in an image |
| Prediction | Estimate an unknown outcome | Predict customer churn |
| Classification | Assign a category | Mark an email as spam |
| Recommendation | Rank useful choices | Recommend a course |
| Reasoning | Derive conclusions | Diagnose a rule-based fault |
| Planning | Select actions toward a goal | Plan a delivery route |
| Optimization | Find a strong solution under constraints | Create a staff schedule |
| Language interaction | Process human language | Answer a question |
| Content generation | Create new content | Generate a summary |

A clear goal is essential. “Add AI” is not a measurable requirement. “Reduce the time required to classify support tickets while maintaining at least the approved precision” is much clearer.

## 7. Types of AI by Capability

AI is often discussed using three capability levels.

### Artificial Narrow Intelligence

**Artificial Narrow Intelligence**, or **ANI**, performs a limited task or set of related tasks.

Examples include:

- Spam detection
- Face recognition
- Product recommendations
- Navigation
- Language translation
- Chat assistants

Modern production AI systems, including current Generative AI systems, are forms of narrow AI even when they can perform many language-related tasks.

### Artificial General Intelligence

**Artificial General Intelligence**, or **AGI**, describes a hypothetical system capable of learning and performing intellectual tasks across domains at a broadly human level.

AGI is a research goal and concept, not an ordinary software dependency that a Java application can simply add.

### Artificial Superintelligence

**Artificial Superintelligence**, or **ASI**, is a hypothetical concept in which machine intelligence exceeds human capability across most intellectual areas.

```text
ANI: Task-specific and available today
AGI: Broad human-level capability; hypothetical
ASI: Beyond broad human capability; hypothetical
```

Use these terms carefully. A fluent response does not prove general intelligence.

## 8. Types of AI by Functionality

Another common classification describes how a system uses past information.

| Type | Description | Example idea |
| --- | --- | --- |
| Reactive machine | Responds only to the current input | A simple game-playing system |
| Limited-memory system | Uses recent or learned historical information | Driving assistance or recommendation model |
| Theory-of-mind AI | Would understand beliefs and emotions deeply | Research concept |
| Self-aware AI | Would possess awareness of itself | Hypothetical concept |

The last two categories should not be confused with present-day product capabilities. A model may imitate emotional language without experiencing an emotion.

## 9. Major Branches of AI

AI contains multiple fields.

```text
Artificial Intelligence
├── Machine Learning
│   └── Deep Learning
├── Natural Language Processing
├── Computer Vision
├── Speech Processing
├── Knowledge Representation and Reasoning
├── Search and Planning
├── Expert Systems
├── Robotics
└── Generative AI
```

| Branch | Focus |
| --- | --- |
| Machine Learning | Learning patterns from data |
| Deep Learning | Learning with multi-layer neural networks |
| NLP | Processing human language |
| Computer Vision | Processing images and video |
| Speech AI | Recognizing or producing speech |
| Expert Systems | Applying encoded specialist rules |
| Robotics | Perception, planning, and action in the physical world |
| Generative AI | Creating new text, images, audio, video, or code |

Many real applications combine branches. A voice assistant may use speech recognition, NLP, search, tool execution, and speech synthesis.

## 10. Data: The Foundation of Modern AI

Modern AI systems learn patterns from data. The data may contain:

- Database rows
- Text documents
- Images
- Audio recordings
- Video
- Sensor measurements
- Application events
- User interactions

Data quality strongly affects model quality.

```text
Incomplete, incorrect, biased, or outdated data
                     |
                     v
              Weak model behaviour
```

Important data-quality dimensions:

| Dimension | Question |
| --- | --- |
| Accuracy | Is the data correct? |
| Completeness | Are important values missing? |
| Consistency | Do formats and meanings agree? |
| Timeliness | Is the data current enough? |
| Relevance | Does it represent the target problem? |
| Representativeness | Does it include the groups and situations the model will encounter? |

More data is not automatically better. Relevant, lawful, representative, well-labelled data is usually more valuable than a larger collection of poor-quality data.

## 11. Features, Labels, and Examples

A **training example** is one item used to teach a model.

A **feature** is an input value used by a model. A **label** is the expected answer in supervised learning.

Example: predicting whether a customer may leave a service.

| Customer ID | Months active | Complaints | Monthly charge | Churn label |
| --- | ---: | ---: | ---: | --- |
| C101 | 3 | 4 | 1200 | Yes |
| C102 | 48 | 0 | 800 | No |

Possible features:

- Months active
- Number of complaints
- Monthly charge

Label:

- Churn: Yes or No

The customer ID is usually an identifier, not a useful predictive feature.

**Feature engineering** is the process of selecting, transforming, or creating useful inputs. Domain knowledge is often important in this work.

## 12. What Is Machine Learning?

**Machine Learning**, or **ML**, is a field in which algorithms learn patterns from data and use those patterns to make predictions or decisions.

Traditional rule:

```java
if (failedLoginCount > 5) {
    markAsSuspicious();
}
```

A machine-learning approach may learn from many signals:

- Failed login count
- Device history
- Login time
- Geographic change
- Request frequency
- Earlier fraud outcomes

```text
Historical Examples
        |
        v
Learning Algorithm
        |
        v
Trained Model
        |
        v
Prediction for New Data
```

The learned model captures statistical relationships. It does not discover guaranteed business rules.

## 13. Supervised Learning

In **supervised learning**, training examples contain inputs and known correct labels.

```text
Input Features + Correct Label → Learning Algorithm → Model
```

Examples:

| Task | Input | Label |
| --- | --- | --- |
| Spam detection | Email content | Spam or not spam |
| Loan-risk prediction | Applicant data | Default or no default |
| Price estimation | Property details | Sale price |
| Image classification | Photograph | Cat, dog, or other |

Two major supervised-learning tasks are:

- **Classification:** Predict a category.
- **Regression:** Predict a numeric value.

Labels require trusted historical outcomes or human annotation. Incorrect labels teach incorrect patterns.

## 14. Unsupervised Learning

In **unsupervised learning**, the training data does not contain target labels. The algorithm searches for structure or patterns.

Common tasks:

- Clustering similar records
- Detecting unusual behaviour
- Reducing the number of dimensions
- Finding associations

Example:

```text
Customer Behaviour Data
          |
          v
Clustering Algorithm
          |
          v
Group A: Frequent low-value buyers
Group B: Occasional high-value buyers
Group C: Inactive customers
```

The algorithm produces groups, but a person must interpret what those groups mean for the business.

## 15. Semi-Supervised and Self-Supervised Learning

### Semi-Supervised Learning

Semi-supervised learning uses a small amount of labelled data and a larger amount of unlabelled data.

This is useful when raw data is plentiful but human labelling is expensive.

```text
Small Labelled Dataset + Large Unlabelled Dataset → Model
```

### Self-Supervised Learning

Self-supervised learning creates learning signals from the data itself.

For example, a language model can learn by predicting hidden or next pieces of text. The text provides the training targets without a person manually labelling every sentence.

Self-supervised learning is important for understanding how large foundation models learn general representations before task-specific use.

## 16. Reinforcement Learning

In **reinforcement learning**, an agent learns by interacting with an environment and receiving rewards or penalties.

```text
Agent observes state
        |
        v
Agent chooses action
        |
        v
Environment changes
        |
        v
Reward or penalty
        |
        └──────────────→ Agent learns
```

Main terms:

| Term | Meaning |
| --- | --- |
| Agent | The learner or decision-maker |
| Environment | The world in which the agent acts |
| State | Current situation |
| Action | A possible choice |
| Reward | Feedback about the action |
| Policy | Strategy for choosing actions |

Applications include game playing, robotics, resource optimization, and some model-alignment techniques.

A poorly designed reward may cause unexpected behaviour. The agent optimizes the measured reward, which may not perfectly represent the real goal.

## 17. The Machine Learning Workflow

A typical workflow contains the following stages:

```text
1. Define Problem
        |
2. Collect Data
        |
3. Clean and Prepare Data
        |
4. Select Features and Algorithm
        |
5. Train Model
        |
6. Validate and Tune
        |
7. Test Model
        |
8. Deploy
        |
9. Monitor and Improve
```

### Define the Problem

Identify the decision, user, desired output, and success metric.

### Collect and Prepare Data

Remove invalid records, handle missing values, standardize formats, and confirm permission to use the data.

### Train and Evaluate

Use historical data to learn model parameters and measure performance on unseen data.

### Deploy and Monitor

Connect the model to an application and watch for errors, drift, bias, latency, and business impact.

Training is only one part of the lifecycle. Production reliability requires software engineering and continuous monitoring.

## 18. Training, Validation, and Test Data

A dataset is commonly divided into three parts.

| Dataset | Purpose |
| --- | --- |
| Training set | Learns model parameters |
| Validation set | Selects settings and compares model versions |
| Test set | Measures final performance on unseen data |

```text
Complete Dataset
├── Training Data   → Learn
├── Validation Data → Tune
└── Test Data       → Final Evaluation
```

The test set should not influence training decisions. Otherwise the reported performance may be misleading.

### Data Leakage

**Data leakage** occurs when training uses information that would not be available when making a real prediction.

Example: using a payment-recovery status recorded after a loan default to predict whether the loan will default.

Leakage can produce excellent test numbers and poor real-world results.

## 19. Classification and Regression

### Classification

Classification predicts a discrete category.

Examples:

- Approved or rejected
- Fraud or legitimate
- Low, medium, or high priority
- Product category

### Regression

Regression predicts a numeric value.

Examples:

- House price
- Delivery time
- Monthly demand
- Energy consumption

```text
Classification → Category
Regression     → Number
```

The business requirement determines the task. Predicting whether delivery will be late is classification; predicting the number of minutes late is regression.

## 20. Common Machine Learning Algorithms

A Java application developer does not need to implement every algorithm, but should recognize common names.

| Algorithm | Common purpose | Simple idea |
| --- | --- | --- |
| Linear regression | Numeric prediction | Fit a relationship between inputs and a number |
| Logistic regression | Classification | Estimate category probability |
| Decision tree | Classification or regression | Follow learned decision branches |
| Random forest | Classification or regression | Combine many decision trees |
| Gradient boosting | Classification or regression | Build learners that correct earlier errors |
| k-nearest neighbours | Classification or regression | Use nearby examples |
| k-means | Clustering | Group data around learned centres |
| Support vector machine | Classification | Find a strong separating boundary |
| Neural network | Many complex tasks | Learn layered transformations |

There is no universally best algorithm. Selection depends on:

- Data size and shape
- Required accuracy
- Explainability
- Training cost
- Prediction latency
- Maintenance needs
- Business risk

Always compare a complex model with a simpler baseline.

## 21. Model, Training, and Inference

These terms are fundamental.

### Model

A **model** is the learned mathematical or computational representation used to produce outputs from inputs.

### Training

**Training** is the process of adjusting model parameters using data so that model errors are reduced.

### Inference

**Inference** is using the trained model on new input.

```text
Training Phase
Historical Data → Algorithm → Trained Model

Inference Phase
New Input → Trained Model → Prediction or Output
```

A Java backend commonly participates in inference rather than training. It prepares a request, calls a deployed model, receives a result, validates it, and applies business rules.

### Parameters and Hyperparameters

- **Parameters** are learned during training.
- **Hyperparameters** are settings chosen for the training process or model configuration.

Examples of hyperparameters include tree depth, learning rate, number of training passes, and some model architecture settings.

## 22. Underfitting and Overfitting

### Underfitting

A model underfits when it is too simple or insufficiently trained to learn important patterns.

Symptoms:

- Poor performance on training data
- Poor performance on unseen data

### Overfitting

A model overfits when it memorizes training details and does not generalize well.

Symptoms:

- Excellent training performance
- Poor performance on unseen data

```text
Underfitting        Good Generalization        Overfitting
Too simple          Useful pattern learned     Training data memorized
```

Ways to reduce overfitting include:

- Collecting more representative data
- Reducing unnecessary complexity
- Regularization
- Cross-validation
- Early stopping
- Removing leaked or irrelevant features

The real goal is not to perform perfectly on remembered examples. It is to perform reliably on relevant new cases.

## 23. Evaluating AI Models

Model evaluation must match the problem.

### Classification Metrics

Consider a fraud detector.

| Term | Meaning |
| --- | --- |
| True positive | Fraud correctly identified |
| True negative | Legitimate transaction correctly accepted |
| False positive | Legitimate transaction incorrectly marked as fraud |
| False negative | Fraud incorrectly accepted |

```text
                    Predicted Fraud   Predicted Legitimate
Actual Fraud        True Positive     False Negative
Actual Legitimate   False Positive    True Negative
```

Common metrics:

- **Accuracy:** Fraction of all predictions that are correct
- **Precision:** Of predicted positives, how many are correct?
- **Recall:** Of actual positives, how many were found?
- **F1 score:** Balance between precision and recall

Accuracy can be misleading for imbalanced data. If only 1 out of 1,000 transactions is fraudulent, always predicting “legitimate” gives 99.9% accuracy but detects no fraud.

### Regression Metrics

Common regression metrics include:

- Mean Absolute Error
- Mean Squared Error
- Root Mean Squared Error
- Coefficient of determination

### Business Metrics

Technical performance is not enough. Also measure:

- Money saved
- Time reduced
- Customer satisfaction
- Manual-review workload
- Safety incidents
- Fairness across relevant groups

## 24. Neural Networks

A neural network is a model made of connected computational units arranged in layers.

```text
Input Layer        Hidden Layers         Output Layer

[x1] ─┐
[x2] ─┼──→ [nodes] ──→ [nodes] ──→ [prediction]
[x3] ─┘
```

Each connection has a weight. During training, the network adjusts weights to reduce prediction error.

Main ideas:

- Inputs enter the network.
- Layers transform the values.
- Activation functions introduce non-linear behaviour.
- The output represents a prediction or generated value.
- A loss function measures error.
- An optimization process adjusts weights.

Neural networks are inspired loosely by biological neurons, but they are mathematical systems and should not be described as artificial brains.

## 25. Deep Learning

**Deep Learning** uses neural networks with multiple processing layers.

It is especially useful for complex, high-dimensional data such as:

- Images
- Audio
- Video
- Natural language
- Sensor sequences

```text
Artificial Intelligence
        └── Machine Learning
                └── Deep Learning
```

Advantages:

- Learns complex patterns
- Can learn useful representations from raw data
- Performs strongly on many perception and language tasks

Challenges:

- Often requires large datasets and computation
- Can be difficult to explain
- Training can be expensive
- Behaviour depends heavily on data
- Failures may be difficult to predict

Large language models are based on deep-learning architectures.

## 26. Natural Language Processing

**Natural Language Processing**, or **NLP**, enables computers to work with human language.

NLP tasks include:

- Text classification
- Sentiment analysis
- Information extraction
- Named-entity recognition
- Search
- Translation
- Summarization
- Question answering
- Text generation

```text
Human Language
      |
      v
Tokenization and Representation
      |
      v
Language Model or NLP Algorithm
      |
      v
Classification, Extraction, Search, or Generation
```

Language is difficult because it contains ambiguity, context, idioms, sarcasm, cultural references, and domain-specific meanings.

Traditional NLP systems often used rules and task-specific statistical models. Modern NLP increasingly uses deep-learning and foundation models.

## 27. Computer Vision and Speech AI

### Computer Vision

Computer Vision enables systems to process images and video.

Common tasks:

- Image classification
- Object detection
- Image segmentation
- Face verification
- Optical character recognition
- Defect detection

Example:

```text
Factory Image → Vision Model → Defect location and confidence
```

### Speech AI

Speech AI includes:

- Speech-to-text
- Speaker recognition
- Speech classification
- Text-to-speech
- Voice assistants

```text
Audio → Speech Recognition → Text
Text  → Speech Synthesis   → Audio
```

Accuracy can vary because of accents, background noise, recording quality, language, and domain vocabulary.

## 28. Expert Systems, Robotics, and Recommendation Systems

### Expert Systems

An expert system applies encoded knowledge and rules to reach conclusions.

```text
Knowledge Base + Inference Rules + User Facts → Recommendation
```

Strengths:

- Rules can be inspected.
- Behaviour is predictable within the encoded domain.

Limitations:

- Knowledge acquisition is difficult.
- Rules become hard to maintain at scale.
- The system may fail outside known situations.

### Robotics

Robotics combines sensors, perception, planning, control, and physical actions.

```text
Sensors → Perception → Planning → Motor Action → Environment
   ^                                              |
   └──────────────── Feedback ────────────────────┘
```

### Recommendation Systems

Recommendation systems rank items that may interest a user.

Approaches include:

- Content-based recommendation
- Collaborative filtering
- Hybrid recommendation

Recommendations influence user choices, so diversity, fairness, transparency, and feedback loops matter.

## 29. AI Agents

An **agent** observes an environment and performs actions to achieve a goal.

```text
Environment → Observation → Agent → Action → Environment
```

An agent may include:

- A goal
- State or memory
- A decision policy
- Tools or allowed actions
- A planning process
- Feedback

Examples:

- A game-playing agent
- A warehouse robot
- A scheduling agent
- A software assistant that calls approved APIs

Agent behaviour should be constrained by permissions, validation, budgets, timeouts, and human approval. Giving a model a tool does not give it authority to use that tool without application checks.

## 30. Knowledge Representation, Reasoning, Search, and Planning

Not all AI is machine learning.

### Knowledge Representation

Knowledge may be represented as:

- Rules
- Facts
- Trees
- Graphs
- Ontologies
- Relationships

### Reasoning

Reasoning derives a conclusion from known information.

```text
Fact: All premium customers receive priority support.
Fact: Customer C101 is premium.
Conclusion: Customer C101 receives priority support.
```

### Search

Search algorithms explore possible states or solutions.

Examples:

- Finding a route
- Solving a puzzle
- Choosing a sequence of moves

### Planning

Planning chooses actions to reach a goal while respecting constraints.

These symbolic techniques may be combined with learned models. Hybrid systems often use AI for uncertain perception and ordinary code or rules for reliable decisions.

## 31. The AI Application Lifecycle

An AI feature has a lifecycle beyond initial development.

```text
Business Goal
    |
Data and Governance
    |
Model Development or Selection
    |
Evaluation
    |
Application Integration
    |
Deployment
    |
Monitoring
    |
Feedback and Improvement
```

Production responsibilities include:

- Versioning models and data
- Reproducible evaluation
- Access control
- Monitoring latency and errors
- Detecting quality changes
- Collecting feedback
- Rolling back unsafe releases
- Documenting limitations
- Retiring outdated models

### Data Drift and Concept Drift

- **Data drift:** Production input changes from the training distribution.
- **Concept drift:** The relationship between input and correct output changes.

Example: buying patterns learned before a major market change may no longer predict current behaviour.

MLOps applies engineering practices to model development, deployment, monitoring, and governance.

## 32. Responsible AI

Responsible AI means designing, building, and operating AI systems with attention to human impact.

Important principles:

| Principle | Meaning |
| --- | --- |
| Fairness | Avoid unjustified differences across people or groups |
| Reliability | Perform consistently under expected conditions |
| Safety | Prevent or reduce harmful outcomes |
| Privacy | Protect personal and confidential data |
| Security | Resist unauthorized access and manipulation |
| Transparency | Communicate when and how AI is used |
| Explainability | Provide useful reasons where required |
| Accountability | Assign human responsibility for decisions |
| Human oversight | Allow review, intervention, and appeal |

Questions to ask before release:

- Who benefits from the system?
- Who may be harmed?
- Is the data permitted and representative?
- What happens when the model is wrong?
- Can a person challenge the result?
- Which decisions require human approval?
- Are inputs, outputs, and logs protected?
- Is model performance monitored for relevant groups?

Responsible AI is an engineering requirement, not only a policy document.

## 33. Limitations and Risks of AI

AI systems have important limitations.

### Probabilistic Behaviour

Many AI outputs are probabilities or statistically likely results, not guaranteed facts.

### Dependency on Data

Models can reproduce errors and biases present in their data.

### Limited Generalization

A model may fail on situations different from its training or evaluation examples.

### Explainability Challenges

Complex models may not provide a simple reason for a prediction.

### Automation Bias

People may trust a computer-generated result too readily.

### Security Risks

Attackers may manipulate inputs, steal models or data, abuse tools, or extract confidential information.

### Feedback Loops

A model's decisions may influence future data. For example, recommending only popular products can make them even more popular and reduce diversity.

Use AI where uncertainty is acceptable and controlled. Use deterministic Java rules for strict constraints, permissions, calculations, and safety boundaries.

## 34. The Role of Java in AI Applications

Python is widely used for model research and training, but Java is important in enterprise AI systems.

Java strengths include:

- Mature web and enterprise frameworks
- Strong typing
- High-performance server applications
- Database integration
- Security libraries
- Observability and operations tooling
- Large existing enterprise codebases
- Concurrency support

A Java application can consume a model through:

- REST APIs
- gRPC services
- Message queues
- Cloud AI services
- Java machine-learning libraries
- Local inference libraries
- Spring AI integrations

```text
Java Service
├── Receives request
├── Validates data
├── Loads business context
├── Calls model
├── Validates model result
├── Applies business rules
└── Returns a controlled response
```

Example provider-neutral Java contract:

```java
public interface PredictionService<I, O> {
    O predict(I input);
}
```

Example DTOs:

```java
public record ChurnInput(
        int monthsActive,
        int complaintCount,
        double monthlyCharge) {
}

public record ChurnPrediction(
        boolean likelyToChurn,
        double probability,
        String modelVersion) {
}
```

The Java service should validate the probability range, record the model version, handle timeouts, and avoid making an unreviewed high-impact decision.

## 35. Predictive AI vs Generative AI

Predictive and generative systems solve different problems.

| Predictive AI | Generative AI |
| --- | --- |
| Estimates a label or number | Creates new content |
| Answers “what category?” or “what is likely?” | Answers “what content should be produced?” |
| Output may be a class, probability, or numeric value | Output may be text, code, image, audio, or video |
| Example: predict fraud | Example: explain why a transaction looks unusual |
| Example: predict delivery time | Example: write a delivery-status message |

```text
Predictive AI
Transaction Data → Model → Fraud Probability: 0.92

Generative AI
Transaction Context + Instruction → Model → Written Explanation
```

A real system may combine both:

1. A predictive model produces a risk score.
2. Deterministic Java rules decide whether review is required.
3. A generative model creates a draft explanation for the reviewer.
4. A human verifies the final decision.

The generative model should not invent the risk score or replace authorization rules.

## 36. From AI Foundations to Generative AI

Generative AI builds on the concepts introduced in these notes.

| Foundation concept | Generative AI connection |
| --- | --- |
| Data | Models learn patterns from large datasets |
| Features and representations | Models learn internal representations of language and media |
| Self-supervised learning | Foundation models learn from large amounts of unlabelled content |
| Neural networks | Generative models use deep neural architectures |
| Training | Model parameters are learned before application use |
| Inference | A prompt is processed to generate an output |
| Evaluation | Quality, relevance, safety, and factuality must be measured |
| Responsible AI | Privacy, bias, security, and human oversight remain necessary |
| Agents | Generative models may plan and request approved tools |

Next learning path:

```text
AI Foundations
      |
      v
Generative Models and Large Language Models
      |
      v
Prompts, Tokens, Context, and Structured Output
      |
      v
Embeddings, Vector Databases, RAG, and Tools
      |
      v
Java and Spring AI Integration
```

Continue with [Generative AI Notes for Java Freshers](generative-ai-notes-for-java-freshers.md).

## 37. Common Misconceptions

| Misconception | Correction |
| --- | --- |
| AI and robots mean the same thing | A robot is a physical system; AI is a broader software and research field |
| Every automated program uses AI | Fixed-rule automation may contain no AI |
| Machine learning and AI are identical | Machine learning is one approach within AI |
| Deep learning and machine learning are separate | Deep learning is a type of machine learning |
| AI always improves with more data | Data must be relevant, lawful, representative, and accurate |
| High accuracy means the model is good | The metric may be unsuitable or hide serious errors |
| A model understands exactly like a human | Models calculate outputs from learned representations and algorithms |
| AI removes the need for business rules | Deterministic rules remain essential for constraints and authority |
| A confident output is always correct | Confidence or fluent wording does not guarantee truth |
| Generative AI is all of AI | Generative AI is one major area within AI |
| AI eliminates human responsibility | People and organizations remain accountable for system use |
| Java cannot be used for AI | Java is widely used to integrate and operate AI in enterprise applications |

## 38. Practice Exercises

### Concept Exercises

1. Classify each system as traditional programming, automation, predictive AI, or Generative AI:
   - Monthly salary calculation
   - Spam detection
   - Scheduled database backup
   - Product-description generation
2. Give three classification examples and three regression examples.
3. Explain why training accuracy alone is insufficient.
4. Identify possible features and a label for employee-attrition prediction.
5. Draw the training and inference flows separately.
6. Give one example of data leakage.
7. Explain a situation in which precision matters more than recall.
8. Explain a situation in which recall matters more than precision.
9. List five risks of an AI-based loan application system.
10. Describe which parts of an AI application should remain deterministic Java code.

### Scenario: Support-Ticket Classification

A company wants to classify support tickets as `BILLING`, `TECHNICAL`, `ACCOUNT`, or `OTHER`.

Answer the following:

1. Is this classification or regression?
2. What is one training example?
3. What is the feature data?
4. What is the label?
5. How would incorrect labels affect the model?
6. Which metrics would you review?
7. What should happen when model confidence is low?
8. What should the Java application log?
9. Which personal data should be removed or protected?
10. How could the system be monitored after deployment?

### Scenario: AI-Assisted Loan Review

Design a responsibility table with these columns:

```text
Operation | AI Model | Java Rule | Human Reviewer
```

Include credit-risk prediction, legal eligibility, missing-document checks, explanation drafting, approval, and appeal handling.

## 39. Frequently Asked Interview Questions

### What Is Artificial Intelligence?

Artificial Intelligence is the field of creating computer systems that perform tasks associated with intelligence, such as learning, prediction, perception, reasoning, planning, and language processing.

### What Is the Difference Between AI and Machine Learning?

AI is the broader field. Machine Learning is an approach within AI in which models learn patterns from data.

### What Is the Difference Between Machine Learning and Deep Learning?

Deep Learning is a type of Machine Learning that uses neural networks with multiple layers.

### What Is Narrow AI?

Narrow AI is designed for a limited task or related group of tasks. Present-day production AI systems are narrow AI.

### What Is a Feature?

A feature is an input value used by a machine-learning model to make a prediction.

### What Is a Label?

A label is the expected output supplied with a supervised-learning example.

### What Is Supervised Learning?

Supervised learning trains a model with input examples that include known correct labels.

### What Is Unsupervised Learning?

Unsupervised learning finds patterns or structure in data without target labels.

### What Is Reinforcement Learning?

Reinforcement learning trains an agent through actions and reward or penalty feedback from an environment.

### What Is a Model?

A model is a learned mathematical or computational representation that converts input into a prediction or output.

### What Is the Difference Between Training and Inference?

Training learns model parameters from data. Inference uses the trained model on new input.

### What Is Overfitting?

Overfitting occurs when a model learns training details too closely and performs poorly on relevant unseen data.

### Why Can Accuracy Be Misleading?

Accuracy can hide failure on rare but important classes. Precision, recall, F1 score, and business impact may be more useful.

### What Is Data Leakage?

Data leakage occurs when model training uses information that would not be available when a real prediction is made.

### What Is an AI Agent?

An AI agent observes an environment and selects actions to achieve a goal, often using state, planning, policies, or tools.

### What Is Responsible AI?

Responsible AI is the practice of building and operating AI with fairness, reliability, safety, privacy, security, transparency, accountability, and human oversight.

### What Is the Difference Between Predictive AI and Generative AI?

Predictive AI estimates a category, probability, or number. Generative AI creates new content such as text, code, images, or audio.

### Can AI Replace Traditional Java Programming?

No. AI handles uncertain, pattern-based tasks, while Java code remains necessary for validation, authorization, calculations, transactions, integration, and other deterministic behaviour.

### Why Is Java Useful in AI Systems?

Java is useful for building secure, scalable enterprise services that integrate models with APIs, databases, business rules, monitoring, and production infrastructure.

---

[← Home](index.md) · [Next: Part 1: Generative AI with Java →](generative-ai-with-java.md)
