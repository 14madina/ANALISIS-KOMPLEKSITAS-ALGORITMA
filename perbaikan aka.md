import matplotlib.pyplot as plt
import pandas as pd

# Data
data = {
    "Metode": ["FIFO", "Manual (tanpa prioritas)", "Sistem Prioritas"],
    "Makespan (menit)": [450, 420, 400],
    "Kepatuhan Deadline (%)": [70, 60, 90],
    "Efisiensi Proses (%)": [80, 75, 90],
}

# DataFrame
df = pd.DataFrame(data)

# Fungsi rekursif untuk menghitung total skor dari kolom metrik
def calculate_total_recursive(data_list, index=0, total=0):
    if index == len(data_list):  # Kondisi penghentian
        return total
    return calculate_total_recursive(data_list, index + 1, total + data_list[index])

# Fungsi iteratif untuk menghitung total skor dari kolom metrik
def calculate_total_iterative(data_list):
    total = 0
    for value in data_list:
        total += value
    return total

# Hitung total skor untuk setiap metode menggunakan rekursif
df["Total Skor (Rekursif)"] = df.apply(
    lambda row: calculate_total_recursive(
        [row["Makespan (menit)"], row["Kepatuhan Deadline (%)"], row["Efisiensi Proses (%)"]]
    ),
    axis=1,
)

# Hitung total skor untuk setiap metode menggunakan iteratif
df["Total Skor (Iteratif)"] = df.apply(
    lambda row: calculate_total_iterative(
        [row["Makespan (menit)"], row["Kepatuhan Deadline (%)"], row["Efisiensi Proses (%)"]]
    ),
    axis=1,
)

# Visualisasi
plt.figure(figsize=(10, 6))
plt.plot(df["Metode"], df["Makespan (menit)"], marker="o", label="Makespan (menit)")
plt.plot(df["Metode"], df["Kepatuhan Deadline (%)"], marker="o", label="Kepatuhan Deadline (%)")
plt.plot(df["Metode"], df["Efisiensi Proses (%)"], marker="o", label="Efisiensi Proses (%)")
plt.plot(df["Metode"], df["Total Skor (Rekursif)"], marker="o", linestyle="--", label="Total Skor (Rekursif)")
plt.plot(df["Metode"], df["Total Skor (Iteratif)"], marker="s", linestyle="--", label="Total Skor (Iteratif)")

plt.title("Perbandingan Kinerja Sistem Prioritas Pengantaran Laundry", fontsize=14)
plt.xlabel("Metode", fontsize=12)
plt.ylabel("Nilai", fontsize=12)
plt.grid(True, linestyle="--", alpha=0.6)
plt.legend()
plt.tight_layout()

plt.show()

# Tampilkan DataFrame
print(df)
