# Milestone Report

Wei Chen ([wc3@andrew.cmu.edu](wc3@andrew.cmu.edu)), Ran You ([rany2@andrew.cmu.edu](rany2@andrew.cmu.edu))

## Contents

* [1 Project Schedule](#1-project-schedule)
* [2 Project Progress](#2-project-progress)
* [3 Goals and Deliverables](3-goals-and-deliverables)
* [4 Issues](4-issues)



## 1 Project Schedule

| Dates         | Tasks                                                        |
| ------------- | ------------------------------------------------------------ |
| 11/22 - 11/23 | Finish implementation of multiverse chess rules<br />Test Cilk coding environment |
| 11/24 - 11/25 | Finish implementation Minimax algorithm<br />Define and implement utility function |
| 11/26 - 11/27 | Parallelize the program in OpenMP                            |
| 11/28 - 11/29 | Parallelize the program in MPI                               |
| 11/30 - 12/01 | Parallelize the program in Cilk                              |
| 12/02 - 12/05 | Optimize parallel programs                                   |
| 12/06 - 12/08 | Conduct experiments and analysis on the parallel program<br />Write project report and prepare for project presentation |
| 12/09         | Project Report                                               |
| 12/10         | Poster Session                                               |



## 2 Project Progress

We have started the design and implementation of a sequential version of the game. However, we’ve fallen behind our schedule because we’ve underestimated the complexity of the game rules and optimization logic. We have simplified the problem and drawn up a more detailed plan for how to progress. 

## 3 Goals and Deliverables
Goals: 

* Design and implement sequential version of parallel chess with multiverse time travel
* Parallelize the game using MPI, OpenMP, and Cilk, and optimize
* Evaluate the speedup of searching M moves in a game with N parallel boards
* (Optional) Optimize the search results by adjusting heuristics and utility functions

Deliverables: 

* A simple introduction of the game (as slides)
* A demonstration of the algorithms, including how the program is parallelized (as graphs), including which parts are parallelized, where are locks and barriers, how messages are communicated
* A comparison of different computing frameworks (as speedup graph)Analysis of experimental results (as slides)

## 4 Issues
We realized the rules of multiverse chess are much more complex than we expected. It has many additional infrastructure built upon the basic chess rules. We plan to simplify the problem by defining our own set of rules. We also realized that building an AI for chess itself is no simple task, which requires using good heuristics, optimization algorithms, and utility functions. We plan to build our AI based on Minimax algorithm with a simple utility function (average distance of pieces to the king of the adversarial with different weights). We will adjust this utility function after we’ve done more research as we progress. We need to set up a simple working game as fast as possible so that we can move on to parallelize the game on MPI, OpenMP, and Cilk. We need to test the Cilk environment on GHC machines. 
