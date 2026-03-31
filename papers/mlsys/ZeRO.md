# ZeRO: Memory Optimizations Toward Training Trillion Parameter Models
Paper: https://arxiv.org/abs/1910.02054

# What is the problem, and why is it important?

ZeRO, or Zero Redundancy Optimizer, is a solution to optimize memory in training large deep learning models. 

Existing solutions have to make tradeoffs between computation and communication, as well as work around the fact that GPU memory can be too small to fit massive models.

ZeRO improves training speed while increasing model size, reduces memory redundancies, and retains low communication volume and high computational granularity. Overall, it makes the full memory capacity of a cluster available.

To recap, data parallelism replicates model states across processors, which may not be feasible with large model sizes. Model parallelism paritions the model
across processors to obtain high memory efficiency, but sacrifices communication/computation efficiency due to communication volume across partitions. More specifically, pipeline parallelism suffers from pipeline bubbles, which can be worked around but may require additional memory to do so (e.g. larger batch size). In model parallel techniques, there requires a lot of communication between layers/tensors, depending on how the model is split. This works well when the GPUs are in the same node, but is inefficient otherwise. 

# The main proposed idea(s)

The researchers classified memory usage in DL model training into 2 parts:
1) model states: (optimizer states such momentum and variance, gradients, and parameters)
2) residual states: activation, temp buffers, unusable fragmented memory

Apart from just the weights and biases, there are other model states and residual states that need to be stored. In mixed-precision training, storing these values can be extra costly, since additional memory is required to store a more precise (more bits) "master" copy of the model states. 

ZeRO-DP removes memory state redundancies across data-parallel process by partitioning and not replicating model states. It uses a dynamic communication schedule that takes advantage of the temporal nature of model states (only forward and backward pass, for example). ZeRO-DP reduces per-device memory footprint linearly with increased DP degree while maintaining DP's efficient communication volume. 

Each ZeRO-DP process stores only the model states (parameters/gradients/optimizer states) corresponding to its partition. This is because each process only updates its corresponding parameter partition, so it only needs the corresponding gradients. This is similar to a Reduce-Scatter operation - gradients corresponding to different parameters are reduced to different processes. If parameters outside of the partition are required for a process, they are broadcasted (minimal communication overhead). 

However, a forward pass requires the full model weights to do computation (data parallelism needs the whole model together), but each GPU only has a subset of weights. ZeRO solves this by pipelining the all-gather process: before computing the forward pass of partition i, the GPU responsible for partition i broadcasts its weights to all GPUs. All GPUs compute that part of the forward pass, and then discard the weights from memory. For backprop, parameters are "all-gathered"-ed again. 

Zero-R optimizes memory consumed by residual states by proactively managing memory, offloading activations to CPU when appropriate, and defining and appropriate size for temporary buffers (used in all-reduce for example). ZeRO-R partitions activation checkpoints (saving only a subset of activations) across GPUs and uses all-gather to reconstruct them. 

After the forward propagation for a layer is computed, the input activations are partitioned across all model parallel processes, and are "all-gather"-ed once they are needed again during back-propagation. This works in conjunction with activation checkpointing. 

Combining these two factors, ZeRO turns out to be an attractive alternative to model parallelism, even in the area that MP is good at - fitting a large model across GPUs - because ZeRO is effective at reducing the per-device memory footprint. However, ZeRO can be used in combination with Model Parallelism for reducing activation memory footprint in very large models or improving convergence if the aggregated batch size with DP alone is too large. 

Ultimately, ZeRO is shown to handle large model sizes, improve training speed (memory efficiency -> higher throughput), improve scalability (increased DP degree -> reduced model state memory footprint -> larger batch size per GPU -> better performance). ZeRO powers data parallelism to fit models with arbitrary size as long as there are sufficient number of devices to share the model states. 


# A summary of different components of the proposed technique, e.g., the purpose of critical design choices.

Three main insights are made in ZeRO-DP:
1) Data Parallelism has better scaling efficiency than model parallelism because of communication overhead as well as reduced computation granularity (if it is too small it can reduce the efficiency per GPU).
2) In Data Parallelism, model states are stored redundantly across data-parallel processes, while in model parallelism they are partitioned to be efficient.
3) Both DP and MP keep all model states over the entire training process, although not all are required all the time.

Two main insights are made in ZeRO-R:
1) Model parallelism often requires replication of activation memory (part of residual states), even if model states are partitioned
2) For large models, arithmetic intensity (computation per iteration : activation checkpoints per iteration ratio) is very large, meaning activation partitions can be moved to CPU memory and the data-movement cost for activation checkpoints will be hidden. 

Memory fragmentation key insights:
1) Fragmentation is the result of interleaving between short lived (activations recomputed during backprop, activation gradients + buffers) and long lived (checkpointed activations from forward pass, parameter gradients from backwards pass) memory objects.
This leads to a lack of contiguous memory chunks, as well as poor memory efficiency in general. ZeRO works around this by pre-allocating contiguous memory chunks and copying values over. 


# What are the technical drawbacks/limitations?

Memory-communication tradeoff: The researchers showed that using Optimizer State and Gradient Partitioning incurs no additional communication (same as regular Data Parallelism). However, Parameter partitioning does, albeit with an upper bound on the factor (< 1.5x communication).

Also, at extremely large model sizes, activation checkpointing can be a bottleneck (requires reassembling the entire model). 

Heterogenous requirements:
1) heterogenous GPU communication
2) heterogenous workloads (earlier gradients larger)
# How to improve them?

However, this can be made even more efficient by overlapping computation and communication, or making parameter preloading asynchronous. Other methods to make the communication time faster are quantization and topology-aware scheduling. 
