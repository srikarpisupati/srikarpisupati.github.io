# Mooncake: Trading More Storage for Less Computation — A KVCache-centric Architecture for Serving LLM Chatbot
Paper: https://www.usenix.org/conference/fast25/presentation/qin

# Each summary (4-5 paragraphs) must follow the template

# What is the problem, and why is it important?
prefill is computationally intensive, decode is bandwidth intensive. 
online serving, scheduling as requests come. more data + larger model + longer context = better, but needs gpus + inference cost + higher latency.
need to reduce cost (system throuhgput, resource utilization) for long context llm inference

prefix caching - Kv cache shared across requests with same prefix to reduce computation
for online workflows: ~50% can be reused in kv cache - save half of gpu computation.

# The main proposed idea(s).
Reduced cost by more than half 
kimi 115% more requests.
but cache hit rate drops if only using local cache of single node (~3m tokens). expand kv cache capacity for more storage for less computation
each token (even if few bytes) needs many times more to be stored in kv cache especially if large model (kv size huge) 
  even with gqa with reduced kv cache
io transfer speeds needed are high 

needs large capacity, low latency transfer, kv cache aware scheduling to balance load and cache locality. 

kv cache centric disaggregated architecture (in scheduling)
mooncake store: fault tolerant kv cache storage and transfer engien (independent of inference engine (e.g.: vllm))
prefix cahce offloading
prefill decode disaggregation infernce cluster 
improve throuhput, realtime

# A summary of your understanding of different components of the proposed technique, e.g., the purpose of critical design choices.
scheduling:
prefill, decode prioritize. chunked prefill
reduce interference 

decouple resources, parallelism to improve model flops utilization

incremental prefill -- transfered into another instance for decoding. avoid interference in prefo, decod in mixed batch. each stage can use different resources, paralleism based on computation

can use prefill & decode pool
distributed kv cache pool acros sinference nodes
high badnwidth transfer between inference engine and storage sys

fault tolerant, supporting elastic scaling of online inference system

system matches bolcks until mismatch, identifies reusable reusable and new kv cache blocks. prefill generates kv cache blocks, transfer into decoding instance. LRU policy. 

transfer engine used by managed store used by inference engine

fast scalable fault tolerant kv cache store and transfer engine

more kvcache reuse, efficient disaggregation, flexible scheduling of kvcache

# What are the technical drawbacks/limitations?

higher bandwidth demand (in distributed kv cache? in pooling?)
impact of finding the right p/d ratio
distributing hot caches (optimally?)

> 1 prefill, > 1 decode instance
> to support flexible inference with vllm
# How will you improve them?
