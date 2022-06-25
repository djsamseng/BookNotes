# Biological and MAchine Intelligence (BAMI)
- Numenta

## Introduction
- the same learning algorithm we use for vision we can use for audio, touch, language. See it as a sensory motor problem not a vision problem
- Hierarchical Temporal Memory (HTM) - theory on how the neocortex functions
- Thousand Brains Theory of Intelligence
  - The brain doesn't learn one model, instead it builds many models using different inputs from different sensors and the models vote to reach a consensus
- neocortex exhibits rhymthic behavior in firing of ensembles of neurons but not needed for HTM because software/hardware already solved that problem of synchronizing
  - the brain saves energy by being sparse. Computers are already sparse because we save unused neurons on disk and just have activated neurons in RAM

## Chapter 2 - HTM Overview
- pyramidal neuron
  - Tree like extensions (dendrites) that connect via thousands of synapses
  - dendrites are active processing units, communication is a dynamic stochastic process
    - **active processing = learner acts on instructional inputs to generate, re-organize, self-explain vs passive processing = recalling material in exactly the same form that it was in originally**
    - vs neural nets which have no dentrites and few highly precise synapses (passive)
  - HTM/brain neuron = (feedback, context, feedforward) -> neuron -> synapses
    - **learning = formation/removal of synapses**
    - HTM uses binary synapses and learns by modeling growth/decay of synapses
  - neural net neurons = (feedforward) -> neuron -> synapses
    - learning = changing the feedforward weights
- sparse distributed representations (SDR) - representation format used by the neocortex
- neocortex learns sequences and makes predictions
- spinal cord receives inputs and generated behaviors. New systems added to the brain taking other brain output as inputs. Then the neocortex formed and is now 75% of the human brain. It looks like a sheet that is folded throughout the huamn brain
- neocortex cells/patterns of connectivity are nearly identical throughout -> same algorithms, different inputs
- The brain is a hierarchy of brain regions, each region interacts with a stack of evolutionalry older regions below
  - Train on phenomes, then on words, then two word sentences, then longer, etc.
- 86 billion neurons in the neocortex. No matter what you look at the activity is always sparse (0.1%-4% activated). activated=rapidly spiking
- HTM uses Sparse Distributed Representations (SDR) as representations
  - vectors with thousands of bits with a small percent 1's. Always needs to be a minimum number of 1's and the percentage of 1's must be low (<2%)
  - SDRs also encode the context. The representation and it's meaning are both in the SDR. Solves the memory reuse problem of computers (context required to know what the memory bits mean)
- Reptile brain
  - senses the environment. gives the ability to act
- neocortex learns how to interact with and control the reptile brain for improved behaviors
- 2 inputs to the neocortex
  - 1) sensory data first goes to the reptile brain and eventually gets to the neocortex for higher level control
  - 2) copy of the motor commands executed by the reptile brain
- outputs from the neocortex
  - generates behavior by sending signals to the reptile brain (doesn't control muscles directly)
  - This is just like how computer software uses the kernel/cuda libraries
- HTM learns to encode inputs into SDRs
- HTM needs constantly changing/streaming inputs
  - Temporal memory = memory of sequences / transitions in a data stream
- **HTM Learning Theory: Excitatory neuron in the neocortex is learning transitions of patterns**
  - majority of synapses on every neuron is dedicated to learning these transistions
  - HTM starts on the assumption that everything the neocortex does is based on memory and recall of sequences of patterns
    - But what if we made it an _active neuron_. It's not memorizing/recalling the inputs, but instead using _generation_ to reorganize and _explain_
  - HTM builds a predictive model of the world. At every point in time HTM is predicting what it expects will happen next
    - But instead we should build a model that doesn't just predict one thing. Instead we either need to predict every possible scenario with a likelihood or rate how surprised we are by the current scenario given the past

## Chapter 3 - Sparse Distributed Representations
