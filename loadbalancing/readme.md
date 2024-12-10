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


    
