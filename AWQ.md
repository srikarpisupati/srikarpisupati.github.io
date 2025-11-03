# AWQ: Activation-aware Weight Quantization for LLM Compression and Acceleration
Paper: https://arxiv.org/abs/2306.00978

# What is the problem, and why is it important?
Running large language models locally has many benefits - it is better for privacy, eliminates cloud communication delays for real-time applications, and can reduce cloud computing costs. However, not everyone has the hardware resources needed to handle the large model sizes. 

This had been solved with quantization (mapping a floating-point number to a lower-bit number). However, this can lead to a drop in accuracy (quantization error), and the other alternative, quantization-aware training requires more time to train. 

# The main proposed idea(s).
Activation-aware Weight Quantization is an approach for low-bit weight-only quantization. It reduces quantization error by taking advantage of the fact that not all weights are equally important, and skips quantization of important weights. The key idea is that important weights are chosen based on the activation distribution, since weight channels corresponding to larger activation magnitudes process more important features. 

AWQ ends up reducing the weight memory by 4 times, which is helpful since the generation stage is memory bound. Kernel fusion is also used to avoid memory transfers. 

# A summary of your understanding of different components of the proposed technique, e.g., the purpose of critical design choices.
These important/salient weight channels are scaled up (scaling is done per-channel) by searching for the optimal scaling that minimizes quantization error under full-weight quantization (all weights are quantized). 

# What are the technical drawbacks/limitations?
The efficiency gains from the theoretical AWQ algorithm are not guaranteed, because some speedups depend on hardware-specific (kernel) implementation. 
# How will you improve them?
This efficiency realization problem could be solved by developing hardware-specific standardized kernels that allow harnessing low-bit integer representations. Furthermore, a next step for this paper could be expanding beyond just weight activation, to KV cache and activation quantization. This could also be extended to dynamic quantization (per-token). 
