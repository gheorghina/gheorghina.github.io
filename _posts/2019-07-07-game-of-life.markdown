---
layout: post
title:  "An Infinite Game Of Life"
date:   2018-07-07 00:18:23 +0700
categories: [angular, typescript, gameoflife]
---

From [Wiki](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life):
   "The universe of the Game of Life is an infinite, two-dimensional orthogonal grid of square cells, each of which is in one of two possible states, alive or dead, (or populated and     unpopulated, respectively). Every cell interacts with its eight neighbours, which are the cells that are horizontally, vertically, or diagonally adjacent. At each step in time, the following transitions occur:


   Any live cell with fewer than two live neighbours dies, as if by underpopulation.
   Any live cell with two or three live neighbours lives on to the next generation.
   Any live cell with more than three live neighbours dies, as if by overpopulation.
   Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.
   The initial pattern constitutes the seed of the system. The first generation is created by applying the above rules simultaneously to every cell in the seed; births and deaths occur simultaneously, and the discrete moment at which this happens is sometimes called a tick. Each generation is a pure function of the preceding one. The rules continue to be applied repeatedly to create further generations.


   Conway originally conjectured that no pattern can grow indefinitely—i.e., that for any initial configuration with a finite number of living cells, the population cannot grow beyond some finite upper limit. In the game's original appearance in "Mathematical Games", Conway offered a prize of fifty dollars to the first person who could prove or disprove the conjecture before the end of 1970. The prize was won in November by a team from the Massachusetts Institute of Technology, led by Bill Gosper; the "Gosper glider gun" produces its first glider on the 15th generation, and another glider every 30th generation from then on."



   So, what would look an implementation which would support the infinite evolution of the "Gosper glider gun"?

**One Implementation Idea**

   The implementation idea is to keep the complexity low and to focus only on the meaningful data.
   It would for sure be very simple to navigate through the entire Universe with each iteration of the evolution and change the states of the cells, but this is no longer a good idea in case
   you have a 2^64 x 2^64 sized Universe, and only 4 alive cells in the center of it. 

   My idea is to work only with the alive cells and their immediate surrounded environment. I will call them `addiacent cells`. 
   Working in this manner will make the algorithm more efficient, bringing the capability to grow the universe more on a limited set of hardware.  

   
### Persisting and loading data

   Data will be saved/loaded from a json file which shall contain only the active cells of the universe.

   The .json file for the "Gosper glider gun" would look like this:

   ```
   {"cells":[{"x":6,"y":1},{"x":6,"y":2},{"x":7,"y":1},{"x":7,"y":2},{"x":6,"y":11},{"x":7,"y":11},{"x":8,"y":11},{"x":9,"y":12},{"x":5,"y":12},{"x":10,"y":13},{"x":10,"y":14},{"x":4,"y":13},{"x":4,"y":14},{"x":7,"y":15},{"x":5,"y":16},{"x":6,"y":17},{"x":7,"y":17},{"x":8,"y":17},{"x":9,"y":16},{"x":7,"y":18},{"x":6,"y":21},{"x":6,"y":22},{"x":5,"y":21},{"x":5,"y":22},{"x":4,"y":21},{"x":4,"y":22},{"x":3,"y":23},{"x":7,"y":23},{"x":3,"y":25},{"x":2,"y":25},{"x":7,"y":25},{"x":8,"y":25},{"x":4,"y":36},{"x":5,"y":36},{"x":4,"y":35},{"x":5,"y":35}]}
   ```

