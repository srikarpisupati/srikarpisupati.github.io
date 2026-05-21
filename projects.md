---
layout: default
title: Projects
permalink: /projects/
---


## Projects & Hackathons

Can be found at my GitHub: github.com/srikarpisupati

<div class="project-header">
  <h4>Distributed Systems</h4>
  <span>Jan 2026 – May 2026</span>
</div>

Implemented in Go from scratch: 
* distributed event logging with centralized logger
* distributed event ordering for replicated objects with total-ordered multicast
* Raft leader election and log consensus algorithm, and
* distributed transaction system for sharded objects (satisfying ACID properties) that can handle up to 5 simultaneous clients, using timestamped ordering and 2PC. 

<div class="project-header">
  <h4>GPT-2 Inference Kernels</h4>
  <span>Aug 2025 – Dec 2025</span>
</div>


* Built forward pass of GPT-2 with CPU orchestration & GPU kernel design for multi-head attention and LayerNorm. 
* Implemented and profiled performance of Flash Attention, Local Attention, Kernel Fusion, and used Tensor Cores.
* Optimized CUDA kernels (vector addition, matrix multiplication, 3D convolution) using shared memory & tiling

<div class="project-header">
  <h4>Distributed ML Training</h4>
  <span>Aug 2025 – Dec 2025</span>
</div>

* Coded parameter server, ring all-reduce using PyTorch P2P primitives to fine-tune Qwen3-0.6B on a CPU cluster
* Developed adaptive ML compiler selection framework to benchmark and route workloads based on model characteristics and hardware profiles

<div class="project-header">
  <h4>Briefly — CS 410 Text Information Systems</h4>
  <span>Aug 2025 – Dec 2025</span>
</div>

* Built legal hybrid retrieval system that allows users to enter a legal topic in natural language and receive the top-n most relevant precedents. 

<div class="project-header">
  <h4>Machine Learning</h4>
  <span>Jan 2025 - May 2025</span>
</div>

* Gradient Descent: Analytically implemented and compared Adam, SGD, and AdaGrad optimizers
* ML Projects: Trained and compared Random Forest, Decision Tree, kNN, and SVM models on UCIMLRepo data
* Implemented VectorDB search; Trained with PyTorch: k-NN, k-means, regression, CNN, and ResNet models
* Built regression model for predicting Revenue-Passenger-Miles from 5+ factors, for Kaggle Air Traffic dataset

#### Course Projects                                                       Jan 2026 - May 2026
* Programming Languages: Implemented various projects in Haskell, including writing an Interpreter, Algebraic Data Types, and implementing Forth.
* System Programming: Built core system utilities in C: custom malloc, concurrent client–server system, parallel make, and a basic shell
* Database Systems: Created a skill–job matching web app: designed DB schema & SQL queries (top companies, in-demand skills).
* Data Structures: Implemented k-d tree to map image sections to nearest tile by color, to build a photo mosaic
* Data Structures: Developed a social network to search related people, shortest path, and connected components (BFS)
* Computer Architecture: Designed and verified single-cycle and pipelined MIPS datapaths with FSM-based control and testbenches in Verilog

<div class="project-header">
  <h4>
    <a href="https://srikarpisupati.github.io/Finwise.pdf">
      FinWise — A Portfolio Manager, Software Engineer
    </a>
  </h4>
  <span>Sep 2024 – Dec 2024</span>
</div>

* Developed Flask-based REST APIs to deliver investment portfolio analysis using FinGPT Dow30 model (PEFT) 
* Fetched Yahoo Finance data (stock data, news) and created prompts to generate stock analysis & predictions

<div class="project-header">
  <h4>
    <a href="https://srikarpisupati.github.io/FairShare.html">
      Fair Share — Undergraduate Research, Software Engineer
    </a>
  </h4>
  <span>Jan 2024 - Dec 2024</span>
</div>

* Researched fair-division algorithms to optimally allocate indivisible resources, given individual preferences
* Developed a user-friendly iOS app (Swift, Cloud Functions) to compute allocations, maximizing Nash Welfare
* Implemented SciPy’s _LP_ and _Min Weight Bipartite Matching_ algorithms, tested performance with synthetic data
* Used Firestore (NoSQL) to store users, devices & groups. Supported group membership using invitation codes.

<div class="project-header">
  <h4>
    <a href="https://devpost.com/software/crowdcompute-nyz3hg">
      CrowdCompute — HackIllinois Hackathon
    </a>
  </h4>
  <span>Jan 2024 - Dec 2024</span>
</div>

* Built a web application using React, facilitating the training of ML models on remote (Docker) machines
* Responsible for backend data storage using Firebase, managing host machine metadata, model files & weights

<div class="project-header">
  <h4>Illine — BuildIllinois Hackathon </h4>
  <span>Nov 2023</span>
</div>

* Developed an iOS app to support device proximity detection, distance-based ranking, and notifications.
* Used CoreBluetooth and P2P pairing to automatically detect nearby devices, and assigned “queue” positions.
* Won 4th place out of 36 teams and 200+ contestants

<div class="project-header">
  <h4>American Sign Language Classifier</h4>
  <span>Oct 2023 - Nov 2023</span>
</div>

* Trained and tuned a CNN based ASL classification model using the Sign Language MNIST dataset
* Created a Python Gradio web app to capture camera image and classify it using the pre-trained ASL model

## Skills
#### Languages: 
C/C++, Python, Java, CUDA, Go, Swift, SQL, R, Verilog, Haskell
#### Cloud: 
AWS Lambda, Bedrock, Athena, S3, DynamoDB, CDK, GCP BigQuery, Firebase
#### Full-stack Platforms: 
iOS, Android, React, Flask
#### ML Tools: 
NumPy, PyTorch, Pandas, Matplotlib, SciPy (stats, linalg, optimize, sparse.csgraph), Scikit-Learn, HuggingFace, Seaborn, TensorFlow, OpenCV, MediaPipe
#### Misc. Development Tools: 
Git, LaTex, Valgrind
