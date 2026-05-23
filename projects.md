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

Implemented the following in Go from scratch: 
* Raft leader election and log consensus algorithm, and
* Distributed transaction system for sharded objects (satisfying ACID properties) that can handle up to 5 simultaneous clients, using timestamped ordering and 2PC.
* Distributed event ordering for replicated objects with total-ordered multicast
* Distributed event logging with centralized logger


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
  <h4>Briefly</h4>
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

<div class="project-header">
  <h4>Skillini</h4>
  <span>Jan 2025 - May 2025</span>
</div>

* Worked in a team of 4 to create a skill–job matching web app that matches students to courses and jobs based on skills they would like to grow.
* Designed DB schema to store jobs, user data, and courses, formulated SQL Queries to retrieve companies with the most jobs, the most in-demand skills.

<div class="project-header">
  <h4>Course Projects</h4>
  <span>Aug 2024 – May 2026</span>
</div>

* **Programming Languages**: Implemented various projects in Haskell, including writing an Interpreter, Algebraic Data Types, and implementing Forth.
* **System Programming**: Built core system utilities in C: custom malloc, concurrent client–server system, parallel make, and a basic shell
* **Data Structures**: Implemented k-d tree to map image sections to nearest tile by color, to build a photo mosaic
* **Data Structures**: Developed a social network to search related people, shortest path, and connected components (BFS)
* **Computer Architecture**: Designed and verified single-cycle and pipelined MIPS datapaths with FSM-based control and testbenches in Verilog

<div class="project-header">
  <h4>
    <a href="https://srikarpisupati.github.io/Finwise.pdf">
      FinWise — A Portfolio Manager
    </a>
  </h4>
  <span>Sep 2024 – Dec 2024</span>
</div>

* Developed an LLM-based web app to help beginner investors understand financial terms, receive personalized buy/sell recommendations, and gain insights into AI investment decisions.
* Focused on delivering portfolio analysis by identifying positive developments, potential concerns, and final recommendations.
* Utilized the FinGPT Dow30 model (based on LLaMA) with Yahoo Finance data, fine-tuned prompts to generate better results, and built REST API endpoints in Flask.
* Collaborated in a team of 4.

<div class="project-header">
  <h4>
    <a href="https://srikarpisupati.github.io/FairShare.html">
      Fair Share — Undergraduate Research
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

<div class="project-header">
  <h4>Rate My Course</h4>
  <span>Oct 2023 - Nov 2023</span>
</div>

* Implemented an Android app that can search and display 300+ sorted courses.
* Created HTTP-based Server to GET individual course details, GET & POST ratings, and bulk-access courses (JSON).

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
