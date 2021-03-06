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
- Problem: our knowledge is not divided into discrete facts with well-defined relationships. Relationships are too numerous and ill-defined to map onto traditional data structures
  - Brains use SDRs? I don't think so
- Bits in an SDR correspond to neurons in the brain, 1 is an active neuron, 0 is an inactive neuron
- Each bit in an SDR has a meaning. The semantic meaning of each bit is learned
- SDRs are not moved around in meory, within a set of neurons an SDR at t links to an SDR at t+1 thus sequences of SDRs are learned
- SDRs can link between modalities (SDR in the audio region with an SDR in the visual region)
- The brain, think about cars -> wheels -> tires -> keep going on and on = expanding web of associations
  - vs computers binary encode 01101101 to be "m" and 01100101 to be "e". It's purely abstract with no meaning/association
  - SDRs use a ton of bits, similar SDRs are close. All letters have SDRs with bits signaling it's a letter activated and bits signaling its other things deactivated
    - This is completely inefficient and a waste of space. We don't need to store the inactive bits. The brain doesn't. The brain uses it's structure to use what is active to activate other things. It doesn't need to check an inactive neurons, it can just maintain a priority queue of all active neurons ignoring the inactive
    - It also doesn't take advantage of hierarchy. Every letter doesn't need to store the fact that it's a letter. Instead they can all be connected to the _letter_ neuron as a graph structure
- **We only need to store the locations of the 1 bits (not the entire SDR). Now randomly choose only 10/200 of the 1 bits and store only those. Comparison has low probability of a false match and are still semantically similar**
- Because SDRs are sparse they can only encode _n choose w_ combinations not the full 2^n (n=num bits, w=max num on bits)
  - n=2048, w=40 -> 2.37 * 10^84 encodings vs 3.23 * 10^616. overlap probability is 1/(n choose w) = 10^-616 which means we can probably assign new encoding at random
- Overlap set = SDRs of size n with w bits on that share exactly b on bits with the target SDR
- Inexact matching
  - probability of a false match between SDR x and SDR y both with size n, w on bits and overlap >= b0 is Sum(b0->w)(OverlapSet)/(n choose w)
  - w=4, b=2 P(false positive) = 1/14,587. w=20 b=10 P(false positive)=10^-13 which means we can have 50% noise but not a lot of false positives
- Subsampling
  - we can store 20/40 of the on bits of a n=2048 SDR with b=10 and P(false positive)=10-^12
- Store a sequence of SDRs by taking the OR of the elements
  - increases chance of false positives - but the human brain often makes this mistake
  - order can be encoded into the SDRs themselves
- O(w) not O(n)
- The brain
  - theory: inter-spike time of neurons encodes information -> Falsified
  - theory: neurons output a scalar = rate of spiking -> Falsified
  - theory: the brain can perform tasks so quickly that the **neurons don't have enough time for even a second spike from each neuron** to contribute to the completion. This is because each neuron would have to wait for 2 spikes before it can start
  - HTM: neurons is either
    - Active (spiking)
    - Inactive (not spiking/slowly spiking)
    - Predicted (deplorarized but not spiking) (ie primed)
    - Active after predicted (mini-burst followed by spiking)
  - any individual neuron can stop working with little effect to the network -> population of active cells is what matters most
  - can add more bits to the SDR to make up for information lost in variable rate encodings not being present
    - no floating point operations
  - when a neurons recognizes a pattern, it generates a local spike which depolarizes the cell without generating an output spike. This indicates a prediction of future activity -> **difference between when an input is predicted vs when an input isn't predicted (feedback)**
- Neuronal Predictions as unions of SDRs
  - take sets of cell SDRs that are deplorized and form a union of them. This is predicting multiple things at once. **If an unexpected input occurs, you are aware of it**. But how do we learn?
- The brain uses **associative memory (SDR is linked to an SDR ...). SDRs are recalled through association with other SDRs**
- Learning a neuron to recognize a pattern of activity
  - neuron forms synapses to the active cells in the pattern
  - we want a set of neurons to recognize a pattern
  - we can SDR pattern A to invoke SDR pattern B
    - achieved by linking >20 synapses from each active cell in pattern B to a random sample of cells in pattern A
    - Pattern A and pattern B can be concurrently active (ex: different areas of the brain but not directly connected like audio and visual)
    - Pattern A connects distal synapses to B -> B becomes predicted at t+1
    - Pattern A connects to the proximal synapses to B -> B becomes active at t+1

