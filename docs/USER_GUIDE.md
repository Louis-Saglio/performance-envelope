# User guide

[Back to README](../README.md)

## What the skill does

It turns a performance question into a lower bound. It counts the work a task must do, finds the one hardware resource that limits it, and divides. The result is the fastest that task could run, not a prediction of what it will run at.

Two floors come out of the analysis. The floor of the current formulation says whether the code is slow. The floor of the task says whether the design asks for too much work.

## When to use it

- You measured something and want to know whether it is worth optimizing.
- You are choosing between two designs and want the cost of each.
- A profiler points at a function and you want to know if the cost is fundamental.
- You need a number before writing code, not after.

## What you get

| item | value |
|---|---|
| work per unit | ... |
| binding resource | ... |
| floor | ... |
| observed | ... |
| ratio | ... |

Then a verdict and the single biggest lever. If the lever is a different formulation or a different design, the answer says so.

## Reading the result

- Within 2-3x of the floor: the code is doing what the hardware allows. Optimizing further is not worth it.
- 10x or more: the waste is somewhere. The phase breakdown says where.
- The lever is in the design: no amount of loop tuning will move the floor.

## Example

A simulation computes 999,000 pairs per update, each with one sqrt and two divides, on 8 cores at 4.5 GHz. The floor is 222 us per update. The measurement is 265 us. The code is at the floor. The remaining 5-8x needs a different formulation: symmetry halves the pairs, and an rsqrt removes the divides.

## Troubleshooting

**The answer is Big-O.** Ask for exact counts of operations and bytes. The constants are the point.

**The agent asks for the machine.** Give it cores, clock, ISA and bandwidth, or let it measure them.

**The estimate disagrees with the profiler.** Compare phase by phase, not end to end. A small phase can be the one far from its floor.
