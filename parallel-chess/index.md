# Parallel Chess with Multiverse Time Travel

15-618 Parallel Computer Architecture and Programming - Final Project

Wei Chen ([wc3@andrew.cmu.edu](wc3@andrew.cmu.edu)), Ran You ([rany2@andrew.cmu.edu](rany2@andrew.cmu.edu))



**SUMMARY**

We will implement a parallel solver of the Chess with the Multiverse Time Travel game using MPI, OpenMP, and Cilk on the GHC machines. 


## Contents

[Background](#background)

[Challenges](#challenges)

[Resources](#resources)

[Goals and Deliverables](#goals-and-deliverables)
- [Plan to achieve](#plan-to-achieve)
- [Hope to achieve](#hope-to-achieve)

[Platform](#platform)

[Schedule](#schedule)

[References](#references)


## Background

[Chess with Multiverse Time Travel](https://store.steampowered.com/app/1349230/5D_Chess_With_Multiverse_Time_Travel/) is a chess game that allows pieces to travel back in time and travel through timelines. In this game, chess pieces can travel to the past, upon which a new timeline (or chess board) will be created exactly as it was at that point in time with the travelled piece added to it. In addition, chess pieces can also travel between timelines. At each turn, players must play a move on all of the active boards. And the game ends when checkmate happens on any chess board.

<img src="https://steamuserimages-a.akamaihd.net/ugc/1499090116262162524/E00E247BD77D0BCAA7677FFCB1E6CBCC8310912B/?imw=5000&imh=5000&ima=fit&impolicy=Letterbox&imcolor=%23000000&letterbox=false" width="500px" />

*Source: https://steamcommunity.com/sharedfiles/filedetails/?id=2204910380*

In the case when the entire board gets too large, we will prune the earliest created boards if there are more than 16 currently active boards in order to reduce search space as well as parallel processes. 

<img src="https://steamuserimages-a.akamaihd.net/ugc/1713031237434740981/67ACC26410AA9118659FFBA381CD8C1C7AB19C82/?imw=1920&&ima=fit&impolicy=Letterbox&imcolor=%23000000&letterbox=false" width="500px" />

*Source: https://steamcommunity.com/app/1349230/screenshots/*

This is an application largely based on an AI of the original chess game. Despite the additional communication, synchronization, and optimization across chess boards, the game proceeds in the exact same rules as normal chess. Our goal is to build a parallel program to solve the game, i.e., find a relatively good move at each step, but not to build a good parallel game interface, although the program will include both a game engine and a game solver. 

**Things to parallel** include searching for the best move on all chess boards, where parallelization is across chess boards. As the search space (4D) is much larger than normal chess, it’s essential to parallelize the searching process within one chess board, where parallelization is across search space. Since different chess boards have data dependencies (chess pieces can move from one chess board to another), and the optimization is carried over all temporal and parallel boards, it requires communication (shared state or message passing) and synchronisation over different processes. 

In case this game is too hard to simulate, one fallback option is bughouse, which has two parallel chess boards, and pieces can be moved across these two boards.


## Challenges

1. **Game logic:** The logic of this game is more complex than the original chess, because there are multiple concurrent boards, game state is much larger, search space is much larger, and different chess boards have data dependencies. 
2. **Utility function:** The utility function is not trivial, and is critical to the optimization. Some basic ideas include maximizing distance between the king and opponent’s pieces (or the other way around), maximizing the number of steps taken for the opponent to eat any of the pieces, minimizing the number of eaten pieces, etc. We will mostly learn from available sources to build the utility function. 
3. **Optimization algorithm:** We will base our optimization algorithm on the [Minimax Algorithm](https://en.wikipedia.org/wiki/Minimax). We hope to make the optimization logic as simple as possible in order to focus on parallelization. 
4. **Historical game states:** We need to design a data structure (boards or moves between boards) in order to recreate any historical board efficiently, and these boards should be easily accessible during optimization. 
5. **Data dependencies:** There are data dependencies across boards when searching for the best move, since pieces can move across boards. There are also data dependencies across game states within one board during the monte carlo tree search. 
6. **Work assignment:** We are still unsure about how exactly to map work to resources. And one of our goals in this project is to experiment with different work assignments. 
7. **Load balancing:** The search space can differ across different boards and timestamps. 
8. **Communication and synchronization:** Different chess boards need to communicate and synchronize in this game. We plan to compare the shared address space model with the message passing model using MPI, OpenMP, and Cilk. 
9. **Evaluation:** There is a tradeoff between a better move and better speedup. We need to figure out how to evaluate the quality of a move, how to evaluate speedup, as well as clearly define the evaluation metrics. 


## Resources

We plan to build our game program from scratch, although we will largely learn from available sources: 
- Chess AI from previous students: https://github.com/ihajhasa/Parallel_Chess_AI
- Chess in 5D (open source):  https://chessin5d.net/

We will also learn from available sources about building chess AI, the optimization algorithms, and how to construct a reasonable utility function. 


## Goals and Deliverables

We will implement a game engine and a game solver,  the game solver is based on a simple AI using the Minimax algorithm, and our main focus is to parallelize and optimize the entire game simulation process. The performance of our program is based on the quality of the searched results and the time it takes to solve a step at a fixed number of concurrent chess boards. 

In the end, we want to demonstrate a game simulation using visuals (hopefully videos). We will present our algorithm, which parts are parallelized, the optimization results, the speedup results, and a comparison across models and frameworks. 

### Plan to Achieve

1. Build a parallel game simulation (including a game engine and a game solver). 
2. Compare the shared address space model with the message passing model. 
3. Compare the three parallel programming frameworks (MPI, OpenMP, and Cilk). 
4. Parallelize the Minimax algorithm, evaluate speedup compared with sequential version. 
5. Parallelize the multiverse chess game, evaluate speedup compared with sequential version. 

### Hope to Achieve

1. More complicated chess AI using other heuristics or optimization algorithms. 
2. Analyze tradeoff between optimization and speedup.  


## Platform

We will be using the GHC machines for development, testing, and experiments. We hope to use [Bridges-2](https://www.psc.edu/resources/bridges-2/) in order to leverage more parallelism, if that’s possible. 

## Schedule

| Week | Tasks                                                        |
| ---- | ------------------------------------------------------------ |
| Week 2: 11/12 - 11/14 | Draft new proposal |
| Week 3: 11/15 - 11/21 | Research about Chess with Multiverse Time Travel<br />Design and implement the sequential version |
| 11/22 | Milestone Report |
| Week 3: 11/22 - 11/28 | Parallelize the program using different parallel frameworks |
| Week 4: 11/29 - 12/05 | Optimize the parallel implementation |
| Week 5: 12/06 - 12/09 | Conduct experiments and analysis on the parallel program<br />Write project report and prepare for project presentation |
| 12/09 | Project Report           |
| 12/10 | Poster Session |

## Resources

* (5D Chess with Multiverse Time Travel)[https://www.5dchesswithmultiversetimetravel.com/]
* (Wikipedia: 5D Chess with Multiverse Time Travel)[https://en.wikipedia.org/wiki/5D_Chess_with_Multiverse_Time_Travel]
* (Wikipedia: Minimax algorithm)[https://en.wikipedia.org/wiki/Minimax]

