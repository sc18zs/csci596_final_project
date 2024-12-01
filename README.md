# CSCI596 Final Project: Methods to Enhance the Parallel Molecular Dynamics (MD) Program

## Group Members
- Zihan Shen
- Yuzhu Tang
- Sichen Zhang
- Zhengtao Yao

## Problem Statement
Molecular Dynamics (MD) simulations are computationally intensive, requiring efficient strategies to address challenges related to memory access, parallel communication, and processing power. This project aims to identify and implement methods to enhance the performance of MD simulations.

### Key Challenges
1. **Inefficient Memory Access Due to Triple Nested Loops**
   - The current implementation uses a triple loop to traverse each cell and accesses neighboring cells via nested loops to compute particle interactions.
   - When the processor processes cells, memory access is often non-contiguous, and the order of accessing different cells may span large memory regions, increasing cache miss rates.

3. **Communication Overhead in Parallel Computing**  
   - MD simulations divide the computational domain among multiple processors, requiring frequent information exchange between adjacent regions.
   - Non-optimized cell access order increases cross-node communication frequency, significantly impacting computation efficiency.

4. **Load Imbalance**  
   - Uneven particle distribution across processors leads to an imbalanced computational load.
   - Some processors handle significantly more calculations than others, reducing resource utilization and overall performance.

## Objectives
1. Use Hilbert and Morton curves to improve cache hit rates, reduce memory access overhead, and enhance computational efficiency. 

Step-by-Step Plan for Optimizing MD Simulation Using Hilbert or Morton Curves：
- Improve memory access locality and minimize cache misses by reordering particles based on spatial locality.
- Use performance analysis tools to monitor cache hit rates and overall execution time. Compare results with the baseline (non-optimized approach) to verify the impact of reordering.

## Previous work
### pmd.c

This program implements a parallel molecular dynamics simulation, utilizing the **MPI** standard. It provides a detailed record of the molecular dynamics simulation, including:

- Time steps (`stepCount * DeltaT`),
- System temperature (`temperature`),
- Per-atom kinetic energy, potential energy, and total energy (`kinEnergy`, `potEnergy`, `totEnergy`).

Additionally, the program reports the total runtime and communication time (`CPU & COMT`) for performance analysis.

Since this program serves as the foundation for parallel molecular dynamics simulations, all future enhancements will build upon its existing structure and functionality.

## Proposed Solutions
**Optimization Plan for the Provided Code Using Hilbert and Morton Curves**
1. Implementing Spatial Mapping with Hilbert/Morton Curves
- Assign a **Hilbert** or **Morton** index to each cell to map the 3D space into a 1D sequence.
- Before the force calculation (`compute_accel`), reorder particles in memory based on their cell's curve index.
- Traverse cells in the order defined by the Hilbert or Morton curve. Access neighboring cells in this order for force calculations to leverage the reordered memory layout.

2. Profiling and Performance Metrics
- Compare the execution time, communication cost, and scalability (e.g., strong and weak scaling) of the optimized code with the original version.


## Expected Results
1. Higher Cache Hit Rates: Reordering particle data based on Hilbert/Morton curves will improve memory locality, leading to significantly reduced cache misses.

2. Reduced Execution Time: Enhanced memory access patterns will optimize the performance of critical sections like compute_accel, resulting in faster force calculations.


## Acknowledgments
This project is part of the CSCI596 course and focuses on enhancing the efficiency of molecular dynamics simulations through innovative computational methods.
