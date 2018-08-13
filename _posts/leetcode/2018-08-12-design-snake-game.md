---
layout: "post"
title: "Leetcode 353: Design Snake Game"
date: "2018-08-12 15:03"
tags:
  - Leetcode
---

# Question
Design a Snake game that is played on a device with screen size = width x height. Play the game online if you are not familiar with the game.

The snake is initially positioned at the top left corner (0,0) with length = 1 unit.

You are given a list of food's positions in row-column order. When a snake eats the food, its length and the game's score both increase by 1.

Each food appears one by one on the screen. For example, the second food will not appear until the first food was eaten by the snake.

When a food does appear on the screen, it is guaranteed that it will not appear on a block occupied by the snake.

Example:
```
Given width = 3, height = 2, and food = [[1,2],[0,1]].

Snake snake = new Snake(width, height, food);

Initially the snake appears at position (0,0) and the food at (1,2).

|S| | |
| | |F|

snake.move("R"); -> Returns 0

| |S| |
| | |F|

snake.move("D"); -> Returns 0

| | | |
| |S|F|

snake.move("R"); -> Returns 1 (Snake eats the first food and right after that, the second food appears at (0,1) )

| |F| |
| |S|S|

snake.move("U"); -> Returns 1

| |F|S|
| | |S|

snake.move("L"); -> Returns 2 (Snake eats the second food)

| |S|S|
| | |S|

snake.move("U"); -> Returns -1 (Game over because snake collides with border)
```

# Solution
```java
class SnakeGame {

    Queue<Integer> snake;
    boolean[][] board;
    int[][] food;
    int foodIndex;
    int height, width;
    int row, col;
    int score;

    /** Initialize your data structure here.
        @param width - screen width
        @param height - screen height
        @param food - A list of food positions
        E.g food = [[1,1], [1,0]] means the first food is positioned at [1,1], the second is at [1,0]. */
    public SnakeGame(int width, int height, int[][] food) {
        this.board = new boolean[height][width];
        this.food = food;
        this.foodIndex = 0;
        this.snake = new LinkedList<>();
        this.snake.offer(0);
        this.board[0][0] = true;

        this.width = width;
        this.height = height;

        this.row = 0;
        this.col = 0;

        this.score = 0;
    }

    /** Moves the snake.
        @param direction - 'U' = Up, 'L' = Left, 'R' = Right, 'D' = Down
        @return The game's score after the move. Return -1 if game over.
        Game over when snake crosses the screen boundary or bites its body. */
    public int move(String direction) {
        if(score == -1) return score;
        if(direction.equals("U")) row--;
        else if(direction.equals("L")) col--;
        else if(direction.equals("R")) col++;
        else if(direction.equals("D")) row++;
        // cross boundary
        if(row < 0 || row >= height || col < 0 || col >= width) {
            score = -1;
            return score;
        }
        // not food
        if(foodIndex == food.length || !(row == food[foodIndex][0] && col == food[foodIndex][1])) {
            int idx = snake.poll();
            board[idx / width][idx % width] = false;
        } else {
            score++;
            foodIndex++;
        }
        if(board[row][col]) { // bite itself
            score = -1;
            return score;
        } else {
            snake.offer(row * width + col);
            board[row][col] = true;
        }
        return score;
    }
}

/**
 * Your SnakeGame object will be instantiated and called as such:
 * SnakeGame obj = new SnakeGame(width, height, food);
 * int param_1 = obj.move(direction);
 */
```
