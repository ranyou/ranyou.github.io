Wei Chen ([wc3@andrew.cmu.edu](wc3@andrew.cmu.edu)), Ran You ([rany2@andrew.cmu.edu](rany2@andrew.cmu.edu))


**SUMMARY**

We will implement a parallel solver of the Chess with the Multiverse Time Travel game using MPI, OpenMP, and Cilk on the GHC machines. 


## Contents

- [Proposal](./proposal.md)

- [Milestone](./milestone.md)

- [Report](./report.md)

- [Source code](https://github.com/ranyou/parallel-chess)

## Checklist

- [X] Finish implementation of multiverse chess rules
- [ ] Test Cilk coding environment
- [X] Finish implementation Minimax algorithm
- [X] Define and implement utility function
- [X] Parallelize the program in OpenMP
- [X] Parallelize the program in MPI
- [ ] Parallelize the program in Cilk
- [ ] Optimize parallel programs
- [X] Conduct experiments and analysis on the parallel program
- [X] Write project report and prepare for project presentation

## Game Rules

### Extra dimensions of 4D Chess
The traditional chessboard coordinates are defined as row,col and row∈[0,7]，col∈[0,7]. 4D chess have two additional dimensions timestep and timeline and Timestep ∈ [1,+∞]  Timeline ∈ [-∞,+∞] 

* Timestep: Number of rounds. The coordinate range is from the first round to the infinite round.
* Timeline : Timeline coordinates. White piece is recorded as positive, black is recorded as negative, and the initial timeline is 0

![img](https://lh5.googleusercontent.com/odr9NS039hv5Wv03Nz_BKPKEWZnq5nYN3CetNeWrrRyl4OgiYureaks3ucOSkQV49pTGOa4NhrKQ_dq7FyhOOgbFdQVB3DRWes-73nqVEqesSDwFzfrSkEDr9AJI1bWy8sWF76wO)



### Moves of chess pieces
The rules of chess moves are the same as original chess. However, when a chess moves across timelines or back in time, these count as extra dimensions. For example, a knight can move 1 block in one dimension and 2 blocks in a perpendicular dimension, and timelines or timesteps are dimensions perpendicular to the ranks/files on a chess board. 


## Algorithm

```
find_moves {

For all timelines			// parallelize over all timelines
		For all historical timesteps	// parallelize over all timesteps
			For all chess pieces	// parallelize over all pieces
				Generate possible moves on current board (2D)
				Add moves to a queue

}

main {
Initialize board
While (is not end) {
		White find best moves for all timelines	// parallelize over timelines
			Build monte carlo tree			// parallelize on subtrees (hard)
				Run find_moves and update moves recursively
			Evaluate utility function on all leaf nodes	// parallel on nodes
			Run minimax algo
		Update white moves
		
		Black find best moves
			… … (same as white)
		Update black moves
	}
}
```

