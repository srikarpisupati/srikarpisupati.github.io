# What is the problem, and why is it important?
A critical fact about Transformer models, or any autoregressive model, is that during inference, decoding or generating K tokens will take K serial runs of the
model. This can be very slow. One alternative could be to use a smaller model, making K serial runs slightly faster. However, large models are much more capable,
and it would be very helpful to have a technique that allows inference for large models to be fast.

# The main proposed idea(s).
Speculative Decoding aims to solve this problem by generating or computing tokens in parallel. This is done without changing the model architecture, without 
needing to retrain the model, and without changing the model output distribution. 

The general idea of speculative execution is that a task is performed in parallel to verify if it is actually needed. The main problem to solve here is creating
an efficient mechanism to suggest tasks that are likely to be needed. 

All we need to do to turn speculative execution into speculative decoding, is to turn the task into "generating tokens". :)

The researchers sampled generations from efficient "approximation models" instead of the slower "target models". The sampling method, speculative sampling, 
is used to maximize the probability of speculative tasks to be accepted (by the target model), while also ensuring that the outputs from the overall system have
the same distribution as those from just the target model. 

The target model is only used to generate tokens if it decides to reject and correct the speculated tokens. The main purpose of the target model is to evaluate
the guessed tokens from the approximation model in parallel. It will accept if the guessed token *can* lead to an identical target distribution, and then sample
an additional token to either add on to the sequence, or to fix the first rejected guess.

# Purpose of critical design choices & different components of the proposed technique
The key observation that the researchers make is that certain inference steps are "easier" and others are "harder". They also realized that it may not help to 
just use less compute resources for "easier" inference steps, because inference for large models is actually not compute-bound, but instead often bottlenecked on
memory and communication bandwidth, meaning compute resources will likely be available and need not be the main thing to optimize for. 

# What are the technical drawbacks/limitations?

There are a couple of limitations of Speculative Decoding. First, it is only worth using if there is a high acceptance rate of the approximate model's guessed
tokens. If not, then generation essentially becomes serial with the added latency of generating guesses. 

Second, it introduces another model that will need to fit into GPU memory. 

# How will you improve them?
If system capacity has been reached, it may be more efficient to allocate all compute for the target model for reliable token generation instead of speculating
with an approximate model that may fail. 
Also, in order to improve the approximate model, it may be worth to train it separately instead of using a static model. 
Furthermore, the paper mentioned that we would need to choose the number of tokens to speculate. This could be chosen dynamically - if there is a lower chance of 
acceptance, it may be better to lower the number of parallel speculative guesses, and vice versa. 
Finally, it could be a good idea to not just have 1 approximate model, but choose from a set of >1 models. 
