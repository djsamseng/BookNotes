This is a great (but quite complicated) explanation to actually understand how it works. https://www.youtube.com/watch?v=3RGEYYJmMtU

Here's my still learning understanding (please correct me where I'm wrong!).
Imagine you have N possible states and you're interested in what causes M of those Ex: I have two dice with 6x6=36 possible states. I'm interested when they sum to 3 (2 possible states). N=36, M=2.
With a classical computer you'd have to test each possibility of N feeding in Value(die1), Value(die2). Then you'd need to check each value to see if the sum is 3.

Quick background
Quantum bit (qbit) = probability of being a 0/1. So we represent this as [P(0), P(1)]
Entanglement = If two qbits are entangled, and we effect one (A), the other (B) is effected. Note: we can do a lot of powerful things like rotations of [P(0), P(1)] so watch the video to learn about that!

With a quantum computer we can take a quantum bit (qbit) A and entangle it with other qbits B,C,D,.. etc. Now feed A through a quantum gate to change its PA(0). PB(0), PC(0), PD(0), etc. are all effected. Thus by putting a SINGLE qbit through a SINGLE quantum gate, we've done calculations on ALL qbits. The idea here is that as we keep operating on this SINGLE qbit, we keep doing calculations on ALL qbits.

Ok so how does this help us with our original problem (finding the cause of M interesting states out of N possible states)?
1. Create qbits (input vector) to represent all N possible states. If this were a classical vector it would represent just a single combination of Value(die1) and Value(die2). Ie [5,6] in bits.
2. Create qbits (output vector) to represent the sum of die1 + die2 for all possible states of Value(die1) and Value(die2). Ie [2,3,4,5,6,7,8,9,10,11,12] in bits.
3. Create a qbit to represent whether or not it's interesting (sum=3) for the current value of the output vector (#2)
4. Feed the input vector (#1) through a quantum circuit. Classically this would calculate the sum of die1 + die2 = 12. So set the 12th element in the output vector to be 1 and all other elements to be 0. This says that this value of die1 and die2 has 100% probability to be 12 and 0% probability to be something else.
5. Do some quantum shenanigans

Ok so I may have lied a little bit. In quantum computing these bits are probabilities, not exact values. So in #1 I wasn't feeding in die1=5 and die2=6. I was feeding in a probability distribution of all possible states (lookup superposition if you don't think this is possible). And guess what? As output from my quantum circuit I also get probabilities. So our updated step 4 is

4. Feed the input vector (#1) through a quantum circuit. Set the output vector to be the probabilities for each possible sum given the input vector's probabilities.
5. Do some quantum shenanigans which increases the output vector's probabilities to be the desired value (3 in our example).
6. Send our modified output vector through our quantum circuit in the opposite direction. This makes our input vector's probabilities to be more probable of the possible solutions.
7. Take this updated input vector and repeat #4-6 a few times and the input vector's probabilities converges to the possible solutions.
8. Measure the input vector so it snaps to a specific value using the new probabilities.
9. It's highly likely that you now have one of the solutions

Voila!
Is this faster? Well according to the video you'd only have to do this sqrt(N/M) times (I believe the video says that doing it that many times guarantees the correct answer minus any measurement noise/thermal noise but I'm still working that one out)
