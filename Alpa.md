
# Alpa: Automating Inter- and Intra-Operator Parallelism for Distributed Deep Learning Paper Summary

## What is the problem the paper is trying to solve?

This paper introduces a compiler system to automate selecting parallelisms for distributed training of large deep learning models. This is an alternative to manually 
selecting parallelism plans, or automatically generating plans albeit from a limited configuration space. Correctly tuning the parallelization startegy is proven
to improve training performance. 

## The main idea

The main idea is separating parallelisms into heirarchical levels: inter and intra operator parallelism. This is because different parallelisms have different 
bandwidth requirements. 
Intra-operator: partitions tensors along one or more tensor axes, dispatches partitions to distributed devices. This results in more communication between splits.
Inter-operator: slices model into disjoint stages, pipelines execution of stages on different sets of devices. This results in possible under utilization and idle time.

(For example, data parallelism belongs to intra-operator parallelism.)

At a high level, Alpa takes the computation graph and the device cluster, cuts the graph into stages (inter-op pass), and within each stage, shards the stage into 
partitions (intra-op pass) and orchestrates running that sharded stage on a mesh of devices. 

Alpa works by dividing the cluster into device meshes, each with devices that have high-bandwidth connections. It then partitions the computation graph (edges are 
multi-dimensional tensors, nodes are computations/operations) into stages, and assigns stages to device meshes. As we know, one iteration of training a DL model consists
of computing a loss by forwarding a batch of data through the graph, deriving updates through a backwards pass, and updating parameters using weight update operations.

In the past, most auto-parallelization techniques have just involved taking a model parallelism approach, and adding data parallelism (i.e. replicating the model across 
machines with subsets of data). 


## Purpose of design components and techniques

Alpa's inter-op pass uses a Dynamic Programming algorithm to assign stages to meshes and parallel execution latency, and invokes the intra-op compilation pass 
on each stage-mesh pair, to receive the execution cost of that assignment. The intra-op compilation pass optimizes the execution plan of the stage-mesh 
combination by minimizing execution cost using Integer Linear Programming (ILP). 

The space that the intra-op pass optimizes on is the space of possible parallel algorithms for a given operator, as different parallel algorithms result in 
different computation/communication ratios and operator layouts. One key assumption that intra-op parallelism makes is that devices within a mesh have the same compute capability, so that Alpa can partition operators evenly among devices SPMD-style. One key assumption that Alpa makes about intra-operator parallelism is that each mesh of devices contains devices that have the same compute capability. 

The ILP formulation takes into account computation and communication costs. Also, computationally trivial operations are merged into single operations, reducing size of the computation graph and making parsing faster. After this, communication optimizations are selected such as replacing all-reduce with reduce-scatter and all-gather. 

This ILP optimization eventually gets us to the minimum latency of executing a stage (slice of operators from graph) on a given submesh of devices. The inter-op Dynamic Programming formulation aims to minimize the total minimum latency for the entire computation graph, while adding 2 constraints:
1) for an operator in the forward pass, colocate it with corresponding backward pass on same submesh
2) sliced submeshes should fully cover the cluster mesh without wasting compute resources.

This finds the optimal substructure/division of work across submeshes. 


## Technical Limitations

For smaller/easier problems, the overhead of computing and optimizing all possible layouts is not worth quick manual algorithm selection. 

## How to improve limitations

These issues can be solved by expanding the framework to allow dynamic execution plans, such that they can adapt to changing workloads.





