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
> + Improve memory access locality and minimize cache misses by reordering particles based on spatial locality.
> + Use performance analysis tools to monitor cache hit rates and overall execution time. Compare results with the baseline (non-optimized approach) to verify the impact of reordering.

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

**Implement load balancing for distributing computational work more evenly**

Proposed method attempts to maintain an even distribution of atoms across processors by adjusting the number of processors in each dimension. The goal is to reduce load imbalance when the number of atoms per processor deviates significantly from the ideal value. Load balancing occurs at regular intervals.

1. The function first calculates the number of atoms each processor should ideally have based on the total number of atoms and the number of processors.
2. Then it checks the imbalance of atoms assigned to the current processor. If the difference (imbalance) between the number of atoms assigned to this processor and the ideal number  exceeds a certain threshold, it tries to redistribute the atoms across processors. In case of significant imbalance, the topology is adjusted to ensure that atoms are more evenly distributed.

## Expected Results
1. Higher Cache Hit Rates: Reordering particle data based on Hilbert/Morton curves will improve memory locality, leading to significantly reduced cache misses.

2. Reduced Execution Time: Enhanced memory access patterns will optimize the performance of critical sections like compute_accel, resulting in faster force calculations.

3. Improved Scalability: The balance load function periodically checks for these imbalances and redistributes the atoms (or domain) to ensure that each process is responsible for approximately the same number of atoms. This reduces idle time and improves the efficiency of the overall simulation.

4. Reduced Simulation Time: Load balancing can adapt dynamically and can optimize resource utilization, leading to a reduced running time.

## Acknowledgments
This project is part of the CSCI596 course and focuses on enhancing the efficiency of molecular dynamics simulations through innovative computational methods.
