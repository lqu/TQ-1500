# Design
At first glance one would think the stone sensing mechanism uses interrupts like the modern touchscreen. This might be true since there are co-processors dedicated to this purpose, and they can pass the result to the main processor. But on the low level, TQ-1500 treats it as memory, like the magnetic-core memory. It continuously polls each of the 361 intersections to see if a stone is newly placed. It captures the state change (placed or removed), but doesn't keep the current status (occupied or empty).

There are 19 vertical wires and 19 horizontal wires underneath the board surface. A vertial wire is 5V when selected, horizontal GROUND. When 2 wires meet at each intersection, they make coils on a shared cylinder. The crossing wires are not connected, but when in a magnetic field, the coils act like a transformer and there will be a glitch/runt in the voltage to be captured.

The stones have magnetic parts built in. When placed or removed, the magnetic field changes, which contributes to the glitch/runt.

Logically, the grid is scanned in a 19x19 nested loop continuously. Physically, shift registers are used to select the next wire. Every step takes 400 us, so every line on the board takes about 8 ms. To scan the whole board it's about 160ms, or 6 Hz. It is fast enough to capture human hand movements.

When the processor detects a glitch/runt in the voltage, it knows which vertical and horizontal wires are currently selected, which tells where the stone is placed. On an oscilloscope, we measure the difference between the start of the nested loop and the time the glitch/runt happens to calculate where the stone is placed. So the problem of WHERE is solved by knowing WHEN.
