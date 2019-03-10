---
layout: post
title: Cellular art-omata 
---
<img src="/images/2019/cellular_automata.png" class="fit image">
Cellular art-omata, or cellular automata as they usually are called, demonstrate how simple mathematical rules can lead to astonishing complexity. 

A automaton is a collections of cells on a grid of a specific dimension and shape, where each cells is assigned or 'colored' into a finite number of states, usually two. 
The automaton then evolves through a number of discrete time steps according to a set of fixed rules based on the states of neighboring cells. 
A simple rule could be that if your right neighbor is colored, then you will copy its color. However, rules can be more complicated, fore more details read [here](http://mathworld.wolfram.com/ElementaryCellularAutomaton.html).
Cellular automata can and have been used to simulate a variety of real-world systems, including biological and chemical ones. 
Here I'm using them to generate cool-looking art.

The top figure shows the dynamics of multiple automata merged together into a 3x14 grid. This uses the classical two state dynamics where each cell can only assume one of 2 states, 0 - white, or 1 - black.

### Adding some color

<center><img src="/images/2019/magma_automata.png" class="fit image"></center>

We can also do something else, we can add color. This can be done i a lot of ways, the simplest is to add a simple gradient to the automata. I.e the state of a cell is still contingent one the state of its neighbors and some pre-defined rules, we just choose to offset the states with a constant that increases according to distance from the top row.

### Adding some movement

<center><img src="/images/2019/dynamics.gif" class="fit image" style="width:300px"></center>

We can also let the dynamics run over a longer timespan and capture snapshots as it changes, merging these into a gif <img src="/images/2019/winking-face.png" width="20">
