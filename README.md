# Computation-Science-Ass---Yguinto-Anjuvh-Baldwin

<img width="1260" height="723" alt="image" src="https://github.com/user-attachments/assets/876056de-a89c-4827-a6ea-7fac92b55750" />

<img width="1599" height="559" alt="image" src="https://github.com/user-attachments/assets/8e08dac7-2074-4085-9372-fc6349c92479" />

<img width="715" height="863" alt="image" src="https://github.com/user-attachments/assets/70aba082-e74b-4168-8036-c3e91750b592" />


Pure Code: 
import decimal
import matplotlib.pyplot as plt

decimal.getcontext().prec = 150
PI_STRING = "3.14159265358979323846264338327950288419716939937510582097494459230781640628620899862803482534211706798214"
true_pi = decimal.Decimal(PI_STRING)


radius = decimal.Decimal("10")
height = decimal.Decimal("20")


def get_cylinder_volume(pi_val):
    return pi_val * (radius ** 2) * height


true_volume = get_cylinder_volume(true_pi)

# PART A: BASELINE (3.1415 vs 3.1416 vs True)
print(f"{'=' * 60}")
print(f"PART A: BASELINE BATTLE (Actual Volumes)")
print(f"{'=' * 60}")

pi_t = decimal.Decimal("3.1415")
pi_r = decimal.Decimal("3.1416")

vol_t = get_cylinder_volume(pi_t)
vol_r = get_cylinder_volume(pi_r)


print(f"1. Truncated (3.1415): {vol_t:.5f}")
print(f"2. TRUE VOLUME       : {true_volume:.5f}")
print(f"3. Rounded   (3.1416): {vol_r:.5f}")

diff_t = abs(true_volume - vol_t)
diff_r = abs(true_volume - vol_r)
print(f"\nDifferences:")
print(f"Trunc is off by: {diff_t:.5f}")
print(f"Round is off by: {diff_r:.5f}")

# PART B: DEEP DIVE

print(f"\n{'=' * 60}")
print(f"PART B: DEEP DIVE (True vs Methods)")
print(f"{'=' * 60}")

decimals_to_test = [20, 40, 60, 100]
trunc_errors = []
round_errors = []

for d in decimals_to_test:
    print(f"\n--- TESTING AT {d} DECIMAL PLACES ---")

    # Create Numbers
    pi_t_val = decimal.Decimal(PI_STRING[:2 + d])
    pi_r_val = decimal.Decimal(PI_STRING).quantize(
        decimal.Decimal("1." + "0" * d),
        rounding=decimal.ROUND_HALF_UP
    )

    # Calculate Volumes
    v_t = get_cylinder_volume(pi_t_val)
    v_r = get_cylinder_volume(pi_r_val)

    # Print Comparison
    print(f"True Volume:  {true_volume}")
    print(f"Trunc Volume: {v_t}")
    print(f"Round Volume: {v_r}")

    # Store Errors
    trunc_errors.append(float(abs(true_volume - v_t)))
    round_errors.append(float(abs(true_volume - v_r)))

    if v_t == v_r:
        print(">>> RESULT: Identical.")
    else:
        print(">>> RESULT: DIFFERENT.")

# VISUALIZATION (UPDATED)

fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(10, 12))
plt.subplots_adjust(hspace=0.4)

# --- GRAPH 1: PART A (ZOOMED IN VOLUME COMPARISON)
offset = 6283
vals_A = [float(vol_t) - offset, float(true_volume) - offset, float(vol_r) - offset]
labels_A = ['Truncated', 'TRUE VOLUME', 'Rounded']
colors_A = ['#ff9999', '#44bd32', '#66b3ff']  # Red, Green (True), Blue

bars = ax1.bar(labels_A, vals_A, color=colors_A)

# Add a reference line for True Volume
ax1.axhline(y=float(true_volume) - offset, color='green', linestyle='--', alpha=0.5)

ax1.set_title("PART A: Zoomed-In Volume Comparison\n(Y-Axis starts at 6283.0)")
ax1.set_ylabel("Decimal Portion of Volume")

for bar, val, full_val in zip(bars, vals_A, [vol_t, true_volume, vol_r]):
    height = bar.get_height()
    ax1.text(bar.get_x() + bar.get_width() / 2, height,
             f"{full_val:.4f}", ha='center', va='bottom', fontweight='bold')

# --- GRAPH 2: PART B (ACCURACY TREND) ---
ax2.semilogy(decimals_to_test, trunc_errors, 'r-o', label='Truncation Difference', linewidth=2)
ax2.semilogy(decimals_to_test, round_errors, 'b--s', label='Rounding Difference', linewidth=2)

# Add "True Volume"
ax2.axhline(y=1e-100, color='green', linestyle='--', alpha=0.5, label='True Volume (Zero Error)')
ax2.text(60, 1e-95, "True Volume (Goal)", color='green', fontweight='bold')

ax2.set_title("PART B: Difference from True Volume (Log Scale)")
ax2.set_xlabel("Decimal Places Used")
ax2.set_ylabel("Difference Magnitude")
ax2.grid(True, which="both", alpha=0.3)
ax2.legend()

plt.show()
