**Overview**

This report analyzes the implementation of dynamic load balancing functionality in a parallel molecular dynamics simulation program using MPI (Message Passing Interface). The original code was enhanced to handle uneven particle distributions across processes.

**Key Changes Overview:**

- Added dynamic load balancing
- Modified process initialization
- Enhanced debugging/monitoring
- Added new configuration parameters
- Modified single_step() structure


**Creating manually a imbalance senario.**

    if (sid == 1) {
        // Process 1 keeps only 40% of its atoms
        n = (int)(n * 0.4);
        printf("Process 1: Reduced to %d atoms\n", n);
    } else if (sid == 0) {
        // Process 0 keeps its atoms plus copies some
        for (j = 0; j < original_n * 0.6; j++) {  // Add 60% more
            for (a = 0; a < 3; a++) {
                r[n][a] = r[j][a];  // Copy positions from existing atoms
            }
            n++;
        }
        printf("Process 0: Increased to %d atoms\n", n);
    }

    
**Modified Single Step**


**Load Balancing Function**

Key Features of the Load Balancing:
    
    Periodic checks: Runs every load_balance_interval steps
    Threshold-based: Only triggers if imbalance exceeds threshold
    Boundary-aware: Transfers atoms near process boundaries
    Limited transfers: Maximum 10 atoms per step to avoid instability is used in the output in the folder. Large values lead to instability and crash.
    Neighbor-only: Transfers only between adjacent processes
    Preserves physics: Maintains periodic boundary conditions during transfers


    


**=== Load Balance Check at Step 100 ===**
Initial atom distribution:
Process 0: 11985 atoms
Process 1: 10705 atoms
Process 2: 27931 atoms
Process 3: 26793 atoms
load threshold is:1.050000
Load imbalance metrics:
Max atoms: 27931, Min atoms: 10705
Current imbalance ratio: 2.609155

Final atom distribution:
Process 0: 11995 atoms
Process 1: 10705 atoms
Process 2: 27921 atoms
Process 3: 26793 atoms


**Improvment - numerical overflow**

Extremely large forces happen and indicate a serious issue with particle positions - likely atoms are far too close to each other, causing numerical overflow in the Lennard-Jones force calculation. We could add a minimum distance check and position validation as will be shown in the code. However, this is very computational expensive and not able to get results for serval results. For each atom, we must calculate forces with all nearby atoms. This involves checking atoms in 27 neighboring cells (3×3×3 region)
Complexity is roughly O(N²) in the worst case for N atoms in neighboring cells.

Square root calculations
Division operations
Multiple floating-point multiplications
Key Changes:
    In compute_accel():
        
        Added minimum separation check (rMin2)
        Added force capping (maxForce)
        Improved cell index calculation with bounds checking
        Added periodic boundary enforcement
    

    In half_kick():

        Added velocity capping (vmax)
        Added warning messages for capped velocities


    In atom_move():

        Added position and velocity validation
        Added checks for NaN and infinity
        Improved periodic boundary enforcement


    In eval_props():

        Added energy validation
        Added error handling for unreasonable energy values
        



