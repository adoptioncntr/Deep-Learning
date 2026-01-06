#My Venture into Deep Learning! Experiments, Models, and Training Strategy
This repository is a personal deep learning playground where I explore **how model design, optimization choices, and training strategy actually affect performance**.

Rather than treating models as black boxes, I use these projects to:
- build strong baselines,
- experiment methodically,
- analyze training dynamics,
- and document what genuinely improves generalization.

Most work is implemented in **PyTorch**, with a focus on **image classification**, **representation learning**, and **sequence modeling**.

---

## What I Care About

- Understanding *why* a model works, not just that it works  
- Fair comparisons (same parameter budget, same data, same metrics)  
- Training stability, overfitting behavior, and efficiency  
- Turning open-ended prompts into structured experiments  

This repo reflects how I approach real ML problems: **hypothesis → experiment → analysis → iteration**.

---

## Projects

### 1) Exploring Architecture & Training Tradeoffs (MNIST)

I started by asking a simple question:  
**How much do architecture and training choices matter when everything else is controlled?**

I built a progression of models:
- a linear softmax classifier as a baseline,
- multi-layer dense networks with carefully controlled parameter counts,
- and convolutional models for comparison.

Instead of blindly scaling models, I:
- derived parameter budgets analytically,
- tested depth vs. width tradeoffs,
- compared optimizers and activation functions,
- and evaluated CNNs vs MLPs based on *efficiency*, not just accuracy.

**What this project taught me:**
- deeper isn’t always better,
- CNNs win not just on accuracy, but parameter efficiency,
- and training curves often reveal more than final accuracy.

📁 `projects/01-mnist-architecture-search/`

---

### 2) Learning Representations & Transferring Knowledge (MNIST → CIFAR-10)

This project focuses on **representation learning** and **transfer learning**.

#### Autoencoders & Latent Structure
I trained autoencoders with different bottleneck sizes to explore how much compression MNIST can tolerate before reconstruction quality degrades. This gave intuition for the *intrinsic dimensionality* of the data.

#### Using Learned Features for Classification
I then reused the encoder for classification under different conditions:
- trained vs random encoders,
- frozen vs finetuned representations.

This let me measure not just final accuracy, but **training efficiency and stability**.

#### Transfer Learning on CIFAR-10
To explore transfer learning more deeply, I pretrained a model on a **proxy task**: detecting whether an image was upright or rotated.  
I then transferred different numbers of early layers into a CIFAR-10 classifier and tested:
- how much transfer helps,
- how deep transfer remains beneficial,
- and whether freezing or finetuning works better.

**What this project taught me:**
- not all learned features are equally reusable,
- shallow features often transfer better,
- and proxy tasks can meaningfully improve convergence.

📁 `projects/02-representation-transfer/`

---

### 3) Sequence Modeling & Learned Memory

This project moves beyond images into **temporal data** and **sequential reasoning**.

#### Change Point Detection
I generated synthetic sequences where the underlying data distribution changes at an unknown timestep.  
I trained both:
- recurrent models (RNN/LSTM),
- and 1D convolutional models,

and evaluated how quickly and accurately each detected the shift.

Instead of raw accuracy, I visualized **detection probability over time**, which made the inductive biases of each model much clearer.

#### Neural Summarization & Querying
I also built a model that:
- summarizes a sequence into a fixed-size vector,
- and answers queries like “Did value X appear?” or “Did X and X+1 appear consecutively?”

This tested whether a learned embedding could act like a compact memory structure.

**What this project taught me:**
- CNNs can outperform RNNs on some temporal tasks,
- embeddings encode more structure than expected,
- and task-specific evaluation is critical for sequence models.

📁 `projects/03-sequence-models/`

---

### 4) CIFAR-10: Pushing Performance with Structured Experimentation

This is an open-ended project where I bring everything together to train the **strongest CIFAR-10 model I can**, while keeping the process reproducible and explainable.

Instead of jumping straight into long training runs, I:
- run fast experiments to identify promising model families,
- compare augmentations and regularization strategies,
- test optimizers and learning rate schedules,
- and only then launch longer, full training runs.

**Focus areas:**
- architecture selection
- data augmentation
- optimizer + schedule tuning
- overfitting control
- explainable gains (no “magic numbers”)

This project reflects how I’d approach model development in practice:  
**validate quickly → scale confidently**.

📁 `projects/04-cifar-best-performance/`

---

## How to Run

### Setup
```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
