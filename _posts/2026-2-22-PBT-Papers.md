---
title: Notes for my Optional Paper Reading
tags: Other
---

## Property-Based Testing in Practice
[The paper](https://andrewhead.info/assets/pdf/pbt-in-practice.pdf) draws from 30 interviews about how programmers at Jane Street use PBT. It is mainly used to test complex code and as a fallback for increasing confidence. An interesting secondary benefit: the properties written for PBT act as documentation explaining what the program should do. On the other hand, it's harder to write properties and evaluate effectiveness. 

**R1:** How should PBT be used in the software dev lifecycle?

**R2:** Where can we improve PBT based on what the devs want?

### Background
The paper uses the good ol' BST example. In OCaml's QuickCheck, you have the following code. 

![OCaml](/assets/img/OCamlQC.png)

QuickCheck.test is a function taking in two arguments. The first is the generator on the first line. Generators can be built easily in OCaml using combinators and base generators, but if we want to ensure accuracy and efficiency, a decent amount of effort will be put into making these generators. The second line tests the property, which here is "insert returns a valid BST". Using the generator, we create thousands of random (x, t) pairs and check that insert returns a valid BST for all of them. Finally, since the output could include a lot of irrelevant things, there are tools to reduce the output, called "shrinking". Thus, the example shows the main anatomy of PBT: a module to be tested, a concise property, a generator, and a shrinker. 

There are a lot of similarities between fuzzing and PBT. Both aim to find bugs by randomly generating test cases. The difference is in their goals, where fuzzers don't care about logical errors and more so look for crashes or memory leaks.
