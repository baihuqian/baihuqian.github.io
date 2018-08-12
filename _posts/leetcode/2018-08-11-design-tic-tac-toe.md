---
layout: "post"
title: "Leetcode 348: Design Tic-Tac-Toe"
date: "2018-08-11 21:53"
tags:
  - Leetcode
---

# Question
Design a Tic-tac-toe game that is played between two players on a n x n grid.

You may assume the following rules:

1. A move is guaranteed to be valid and is placed on an empty block.
1. Once a winning condition is reached, no more moves is allowed.
1. A player who succeeds in placing n of their marks in a horizontal, vertical, or diagonal row wins the game.

Example:

```
Given n = 3, assume that player 1 is "X" and player 2 is "O" in the board.

TicTacToe toe = new TicTacToe(3);

toe.move(0, 0, 1); -> Returns 0 (no one wins)
|X| | |
| | | |    // Player 1 makes a move at (0, 0).
| | | |

toe.move(0, 2, 2); -> Returns 0 (no one wins)
|X| |O|
| | | |    // Player 2 makes a move at (0, 2).
| | | |

toe.move(2, 2, 1); -> Returns 0 (no one wins)
|X| |O|
| | | |    // Player 1 makes a move at (2, 2).
| | |X|

toe.move(1, 1, 2); -> Returns 0 (no one wins)
|X| |O|
| |O| |    // Player 2 makes a move at (1, 1).
| | |X|

toe.move(2, 0, 1); -> Returns 0 (no one wins)
|X| |O|
| |O| |    // Player 1 makes a move at (2, 0).
|X| |X|

toe.move(1, 0, 2); -> Returns 0 (no one wins)
|X| |O|
|O|O| |    // Player 2 makes a move at (1, 0).
|X| |X|

toe.move(2, 1, 1); -> Returns 1 (player 1 wins)
|X| |O|
|O|O| |    // Player 1 makes a move at (2, 1).
|X|X|X|
```

Follow up:

Could you do better than $$O(n^2)$$ per `move()` operation?

# Solution
```java
class TicTacToe {
    class PlayerInfo {
        ArrayList<Set<Integer>> cond;
        boolean win;

        public PlayerInfo(int n) {
            cond = new ArrayList<Set<Integer>>(2 * n + 2);
            for(int i = 0; i < 2 * n + 2; i++) {
                cond.add(new HashSet<>());
            }
            win = false;
        }

        public void addMove(int row, int col) {
            // row
            if(cond.get(row) != null) {
                cond.get(row).add(col);
                if(cond.get(row).size() == n) win = true;
            }
            // col
            if(cond.get(n + col) != null) {
                cond.get(n + col).add(row);
                if(cond.get(n + col).size() == n) win = true;
            }
            // top left to bottom right
            if(row == col && cond.get(2 * n) != null) {
                cond.get(2 * n).add(row);
                if(cond.get(2 * n).size() == n) win = true;
            }
            // bottom left to top right
            if(row + col == n - 1 && cond.get(2 * n + 1) != null) {
                cond.get(2 * n + 1).add(row);
                if(cond.get(2 * n + 1).size() == n) win = true;
            }
        }

        private void counterMove(int row, int col) {
            if(cond.get(row) != null) {
                cond.set(row, null);
            }
            if(cond.get(n + col) != null) {
                cond.set(n + col, null);
            }
            if(row == col && cond.get(2 * n) != null) {
                cond.set(2 * n, null);
            }
            if (row + col == n - 1 && cond.get(2 * n + 1) != null) {
                cond.set(2 * n + 1, null);
            }
        }
    }
    PlayerInfo player1, player2;
    int winner, n;

    /** Initialize your data structure here. */
    public TicTacToe(int n) {
        this.n = n;
        player1 = new PlayerInfo(n);
        player2 = new PlayerInfo(n);
        winner = 0;
    }

    /** Player {player} makes a move at ({row}, {col}).
        @param row The row of the board.
        @param col The column of the board.
        @param player The player, can be either 1 or 2.
        @return The current winning condition, can be either:
                0: No one wins.
                1: Player 1 wins.
                2: Player 2 wins. */
    public int move(int row, int col, int player) {
        if(winner != 0) return winner;
        if(player == 1) {
            player1.addMove(row, col);
            player2.counterMove(row, col);
            if(player1.win) winner = 1;
        } else {
            player2.addMove(row, col);
            player1.counterMove(row, col);
            if(player2.win) winner = 2;
        }
        return winner;
    }
}

/**
 * Your TicTacToe object will be instantiated and called as such:
 * TicTacToe obj = new TicTacToe(n);
 * int param_1 = obj.move(row,col,player);
 */
```
