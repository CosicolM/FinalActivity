# FinalActivity
Case 1 Source Code

import matplotlib.pyplot as plt

# Given data
years = [2020, 2021, 2022, 2023, 2024]
population = [10000, 10800, 11900, 13200, 14800]

# Compute growth rates using central difference
growth_years = [2021, 2022, 2023]
growth_rates = []

for i in range(1, len(population) - 1):
    rate = (population[i + 1] - population[i - 1]) / 2
    growth_rates.append(rate)

# -------------------------------
# Graph 1: Population vs Time
# -------------------------------
plt.figure()
plt.plot(years, population, marker='o')
plt.title("Population vs Time")
plt.xlabel("Year")
plt.ylabel("Population")
plt.grid()

plt.show()

# -------------------------------
# Graph 2: Growth Rate vs Time
# -------------------------------
plt.figure()
plt.plot(growth_years, growth_rates, marker='o')
plt.title("Growth Rate vs Time")
plt.xlabel("Year")
plt.ylabel("Growth Rate (people/year)")
plt.grid()

plt.show()

Graph 1
<img width="1057" height="647" alt="image" src="https://github.com/user-attachments/assets/62f5fcdd-9d7a-403f-a397-d0b56d463bac" />

Graph 2
<img width="959" height="720" alt="image" src="https://github.com/user-attachments/assets/0647f351-8d64-43d7-b21d-cb3583a56af0" />



Case 2 Source

import numpy as np
import matplotlib.pyplot as plt

# Given data
t = np.array([0,1,2,3,4,5], dtype=float)
x = np.array([0,5,15,30,50,75], dtype=float)

# Central difference velocities (only valid points)
t_v = t[1:-1]  # [1,2,3,4]
v = []

for i in range(1, len(t)-1):
    vel = (x[i+1] - x[i-1]) / (t[i+1] - t[i-1])
    v.append(vel)

v = np.array(v)

# Display results
print("Velocity (Central Difference):")
for i in range(len(t_v)):
    print(f"t={t_v[i]:.0f} s -> v={v[i]:.2f} m/s")


# --- Acceleration (central difference on velocity) ---
t_a = t_v[1:-1]   # [2,3]
a = []

for i in range(1, len(v)-1):
    acc = (v[i+1] - v[i-1]) / (t_v[i+1] - t_v[i-1])
    a.append(acc)

a = np.array(a)
print("----------------------------------------------------")

print("\nAcceleration:")
for i in range(len(t_a)):
    print(f"t={t_a[i]:.0f} s -> a={a[i]:.2f} m/s^2")

    
# --- Add endpoint velocities ---
v0 = (x[1] - x[0]) / (t[1] - t[0])        # forward difference at t=0
v5 = (x[-1] - x[-2]) / (t[-1] - t[-2])    # backward difference at t=5

# Combine full velocity list
v_full = np.concatenate(([v0], v, [v5]))
t_full = t  # now matches 0 to 5

# --- Trapezoidal Rule (correct full interval) ---
distance_est = 0

for i in range(len(t_full)-1):
    dt = t_full[i+1] - t_full[i]
    distance_est += (v_full[i] + v_full[i+1]) / 2 * dt

actual_disp = x[-1] - x[0]
print("----------------------------------------------------")
print(f"\nEstimated distance (from velocity): {distance_est:.2f} m")
print(f"Actual displacement: {actual_disp:.2f} m")

# --- Plot Position ---
plt.figure()
plt.plot(t, x, marker='o')
plt.title("Position vs Time")
plt.xlabel("Time (s)")
plt.ylabel("Position (m)")
plt.grid()
plt.show()

# --- Plot Velocity (ONLY valid points) ---
plt.figure()
plt.plot(t_v, v, marker='o', color='orange')
plt.title("Velocity vs Time (Central Difference)")
plt.xlabel("Time (s)")
plt.ylabel("Velocity (m/s)")
plt.grid()
plt.show()

Graph 1 and 2
<img width="1920" height="1200" alt="Screenshot 2026-04-17 212255" src="https://github.com/user-attachments/assets/b929a73c-cbc0-41e8-8b7a-c8408dbe0530" />

Case 3 Source Code

import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation

plt.style.use("dark_background")

# Data
x = np.array([0, 2, 4, 6, 8, 10])
T = np.array([100, 80, 65, 55, 48, 45])

h = 2

# Gradient (central difference)
x_grad = x[1:-1]
gradient = (T[2:] - T[:-2]) / (2*h)

# Simpson's Rule
simpson = (h/3) * (
    T[0] + T[-1]
    + 4*(T[1] + T[3])
    + 2*(T[2] + T[4])
)

print("Integrated Heat (Simpson's Rule):", round(simpson,2))

# Create 2 graphs in one window
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12,5))

# Temperature graph
ax1.set_title("Temperature vs Position")
ax1.set_xlabel("Position (cm)")
ax1.set_ylabel("Temperature (°C)")
ax1.set_xlim(0, 10)
ax1.set_ylim(40, 105)
line1, = ax1.plot([], [], marker='o')

# Gradient graph
ax2.set_title("Gradient vs Position")
ax2.set_xlabel("Position (cm)")
ax2.set_ylabel("dT/dx")
ax2.set_xlim(2, 8)
ax2.set_ylim(-9.5, -2)
line2, = ax2.plot([], [], marker='o')

def animate(i):
    # temperature animation
    line1.set_data(x[:i], T[:i])
    
    # gradient animation
    if i > 1:
        line2.set_data(x_grad[:i-1], gradient[:i-1])
    
    return line1, line2

ani = FuncAnimation(
    fig,
    animate,
    frames=len(x)+1,
    interval=700,
    repeat=False
)

plt.tight_layout()
plt.show()

Graph 1 and 2
<img width="1522" height="666" alt="image" src="https://github.com/user-attachments/assets/e00af082-4a64-4b47-9fd4-7203c53d277a" />

