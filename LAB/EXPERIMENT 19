Program 19 – Python: handoff simulation with hysteresis
To simulate a mobile station that moves from one base station to another, to apply a handoff rule with 
hysteresis and to measure the number of handoffs, the ping-pong handoffs and the handoff point.


Code : 
#Program_19_Handoff_Simulation
import numpy as np
D, P0, n, SIGMA, V = 1000.0, -30.0, 3.5, 4.0, 10.0
def run(H, rng, step=0.5):
    serving = 0
    handoffs = 0
    pingpong = 0
    last_t = -99
    last_from = None
    first_pos = None
    for t in np.arange(0, D / V, step):
        x = V * t
        p = [
            P0 - 10*n*np.log10(max(abs(x - bx), 1))
            + rng.normal(0, SIGMA)
            for bx in (0, D)
        ]
        new = 1 - serving
        if p[new] > p[serving] + H:
            if last_from == new and t - last_t < 10:
                pingpong += 1
            handoffs += 1
            last_from, last_t, serving = serving, t, new
            if first_pos is None:
                first_pos = x
    return handoffs, pingpong, first_pos
rng = np.random.default_rng(1)
runs = 300
print(f"{'H (dB)':>7} {'handoffs':>9} {'ping-pong':>10} {'1st handoff at (m)':>19}")
for H in (0, 2, 4, 6, 8, 10):
    res = [run(H, rng) for _ in range(runs)]
    ho = np.mean([r[0] for r in res])
    pp = np.mean([r[1] for r in res])
    positions = [r[2] for r in res if r[2] is not None]
    pos = np.mean(positions)
    print(f"{H:7d} {ho:9.2f} {pp:10.2f} {pos:19.0f}")
import matplotlib.pyplot as plt
H_values = [0, 2, 4, 6, 8, 10]
handoff_values = []
pingpong_values = []
rng = np.random.default_rng(1)
for H in H_values:
    res = [run(H, rng) for _ in range(300)]
    handoff_values.append(np.mean([r[0] for r in res]))
    pingpong_values.append(np.mean([r[1] for r in res]))
plt.figure(figsize=(9, 5))
plt.plot(H_values, handoff_values, marker='o',
         label='Total handoffs')
plt.plot(H_values, pingpong_values, marker='s',
         label='Ping-pong handoffs')
plt.xlabel('Hysteresis margin (dB)')
plt.ylabel('Average number of handoffs')
plt.title('Effect of Hysteresis on Handoff Simulation')
plt.grid(True)
plt.legend()
plt.show()
