# SGLang: Efficient Execution of Structured Language Model Programs
Paper: https://arxiv.org/abs/2312.07104

# What is the problem, and why is it important?
LLMs have started to be used as Agents, and this involves capabilities such as multi-round planning, reasoning, and interacting with environments (tool usage). This involves multiple dependent generation steps, and this is handled through an "LM program", which control and schedule these generation steps. They key properties of LM programs are that they intersperse LLM calls with control flow, and they receive and produce structured inputs and outputs. 

However, there is no efficient system to execute these LM programs -- existing systems are too far general and removed from the specific workload to be computation and memory-efficient (no reuse of KV Cache across generation calls, batching across requests with similar structure not fully developed). 

This is an important problem because having efficient ways to integrate LLMs into real world environments is critical. 

# The main proposed idea(s).
Structured Generation Language offers a way to efficiently execute LM programs. It is made up of a front-end and a back-end. 

SGLang introduces RadixAttention, which allows reusing KV Cache across multiple generation calls. It also uses a compressed FSM for decoding for structured outputs, like JSON (compile into/represent constraints, allowing decoding of multiple tokens instead of one-at-a-time). Finally, SGLang supports API-only models, and speculative execution for multi-call API model programs. It supports flexible parallelism schemes, disaggregated prefill/decode, continuous batching, load balancing, etc. 

Overall, SGLang achieves 6.4x higher throughput. 

# A summary of your understanding of different components of the proposed technique, e.g., the purpose of critical design choices.
The frontend provides primitives to allow adding content to the prompt, generation calls, and parallelizing the workflow. This allows users to easily see the full reasoning flow in a multi agent system, for example. 

In the backend runtime, the KV Cache is reused for requests that share a prefix (system prompt in chained generation calls, etc). This is stored in a radix tree, which is specific type of prefix tree, allowing new requests to reuse the KV cache of the longest matching prefix in the tree. This tree has an LRU policy (also ensures to not evict entries used by running requests). This improves throughput. This routing in the prefix tree is done by taking into account which worker has the highest chance of a prefix cache hit. 

Also in the runtime, parallel branches (many prompts) can be scheduled, using "fork". These parallel generation calls enable speculative execution, with the added bonus of a shared KV cache. They are scheduled with cache-awareness, avoiding thrashing by scheduling related requests together (shared prefix). 

In addition, the "rules" regarding what can be decoded are compressed into a finite state automata. This can be thought of as converting a regular expression into a deterministic FSM - it ends up reducing decoding overhead. SGLang improves this by compressing adjacent single-transition edges into one edge. This means that if there is a series of generation calls where there is only one possible token at each step, all of those tokens are generated in one forward pass together instead of multiple passes, saving time. Further, each of these parallel generation processes can use the FSM to decode in parallel. 

Finally, SGLang accelerates the decoding process even if an API endpoint is called, by using speculative execution for multi-call patterns (especially if they share the same input context). This is done by letting the first call generate extra tokens, and reuse those extra tokens for later calls. 

SGLang programs can be executed asynchronously, or compiled as computational graphs. 

# What are the technical drawbacks/limitations?
The prefix tree + KV cache can be a memory bottleneck if it ends up not being used (there are not many requests that have the same system prompt). Also, the FSM approach does not work for user defined structured outputs. Finally, there is the possibility of starvation in cache-aware scheduling. 
# How will you improve them?
1. The exact prefix match can be made into a fuzzy prefix match (semantic similarity), and the system could still reuse kernels or attention states.
2. Also, heirarchical caching could be used to avoid memory blowup.
3. Further, an adaptive caching policy could be used -- sometimes, it may just be more efficient to re-compute instead of using the cache if the match is not precise.
4. Also, it may not be worth to cache a short prefix.
5. And, there could be techniques to schedule in a way that maximizes the prefix matches.
6. Finally, it may be worth it to use the same kernel to avoid loading KV cache, but if we must migrate the KV cache tree across machines, we could do "LATE" scheduling (wait for a busy worker to become available).
7. To make the system even more efficient, this "level of cache match" metric could be used in the load balancer to route based on how much compute is needed. This, along with static optimizations like scheduling and allocating, can help solve the starvation problem. 
