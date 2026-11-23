Program 21 – Python: effect of the OFDMA downlink scheduler on user throughput and fairness
To simulate the downlink of an OFDMA cell with round-robin, maximum C/I and proportional-fair 
scheduling, and to compare the user throughput, the total throughput and the fairness.


Code:
#Experiment - 21
import numpy as np
rng = np.random.default_rng(3)
U, RB, TTI = 5, 50, 3000
dist = np.array([100, 200, 300, 400, 500.0])
mean_snr_db = 40 - 35 * np.log10(dist / 100)
mean_snr = 10 ** (mean_snr_db / 10)
def rate(snr):
    return np.minimum(np.log2(1 + snr), 6.0) * 180.0
def simulate(sched):
    thr = np.zeros(U)
    avg = np.ones(U)
    for t in range(TTI):
        fade = rng.exponential(1.0, (U, RB))
        r = rate(mean_snr[:, None] * fade)
        if sched == "RR":
            owner = (np.arange(RB) + t) % U
        elif sched == "MAXCI":
            owner = r.argmax(axis=0)
        else:
            owner = (r / avg[:, None]).argmax(axis=0)
        got = np.zeros(U)
        for b in range(RB):
            got[owner[b]] += r[owner[b], b]
        avg = 0.99 * avg + 0.01 * got
        thr += got
    return thr / TTI / 1000 
def jain(x):
    return x.sum() ** 2 / (len(x) * (x ** 2).sum())
print(f"{'Sched':6}" +
      "".join(f"{'UE' + str(i+1):>8}" for i in range(U)) +
      f"{'Total':>9}{'Jain':>8}")
for s in ("RR", "MAXCI", "PF"):
    x = simulate(s)
    print(f"{s:6}" +
          "".join(f"{v:8.2f}" for v in x) +
          f"{x.sum():9.2f}{jain(x):8.3f}")
