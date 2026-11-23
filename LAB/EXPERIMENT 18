Program 18 – Python: call blocking probability using the Erlang B formula
To compute the blocking probability of a cell with C channels for a given offered traffic, and to 
dimension the number of channels for a target grade of service.

Code : 
# Experiment 18: Call Blocking Probability using Erlang B
def erlang_b(E, C):
    """Blocking probability for offered traffic E and C channels."""
    b = 1.0
    for m in range(1, C + 1):
        b = E * b / (m + E * b)
    return b
def channels_needed(E, gos):
    C = 1
    while erlang_b(E, C) > gos:
        C += 1
    return C
def max_load(C, gos, hi=500.0):
    lo = 0.0
    for _ in range(60):
        mid = (lo + hi) / 2
        if erlang_b(mid, C) < gos:
            lo = mid
        else:
            hi = mid
    return lo
print("CALL BLOCKING PROBABILITY USING ERLANG B")
print("-----------------------------------------")
print("Blocking probability B(E,C)\n")
print(f"{'C':>5} {'E=2':>10} {'E=5':>10} "
      f"{'E=10':>10} {'E=20':>10}")
for C in (5, 10, 20, 30):
    values = [
        erlang_b(E, C)
        for E in (2, 5, 10, 20)
    ]
    print(f"{C:5d} " +
          " ".join(f"{v:10.4f}" for v in values))
users = 2000
calls_per_hour = 1.5
hold_s = 120
E = users * calls_per_hour * hold_s / 3600
print("\nOFFERED TRAFFIC CALCULATION")
print("---------------------------")
print(f"Number of users = {users}")
print(f"Calls per hour = {calls_per_hour}")
print(f"Average call duration = {hold_s} seconds")
print(f"Offered traffic = {E:.1f} Erlangs")
print("\nCHANNELS REQUIRED FOR DIFFERENT GOS")
for gos in (0.05, 0.02, 0.01):
    C = channels_needed(E, gos)
    print(f"GOS {gos:.0%}: Channels needed = {C}")
print("\nMAXIMUM TRAFFIC CALCULATION")
load = max_load(20, 0.02)
print("Channels = 20")
print("Target GOS = 2%")
print(f"Maximum load = {load:.2f} Erlangs")
import matplotlib.pyplot as plt
traffic = [x / 10 for x in range(1, 201)]
blocking = [erlang_b(E, 10) for E in traffic]
plt.figure(figsize=(9, 5))
plt.plot(traffic, blocking, label="10 Channels")
plt.xlabel("Offered Traffic (Erlangs)")
plt.ylabel("Blocking Probability")
plt.title("Erlang B: Blocking Probability vs Offered Traffic")
plt.grid(True)
plt.legend()
plt.show()
