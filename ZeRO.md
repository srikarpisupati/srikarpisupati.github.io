# What is the problem, and why is it important?

ZeRO, or Zero Redundancy Optimizer, is a solution to optimize memory in training large deep learning models. 

Existing solutions have to make tradeoffs between computation and communication, as well as work around the fact that GPU memory can be too small to fit massive 
models.
To recap, data parallelism replicates model states across processors, which may not be feasible with large model sizes. Model parallelism paritions the model
across processors to obtain high memory efficiency, but sacrifices communication/computation efficiency due to communication volume across partitions. 

ZeRO improves training speed while increasing model size, reduces memory redundancies, and retains low communication volume and high computational granularity. 

# The main proposed idea(s)

In model parallel techniques, there requires a lot of communication between layers/tensors, depending on how the model is split. This works well when the GPUs 
are in the same node, but is inefficient otherwise. 

The researchers classified memory usage in DL model training into 2 parts:
1) model states: (optimizer states such momentum and variance, gradients, and parameters)
2) residual states: activation, temp buffers, unusable fragmented memory

However, not all model states are required all the time during training. ZeRO-DP removes memory state redundancies across data-parallel 
# A summary of different components of the proposed technique, e.g., the purpose of critical design choices.

# What are the technical drawbacks/limitations?

# How to improve them?
