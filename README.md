# Autonomous Control System for Virtual Vehicle with Genetic Algorithm

Welcome to the repository! This project, developed in Unity with C#, simulates and trains autonomous vehicles to navigate a 2D track efficiently using artificial neural networks (ANNs) optimized by a Genetic Algorithm (GA). Designed for academic exploration, it tests autonomous control methods for virtual vehicles.

---
### **Acknowledgments**

This project was made possible by the dedicated contributions of **Lucas de Biace Torres** and **Gabriel Guimarães Moia**. We are deeply grateful for their hard work and commitment.


### **Project Overview**

The goal is to train a virtual car to complete a track quickly without collisions, using 11 raycast sensors to "see" obstacles and a neural network to decide how to steer and accelerate. A PID controller is included for manual testing or alternative control, and an early elimination mechanism speeds up training by discarding underperforming cars.

* **Environment**: A 2D track in Unity, with cars spawning at a fixed point.
* **Sensors**: 11 raycasts in different directions to detect obstacles.
* **Neural Network**: Configurable ANN (default: 12 inputs, 3-4-3 hidden layers, 2 outputs for horizontal/vertical control).
* **Genetic Algorithm**: Optimizes neural network weights with elitism, crossover, and mutation.
* **PID Controller**: Manages steering and speed for manual or alternative control.
* **Optimization**: Early elimination of low-scoring cars and file-based weight saving/loading.

The core logic is in the `Paralelgerenciadorevolucao` script (attached to the "GENETIC Algorithm MANAGER" GameObject), which handles population creation, simulation, evaluation, and evolution.

---

### **User Guide**

#### **1. System Requirements**

* **Unity**: Version 6000.0.38f1 (or compatible).
* **Hardware**: A graphics card for Unity rendering.
* **File Paths**: Hardcoded paths (e.g., "E:\jogo...") for weight files. Consider switching to relative paths (e.g., `Application.dataPath`) to avoid errors.

#### **2. Project Setup**

* Open the Unity project and load the scene named "scene done".
* In the `Hierarchy`, select the **"GENETIC Algorithm MANAGER"** GameObject.
* In the `Inspector`, configure the parameters.

#### **3. Adjusting Genetic Algorithm Parameters**

* **Initialization**:
    * `testedozero`: Set to `true` for a fresh start with random weights, or `false` to load pre-trained weights.
    * `deltaPontuacaoMinima`: Minimum score difference for diversity when loading weights (default: 500).
    * `caminhoArquivoInicial`: Path to the initial weights file.
* **Population Configuration**:
    * `populacaoTamanho`: Number of cars per generation (default: 20).
    * `tempoMaximo`: Maximum simulation time per generation (default: 30 seconds).
* **Neural Network**:
    * `estruturaRede`: An array defining the ANN layers (e.g., `{12, 3, 4, 3}`).
    * `taxaMutacao`: Mutation probability per weight (default: 0.05).
    * `intensidadeMutacao`: Maximum mutation change (default: 0.5).
* **Exclusion Criteria**:
    * `pontuacaoMinima`: Minimum score to avoid early elimination (default: 20).
    * `divisorTempoPontuacao`: Divides `tempoMaximo` for periodic performance checks (default: 3).

#### **4. Running the Simulation**

* Click **Play** in the Unity Editor.
* Cars will spawn and evolve automatically.
* Monitor the `Console` for logs on eliminations, deaths, and generation saves.

---

### **How It Works**

#### **Genetic Algorithm**

The GA mimics natural evolution to optimize the neural network weights.

* **Initialization (`Start()`)**: Creates the initial population. If `testedozero` is `false`, it loads weights from a file, ensuring diversity.
* **Simulation Loop (`FixedUpdate()`)**:
    * Tracks remaining time.
    * **Early Elimination**: Every `tempoMaximo / divisorTempoPontuacao` seconds, it removes cars with low scores (below `pontuacaoMinima` or less than 25% of the best score).
    * **Generation End**: When time expires or all cars are dead, it triggers evolution.
* **Evolution (`EvoluirPopulacao()`)**:
    * Sorts cars by final score.
    * **Elitism**: Preserves the top 20% of performers.
    * **Crossover and Mutation**: Selects two random elite parents, mixes their weights (`CruzarPesos()`), and mutates the child's weights (`MutarPesos()`). This process repeats until the new population is full.

#### **Neural Network Integration**

Each car's `ParalelControlaIA` script:
* Feeds sensor data and speed into the ANN.
* Applies the output to control the car's movement via `carController`.
* The GA evolves only the network's weights, not its structure.

---

### **Alternative Control: The PID Controller** 🛠️

The `PIDcomVelocidade` script provides an alternative control method for steering and speed.

* **How to Access**: In the "scene done" `Hierarchy`, find the **"Car PID"** GameObject.
* **Disabling the AI**: In the `Inspector`, uncheck the `ParalelControlaIA` component to disable the neural network.
* **Adjusting the PID**: On the `PIDcomVelocidade` component, you can adjust the gains for steering (`Kp`, `Ki`, `Kd`) and speed (`Kp Speed`, `Ki Speed`, `Kd Speed`).

---

### **How to Manually Control the Car** 🕹️

With the AI disabled, you can take full control.

* **WASD or Arrow Keys**: Use to accelerate, brake, and steer.
* **Brake (S or Down Arrow)**: Note that this does not engage reverse.
* **Shift Gears**: Use **"E"** to shift up and **"Q"** to shift down.

---

### **Contributing**

Contributions are welcome!
1.  Fork the repository.
2.  Create a branch (`git checkout -b feature/your-feature`).
3.  Commit your changes (`git commit -m "Add your feature"`).
4.  Push to the branch (`git push origin feature/your-feature`).
5.  Open a Pull Request.
