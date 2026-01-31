# Reinforcement Learning for Autonomous Racing Agents

This repository contains the development of Autonomous Racing Agents based on Reinforcement Learning, designed to learn vehicle control in a 2D simulated environment. The project compares tabular methods against Deep Learning approaches to solve navigation and obstacle avoidance tasks.

## Project Objective
The goal is to design agents capable of learning optimal driving policies in a custom-built environment without prior knowledge of the track layout. The system compares the performance of **Q-Learning** (discrete state space) and **Deep Q-Networks (DQN)** (continuous state space) in terms of convergence speed, lap times, and collision avoidance reliability.

---

## System Architecture
The system follows a closed-loop architecture where the agent perceives the environment state via simulated sensors and executes discrete actions to maximize a reward function.

### 1. Perception and Environment
The simulation environment is built using **Pygame**.
* **Sensory Input**: The vehicle is equipped with a Ray-Casting Ultrasonic system (5 rays) that calculates the distance to track walls in real-time.
* **State Representation**:
    * *Q-Learning*: The continuous sensor data is discretized into buckets to form a finite state space accessible via a Q-Table.
    * *DQN*: The raw continuous normalized distances are fed directly into the neural network.

### 2. Learning Logic (Algorithms)
Two distinct agents were implemented for comparative analysis:

* **Q-Learning (Tabular Approach)**:
    * Uses an epsilon-greedy strategy for exploration.
    * Updates a Q-Table based on the Bellman equation.
    * *Constraint*: Requires heavy state discretization, leading to the "curse of dimensionality."

* **Deep Q-Network (DQN)**:
    * Approximates the Q-function using a **Feed-Forward Neural Network** built with **PyTorch**.
    * **Experience Replay**: Stores transitions in a buffer to break correlation between consecutive samples during training.
    * **Target Network**: Uses a separate network for stable Q-value target calculation.

### 3. Reward Function
The reward structure is designed to encourage speed while severely penalizing collisions:
* **Positive Reward**: Proportional to speed and checkpoints reached without crashing.
* **Negative Reward**: Heavy penalty for colliding with track walls.

---

## Scenarios and Results
The agents were evaluated on custom track layouts categorized by difficulty:
* **Training Track**: The agents are trained on a specific circuit for 1000 epochs to learn how to drive.
* **Test Track**: After the training phase, the agents are validated on a completely new track layout they have never seen before to assess generalization.

**Key Findings:**
* **Q-Learning**: Shows better generalization on simpler circuits compared to the training track, but suffers from slower lap times and struggles with smooth control due to state discretization.
* **DQN**: Generalizes better to harder circuits compared to the training track with significantly faster lap times. However, it shows a higher standard deviation in performance, making it less reliable than the tabular approach.
---

## Project Structure
* `QLearning_vs_DQN_Autonomous_Racing.ipynb`: Main Jupyter Notebook containing the simulation engine, agent classes (Q-Learning & DQN), training loops, and visualization code.
* `q_table_model.pkl`: Serialized Q-Table for the tabular agent (generated after training).
* `dqn_model.pth`: Saved PyTorch weights for the DQN neural network (generated after training).

## Authors
* **David Cazzaniga** – Politecnico di Milano

## Requirements
* Python 3.10+
* PyTorch
* Pygame
* NumPy
* Matplotlib