### The Data Model 

   The center piece is the `Cell` itself which shall know everything about its `state`, its `neighbours` and it shall have the behaviour of `evolving` its state depending on the surrounded condition.

   ```
      import { Position } from './position.model';

      export class Cell {
         private isAlive: boolean;
         x: number;
         y: number;
         neighbours;

         constructor(x, y, isAlive = false) {
            this.x = x;
            this.y = y;
            this.isAlive = isAlive;

            this.neighbours = [new Position(this.x - 1, this.y - 1),
            new Position(this.x - 1, this.y), new Position(this.x - 1, this.y + 1), new Position(this.x, this.y - 1),
            new Position(this.x, this.y + 1), new Position(this.x + 1, this.y - 1), new Position(this.x + 1, this.y),
            new Position(this.x + 1, this.y + 1)];
         }

         getIsAlive() {
            return this.isAlive;
         }

         changeCellState() {
            this.isAlive = !this.isAlive;
         }

         evolveFrom(oldGeneration) {
            var oldAliveNeighboursCount = this.getAliveNeighboursCount(oldGeneration);
            this.evolveState(oldAliveNeighboursCount);
         }

         private evolveState(oldAliveNeighboursCount) {
            if (!this.isAlive && oldAliveNeighboursCount == 3) {
                  this.isAlive = true;
                  return;
            }

            if (this.isAlive && (oldAliveNeighboursCount < 2 || oldAliveNeighboursCount > 3)) {
                  this.isAlive = false;
                  return;
            }
         }

         private getAliveNeighboursCount(oldGeneration) {

            let count = 0;

            for (var i = 0; i < oldGeneration.length; i++) {
                  for(var n=0; n < this.neighbours.length; n++){
                     if (this.neighbours[n].x == oldGeneration[i].x && this.neighbours[n].y == oldGeneration[i].y) {
                        count++;
                     }   
                  }          
            }       

            return count;
         }
      }
   ```

   Then, the `AddiacentCells` will hold the knowledge of computing the minimum required square area around the living cells, taking into consideration the universe size.

   ```
      export class AddiacentCellsGroup {
         cellsGroup = [];
         seenCells = [];
         private universeSize;
         private minX;
         private minY;
         private maxX = 0;
         private maxY = 0;

         constructor(universeSize) {
            this.universeSize = universeSize;
         }

         addCell(cell) {

            if (cell == undefined || this.seenCells[this.getKey(cell.x, cell.y)]) {
                  return;
            }

            this.cellsGroup.push(cell);
            this.updateLimits(cell);
            this.seenCells[this.getKey(cell.x, cell.y)] = true;
         }

         getMinX() {
            return (this.minX > 0) ? (this.minX - 1) : this.minX;
         }

         getMinY() {
            return (this.minY > 0) ? (this.minY - 1) : this.minY;
         }

         getMaxX() {
            return (this.maxX < this.universeSize) ? (this.maxX + 1) : this.maxX;
         }

         getMaxY() {
            return (this.maxY < this.universeSize) ? (this.maxY + 1) : this.maxY;
         }

         private updateLimits(newCell) {

            this.minX = (this.minX === undefined) ? this.cellsGroup[0].x : this.minX;
            this.minY = (this.minY === undefined) ? this.cellsGroup[0].y : this.minY;

            this.minX = Math.min(newCell.x, this.minX);
            this.minY = Math.min(newCell.y, this.minY);
            this.maxX = Math.max(newCell.x, this.maxX);
            this.maxY = Math.max(newCell.y, this.maxY);
         }

         private getKey(x, y){
            return x + '-' + y;
         }
      }
   ```

   The next important piece is the `Universe` which will hold the key to evolution.

   **to be continued**


### The Universe Evolution

   1. A list of computed addiacent cells is created: "the addiacent groups"
   The addiacent groups are computed in a recursive way:
        - for a given list of alive cells, one cell is extracted
        - for the extracted cell, a search is performed to extract the list of addiacent ones, and added to the same group
        - if there are any remaining cells in the list after the above step, then a new cell is extracted, and put in a new group
        - for the new extracted cell, the same approach is followed for computing its addiacent cells

   2. For each group of addiacent cells, it is calculated the Min and Max XY coordinates for being able to identify a smaller square universe around the alive cells. The coordinates are defined in such a way that this universe shall contain also the dead cells which might be resurected

   3. The evolution is calculated then for each group of addiacent cells, going from cell to cell in the smaller computed area around the living cells, given by the Min-Max computed coordinates 

   4. In order not to calculate the index of the living cells in the generation for identifying which one is alive and which one is dead, a new list is kept only with the information if alive cells were added to the generation. 
   For an easy interogation of the list, a key is computed, containing the row and col of each alive cell

   5. Then for each cell, the evolution mechanism applies with the Game of Life rules

   6. For being able to expand the universe, an additional check is performed for seeing if the margin of the current universe was hit



**The Complete Solution** 


[GameOfLife](https://github.com/gheorghina/GameOfLife)


Let me know what you think! 


