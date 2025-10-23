# What is the problem, and why is it important?
An LLM service responds to a query in two phases: prefill and decoding. Prefill processes the user's prompt to generate the first token of the response in one
step. The decoding phase sequentially generates subsequent tokens, with each decoding step generating a new token based on previously generated tokens. This 
means latency is measured in Time to First Token (TFTT) and (average) Time per Output Token (TPOT). TFTT is computationally intensive, because it involves 
processing the entire input (calculating attention scores for each input against each input before it) and generating the KV Cache. TPOT is memory-bound, 
because it requires many accesses to the KV Cache but only requires computation of attention scores for the token to be generated. 

Existing systems colocate both phases due to the need of the KV cache in both. They maximize the overall throughput by batching the phases across requests,
and because of these differing phases of computation, over-allocate resources to meet latency requirements. 

# The main proposed idea(s).
DistServe improves performance by disaggregating prefill and decoding computations, to eliminate prefill-decoding interferences. This means that systems are 
allowed to prioritize both TTFT (prefill) along with TPOT (decoding) instead of one or the other. 

This works by assigning the prefill and decoding computations to different GPUs, allowing optimization of resource allocation and parallelism strategy
tailored to each phase to boost scaling. These phases are placed according to the cluster's bandwidth, in order to minimize communication overhead across phases.

# A summary of your understanding of different components of the proposed technique, e.g., the purpose of critical design choices.
The researchers clarify that different applications prioritize TFTT over TPOT or vice versa. They set the goal as being able to balance these priorities and 
maximize per-GPU goodput, which they define to be the maximum request rate that can be served (higher goodput = lower cost per query). 

# What are the technical drawbacks/limitations?
The main drawback is that the model is replicated for each phase, increasing memory footprint. It also depends on high-bandwidth interconnects for communication
between phases. Also, if there are not that many GPUs available in a cluster, DistServe doesn't serve much of a purpose. 

# How will you improve them?
Memory footprint can be improved using quantization techniques. Otherwise, parameters can be shared. Also, if there are less computationally intensive prefill
stages (small prompt), they could be dynamically merged/colocated with a decoding phase (the paper mentioned a chunked-based version of this causes contention and 
interference, but it may work for similarly bounded operations). 

This idea of disaggregating prefill and decoding stages can be applied to Speculative Decoding, because we know that Speculative Decoding can decrease TPOT.
