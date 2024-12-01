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
- Partition the 3D simulation space into smaller cells or grids, where each cell contains a certain number of particles based on spatial distribution.
- Generate Hilbert or Morton curve indices for each cell to map the 3D spatial structure into a 1D sequence while preserving spatial locality. Store cells in memory based on this 1D indexing to improve data access patterns.
- Process cells in the order defined by the Hilbert or Morton curve.
- Use performance analysis tools to monitor cache hit rates and overall execution time. Compare results with the baseline (non-optimized approach) to verify the impact of reordering.

## Previous work
#pmd.c
This program implements a parallel molecular dynamics simulation, utilizing the **MPI** standard. It provides a detailed record of the molecular dynamics simulation, including:

- Time steps (`stepCount * DeltaT`),
- System temperature (`temperature`),
- Per-atom kinetic energy, potential energy, and total energy (`kinEnergy`, `potEnergy`, `totEnergy`).

Additionally, the program reports the total runtime and communication time (`CPU & COMT`) for performance analysis.

Since this program serves as the foundation for parallel molecular dynamics simulations, all future enhancements will build upon its existing structure and functionality.

## Proposed Solutions
- Refactor the triple nested loops for better memory locality.
- Implement optimized communication protocols for parallel computation.
  
- Proposed method attempts to maintain an even distribution of atoms across processors by adjusting the number of processors in each dimension (represented by the vproc array). The goal is to reduce load imbalance when the number of atoms per processor deviates significantly from the ideal value.
  
     The function first calculates the number of atoms each processor should ideally have (atoms_per_proc) based on the total number of atoms (nglob) and the number of processors. Then it checks the imbalance of atoms assigned to the current processor. If the difference (imbalance) between the number of atoms assigned to this processor and the ideal number (atoms_per_proc) exceeds a certain threshold (THRESHOLD), it tries to redistribute the atoms across processors. In case of significant imbalance, the topology (i.e., the vproc array, which likely defines the grid of processors) is adjusted to ensure that atoms are more evenly distributed.


## Deliverables
- Optimized MD program addressing memory access inefficiencies.
- Reduced communication overhead with an improved parallel framework.
- Balanced load distribution across processing units to enhance performance. With load balancing, idle time should be reduced and efficiency of the overall simulation is improved, utilizing the CPU resources more evenly. It also allows the system to adapt to changes, such as varying numbers of atoms or computational challenges.



## Acknowledgments
This project is part of the CSCI596 course and focuses on enhancing the efficiency of molecular dynamics simulations through innovative computational methods.
