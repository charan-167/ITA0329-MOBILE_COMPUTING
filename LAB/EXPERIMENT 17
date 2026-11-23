Program 17 – Python: frequency (co-channel) reuse, cluster size, capacity and S/I
To compute the cluster size, the channels per cell, the system capacity and the signal-to-interference ratio 
of a hexagonal cellular layout, and to find the smallest cluster size that meets a given S/I requirement.

Code:
#Experiment_17_Frequency_Reuse.ipynb
import math
# Function to generate valid cluster sizes
def valid_cluster_sizes(limit=20):
    """N = i^2 + i*j + j^2"""
    return sorted({
        i*i + i*j + j*j
        for i in range(5)
        for j in range(5)
    } - {0})[:limit]
# Function to calculate frequency reuse metrics
def reuse_metrics(N, S=336, M=21, n=4.0):
    k = S // N
    q = math.sqrt(3 * N)
    si_db = 10 * math.log10(q ** n / 6)
    return k, M * k, q, si_db
# Given parameters
S = 336
M = 21
n = 4.0
req = 18.0
print("FREQUENCY REUSE AND CELLULAR CAPACITY")
print("--------------------------------------")
print(f"Total channels = {S}")
print(f"Number of cells = {M}")
print(f"Path loss exponent = {n}")
print(f"Required S/I = {req} dB")
print("\nCluster Size Analysis")
print(f"{'N':>3} {'D/R':>6} {'ch/cell':>8} "
      f"{'capacity':>9} {'S/I(dB)':>9} {'Meets?':>10}")
best = None
for N in valid_cluster_sizes(10):
    k, C, q, si = reuse_metrics(N, S, M, n)
    ok = si >= req
    if ok and best is None:
        best = N
    print(f"{N:3d} {q:6.2f} {k:8d} "
          f"{C:9d} {si:9.2f} {'yes' if ok else 'no':>10}")
print("\nSmallest cluster size meeting S/I requirement:")
print("N =", best)
