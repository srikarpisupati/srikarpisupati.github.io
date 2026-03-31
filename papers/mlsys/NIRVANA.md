# NIRVANA: Approximate Caching for Efficiently Serving Text-to-Image Diffusion Models
Paper: https://www.usenix.org/conference/nsdi24/presentation/agarwal-shubham

# What is the problem, and why is it important?
Diffusion models are popular due to their ability to generate high quality images from text. However, they are resource intensive and go through many denoising steps to do so. This is especially true for larger models, which require more capable and expensive GPUs. 

# The main proposed idea(s).
This paper introduces an approximate-caching technique to reduce iterative denoising steps by reusing intermediate noise states from a prior image generation. The team achieves 21% GPU savings, 19.8% latency reduction (end-to-end), and 19% dollar savings. 

# A summary of your understanding of different components of the proposed technique, e.g., the purpose of critical design choices.
The team found that there were not that many existing optimizations that could balance quality (distillation/pruning leads to quality drop) and speed (sampling optimizations still take ~same amount of time). They found that previous works overlooked the use of text-to-image systems across multiple generations. 

In diffusion models, different aspects of the image form across different denoising steps of the generation process. The opportunity here is that these intermediate steps could be used as the seed/start step for a later request (instead of starting from random noise), and it would help generation as long as it is a similar prompt. In this way, we could essentially "skip" the first K steps. However, as K increases, the hit rate drops. The start has the highest hit rate, as it is the least "specific" image generated. The hit rate decreases as we form more and more features. 

The team found that more steps can be skipped (higher K) for a prompt if its cache is more similar. They determined the max K value, which signifies that skipping more than that amount would not lead to good results. 

The cache that the system used is a Least Computationally Benefitial Frequently Used (LCBFU). This arises from the key insight that noises generated at smaller K values are less computationally beneficial (more noisy, plus not worth using cache if mostly still recomputing). Each entry is assinged a score, which is the product of K and the frequency used, and this results in an eviction policy that combines frequency with importance. 

If a match is not found in the cache, the system runs the diffusion model from the start. Otherwise, it uses the seed noise entry and then runs the diffusion model from there. The paper also introduces a one-class SVM classifier to predict whether there will be a match in the class, to directly call the diffusion model if there will likely not be a cache hit. This minimizes retrieval overhead. 

In the full pipeline, NIRVANA uses an embedding generator to match the input prompt with the cached prompts' embeddings (using a vector database). They are compared for similarity, and then the system dynamically determines the K value based on that similarity. The system retrieves (elastic file system) the trajectory associated with the maximum similarity prompt, and skips steps along that trajectory up to a value K (if it exists), and uses that latent noise state. 

# What are the technical drawbacks/limitations?

If the maximum similarity is below some threshold (novel prompt), it may not be worth to even use cached latent representations (skip the traversal of the trajectory as well). Also, the cache takes up a lot of storage, because we are storing many latent representations. Finally, previously cached prompts with smaller steps used to generate the final image will not be benefited, because their representations will likely be evicted under the LCBFU policy (their K values are small). 

# How will you improve them?

If we encounter a novel prompt, meaning the maximum similarity is below some threshold, we could skip accessing the cache as it will likely be a miss. Second, it may be worth caching a compressed version of the latent state, to minimize storage overhead. With this additional storage, we could also have a "minimum number of elements per prior query", so that past queries with smaller trajectories can still benefit from the cache. 
