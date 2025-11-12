# Mooncake: Trading More Storage for Less Computation — A KVCache-centric Architecture for Serving LLM Chatbot
Paper: https://www.usenix.org/conference/fast25/presentation/qin

# What is the problem, and why is it important?
In LLM inference, the prefill stage is computationally intensive, while decode is memory bandwidth intensive. 
In online serving, requests arrive dynamically, and longer contexts or larger models lead to higher inference cost and latency. We need to reduce this cost (increasing system throughput, resource utilization) for long context LLM inference.

Existing solutions rely on KV caching to reuse computation (e.g., prefix caching, where the KV cache is shared across requests with same the prefix to reduce computation), and this leads to ~50% reuse for typical online workloads (saves half of GPU computation, replacing with cache accesses).

# The main proposed idea(s).
Mooncake proposes a KVCache-centric disaggregated architecture that trades more storage for less computation. Instead of only local caching, it expands cache capacity across nodes and separates computation (prefill/decode) from cache management. This reduces the cost by more than half, and the resulting model can handle 115% more requests. 

This improvement is the result of a large capacity storage system that uses low latency transfers, as well as KV cache aware scheduling to balance load along with cache locality. 

The system introduces:
Mooncake Store – A fault-tolerant, high-bandwidth KV cache storage and transfer engine, independent of the inference engine (e.g., vLLM).
KV-cache–aware scheduling – Schedules requests to maximize cache reuse (KV cache centric) and minimize interference (disaggregated architecture).
Prefill/Decode Disaggregation – Separate resource pools for prefill (compute-heavy) and decode (memory-heavy) stages, improving GPU utilization and scalability.

This ultimately increases throughput of the system, shown through faster real-time responses. 

# A summary of your understanding of different components of the proposed technique, e.g., the purpose of critical design choices.

The reason for the distributed KV cache system is because the cache hit rate drops if only using local cache of single node (which is limited to around ~3m tokens). We need to expand KV cache capacity, using more storage in order to do less computation. This is because each token, even if it is just a few bytes, requires a lot more bytes to have its representation stored in the KV cache, especially if it is a large model (due to large intermediate dimension), even with grouped query attention optimizations that reduce the number of KV caches needed.

The purpose of disaggregating prefill and decode, illustrated in other papers such as DistServe, are to minimize cross-stage interference. Mooncake introduces incremental prefill, which works like the following: 

The existing KV cache results from prefill instances are loaded into CPU memory, then transfered into decoding instances (GPU) for a new request. The system hits the cache until a mismatch, at which point the uncached portion of input tokens that were not covered by the prefix cache are processed, and the newly generated KV cache (incremental part) is stored back into CPU memory. This process can be chunked and pipelined if the uncached portion exceeds a threshold. 

This requires high i/o transfer speeds (done asynchronously, overlapped with the incremental prefilling), and thus, the distributed KV cache pool is shared across inference nodes with high-bandwidth links to minimize recomputation. The KV cache policy is LRU (least recently used). The scheduling policy also is aware of the distributed KV cache + disaggregated architecture of the system. 

Mooncake store supports recovery and online scaling independently of stage, and also allows decoupling of resources/parallelism scheme. Tihs ultimately improves the model FLOPs utilization. This improvement is extremely valuable in mixed batches, where different requests are in different stages of generation. Each stage can use different resources and parallelism scheme. 

This sytem overall leads to more KV cache reuse, efficient disaggregation, and flexible KV cache aware scheduling. 

# What are the technical drawbacks/limitations?

1. Distributed KV cache transfers demand very fast interconnects. 
2. Finding the optimal number of prefill vs. decode instances is workload-dependent.
3. distributing hot caches optimally across nodes is complex.
4. Expanding cache capacity increases storage resource usage

# How will you improve them?
1. We can incorporate smarter cache admission/eviction policies (frequency-based or ML-guided) to reduce bandwidth load
2. We can also use hierarchical cache layers to balance speed and cost (GPU, CPU, SSD)
3. We can optimize network topology/use topology aware scheduling to reduce cross-node KV cache transfers
4. We can apply adaptive prefill/decode balancing using learning methods or profiling to tune the balance based on workload. 

