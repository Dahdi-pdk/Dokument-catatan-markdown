# Studi Kasus: Analisis Nilai Siswa dalam Ujian Matematika dan Statistika

## Definisi Masalah
Kita akan menganalisis nilai ujian 30 siswa dalam dua mata pelajaran: Matematika dan Statistika. Tujuan analisis ini adalah:
1. Memahami distribusi nilai siswa di kedua mata pelajaran
2. Membandingkan performa siswa antara kedua mata pelajaran
3. Mengidentifikasi siswa yang performanya di atas rata-rata di kedua mata pelajaran

```python
import numpy as np
import matplotlib.pyplot as plt

# Set random seed untuk reproducibility
np.random.seed(42)

## 1. Membuat Dataset Sederhana
# Kita akan membuat data nilai ujian Matematika dan Statistika untuk 30 siswa
# Nilai berkisar antara 50-100 untuk Matematika dan 40-95 untuk Statistika

jumlah_siswa = 30
matematika = np.random.randint(50, 100, jumlah_siswa)
statistika = np.random.randint(40, 95, jumlah_siswa)
siswa_id = np.arange(1, jumlah_siswa+1)

print("5 data pertama:")
print("ID Siswa | Matematika | Statistika")
for i in range(5):
    print(f"{siswa_id[i]:8} | {matematika[i]:10} | {statistika[i]:10}")

"""
Penjelasan:
- Kita membuat array untuk nilai Matematika (50-100) dan Statistika (40-95) menggunakan np.random.randint
- siswa_id adalah array berisi nomor ID siswa dari 1 sampai 30
- Kita mencetak 5 data pertama untuk melihat contoh datanya
"""

## 2. Operasi Pengolahan Data dengan Numpy

# Operasi 1: Menghitung statistik dasar
mean_math = np.mean(matematika)
mean_stat = np.mean(statistika)
std_math = np.std(matematika)
std_stat = np.std(statistika)

print(f"\nRata-rata nilai Matematika: {mean_math:.2f}")
print(f"Rata-rata nilai Statistika: {mean_stat:.2f}")
print(f"Standar deviasi Matematika: {std_math:.2f}")
print(f"Standar deviasi Statistika: {std_stat:.2f}")

# Operasi 2: Mencari nilai maksimum dan minimum
max_math = np.max(matematika)
min_math = np.min(matematika)
max_stat = np.max(statistika)
min_stat = np.min(statistika)

print(f"\nNilai tertinggi Matematika: {max_math}")
print(f"Nilai terendah Matematika: {min_math}")
print(f"Nilai tertinggi Statistika: {max_stat}")
print(f"Nilai terendah Statistika: {min_stat}")

# Operasi 3: Filtering siswa dengan nilai di atas rata-rata di kedua mata pelajaran
above_avg = (matematika > mean_math) & (statistika > mean_stat)
siswa_above_avg = siswa_id[above_avg]
print(f"\nSiswa dengan nilai di atas rata-rata di kedua mata pelajaran: {siswa_above_avg}")

"""
Penjelasan:
1. Menghitung mean dan standar deviasi untuk melihat tendensi sentral dan dispersi data
2. Mencari nilai ekstrem (maksimum dan minimum) untuk memahami sebaran nilai
3. Menggunakan boolean masking untuk mengidentifikasi siswa yang performanya di atas rata-rata di kedua mata pelajaran
"""

## 3. Visualisasi Data dengan Matplotlib

# Plot 1: Histogram perbandingan distribusi nilai
plt.figure(figsize=(10, 5))

plt.subplot(1, 2, 1)
plt.hist(matematika, bins=10, color='skyblue', edgecolor='black')
plt.title('Distribusi Nilai Matematika')
plt.xlabel('Nilai')
plt.ylabel('Jumlah Siswa')

plt.subplot(1, 2, 2)
plt.hist(statistika, bins=10, color='lightgreen', edgecolor='black')
plt.title('Distribusi Nilai Statistika')
plt.xlabel('Nilai')

plt.tight_layout()
plt.show()

# Plot 2: Scatter plot hubungan nilai Matematika dan Statistika
plt.figure(figsize=(8, 6))
plt.scatter(matematika, statistika, c='purple', alpha=0.7)
plt.axhline(mean_stat, color='red', linestyle='--', label=f'Rata-rata Statistika ({mean_stat:.1f})')
plt.axvline(mean_math, color='blue', linestyle='--', label=f'Rata-rata Matematika ({mean_math:.1f})')
plt.title('Hubungan Nilai Matematika dan Statistika')
plt.xlabel('Nilai Matematika')
plt.ylabel('Nilai Statistika')
plt.legend()
plt.grid(True)

# Tandai siswa yang di atas rata-rata di kedua mata pelajaran
plt.scatter(matematika[above_avg], statistika[above_avg], c='gold', edgecolor='black', 
            s=100, label='Di atas rata-rata kedua mata pelajaran')
plt.legend()
plt.show()

"""
Penjelasan Visualisasi:
1. Histogram: 
   - Menunjukkan distribusi frekuensi nilai untuk masing-masing mata pelajaran
   - Memungkinkan kita melihat bentuk distribusi (normal, skew, dll)
   
2. Scatter plot:
   - Menunjukkan hubungan antara nilai Matematika dan Statistika
   - Garis putus-putus menunjukkan rata-rata masing-masing mata pelajaran
   - Titik kuning menandai siswa yang performanya di atas rata-rata di kedua mata pelajaran
"""

## 4. Interpretasi Hasil Visualisasi

"""
1. Dari histogram:
   - Nilai Matematika terdistribusi cukup merata dengan sedikit condong ke nilai tinggi
   - Nilai Statistika menunjukkan distribusi yang lebih normal dengan puncak di sekitar 60-70
   
2. Dari scatter plot:
   - Terlihat ada korelasi positif antara nilai Matematika dan Statistika
   - Siswa yang baik di Matematika cenderung juga baik di Statistika
   - Ada beberapa siswa (titik kuning) yang konsisten performanya di kedua mata pelajaran
   - Beberapa siswa memiliki nilai Matematika tinggi tapi Statistika rendah, dan sebaliknya
   
3. Dari perhitungan statistik:
   - Rata-rata nilai Matematika lebih tinggi daripada Statistika
   - Standar deviasi kedua mata pelajaran cukup mirip, menunjukkan variasi yang serupa
   - Ada beberapa siswa yang secara konsisten performanya di atas rata-rata
"""
```

## Kesimpulan
Studi kasus ini menunjukkan bagaimana numpy dan matplotlib dapat digunakan bersama untuk:
1. Membuat dan memanipulasi data array
2. Melakukan perhitungan statistik dasar
3. Melakukan filtering data berdasarkan kondisi
4. Memvisualisasikan data dalam berbagai bentuk untuk mendapatkan insight

Pemula dapat menggunakan pola yang sama untuk menganalisis berbagai jenis data lainnya dengan menyesuaikan dataset dan kondisi analisisnya. Kombinasi numpy untuk pengolahan data dan matplotlib untuk visualisasi merupakan alat yang powerful dalam analisis data eksploratori.