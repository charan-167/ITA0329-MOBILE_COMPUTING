Program 16 – Python: free-space path loss and received power
To compute the free-space path loss and the received power of a radio link as a function of distance and 
frequency, and to find the maximum range for a given receiver sensitivity.

#Experiment - 16
#fspl.py
import math
# Function to calculate Free Space Path Loss
def fspl_db(d_km, f_mhz):
    return 20 * math.log10(d_km) + 20 * math.log10(f_mhz) + 32.44
# Function to calculate received power
def rx_power_dbm(pt_dbm, gt_db, gr_db, d_km, f_mhz):
    return pt_dbm + gt_db + gr_db - fspl_db(d_km, f_mhz)
# Input parameters
f = 2400.0       # Frequency in MHz
pt = 20.0        # Transmitter power in dBm
gt = 2.0         # Transmitter antenna gain in dBi
gr = 2.0         # Receiver antenna gain in dBi
sens = -90.0     # Receiver sensitivity in dBm
print("Free Space Path Loss and Received Power")
print("---------------------------------------")
print(f"Frequency = {f} MHz")
print(f"Transmitter Power = {pt} dBm")
print(f"Receiver Sensitivity = {sens} dBm")
# Calculate received power for different distances
distances = (0.01, 0.05, 0.1, 0.5, 1, 2, 5, 10)
print()
print(f"{'Distance':>10} {'FSPL':>12} {'Pr (dBm)':>12} {'Pr (W)':>14} {'Link':>8}")
for d in distances:
    loss = fspl_db(d, f)
    pr = rx_power_dbm(pt, gt, gr, d, f)
    watts = 10 ** ((pr - 30) / 10)
    status = "OK" if pr >= sens else "FAIL"
    print(f"{d:10.2f} {loss:12.2f} {pr:12.2f} "
          f"{watts:14.3e} {status:>8}")
# Calculate maximum communication range
allowed_loss = pt + gt + gr - sens
d_max = 10 ** (
    (allowed_loss - 32.44 - 20 * math.log10(f)) / 20
)
print()
print("Maximum Range Calculation")
print("-------------------------")
print(f"Allowed path loss = {allowed_loss:.2f} dB")
print(f"Maximum range = {d_max:.2f} km")
# Verify the effect of doubling distance
difference = fspl_db(2, f) - fspl_db(1, f)
print(f"Loss increase when distance doubles = {difference:.2f} dB")
# Compare different carrier frequencies at 1 km
print()
print("Effect of Carrier Frequency at 1 km")
print("-----------------------------------")
for fm in (900, 1800, 2400, 3500):
    loss = fspl_db(1, fm)
    pr = rx_power_dbm(pt, gt, gr, 1, fm)
    print(f"{fm:5d} MHz : FSPL = {loss:7.2f} dB, "
          f"Received Power = {pr:7.2f} dBm")
