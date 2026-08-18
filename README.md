# Evaluating the Performance of Classical, Quantum, and Hybrid Quantum Neural Networks in Solving Differential Equations

This repository contains the code and trained models associated with the research article:

> **Evaluating the performance of classical, quantum, and hybrid quantum neural networks in solving differential equations**

**Navid Markazi and Behrouz Mirza**  
*Scientific Reports*, Volume 15, Article 39332 (2025)

**DOI:** [10.1038/s41598-025-22950-y](https://doi.org/10.1038/s41598-025-22950-y)  
**Published:** 10 November 2025

[Read the paper on Nature](https://www.nature.com/articles/s41598-025-22950-y)  
[View the source code on GitHub](https://github.com/NavidMKZ/Evaluating-the-performance-of-classical-quantum-and-hybrid-NNs-in-solving-differential-equations)

---

## Overview

This project investigates the use of **classical neural networks (CNNs), quantum neural networks (QNNs), and hybrid quantum-classical neural networks (HQNNs)** for solving differential equations.

The work combines physics-informed machine learning with variational quantum circuits and evaluates the three neural-network paradigms in terms of:

- prediction accuracy;
- optimization and convergence behavior;
- point-wise prediction error;
- number of trainable parameters;
- robustness to parameter initialization; and
- performance across different physical problems.

Three problems with different mathematical and physical structures are considered:

1. **Damped harmonic oscillator**
2. **Einstein field equations for the Schwarzschild metric**
3. **Time-independent Schrödinger equation for a particle in a two-dimensional infinite potential box**

Physics-informed supervised neural networks are used for the damped harmonic oscillator and Einstein field equations. For the Schrödinger equation, an unsupervised neural-network approach based on a shooting-method formulation is developed for solving the eigenvalue problem.

---

## Research Paper

The complete scientific description of the methods, mathematical formulation, experiments, and results is available in the published article:

**Markazi, N., & Mirza, B. (2025).**  
*Evaluating the performance of classical, quantum, and hybrid quantum neural networks in solving differential equations.*  
**Scientific Reports, 15, 39332.**

**DOI:** [https://doi.org/10.1038/s41598-025-22950-y](https://doi.org/10.1038/s41598-025-22950-y)

The repository is intended to provide the computational implementation accompanying the publication.

---

# Problems Studied

## 1. Damped Harmonic Oscillator

The first problem considers an underdamped harmonic oscillator and is solved using a **physics-informed neural-network formulation**.

The neural networks learn the solution of the governing differential equation while satisfying the relevant initial condition. The training data are sampled from the time interval

\[
t \in [0,8].
\]

A total of **124 points** are used for training and **500 points** are reserved for testing. Because the initial-condition point is included in the batches, each effective training batch contains 32 data points.

### Classical neural network

The classical architecture contains **three hidden layers** with the **SiLU** activation function.

The network produces two outputs, which are multiplied together to construct the final approximation to the solution. This multiplicative output structure was introduced to improve expressivity without increasing the network depth substantially. The same structural idea is used in the corresponding hybrid architecture.

Three classical configurations are investigated:

| Configuration | Hidden-layer size |
|---|---:|
| 1 | 10 |
| 2 | 30 |
| 3 | 50 |

The classical learning rate is **0.001**.

### Quantum neural network

The QNN uses **two qubits**.

Its physics-informed quantum feature map combines:

- exponential gates;
- \(R_Y\) rotation gates; and
- CNOT entangling gates.

The feature-map structure is motivated by the qualitative form of the damped oscillator solution, which contains both an exponentially decaying envelope and oscillatory behavior.

The exponential part is therefore used to represent the decaying component, while the \(R_Y\) component represents oscillatory behavior. The variational circuit contains \(R_Y\) rotations and CNOT entanglement.

Three quantum configurations are investigated using:

| Configuration | Variational layers |
|---|---:|
| 1 | 1 |
| 2 | 2 |
| 3 | 3 |

For this problem, the QNN uses a learning rate of **0.05**.

### Hybrid quantum neural network

The hybrid architecture combines a classical neural network with a three-qubit quantum circuit.

For this problem, the classical component uses a single hidden layer and produces outputs that are encoded into \(R_Y\) gates in the quantum circuit. The circuit contains four quantum layers and uses linear CNOT entanglement. The outputs are subsequently combined using the same multiplicative structure used by the classical network.

The three HQNN configurations use hidden-layer sizes of:

- 10 neurons;
- 30 neurons;
- 50 neurons.

The hybrid learning rate is **0.001**.

### Representative result

For random seed 42, the lowest reported MSE values for the damped harmonic oscillator were:

| Model | Lowest MSE |
|---|---:|
| Classical | \(6.09\times10^{-7}\) |
| Quantum | \(2.52\times10^{-12}\) |
| Hybrid | \(5.13\times10^{-8}\) |

For this particular experiment, the QNN produced the lowest MSE, followed by the HQNN and then the classical network. The paper reports the corresponding reductions in MSE relative to the classical result as 99.99% for the QNN and 91.57% for the HQNN.

---

# 2. Einstein Field Equations for the Schwarzschild Metric

The second problem investigates the Einstein field equations associated with the **Schwarzschild metric**.

The neural networks learn the relevant metric functions while satisfying the Einstein-equation residuals and an asymptotic boundary condition.

The radial domain is

\[
r \in [100,300].
\]

The computational setup uses **128 training points** and **500 test points**. Training is performed for **20 epochs**, with **50 iterations per epoch** and a batch size of **32**. The calculations use \(G=1\) and \(M=15\).

The loss function includes weighted contributions from the relevant Einstein-equation terms and a boundary-condition term. The weights are

\[
\{W_0,W_1,W_2,W_3\}
=
\{10,10,0.001,10\}.
\]

The boundary condition enforces the appropriate asymptotic behavior of the metric component \(g_{00}\), recovering the expected Schwarzschild/Newtonian limit at large radial distances.

### Classical neural network

The classical architecture contains **six hidden layers** using the **LogSigmoid** activation function.

The final layer has two outputs corresponding to the learned functions \(\alpha(r)\) and \(\beta(r)\).

Three configurations are studied using hidden-layer sizes of:

| Configuration | Hidden-layer size |
|---|---:|
| 1 | 10 |
| 2 | 30 |
| 3 | 50 |

The classical learning rate is **0.0005**.

### Quantum neural network

The QNN uses **two qubits**.

Before quantum encoding, the radial input is scaled by dividing by 100. The feature-map design is motivated by the approximately \(1/r\)-type behavior of the Schwarzschild solution.

The model uses a trainable feature map followed by a variational circuit containing \(R_Y\) rotations and CNOT entanglement. An inverse-sine transformation is applied to the measured output to increase the model's nonlinear representational capability.

The quantum learning rate is **0.025**.

### Hybrid quantum neural network

The HQNN combines a classical neural network with a quantum component and a final classical layer.

The architecture is adapted to the coupled structure of the Einstein equations and uses a three-qubit quantum circuit. Classical outputs are encoded into the quantum circuit, processed by variational and entangling layers, and passed to a final classical layer.

For the Schwarzschild problem, the hybrid model achieved the highest overall prediction accuracy in the paper's comparison based on the relevant metric components, although the classical model achieved lower MSE values for the individual metric components across the evaluated seeds.

---

# 3. Time-Independent Schrödinger Equation

The third problem considers a particle in a **two-dimensional infinite potential box** and solves the time-independent Schrödinger equation as an eigenvalue problem.

Unlike the first two problems, this problem uses an **unsupervised neural-network formulation based on a shooting-method approach**.

The method is designed to obtain both the wave function and the corresponding energy eigenvalue.

## Unsupervised shooting-based approach

The loss function is constructed from the Schrödinger equation and a normalization condition rather than introducing additional loss terms specifically constraining individual excited states.

The approach allows the optimization to start from an energy estimate and move toward different eigenstates. The formulation is particularly useful for studying the first several energy levels of the two-dimensional box.

### Classical neural network

The classical implementation uses a neural architecture for representing the wave function together with a separate mechanism for predicting the energy.

The study evaluates hidden-layer sizes of:

- 10 neurons;
- 30 neurons;
- 50 neurons.

The classical learning rate for this problem is **0.002**.

The first three energy levels are investigated.

### Quantum neural network

The quantum model uses **three qubits**.

The first two qubits represent the spatial wave-function output, while the third qubit is used for energy prediction.

The inputs \(x\), \(y\), and \(\lambda\) are encoded into corresponding \(R_Y\) rotations. Controlled operations introduce correlations between the energy prediction and the spatial wave function.

The physics-informed circuit design reflects the oscillatory structure expected for a particle in a two-dimensional infinite potential well.

The feature-map parameters are initialized in the range

\[
[3,3.5],
\]

which was selected to provide sufficient oscillatory behavior for representing excited states.

The QNN learning rate is **0.05**.

### Hybrid quantum neural network

The HQNN combines classical and quantum components and produces the wave-function representation and energy prediction through separate classical/quantum pathways.

The quantum parameters are initialized from a normal distribution with mean 0 and standard deviation 0.2, while the classical component uses Xavier initialization.

The learning rate for the hybrid model is **0.005**.

The study evaluates the first three energy levels. The hybrid architecture performs well for the ground and first excited states but does not successfully predict the second excited state in the reported experiments. The QNN also shows optimization difficulties for some excited-state configurations, including extended flat regions in the loss landscape.

---

# Neural Network Paradigms

The repository contains three broad model classes.

## Classical Neural Networks

The classical models are conventional feedforward neural networks used within a physics-informed learning framework.

Their outputs are used to construct approximate solutions to the governing differential equations, while automatic differentiation is used to evaluate the required derivatives and residuals.

## Quantum Neural Networks

The QNNs are composed of three main components:

1. **Quantum feature map**
2. **Variational quantum circuit**
3. **Quantum measurement**

The general quantum state can be represented as

\[
|\phi(X,\hat{\theta}_1,\hat{\theta}_2)\rangle
=
W(\hat{\theta}_2)
U(X,\hat{\theta}_1)
|0\rangle^{\otimes n},
\]

where \(U\) is the feature-map circuit and \(W\) is the trainable variational circuit.

The network output is obtained from quantum measurements, using Pauli-\(Z\) expectation values and trainable scaling parameters.

A central design principle of the work is the use of **physics-informed quantum feature maps**. Rather than using the same generic encoding for every problem, the feature map is selected according to the expected qualitative behavior of the physical solution.

Examples include:

- exponential encoding for decay in the damped oscillator;
- power-law-related encoding for the Schwarzschild problem; and
- oscillatory \(R_Y\)-based encoding for the two-dimensional Schrödinger equation.

## Hybrid Quantum-Classical Neural Networks

The hybrid architectures combine:

\[
\text{Classical Neural Network}
\rightarrow
\text{Quantum Circuit}
\rightarrow
\text{Classical Output Layer}.
\]

Classical outputs are encoded into the quantum circuit through trainable or data-dependent rotation gates. Measurements of the quantum circuit are subsequently processed by a classical layer.

The exact architecture varies between physical problems to accommodate their different mathematical structures.

---

# Training and Evaluation

To provide a common comparison, each model type is tested under **three configurations**.

For the classical and hybrid models, the configurations correspond to hidden-layer sizes of:

\[
10,\;30,\;50.
\]

For the quantum models, the configurations correspond to:

\[
1,\;2,\;3
\]

variational quantum layers.

The models are evaluated using four random seeds:

\[
14,\;42,\;86,\;195.
\]

The main figures and detailed configuration comparisons in the paper use **seed 42**, while the statistical analysis across seeds reports the mean and standard deviation of the final errors.

All models are optimized using the **Adam optimizer** with:

\[
\beta_1=0.9,
\qquad
\beta_2=0.99,
\qquad
\epsilon=10^{-8}.
\]

The models and hyperparameters were tuned within the available computational budget while maintaining comparable conditions across the three model paradigms. Because of computational constraints, relatively small training sets were used, while larger test sets were used to evaluate generalization over unseen inputs.

---

# Implementation

The neural networks were implemented using:

- **Python**
- **PyTorch**
- **PennyLane**

PyTorch is used for the classical neural-network and optimization components, while PennyLane provides the differentiable quantum-circuit implementations.

The repository is organized around the three physical problems, with separate source-code notebooks, analysis notebooks, and trained-model directories.

---

# Repository Structure

```text
.
├── Damped Harmonic Oscillator/
│   ├── source and analysis/
│   │   ├── damped_harmonic_oscillator_source_code.ipynb
│   │   ├── damped_harmonic_oscillator_analysis_seed42.ipynb
│   │   └── mean_and_deviation_damped_harmonic_oscillator.ipynb
│   └── trained_models/
│       ├── classical damped harmonic oscillator/
│       ├── quantum damped harmonic oscillator/
│       └── hybrid damped harmonic oscillator/
│
├── Einstein Field Equations for Schwarzschild Metric/
│   ├── source and analysis/
│   │   ├── einstein_field_equations_source_code.ipynb
│   │   ├── einstein_field_equations_analysis_seed42.ipynb
│   │   └── mean_and_deviation_einstein_field_equations.ipynb
│   └── trained_models/
│
├── Time-Independent Schrödinger Equation for Particle
│   in a Two Dimensional Box/
│   ├── source and analysis/
│   │   ├── time-independent_schrödinger_source_code.ipynb
│   │   ├── time-independent_schrödinger_ analysis_seed42.ipynb
│   │   └── mean_and_deviation_time-independent_schrödinger.ipynb
│   └── trained_models/
│
├── CITATION.cff
├── LICENSE
└── README.md
```

The repository currently contains dedicated source and analysis notebooks for each of the three problems. The damped-oscillator and Einstein-equation directories, for example, separately provide source-code, seed-42 analysis, and mean/deviation notebooks.

---

# Notebook Organization

For each problem, the repository separates the computational workflow into three main notebook types.

### Source-code notebook

The source-code notebook contains the model definitions, quantum circuits, training procedure, loss functions, and numerical implementation used for the corresponding problem.

### Seed-42 analysis notebook

The analysis notebook focuses on the detailed evaluation corresponding to the main seed-42 results presented in the paper.

### Mean-and-deviation notebook

The mean-and-deviation notebook evaluates the models across the four random seeds and is used to obtain the statistical comparison of model errors.

For example, the damped harmonic oscillator directory contains:

- `damped_harmonic_oscillator_source_code.ipynb`
- `damped_harmonic_oscillator_analysis_seed42.ipynb`
- `mean_and_deviation_damped_harmonic_oscillator.ipynb`

The corresponding structure is used for the Einstein field equations and Schrödinger-equation experiments.

---

# Reproducing the Experiments

The notebooks are intended to provide the computational implementation associated with the published study.

A typical workflow is:

1. Clone the repository.
2. Install the required Python dependencies.
3. Open the source-code notebook for the problem of interest.
4. Run the notebook to initialize the model and training procedure.
5. Use the corresponding analysis notebook to reproduce the evaluation and visualizations.
6. Use the mean-and-deviation notebook to evaluate robustness across the four random seeds.

Example:

```bash
git clone https://github.com/NavidMKZ/Evaluating-the-performance-of-classical-quantum-and-hybrid-NNs-in-solving-differential-equations.git

cd Evaluating-the-performance-of-classical-quantum-and-hybrid-NNs-in-solving-differential-equations
```

The notebooks can then be opened using Jupyter Notebook or JupyterLab.

```bash
jupyter notebook
```

Because the original experiments involve differentiable quantum circuits and repeated model training, execution time depends on the available classical hardware and the selected configuration.

---

# Reproducibility Considerations

The experiments were deliberately evaluated under multiple random initializations.

The four seeds used throughout the study are:

```text
14
42
86
195
```

Seed 42 is used for the detailed representative plots and configuration comparisons, while the mean and standard deviation across all four seeds provide a broader assessment of robustness.

The results demonstrate that initialization can substantially influence optimization. In particular, the quantum models show greater variability across different problems and initializations than the classical models. Consequently, individual best-case results should not be interpreted as universally representative of all initializations.

---

# Main Findings

The study shows that the relative performance of classical, quantum, and hybrid models depends strongly on the physical problem, architecture, and parameter initialization.

The main observations are:

### Damped harmonic oscillator

The QNN achieved the lowest reported MSE among the three paradigms for the representative seed-42 experiment. The HQNN also substantially improved on the classical model in that experiment.

### Einstein field equations

The HQNN achieved the highest overall prediction accuracy when considering the relevant metric components together. At the same time, the classical network achieved lower MSE values for the individual metric components across the evaluated seeds. This illustrates why accuracy should be assessed using more than a single loss metric.

### Time-independent Schrödinger equation

The quantum and hybrid approaches can represent the ground and low-lying excited states, but optimization becomes more difficult for higher excited states. The reported hybrid models successfully identify the ground and first excited states but fail to predict the second excited state in the tested configurations.

### Overall comparison

Across the studied problems, the paper finds that QNNs and HQNNs can use fewer trainable parameters than the corresponding classical models and often exhibit rapid convergence. However, the quantum approaches also show greater sensitivity to initialization and can exhibit unstable or highly variable optimization behavior.

The results therefore do **not** imply that quantum neural networks universally outperform classical neural networks. Rather, they demonstrate that physics-informed quantum and hybrid architectures can provide competitive or superior performance for particular differential-equation problems and configurations, while also introducing distinct optimization challenges.

---

# Physics-Informed Quantum Feature Maps

One of the central ideas explored in this work is that the quantum feature map should reflect the expected structure of the physical solution.

Instead of treating quantum encoding as a generic preprocessing operation, the approach incorporates prior physical knowledge into the circuit design.

The general idea is:

\[
\text{physical behavior}
\longrightarrow
\text{feature-map design}
\longrightarrow
\text{quantum representation}
\longrightarrow
\text{variational optimization}.
\]

Examples in this repository include:

| Problem | Qualitative behavior | Feature-map strategy |
|---|---|---|
| Damped harmonic oscillator | Exponential decay + oscillation | Exponential and \(R_Y\) encoding |
| Schwarzschild metric | Approximately power-law / \(1/r\) behavior | Physics-motivated nonlinear encoding |
| 2D Schrödinger equation | Oscillatory wave function | \(R_Y\)-based oscillatory encoding |

This problem-specific design is intended to improve expressivity while keeping the circuits relatively small and compatible with simulation on classical hardware.

---

# Limitations

The results should be interpreted in the context of the experimental setup.

The quantum circuits are intentionally small, and the experiments are performed using classical simulation rather than large-scale execution on fault-tolerant quantum hardware.

The training datasets are relatively small because of computational limitations, while larger test sets are used to study generalization.

The results are also sensitive to parameter initialization. In particular, the QNNs can exhibit substantial variability between different random seeds and may encounter difficult optimization regions for some configurations and excited-state problems.

Therefore, the purpose of this work is not to claim a universal computational advantage of quantum neural networks, but to investigate their accuracy, convergence behavior, parameter efficiency, and applicability to representative differential-equation problems.

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
  article-number = {39332},
  year    = {2025},
  doi     = {10.1038/s41598-025-22950-y},
  url     = {https://doi.org/10.1038/s41598-025-22950-y}
}
```

A `CITATION.cff` file is also included in this repository to provide structured citation metadata for the code and associated publication.

---

# Authors

**Navid Markazi**  
Department of Physics, Isfahan University of Technology, Isfahan, Iran

**Behrouz Mirza**  
Department of Physics, Isfahan University of Technology, Isfahan, Iran

For the complete author information and affiliations, please refer to the published article.

---

# License

The code in this repository is distributed under the **MIT License**. See the [`LICENSE`](LICENSE) file for the full license text.

The published article is separately subject to the license specified by the publisher. Please refer to the [Nature article](https://www.nature.com/articles/s41598-025-22950-y) for the applicable publication and reuse terms.

---

# Related Links

- **Research paper:** https://www.nature.com/articles/s41598-025-22950-y
- **DOI:** https://doi.org/10.1038/s41598-025-22950-y
- **GitHub repository:** https://github.com/NavidMKZ/Evaluating-the-performance-of-classical-quantum-and-hybrid-NNs-in-solving-differential-equations

---

## Acknowledgment

This repository accompanies the research presented in the published *Scientific Reports* article and is provided to facilitate inspection, reuse, and further research on classical, quantum, and hybrid neural-network approaches to differential equations.
