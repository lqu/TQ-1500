# Design
There are 76 LEDs (4 x 19) around the board, one LED at either end of each line. To highlight an intersection, 4 LEDs will flash. Top and right sides LEDs will be illustrated here. The other half is symmetric and shares the same signal and circuit.

There are effectively 38 distinct LEDs, so 6 bits (2 ^ 6 = 64) should be enough to control them. There are 6 GPIO lines from the processor, 3 for the group and 3 for the index inside the group. A BCD to decimal decoder/demultiplexer will decode the 'group' and 'index' respectively and combine them to select a specific line. Decoded 'group' will drive the anodes of the LEDs, active HIGH; decoded 'index' will drive the cathodes of the LEDs, active LOW.

All GPIO lines are pull-high 111111. Group 7 doesn't exist. So nothing is on by default.

<pre>
      X
  ┌───────►
  │
Y │
  │
  ▼     │        Group 0                │        Group 1               │ Group 2
        └───────────────────────────    └───────────────────────────   └────────
        0   1   2   3   4   5   6   7   0   1   2   3   4   5   6   7   0   1   2
        ┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐  0    │
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │       │
        ├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤  1    │
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │       │
        ├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤  2    │
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │       │
        ├───┼───┼───O───┼───┼───┼───┼───┼───O───┼───┼───┼───┼───┼───$───┼───┼───┤  3    │ Group 3
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │       │
        ├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤  4    │
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │       │
        ├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤  5    │
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │       │
        ├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤  6    │
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │       │
        ├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤  7 ───┘
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │
        ├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤  0    │
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │       │
        ├───┼───┼───O───┼───┼───┼───┼───┼───O───┼───┼───┼───┼───┼───O───┼───┼───┤  1    │
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │       │
        ├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤  2    │
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │       │
        ├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤  3    │ Group 4
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │       │
        ├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤  4    │
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │       │
        ├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤  5    │
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │       │
        ├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤  6    │
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │       │
        ├───┼───┼───O───┼───┼───┼───┼───┼───O───┼───┼───┼───┼───┼───O───┼───┼───┤  7 ───┘
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │
        ├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───¥───┼───┼───┼───┤  0    │
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │       │ Group 5
        ├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤  1    │
        │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │   │       │
        └───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘  2 ───┘

</pre>

# Example 1
To highlight the top right STAR position (X=16, Y=4) --'$' above -- we need to turn on LED #7 in Group 1 and LED #3 in Group 3. The GPIO lines will be 001111 for LED-1-7; and 011011 for LED-3-3. Only one LED can be turned on this way. The two LEDs for X and Y dimensions must time-share. Signal alternates every 8 ms.

# Example 2 
For position (X=15, Y=17) -- '¥' above -- it's LED-1-6 and LED-5-0. Control will be 001110 and 101000.

<img src=scope-15-17.png width="50%">
