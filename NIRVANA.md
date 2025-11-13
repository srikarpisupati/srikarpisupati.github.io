# NIRVANA: Approximate Caching for Efficiently Serving Text-to-Image Diffusion Models
Paper: https://www.usenix.org/conference/nsdi24/presentation/agarwal-shubham

# What is the problem, and why is it important?
Diffusion models are popular due to their ability to generate high quality images from text. However, they are resource intensive and go through many denoising steps to do so. This is especially true for larger models, which require more capable and expensive GPUs. 

# The main proposed idea(s).
This paper introduces an approximate-caching technique to reduce iterative denoising steps by reusing intermediate noise states from a prior image generation. The team achieves 21% GPU savings, 19.8% latency reduction (end-to-end), and 19% dollar savings. 

# A summary of your understanding of different components of the proposed technique, e.g., the purpose of critical design choices.
The team found that there were not that many existing optimizations that could balance quality (distillation/pruning leads to quality drop) and speed (sampling optimizations still take ~same amount of time). They found that previous works overlooked the use of text-to-image systems across multiple generations. 

In diffusion models, different aspects of the image form across different denoising steps of the generation process. The opportunity here is that these intermediate steps could be used as the seed/start step for a later request (instead of starting from random noise), and it would help generation as long as it is a similar prompt. This could not only help at the start, but at later steps. However, the start has the highest hit rate, as it is the least "specific" image generated. The hit rate decreases as we form more and more features. 



# What are the technical drawbacks/limitations?
# How will you improve them?