## Chapter 4 - Encoders
- cochlea converts frequencies and amplitudes of sounds into a sparse set of active neurons
  - each hair responds to a range of frequencies and the ranges overlap with nearby hair cells
- encoders
  - similar data should overlap
  - the same inputs always produces the same SDR as output
  - output should have similar sparsity for all inputs. Enough 1 bits to handle noise and subsambling. >= 20 one bits
  - for noisy signals you want smaller buckets. For precise signals you want bigger buckets
- cochlea for humans is 20hz to 20khz
  - log encoding, 1000 to 2000 has smaller effect than 5 to 10
  - delta encoder, predicting differences

## Chapter 5 - Spatial Pooling Algorithm
- terminonlogy
  - column = an SPR for a given cell
  - receptive field = input space a column can connect to
  - permanence value = amount of growth (connection strength?) between a mini-column and one of the cells in its receptive field
  - permanence threshold = if > threshold, fully connected. in range 0 to 1. Not learnable but randomly initialized and fixed per SDR
  - synapse = junction between cells. synapses on an SDR connect to bits in the input space. a synapse is connected (above threshold), potential (below threshold), unconnected (no ability to connect)
- algorithm
  1. input = sensory and/or from other SDRs
  2. initialize with fixed number of columns (SDRs) to receive that input. Randomly initialize connections from the inputs to the synapses in the SDRs. Randomly initialize permanence thresholds
  3. determine the number of connected synapses on each column (SDR) that are connected to ON input bits (ie go from the on input bits to the SDR synapses)
  4. Num connected synapses * boost. boost = dynamically determined by how often a column is active vs its neighbors
  5. A small percent of columns with the highest activations after boosting become active
  6. Spatial Pooling learning rule
  7. Repeat from 3
- Spatial Pooling learning rule
  1. For each active column, adjust the permanence values (not threshold, but its actual value?) of all the potential synapses (below threshold)
  2. Increase the permanence of synapses aligned with active input bits.
  3. Decreate the permanence of synapses aligned with inactive input bits.
- Spatial Pooling Pseudocode
  1. Initialization. Compute potential destination synapses for each column. Done by creating a random input set and represent that by a synapse and assign a random permanence value
  2. Compute overal with input for each column * boost
  3. Compute winning dest columns
  4. Learn
    1. For winning columns, increase it's permanence if the synapse is active, otherwise decrease. Do not modify non-winning columns
    2. Boost updates. If a column does not win enough relative to neighbors, increase it's boost and vice versa to encourage more equal winning

## Chapter 6 - Temporal Memory
- learn sequences of SDRs and make predictions of what the next SDR will be
- predict current input given previous inputs
- proximal vs distal dendrite segments
  - proximal dendrite forms synapses with feed-forward inputs. Linearly sum the active synapses to determine the feed forward activation for this column
  - distal dendrite forms synapses with cells within the layer. If the sum of the active synapses on a distal segment exceeds a threshold then that cell in the column enters the predicted state
- Spatial pooling learns feed foward connections between inputs and columns
- Temporal memory learns connections between cells in the same layer
- multiple cells per column is like an RNN. 2 cells is an RNN cell. 3 cells would be a 3 step loop back onto itself
- When an input comes in, if one cell is in a predictive state then it becomes active = I expected that input! If no cells are in a predictive state then they all become active as an unexpected input (I don't know why I activated but I did)
- When a cell becomes active, it forms connections to a subset of nearby cells that were active immediately prior
- grow synapses to cells that were previously active (ie from the previous state to the new state)
- Pseudocode
  1. Activate predicted columns that match
  2. Punish predicted columns that don't match
  3. Reinforce matching predicted columns synapses that contributed. Punish synapses that didn't contribute

## Chapter 7: Voting Across Columns
- lateral connections allow columns to integrate inputs over space (x,y in an image) so that all columns get the context of the remainder
- (paper)[https://www.frontiersin.org/articles/10.3389/fncir.2017.00081/full]

## Chapter 8: Location Layer in Grid Cells
- neocortex learn objects by learning sets of sensory features at locations
- (follow on paper to chapter 7)[https://www.frontiersin.org/articles/10.3389/fncir.2019.00022/full]