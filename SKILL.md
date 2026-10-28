---
name: performance-envelope
description: Derive the theoretical speed of a program from the work it must do and the hardware limit it hits, then compare it with the measurement. Use when asked for a back-of-the-envelope calculation, the theoretical or maximum speed, how fast something should run, its performance floor or speed of light, whether it is fast or slow, or how close it is to optimal.
---

# Performance envelope

Casey Muratori's back-of-the-envelope calculation: find the lower bound on time from the work and the binding resource, then compare it with what the code actually does.

## Method

1. Pick the unit: one frame, request, update, file. Count the irreducible work exactly: operations, bytes touched, dependent loads. Not Big-O. The constants are the point.
2. Identify the binding resource. Only one sets the floor:
   - compute: FMA or divider/sqrt throughput
   - memory: bytes divided by bandwidth
   - latency: dependent loads times memory latency
   - I/O: bytes divided by device bandwidth
   - serial chain: instructions that cannot overlap
3. Write down the machine: cores, clock, ISA, FLOP per cycle, bandwidth. If you do not know it, measure it or state the assumption and give the floor per core-GHz so it scales.
4. floor = work divided by peak throughput. This is a lower bound on time, not a prediction.
5. Measure the same unit with component timings, not just a total. Compare like with like.
6. ratio = observed divided by floor. Within 2-3x means it is at the floor. 10x or more means find the waste.

## Rules

- Derive two floors: the floor of the current formulation, and the floor of the task. The first says whether the code is slow. The second says whether the design is.
- Check that the binding resource is fundamental. sqrt plus two divides can become one rsqrt plus Newton on the FMA units. A full repaint can become a dirty delta. Same task, different floor.
- Count all the bytes: clear, copy, intermediate buffers. Useful output is not the traffic.
- Bandwidth is not latency. A pointer chase costs latency per link, no matter how much bandwidth is available.
- Peak is not achievable. memset reaches 60-70% of DRAM peak, divider throughput differs by microarchitecture, and the compiler target changes the instruction mix. Check the hot loop in the disassembly when it matters.
- Parallel speedup is not linear. Measure with the real thread count and report the range over several runs.
- Do not let the end-to-end number hide the phase breakdown. A 3% phase can be the one at 20x its floor.

## Output

State the assumptions and the machine, then:

| item | value |
|---|---|
| work per unit | ... |
| binding resource | ... |
| floor | ... |
| observed | ... |
| ratio | ... |

End with a verdict and the single biggest lever. If the lever is a different formulation or a different design, say so. Do not offer a list of micro-optimizations that cannot move the floor.

## Example

1e6 pairs, each with one sqrt and two divides, 8 cores at 4.5 GHz, 4 cycles per divider instruction, two divides packed into one instruction. Floor = 1e6 x 8 cycles / 8 cores / 4.5e9 Hz = 222 us. Measured 265 us. At the floor. The 5-8x left is not in the loop: halve the pairs with symmetry, replace the divides with an rsqrt, and the floor moves to ~30-50 us.
