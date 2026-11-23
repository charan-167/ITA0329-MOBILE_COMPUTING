#Program 20 – Python: Code Division Multiple Access (CDMA): spreading and despreading
#To implement direct-sequence spreading and despreading with orthogonal Walsh codes, to show that 
#several users can share the same channel at the same time, and to study the effect of noise.


#Code :
#Program_20_CDMA_Simulation
import numpy as np
def walsh(N):
    H = np.array([[1]])
    while H.shape[0] < N:
        H = np.block([
            [H, H],
            [H, -H]
        ])
    return H
N = 8                
nbits = 6             
num_users = 3
rng = np.random.default_rng(1)
codes = walsh(N)[:num_users]
print("Walsh codes (chips):")
print(codes)
print("\nCode correlation matrix / N:")
print(codes @ codes.T / N)
bits = rng.choice([-1, 1], size=(num_users, nbits))
spread = np.zeros((num_users, nbits, N))
for user in range(num_users):
    spread[user] = bits[user, :, None] * codes[user]
channel = np.sum(spread, axis=0).flatten()
def despread(signal, code):
    received = signal.reshape(nbits, N)
    values = received @ code / N
    return np.where(values >= 0, 1, -1)
for sigma in (0.0, 1.0, 2.0):
    noise = rng.normal(0, sigma, size=channel.shape)
    received_signal = channel + noise
    errors = 0
    for user in range(num_users):
        estimated = despread(received_signal, codes[user])
        errors += np.sum(estimated != bits[user])
    print(
        f"noise sigma = {sigma:.1f}: "
        f"bit errors = {errors} of {num_users * nbits}"
    )
noise = rng.normal(0, 2.0, size=channel.shape)
received_signal = channel + noise
est = despread(received_signal, codes[0])
print("\nSent     user 1:", bits[0])
print("Received user 1:", est, "(last run, sigma = 2.0)")
rx0 = channel.reshape(nbits, N)
wrong_code_result = np.sign(rx0 @ walsh(N)[7] / N).astype(int)
print(
    "Wrong code (user 4 = Walsh row 7):",
    wrong_code_result
)
