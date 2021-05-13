+++
title = "Thesis - DHSVM Parallel"
weight = 0
date = 2019-11-27
description = "My Graduate thesis consisted of taking an old C program desinged to simulate water sheds and making key parts of it run in parallel. In addition I modified the program to run many instances at a time, and allowed random ranges to be specified in the parameter files."

[extra]
heroimage = "/images/cv/thesis.png"
+++

# Description

This project takes the scientific simulation made by the Univerisity of Washington [here](https://www.pnnl.gov/projects/distributed-hydrology-soil-vegetation-model) and optimizes it serially and makes it run in parallel. This project was completed and defended in 2018.

This project has a number of interesting challenges. First off, from a Software Engineering standpoint, I'm taking a nontrivial amount of unknown code and having to write a test case suite to ensure integrity over time as I work on optimizing it. From a performance stand point this program has a variety of optimizations that can be made from compacting needlessly duplicated for loops, to large loops that can be distributed across many cores. Finally, this project isn't purely academic. This project was used by another master thesis in the Agriculture Science Department at CalPoly. This follow-up thesis dictated the direction my work took.

[Github page here](https://github.com/aadriance/DHSVM-Parallel)

[Original Thesis paper here](https://digitalcommons.calpoly.edu/cgi/viewcontent.cgi?article=3174&context=theses)

[Published version of the paper here](https://link.springer.com/chapter/10.1007/978-3-030-16205-4_14)

[Work used by this master thesis here](https://digitalcommons.calpoly.edu/theses/2093/)

[Original credit to Wigmosta et al. (1994)](http://onlinelibrary.wiley.com/doi/10.1029/94WR00436/abstract)

An official parallel update [now exists](https://github.com/pnnl/DHSVM-PNNL/tree/parallel) based off of [this research](https://www.sciencedirect.com/science/article/pii/S1364815219303226?via%3Dihub). Which takes a different approach to my implementation.  However, it references my work which is neat.