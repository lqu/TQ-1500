# TQ-1500
Connecting antique electronic go board with modern A.I.

## Goal
This project is an attempt to add modern features to a 40-year-old electronic Go board. The ultimate goal is to reuse the TQ-1500 hardware and connect it to GO A.I. programs.

## Requirements
* Sense the stone newly placed on the board
* Send game state updates to KataGo running on NVIDIA Jetson Xavier NX
* NX recommends the next move
* Go board flashes LEDs to guide human user to place the next stone
* Repeat until game ends

## Background
TQ-1500 is an electronic Go board produced by National/Matsushita/Panasonic in 1981, when Japan had a prosperous economy, strong electronics industry, and the best Go players of the world. Initially it was sold at ¥198,000 (about $2,000). It has following features.
1. Magnetic Go stones tells its internal computer where the last move was placed
2. Four arrays of LEDs around the board to indicate where the next move should be in (X, Y) coordinate
3. "REPLAY Mode" guides users through a game by flashing LEDs
    1. Saved games/problems are read from magnetic cards
    2. Proceeds if the user follows, and the stones are placed correctly
    3. Pauses and warns the user if stone positions deviate from the recorded game
    4. Voice to indicate who plays next (black or white), and to remind the user to pick up the captured stones
    5. Occationally, hides the next move and let user guess
4. "RECORD Mode" saves user game to internal memory or magnetic cards for later replay

## Video
https://youtu.be/xoZWydwXgu4
