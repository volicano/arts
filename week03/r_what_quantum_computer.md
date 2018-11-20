## ARTS

##### Review

## What is a quantum computer? Explained with a simple example.


Hi everyone!

The other day, I visited [D-Wave Systems](https://www.dwavesys.com/) in Vancouver, Canada. It’s a company that makes cutting-edge quantum computers.

I got to learn a lot about quantum computers there, so I’d like to share some of what I learned there with you in this article.

The goal of this article is to give you an accurate intuition of what a quantum computer is using a simple example.

This article will not require you to have prior knowledge of either quantum physics or computer science to be able to understand it.

Okay, let’s get started.

### What is a quantum computer?

Here is a one-sentence summary of what a quantum computer is:

>*A quantum computer is a type of computer that uses quantum mechanics so that it can perform certain kinds of computation more efficiently than a regular computer can.*

There is a lot to unpack in this sentence, so let me walk you through what it is exactly using a simple example.

To explain what a quantum computer is, I’ll need to first explain a little bit about regular (non-quantum) computers.

### How a regular computer stores information

Now, a regular computer stores information in a series of 0’s and 1’s.

Different kinds of information, such as numbers, text, and images can be represented this way.

Each unit in this series of 0’s and 1’s is called a bit. So, a bit can be set to either 0 or 1.

Now, what about quantum computers?

A quantum computer **does not** use bits to store information. Instead, it uses something called qubits.

Each qubit can not only be set to 1 **or** 0, but it can also be set to 1 **and** 0. But what does that mean exactly?

Let me explain this with a simple example. This is going to be a somewhat artificial example. But it’s still going to be helpful in understanding how quantum computers work.

### A simple example for understanding how quantum computers work

Now, suppose you’re running a travel agency, and you need to move a group of people from one location to another.

To keep this simple, let’s say that you need to move only 3 people for now — Alice, Becky, and Chris.

And suppose that you have booked 2 taxis for this purpose, and you want to figure out who gets into which taxi.

Also, suppose here that you’re given information about who’s friends with who, and who’s enemies with who.

Here, let’s say that:

* Alice and Becky are friends
* Alice and Chris are enemies
* Becky and Chris are enemies

And suppose that your goal here is to divide this group of 3 people into the two taxis to achieve the following two objectives:

* Maximize the number of **friend pairs **that share the same car
* Minimize the number of **enemy pairs **that share the same car

Okay, so this is the basic premise of this problem. Let’s first think about how we would solve this problem using a regular computer.

**Solving this problem with a regular computer**

To solve this problem with a regular, non-quantum computer, you’ll need first to figure out how to store the relevant information with bits.

Let’s label the two taxis Taxi #1 and Taxi #0.

Then, you can represent who gets into which car with 3 bits.

For example, we can set the three bits to **0**, **0**,* *and **1** to represent:

* Alice gets into Taxi #0
* Becky gets into Taxi #0
* Chris gets into Taxi #1

Since there are two choices for each person, there are 2*2*2 = 8 ways to divide this group of people into two cars.

Here’s a list of all possible configurations:

A | B | C

0 | 0 | 0

0 | 0 | 1

0 | 1 | 0

0 | 1 | 1

1 | 0 | 0

1 | 0 | 1

1 | 1 | 0

1 | 1 | 1

Using 3 bits, you can represent any one of these combinations.

Computing the score for each configuration

Now, using a regular computer, how would we determine which configuration is the best solution?

To do this, let’s define how we can compute the score for each configuration. This score will represent the extent to which each solution achieves the two objectives I mentioned earlier:

* Maximize the number of **friend pairs **that share the same car
* Minimize the number of **enemy pairs **that share the same car

Let’s simply define our score as follows:

(the score of a given configuration) = (# friend pairs sharing the same car) - (# enemy pairs sharing the same car)

For example, suppose that Alice, Becky, and Chris all get into Taxi #1. With three bits, this can be expressed as **111**.

In this case, there is only **one friend pair** sharing the same car — Alice and Becky.

However, there are **two enemy pairs** sharing the same car — Alice and Chris, and Becky and Chris.

So, the total score of this configuration is 1-2 = -1.

Solving the problem

With all of this setup, we can finally go about solving this problem.

With a regular computer, to find the best configuration, you’ll need to essentially go through all configurations to see which one achieves the highest score.

So, you can think about constructing a table like this:

A | B | C | Score

0 | 0 | 0 | -1

0 | 0 | 1 | 1 <- one of the best solutions

0 | 1 | 0 | -1

0 | 1 | 1 | -1

1 | 0 | 0 | -1

1 | 0 | 1 | -1

1 | 1 | 0 | 1 <- the other best solution

1 | 1 | 1 | -1

As you can see, there are two correct solutions here — 001 and 110, both achieving the score of 1.

This problem is fairly simple. It quickly becomes too difficult to solve with a regular computer as we increase the number of people in this problem.

We saw that with 3 people, we need to go through 8 possible configurations.

What if there are 4 people? In that case, we’ll need to go through 2*2*2*2 = 16 configurations.

With n people, we’ll need to go through (2 to the power of n) configurations to find the best solution.

So, if there are 100 people, we’ll need to go through:

* 2¹⁰⁰ ~= 10³⁰ = one million million million million million configurations.

This is simply impossible to solve with a regular computer.

Solving this problem with a quantum computer

How would we go about solving this problem with a quantum computer?

To think about that, let’s go back to the case of dividing 3 people into two taxis.

As we saw earlier, there were 8 possible solutions to this problem:

A | B | C

0 | 0 | 0

0 | 0 | 1

0 | 1 | 0

0 | 1 | 1

1 | 0 | 0

1 | 0 | 1

1 | 1 | 0

1 | 1 | 1

With a regular computer, using 3 bits, we were able to represent only one of these solutions at a time — for example, 001.

However, with a quantum computer, using 3 **qubits**, we can represent **all 8 of these solutions at the same time**.

There are debates as to what it means exactly, but here’s the way I think about it.

First, examine the first qubit out of these 3 qubits. When you set it to **both*** *0 and 1, it’s sort of like creating two parallel worlds. (Yes, it’s strange, but just follow along here.)

In one of those parallel worlds, the qubit is set to 0. In the other one, it’s set to 1.

Now, what if you set the second qubit to 0 **and** 1, too? Then, it’s sort of like creating 4 parallel worlds.

In the first world, the two qubits are set to 00. In the second one, they are 01. In the third one, they are 10. In the fourth one, they are 11.

Similarly, if you set all three qubits to both 0 and 1, you’d be creating 8 parallel worlds — 000, 001, 010, 011, 100, 101, 110, and 111.

This is a strange way to think, but it is one of the correct ways to interpret how the qubits behave in the real world.

Now, when you apply some sort of computation on these three qubits, you are actually applying the same computation in all of those 8 parallel worlds at the same time.

So, instead of going through each of those potential solutions sequentially, we can compute the scores of all solutions at the same time.

With this particular example, in theory, your quantum computer would be able to find one of the best solutions in a few milliseconds. Again, that’s 001 or 110 as we saw earlier:

A | B | C | Score

0 | 0 | 0 | -1

**0 | 0 | 1 | 1 <- one of the best solutions**

0 | 1 | 0 | -1

0 | 1 | 1 | -1

1 | 0 | 0 | -1

1 | 0 | 1 | -1

**1 | 1 | 0 | 1 <- the other best solution**

1 | 1 | 1 | -1


In reality, to solve this problem, you would need to give your quantum computer two things:

* All potential solutions represented with qubits
* A function that turns each potential solution into a score. In this case, this is the function that counts the numbers of friend pairs and enemy pairs sharing the same car.

Given these two things, your quantum computer will spit out one of the best solutions in a few milliseconds. In this case, that’s 001 or 110 with a score of 1.

Now, in theory, a quantum computer is able to find one of the best solutions every time it runs.

However, in reality, there are errors when running a quantum computer. So, instead of finding the best solution, it might find the second-best solution, the third best solution, and so on.

These errors become more prominent as the problem becomes more and more complex.

So, in practice, you will probably want to run the same operation on a quantum computer dozens of times or hundreds of times. Then pick the best result out of the many results you get.

How a quantum computer scales

Even with the errors I mentioned, the quantum computer does not have the same scaling issue a regular computer suffers from.

When there are 3 people we need to divide into two cars, the number of operations we need to perform on a quantum computer is 1. This is because a quantum computer computes the score of all configurations at the same time.

When there are 4 people, the number of operations is still 1.

When there are 100 people, the number of operations is still 1. With a single operation, a quantum computer computes the scores of all **2¹⁰⁰** ~= **10³⁰** = **one million million million million million** configurations at the same time.

As I mentioned earlier, in practice, it’s probably best to run your quantum computer dozens of times or hundreds of times and pick the best result out of the many results you get.

However, it’s still much better than running the same problem on a regular computer and having to repeat the same type of computation one million million million million million times.

Wrapping up

Special thanks to everyone at D-Wave Systems for patiently explaining all of this to me.

D-Wave recently launched a cloud environment for interacting with a quantum computer.

If you’re a developer and would like actually to try using a quantum computer, it’s probably the easiest way to do so.

It’s called Leap, and it’s at [https://cloud.dwavesys.com/leap](https://cloud.dwavesys.com/leap). You can use it for free to solve thousands of problems, and they also have easy-to-follow tutorials on getting started with quantum computers once you sign up.

**Footnote:**
* In this article, I used the term “regular computer” to refer to a non-quantum computer. However, in the quantum computing industry, non-quantum computers are usually referred to as classical computers.

个人理解：
通过三个人分两辆出租车来解决二个问题：

1、最大化共享同一辆车的朋友对的数量

2、最大限度地减少共享同一辆汽车的敌人对的数量

好抽象似懂非懂。

[https://medium.freecodecamp.org/what-is-a-quantum-computer-explained-with-a-simple-example-b8f602035365](https://medium.freecodecamp.org/what-is-a-quantum-computer-explained-with-a-simple-example-b8f602035365)

