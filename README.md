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
   - The current implementation employs a triple loop to iterate over each cell, with nested loops accessing neighboring cells to compute particle interactions.
   - Non-contiguous memory access leads to a high cache miss rate, slowing down overall performance.

2. **Communication Overhead in Parallel Computing**  
   - MD simulations divide the computational domain among multiple processors, requiring frequent information exchange between adjacent regions.
   - Non-optimized cell access order increases cross-node communication frequency, significantly impacting computation efficiency.

3. **Load Imbalance**  
   - Uneven particle distribution across processors leads to an imbalanced computational load.
   - Some processors handle significantly more calculations than others, reducing resource utilization and overall performance.

## Objectives
- Optimize memory access patterns to reduce cache miss rates.
- Minimize communication overhead by optimizing inter-node communication.
- Address load imbalance to ensure efficient resource utilization across processing units.

## Proposed Solutions
- Refactor the triple nested loops for better memory locality.
- Implement optimized communication protocols for parallel computation.
  
- Adaptive partitioning attempts to maintain an even distribution of atoms across processors by adjusting the number of processors in each dimension (represented by the vproc array). The goal is to reduce load imbalance when the number of atoms per processor deviates significantly from the ideal value.
     The function first calculates the number of atoms each processor should ideally have (atoms_per_proc) based on the total number of atoms (nglob) and the number of processors. Then it checks the imbalance of atoms assigned to the current processor. If the difference (imbalance) between the number of atoms assigned to this processor and the ideal number (atoms_per_proc) exceeds a certain threshold (THRESHOLD), it tries to redistribute the atoms across processors. In case of significant imbalance, the topology (i.e., the vproc array, which likely defines the grid of processors) is adjusted to ensure that atoms are more evenly distributed.

- 
## Deliverables
- Optimized MD program addressing memory access inefficiencies.
- Reduced communication overhead with an improved parallel framework.
- Balanced load distribution across processing units to enhance performance.



## Acknowledgments
This project is part of the CSCI596 course and focuses on enhancing the efficiency of molecular dynamics simulations through innovative computational methods.
