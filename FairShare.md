# FairShare: An App for Fair Division

## Introduction to the Problem

The Fair Division Problem is a fascinating blend of mathematics, computer science, and social decision-making. It asks: how can we divide resources (goods or chores) among a group of individuals in a way that is fair and optimal? In our app, FairShare, we tackled this challenge. Goods provide positive benefits to an agent, while chores yield negative utility. The goal is to allocate these resources so that everyone feels they’ve received their fair share, minimizing envy and maximizing efficiency.

In everyday life, this problem manifests in many ways: splitting rent among roommates, dividing tasks in a group project, or allocating limited resources in a company. FairShare aims to ensure fairness and mitigate pain points when making decisions about resources or tasks.

This project’s mathematical backbone—concepts like Pareto Optimality, Nash Social Welfare, and Envy Freeness—reveal the complexity of equitable resource allocation. By integrating algorithms with user-friendly technology, FairShare seeks to bridge the gap between theory and practical application.

## Background

At its core, FairShare is about being fair in resource allocation. Here are some important things to know:

Pareto Optimality: An allocation is Pareto optimal if no reallocation can make someone better off without making someone else worse off. This ensures efficiency but doesn’t necessarily guarantee fairness.

Envy in Game Theory: Envy arises when one agent prefers another’s allocation over their own. The goal is to minimize envy, ensuring perceived fairness.

EF1 (Envy Freeness up to One Good): A relaxed fairness criteria, making it so that that envy can be eliminated by removing one good from someone else's bundle or one chore from the agent's own bundle.

Nash Social Welfare: The product of the agents' valuations of their allocated bundles. Maximizing Nash Social Welfare balances fairness (equal distribution of utility) and efficiency (maximizing overall satisfaction).

To compute these allocations, we used techniques like:

Integer Linear Programming (ILP): Optimizing allocations where agents distribute 100 credits among goods based on preference.

Max Weight Bipartite Matching: Constructing a bipartite graph with agents and goods, optimizing allocations when goods are fewer than agents.

Round Robin Algorithm: Ensures EF1 but sacrifices Pareto Optimality.

## Tech Stack and Development Journey

As part of the iOS team, our initial prototype was built entirely in Xcode, running brute-force algorithms to compute optimal allocations. These computations explored all permutations of allocations—a functional but computationally expensive method. In addition, we made it so that users of the app only needed one device, where everyone inputs their names, and each person gets to input their valuations and receive an allocation. 

In our next iteration, to scale and improve efficiency, we decided to use:

Firebase: We integrated Firestore (NoSQL) to support multi-device syncing and privacy. Each session stored user data, group membership, and valuations securely.

Firebase Functions: Moved computation-heavy algorithms to the backend, improving app responsiveness.

SciPy: Utilized SciPy's Linear Programming (LP) solver and Min Weight Bipartite Matching algorithms for maximizing the Nash Welfare Function. 

My favorite addition, which I helped ideate, involved building the Sessions feature. We used firebase to allow users to sync multiple devices. This allowed for more privacy, since only they could see their valuations for each good and users wouldn't be influenced. 


Working alongside a Ph.D. student and other undergrads was an invaluable learning experience. Collaborating across disciplines and platforms taught me not only about algorithms but also about the nuances of teamwork.

I learned a lot about the research process, where we are not operating to make a profit but to build something better. I was able to apply something I learned from an article I had read about R&D: because research takes time, it is important to get iterations out to the consumer, in order to both measure progress as well as focus efforts towards solving a real problem. Because we made risky and novel decisions, such as implementing the Sessions feature, we were able to make a more accessible and secure app. 

## Obstacles

Scalability of Algorithms: Initial brute-force methods struggled as the number of agents and goods increased. Transitioning to ILP and bipartite matching mitigated this but introduced new complexities in implementation.

User Privacy: Ensuring that individual valuations remained private was critical. Firebase enabled secure storage, but implementing seamless multi-device syncing was challenging. I was able to test this system out using Xcode simulators. 

Cross-Platform Consistency: Coordinating development between iOS and Android required careful design to ensure a uniform user experience.

Algorithm Complexity: Concepts like EF1 and Nash Social Welfare are mathematically intricate. Balancing theoretical rigor with practical efficiency required constant iteration and testing.

## Applications and Impact

Fair division algorithms have applications far beyond our app. Potential impact areas include:

Auctions and Marketplaces: Ensuring fair distribution of resources like advertising slots or storage space in shared networks.

Resource Allocation: Allocating computational resources in cloud services or bandwidth in peer-to-peer networks.

Distributed Systems: Algorithms like EF1 and Nash Welfare can enhance fairness in federated learning or distributed AI training.

Conflict Resolution: Fair division models can help mediate disputes over shared resources, from international water rights to local community budgeting.

By embedding fairness into these domains, FairShare showcases the transformative potential of equitable algorithms.

## What Could Be Done Differently

Reflecting on the project, several avenues for improvement come to mind.

### More Advanced Algorithms:

Reinforcement Learning (RL): Training an RL agent to learn optimal allocation strategies could balance fairness and efficiency dynamically.

Neural Networks: Leveraging neural networks to predict user preferences and expedite allocations in large-scale problems.

Generalized Envy Graph Algorithms: A deeper exploration of these algorithms could improve fairness guarantees.

### We are planning on running User Experience Enhancements:
Allowing users to understand allocation outcomes before finalizing inputs.
Adding visualizations of envy graphs or Pareto frontiers for educational purposes.

As I learned in one of my classes, BADM 261, this data will allow us to build a better product.  By collecting user data, we will be able to interpret it for product development, allowing us to help users make better decisions. 


### Scalability Improvements:
Exploring distributed computing frameworks like Apache Spark to handle larger datasets and computations in real-time.

## Conclusion

Building FairShare was a journey through the intersection of mathematics, technology, and human psychology, subjects that I have been interested in. The challenge of ensuring fairness—an inherently subjective concept—through objective algorithms pushed me to explore new ideas and refine old ones. By incorporating advanced techniques and expanding the app's use cases, FairShare has the potential to become a tool that fosters trust and equity in diverse fields. In a world increasingly reliant on collaboration and shared resources, this pursuit of fairness feels not just technical, but deeply human.
