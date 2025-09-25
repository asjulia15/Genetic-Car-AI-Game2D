# Autonomous Control System for Virtual Vehicle with Genetic Algorithm

Welcome to the repository! This project, developed in Unity with C#, simulates and trains autonomous vehicles to navigate a 2D track efficiently using artificial neural networks (ANNs) optimized by a Genetic Algorithm (GA). Designed for academic exploration, it serves as a robust platform for testing autonomous control methods for virtual vehicles.

This README provides a comprehensive, developer-oriented guide to understanding, setting up, and extending the system. It covers the core principles, technical implementation details, and practical usage instructions.

---

### **Acknowledgments**

This project was made possible by the dedicated contributions of **Lucas de Biace Torres** and **Gabriel Guimarães Moia**. We are deeply grateful for their hard work and commitment.

---

### **Table of Contents**

* [Project Overview](#project-overview)
* [Core Concepts & Technical Implementation](#core-concepts--technical-implementation)
    * [Genetic Algorithm (GA) Explained](#genetic-algorithm-ga-explained)
    * [Neural Network and Perception](#neural-network-and-perception)
    * [Unity 2D Mechanics](#unity-2d-mechanics)
* [Setup & Execution Guide](#setup--execution-guide)
    * [Requirements](#requirements)
    * [Configuration](#configuration)
    * [Running the Simulation](#running-the-simulation)
* [Alternative Control: The PID Controller](#alternative-control-the-pid-controller)
    * [Manual Control](#manual-control)
* [Extensibility and Contribution](#extensibility-and-contribution)
* [License](#license)

---

### **Project Overview**

The primary objective is to train a virtual car to complete a 2D track as fast as possible without crashing. The vehicle's control system is entirely **autonomous**, driven by a combination of a **Neural Network** and a **Genetic Algorithm**.

The car's "perception" is based on **11 raycast sensors** that measure distances to obstacles. A neural network processes these inputs and outputs steering and acceleration commands. The Genetic Algorithm then acts as a sophisticated optimizer, evolving the neural network's internal weights across successive generations to improve performance.

The project also includes a **PID (Proportional-Integral-Derivative) controller** as an alternative control mechanism for manual testing or fine-tuning. An early elimination mechanism is implemented to speed up training by culling underperforming individuals.

---

### **Core Concepts & Technical Implementation**

#### **Genetic Algorithm (GA) Explained**

The script `Paralelgerenciadorevolucao.cs` is the central orchestrator of the entire evolutionary process.

1.  **Initialization (`CriarNovaPopulacao()`)**: The process begins by creating a new population of cars. The `testedozero` variable determines if the neural network weights are randomly generated (`true`) or loaded from a pre-trained file (`false`) using `CarregarPesosDeArquivo()`.

2.  **Fitness Evaluation & Elimination (`FixedUpdate()`)**: During a simulation, each car's performance is measured by its **`pontuacao`** (fitness score). To save resources, a performance check is done periodically, and any car with a low score (below `pontuacaoMinima` or less than 25% of the best score) is removed.

3.  **Selection & Elitism (`EvoluirPopulacao()`)**: After the simulation, the cars are sorted by their final score. The top 20% of individuals are selected as the **"elite,"** and their neural network weights are directly copied to the new generation.

4.  **Crossover & Mutation**: The rest of the new population is created through genetic recombination. **Crossover** (`CruzarPesos()`) combines weights from two elite parents. A **mutation** (`MutarPesos()`) is then applied, introducing small random changes to the weights to promote innovation.

#### **Neural Network and Perception**

The neural network is the car's "brain." It's a feedforward network with a configurable architecture, defaulting to `{12, 3, 4, 3}`. The 12 inputs come from the **11 raycast sensors** and the car's current speed. The outputs control the car's horizontal and vertical movement.

#### **Unity 2D Mechanics**

The project uses Unity's built-in 2D physics. The car's perception is handled by raycasts. The `ParalelControlaIA` script processes the neural network's output and translates it into physical forces or torques applied to the car's `carController` component.

---

### **Setup & Execution Guide**

#### **Requirements**

* **Unity**: Version 6000.0.38f1 (or compatible).
* **Hardware**: A graphics card is required.

#### **Configuration**

1.  Open the Unity project and load the scene named **"scene done"**.
2.  In the `Hierarchy`, select the **`GENETIC Algorithm MANAGER`** GameObject.
3.  In the `Inspector`, you can configure the GA parameters like **`testedozero`**, **`populacaoTamanho`**, and **`taxaMutacao`**. Ensure you review the hardcoded `caminhoArquivoInicial` path.

#### **Running the Simulation**

* Click **Play** in the Unity Editor.
* Cars will spawn and begin their evolutionary journey.
* Monitor the `Console` for real-time logs.

---

### **Alternative Control: The PID Controller** 🛠️

A **PID controller** is included for precise control of the vehicle.

#### **How to Access and Configure**

1.  In the `Hierarchy` of the "scene done" scene, find the GameObject **"Car PID"**.
2.  To use manual controls, disable the AI by unchecking the enabled checkbox for the **`ParalelControlaIA`** component.
3.  On the same object, locate the **`PIDcomVelocidade`** component to adjust the `Steering Gains` (`Kp`, `Ki`, `Kd`) and `Speed Gains` (`Kp Speed`, `Ki Speed`, `Kd Speed`).

#### **Manual Control**

With the AI controller disabled, you can drive the car manually.

* **WASD or Arrow Keys**: Control steering and acceleration/braking.
* **Brake (S / Down Arrow)**: Note that this does not engage reverse.
* **E / Q**: Shift gears up and down.

---

### **Extensibility and Contribution**

This project provides a solid foundation for further research. Contributions are welcome!

1.  Fork the repository.
2.  Create a branch (`git checkout -b feature/your-feature`).
3.  Commit your changes (`git commit -m "Add your feature"`).
4.  Push to the branch (`git push origin feature/your-feature`).
5.  Open a Pull Request.
