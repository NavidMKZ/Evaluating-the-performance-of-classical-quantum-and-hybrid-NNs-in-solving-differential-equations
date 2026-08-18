# Evaluating the Performance of Classical, Quantum, and Hybrid Quantum Neural Networks in Solving Differential Equations

This repository contains the code and trained models associated with the research article:

> **Evaluating the performance of classical, quantum, and hybrid quantum neural networks in solving differential equations**

**Navid Markazi and Behrouz Mirza**  
*Scientific Reports*, Volume 15, Article 39332, 2025

**DOI:** [10.1038/s41598-025-22950-y](https://doi.org/10.1038/s41598-025-22950-y)  
**Published:** 10 November 2025

[Read the paper on Nature](https://www.nature.com/articles/s41598-025-22950-y)  
[View the GitHub repository](https://github.com/NavidMKZ/Evaluating-the-performance-of-classical-quantum-and-hybrid-NNs-in-solving-differential-equations)

---

## Overview

This project investigates the performance of **classical neural networks (CNNs), quantum neural networks (QNNs), and hybrid quantum neural networks (HQNNs)** for solving differential equations.

The study evaluates these three model types using both supervised and unsupervised learning approaches and compares their:

- prediction accuracy;
- convergence behavior;
- number of trainable parameters;
- sensitivity to parameter initialization; and
- performance across different physical problems.

Three problems are investigated:

1. **Damped harmonic oscillator**
2. **Einstein field equations for the Schwarzschild metric**
3. **Time-independent Schrödinger equation for a particle in a two-dimensional infinite potential box**

Physics-informed supervised neural networks are used for the damped harmonic oscillator and Einstein field equations. For the Schrödinger equation, an unsupervised neural-network method based on the shooting method is developed to obtain the eigenvalues and corresponding wave functions.

---

## Research Paper

**Markazi, N., & Mirza, B. (2025).**  
*Evaluating the performance of classical, quantum, and hybrid quantum neural networks in solving differential equations.*  
**Scientific Reports, 15, 39332.**

**DOI:** [https://doi.org/10.1038/s41598-025-22950-y](https://doi.org/10.1038/s41598-025-22950-y)

The complete mathematical formulation, model architectures, experimental setup, and detailed results are provided in the published article.

---

# Problems Studied

## 1. Damped Harmonic Oscillator

The first problem considers an underdamped harmonic oscillator and uses a physics-informed neural-network formulation to learn its solution.

The model is trained over the time interval:

**t ∈ [0, 8]**

A total of **124 training points** and **500 test points** are used.

The initial condition is incorporated directly into the physics-informed formulation.

### Classical Neural Network

The classical network contains **three hidden layers** with the **SiLU** activation function.

The model produces two outputs that are combined through a multiplicative structure to construct the final approximation to the solution.

Three classical configurations are investigated:

| Configuration | Hidden-layer size |
|---|---:|
| 1 | 10 |
| 2 | 30 |
| 3 | 50 |

The learning rate is **0.001**.

### Quantum Neural Network

The QNN uses **two qubits**.

The quantum feature map combines exponential gates with `RY` rotations and CNOT entangling operations. The feature-map design is motivated by the qualitative structure of the damped-oscillator solution, which contains both an exponentially decaying component and oscillatory behavior.

Three quantum configurations are investigated:

| Configuration | Variational layers |
|---|---:|
| 1 | 1 |
| 2 | 2 |
| 3 | 3 |

The QNN learning rate is **0.05**.

### Hybrid Quantum Neural Network

The hybrid architecture combines a classical neural network with a three-qubit quantum circuit.

The classical network produces values that are encoded into quantum rotation gates. The quantum circuit then processes the encoded information using variational and entangling operations. The resulting quantum outputs are used to construct the final prediction.

Three hybrid configurations are investigated using hidden-layer sizes of:

- 10 neurons;
- 30 neurons;
- 50 neurons.

The hybrid learning rate is **0.001**.

### Representative Result

For the representative seed-42 experiment, the lowest reported MSE values are:

| Model | Lowest MSE |
|---|---:|
| Classical | 6.09 × 10⁻⁷ |
| Quantum | 2.52 × 10⁻¹² |
| Hybrid | 5.13 × 10⁻⁸ |

For this experiment, the QNN obtained the lowest MSE, followed by the HQNN and the classical neural network.

---

# 2. Einstein Field Equations for the Schwarzschild Metric

The second problem investigates the Einstein field equations associated with the Schwarzschild metric.

The neural networks learn the metric functions while satisfying the corresponding Einstein-equation residuals and the required asymptotic boundary behavior.

The radial domain is:

**r ∈ [100, 300]**

The experimental setup uses **128 training points** and **500 test points**.

The calculations use:

- **G = 1**
- **M = 15**

### Loss Function

The loss contains weighted contributions from the relevant Einstein-equation residuals and the boundary condition.

The loss-function weights are:

**W₀ = 10, W₁ = 10, W₂ = 0.001, W₃ = 10**

The boundary condition enforces the appropriate asymptotic behavior of the metric at large radial distance.

### Classical Neural Network

The classical network contains **six hidden layers** and uses the **LogSigmoid** activation function.

The final layer predicts the metric functions used in the Schwarzschild solution.

Three classical configurations are evaluated:

| Configuration | Hidden-layer size |
|---|---:|
| 1 | 10 |
| 2 | 30 |
| 3 | 50 |

The classical learning rate is **0.0005**.

### Quantum Neural Network

The QNN uses **two qubits**.

The radial input is rescaled before being encoded into the quantum circuit. The feature-map design is motivated by the approximate inverse-radial behavior appearing in the Schwarzschild solution.

The circuit contains trainable rotations and CNOT entangling operations. A nonlinear post-processing transformation is applied to the measured quantum output.

The quantum learning rate is **0.025**.

### Hybrid Quantum Neural Network

The HQNN combines classical and quantum components and uses a three-qubit quantum circuit.

The classical part processes the radial coordinate before encoding the resulting information into the quantum circuit. The measured quantum outputs are then processed classically to obtain the final prediction.

The hybrid architecture is designed to represent the coupled structure of the metric functions and the underlying Einstein equations.

### Main Observation

The relative performance of the models depends on the metric component and the initialization.

The paper reports that the HQNN can achieve strong overall prediction accuracy, while the classical network can obtain lower MSE values for individual metric components in the evaluated configurations. This demonstrates that model comparison should not rely on a single metric alone.

---

# 3. Time-Independent Schrödinger Equation

The third problem considers a particle in a **two-dimensional infinite potential box**.

Unlike the first two problems, this problem is treated using an **unsupervised neural-network approach based on the shooting method**.

The method is designed to determine both the wave function and its associated energy eigenvalue.

## Unsupervised Shooting-Based Approach

The optimization is performed using the Schrödinger equation and normalization conditions rather than supervised target data.

The approach is used to investigate the low-lying energy states of the two-dimensional box.

### Classical Neural Network

The classical model represents the wave function and energy using neural-network-based functions.

Three configurations are investigated using hidden-layer sizes of:

- 10 neurons;
- 30 neurons;
- 50 neurons.

The classical learning rate is **0.002**.

The study investigates the first three energy levels.

### Quantum Neural Network

The QNN uses **three qubits**.

The quantum circuit encodes the spatial variables and the energy-related input using `RY` rotations and controlled operations.

The feature-map design is chosen to represent the oscillatory behavior expected for the wave functions of a particle in an infinite potential box.

The quantum feature-map parameters are initialized in the range **[3, 3.5]**.

The QNN learning rate is **0.05**.

### Hybrid Quantum Neural Network

The HQNN combines classical processing with a quantum circuit and is used to predict the spatial wave function and the corresponding energy.

The quantum parameters are initialized using a normal distribution, while the classical parameters use Xavier initialization.

The hybrid learning rate is **0.005**.

The model is tested on the first three energy levels.

### Main Observation

The quantum and hybrid approaches can represent the ground state and low-lying excited states, but optimization becomes increasingly difficult for higher excited states.

In the reported experiments, the hybrid model successfully predicts the ground and first excited states but does not successfully predict the second excited state. The quantum models also exhibit increased optimization difficulty for some excited-state configurations.

---

# Neural Network Architectures

## Classical Neural Networks

The classical models are feedforward neural networks embedded within physics-informed or unsupervised learning frameworks.

The networks learn the solution functions while the loss functions incorporate the governing differential equations and relevant physical constraints.

Automatic differentiation is used to calculate the required derivatives.

---

## Quantum Neural Networks

The QNNs consist of three main stages:

**Quantum feature map → Variational quantum circuit → Measurement**

The general circuit structure can be summarized as:

**Input data → Quantum encoding → Trainable quantum layers → Measurement → Model output**

The quantum feature maps are designed specifically for the physical problem rather than using a single generic encoding for all experiments.

The study introduces **three variational parameterized quantum feature maps** and investigates different quantum configurations across the problems.

Examples include:

- exponential encoding for the decaying behavior of the damped harmonic oscillator;
- physics-motivated nonlinear encoding for the Schwarzschild problem; and
- oscillatory `RY`-based encoding for the Schrödinger equation.

---

## Hybrid Quantum Neural Networks

The hybrid architectures combine classical neural networks and quantum circuits.

A typical hybrid structure can be represented as:

**Classical Neural Network → Quantum Circuit → Classical Output Layer**

The precise architecture differs between the physical problems.

Classical outputs are encoded into the quantum circuit through quantum rotation gates. The resulting quantum measurements are then processed to obtain the final model prediction.

The paper introduces **two quantum-circuit architectures** for the hybrid models.

---

# Training and Evaluation

Each model type is evaluated using **three configurations**.

For the classical and hybrid models, the configurations correspond to hidden-layer sizes of:

- 10;
- 30;
- 50 .

For the quantum models, the configurations correspond to:

- 1 variational layer;
- 2 variational layers;
- 3 variational layers.

The experiments are evaluated using the following four random seeds:

```text
14
42
86
195
```

Seed 42 is used for the detailed representative analysis, while the results across all four seeds are used to evaluate the mean and standard deviation of the model errors.

All models use the **Adam optimizer**.

The Adam parameters are:

```text
β₁ = 0.9
β₂ = 0.99
ε  = 10⁻⁸
```

The learning rate varies by model and physical problem as described in the corresponding sections above.

---

# Parameter Efficiency

An important part of the comparison is the number of trainable parameters.

The quantum and hybrid quantum neural networks use substantially fewer trainable parameters than the corresponding classical models in the investigated configurations.

The study therefore evaluates not only prediction accuracy but also the parameter count required by each architecture.

---

# Convergence Behavior

The study also compares the optimization behavior of the three model types.

The reported results show that the quantum and hybrid models can converge faster to the solutions than the classical models in the investigated experiments.

However, convergence behavior depends on the problem and parameter initialization. Quantum models can also exhibit greater variability and optimization difficulties, particularly for some excited-state Schrödinger-equation configurations.

---

# Sensitivity to Parameter Initialization

Parameter initialization has a substantial effect on all three model types.

The experiments therefore use four different random seeds rather than relying on a single initialization.

The reported results show that:

- all three model classes are sensitive to initialization;
- QNNs display the largest variability across the investigated problems;
- individual best-case results should therefore not be interpreted as universally representative.

This is an important consideration when comparing classical and quantum machine-learning models.

---

# Main Findings

The main conclusions of the study can be summarized as follows.

### Damped harmonic oscillator

The QNN achieves the best accuracy in the representative seed-42 experiment.

### Einstein field equations

Hybrid and classical models can both provide strong predictions, but their relative performance depends on the metric component and initialization.

### Time-independent Schrödinger equation

Quantum and hybrid approaches can reproduce low-lying energy states, while higher excited states present greater optimization challenges.

### Overall comparison

The study finds that:

- QNNs and HQNNs can use fewer trainable parameters than classical models;
- QNNs and HQNNs can converge faster to the solutions in the investigated experiments;
- QNNs can achieve particularly strong accuracy for the damped harmonic oscillator;
- HQNNs achieve higher accuracy than classical models in most of the favorable-initialization cases reported in the paper;
- quantum models are more sensitive to parameter initialization;
- performance depends strongly on the physical problem and model configuration.

The results therefore should **not** be interpreted as evidence that quantum neural networks universally outperform classical neural networks. Instead, they demonstrate that physics-informed quantum and hybrid architectures can be competitive and, under favorable conditions, superior for particular differential-equation problems.

---

# Physics-Informed Quantum Feature Maps

A central idea of this work is to incorporate information about the underlying physical solution into the design of the quantum feature map.

The general approach is:

**Physical behavior → Feature-map design → Quantum representation → Variational optimization**

This allows the quantum circuit to reflect known qualitative properties of the solution.

Examples studied in this work include:

| Physical problem | Relevant behavior | Feature-map idea |
|---|---|---|
| Damped harmonic oscillator | Exponential decay and oscillation | Exponential and `RY` encoding |
| Schwarzschild metric | Approximate inverse-radial behavior | Physics-motivated nonlinear encoding |
| 2D Schrödinger equation | Oscillatory wave function | Oscillatory `RY` encoding |

The goal is to improve the representation of the target function while keeping the quantum circuits relatively small.

---

# Software and Libraries

The implementation uses:

- **Python**
- **PyTorch**
- **PennyLane**

PyTorch is used for the classical neural-network and optimization components, while PennyLane is used to construct and differentiate the quantum circuits.

---

# Repository Structure

The repository is organized around the three physical problems:

```text
.
├── Damped Harmonic Oscillator/
│   ├── source and analysis/
│   └── trained_models/
│
├── Einstein Field Equations for Schwarzschild Metric/
│   ├── source and analysis/
│   └── trained_models/
│
├── Time-Independent Schrödinger Equation for Particle in a Two Dimensional Box/
│   ├── source and analysis/
│   └── trained_models/
│
├── CITATION.cff
├── LICENSE
└── README.md
```

The repository currently contains separate directories for the damped harmonic oscillator, Einstein field equations, and time-independent Schrödinger equation, each with source/analysis materials and trained models.

---

# Notebook Organization

For each physical problem, the repository provides notebooks for the computational implementation and analysis.

The general organization is:

### Source-Code Notebook

Contains the implementation of:

- model architectures;
- quantum circuits;
- feature maps;
- loss functions;
- training procedures; and
- numerical calculations.

### Seed-42 Analysis Notebook

Contains the detailed analysis associated with the representative seed-42 results.

### Mean-and-Deviation Notebook

Analyzes the results obtained using the four random seeds and calculates the corresponding statistical quantities.

For example, the damped harmonic oscillator directory contains separate source, seed-42 analysis, and mean/deviation notebooks. The other two problem directories follow the same general organization.

---

# Reproducing the Experiments

The notebooks in this repository provide the computational implementation used for the experiments reported in the paper.

A typical workflow is:

1. Clone the repository.
2. Install Python and the required libraries.
3. Open the source-code notebook for the selected physical problem.
4. Run the model and training cells.
5. Open the corresponding analysis notebook to reproduce the reported evaluations.
6. Use the mean-and-deviation notebook to study the results across the four random seeds.

## Clone the repository

```bash
git clone https://github.com/NavidMKZ/Evaluating-the-performance-of-classical-quantum-and-hybrid-NNs-in-solving-differential-equations.git
```

Then enter the repository:

```bash
cd Evaluating-the-performance-of-classical-quantum-and-hybrid-NNs-in-solving-differential-equations
```

The notebooks can be opened using Jupyter Notebook or JupyterLab.

```bash
jupyter notebook
```

Because the experiments involve neural-network optimization and differentiable quantum-circuit simulation, execution time depends on the available computational resources and the selected configuration.

---

# Reproducibility

The experiments use four random seeds:

```text
14
42
86
195
```

Using multiple seeds is important because all three model classes are sensitive to parameter initialization, with QNNs showing particularly strong variability in the reported experiments.

The detailed seed-42 results provide a representative comparison, while the four-seed analysis gives a broader view of the robustness of the conclusions.

The repository also includes trained-model directories corresponding to the investigated model configurations.

---

# Limitations

The results should be interpreted within the scope of the experimental setup.

The quantum circuits are relatively small and the experiments are performed using quantum-circuit simulation rather than large-scale fault-tolerant quantum hardware.

The training datasets are also relatively small because of computational limitations.

In addition, the performance of the quantum and hybrid models depends significantly on parameter initialization, and some excited-state Schrödinger-equation configurations are difficult to optimize.

Therefore, the purpose of this work is **not** to claim a universal computational advantage for quantum neural networks. The purpose is to investigate how classical, quantum, and hybrid architectures compare for representative differential-equation problems under physics-informed and unsupervised learning frameworks.

---

# Citation

If you find this code useful in your research, we kindly ask that you cite the associated paper.

### BibTeX

```bibtex
@article{Markazi2025,
  author  = {Markazi, Navid and Mirza, Behrouz},
  title   = {Evaluating the performance of classical, quantum, and hybrid quantum neural networks in solving differential equations},
  journal = {Scientific Reports},
  volume  = {15},
  number  = {1},
  pages   = {39332},
  year    = {2025},
  doi     = {10.1038/s41598-025-22950-y},
  url     = {https://doi.org/10.1038/s41598-025-22950-y}
}
```

A `CITATION.cff` file is included in this repository to provide structured citation information for the code and its associated publication.

---

# Authors

**Navid Markazi**  
Department of Physics, Isfahan University of Technology, Isfahan, Iran

**Behrouz Mirza**  
Department of Physics, Isfahan University of Technology, Isfahan, Iran

For the complete author and affiliation information, please refer to the published article.

---

# License

The code in this repository is distributed under the **MIT License**.

See the [`LICENSE`](LICENSE) file for the complete license text.

The published research article has its own publication license and reuse conditions. Please refer to the [published article](https://www.nature.com/articles/s41598-025-22950-y) for those terms.

---

# Related Links

- **Research paper:** https://www.nature.com/articles/s41598-025-22950-y
- **DOI:** https://doi.org/10.1038/s41598-025-22950-y
- **GitHub repository:** https://github.com/NavidMKZ/Evaluating-the-performance-of-classical-quantum-and-hybrid-NNs-in-solving-differential-equations

---

# Acknowledgment

This repository accompanies the research presented in the published *Scientific Reports* article and is provided to facilitate inspection, reproduction, reuse, and further research on classical, quantum, and hybrid neural-network approaches to solving differential equations.
