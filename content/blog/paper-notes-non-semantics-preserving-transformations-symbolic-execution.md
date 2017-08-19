---
title: "Paper Notes: Non-Semantics-Preserving Transformations For Higher-Coverage Test Generation Using Symbolic Execution"
date: 2017-08-19T18:55:38+03:00
draft: false
---

Title: Non-Semantics-Preserving Transformations For Higher-Coverage Test Generation Using Symbolic Execution [[PDF]](../../papers/Non-Semantics-Preserving.Transformations.Converse.2017.pdf)

In this paper a number of non-semantics-preserving transformations are proposed
in order to increase code coverage on symbolic execution. These transformations
produce a simpler program by removing basic blocks based on metrics like path
length, hence the non-semantics-preserving part.

The most performant transformation appears to be the one that causes all loops
to return after a single iteration, which of course greatly reduces the state
space and allows symbolic execution to reach deeper paths.

There is an argument that these transformations are not suitable for all
programs since part of the functionality is removed and the results may not be
reproducible in the original. From the paper's results however it seems that
for the purpose of producing better test cases it has the potential to produce
good results.
