# ETL-Assesment-Green-Finance-Data-Analisys

#Berikut adalah tugas dan jawaban dari **Anggota Kelompok 5**

 | No| Nama | No Absen |
| --- | --- | --- |
| 1. | Bavari | 9.029.DB2025 |
| 2. | Deka | 9.037.DB2025 |
| 3. | Farhansyah | 10.049.DB2025 |
| 4. | Maksum | 9.045.DB2025 |

## 1. Pendahuluan

Tugas ini merupakan bagian dari program asesmen analitik data bertema Green Finance yang bertujuan untuk meningkatkan pemahaman teknis peserta dalam mengolah data lingkungan, ekonomi, dan sosial terkait proyek energi hijau di Indonesia. Melalui pendekatan berbasis Python, peserta diminta menyelesaikan serangkaian soal dengan pendekatan pemrograman dasar hingga machine learning untuk mendukung pengambilan keputusan berkelanjutan.

---

## 2. Tugas Soal

Tugas terdiri dari tujuh soal utama dan satu soal bonus. Soal 1 hingga 7 meliputi berbagai konsep dasar pemrograman Python, seperti :

. Penggabungan data (`merge`), 
. Logika `if-else`
. Perulangan `for` dan `while`
. Penggunaan `dictionary`
. pembuatan `fungsi`
. Serta penanganan kesalahan (`try-except`). 

Sementara itu, soal bonus menantang peserta membangun model machine learning menggunakan algoritma **Decision Tree Classifier** untuk memprediksi tingkat _Daya Tarik Investasi_ sebuah proyek berdasarkan indikator numerik.

---

## 3. Output dari Soal

Output dari setiap soal mencakup:

. Soal 1: klasifikasi efisiensi pengurangan emisi CO2.
. Soal 2: rata-rata CO2 reduction proyek PLTM.
. Soal 3: informasi lahan dan konflik sosial dari input `Project_ID`.
. Soal 4: daftar proyek dengan investasi tinggi dan konflik rendah.
. Soal 5: total investasi lokasi efisien.
. Soal 6: fungsi efisiensi CO2 dengan error handling.
. Soal 7: rata-rata output energi hanya untuk proyek yang valid.
. Bonus: akurasi model machine learning dan prediksi daya tarik investasi untuk proyek baru.



## 4. Dataset yang Dipakai
Ada 5 dataset digunakan dalam tugas ini, yaitu:

1. `Environmental_Dataset.xlsx` – berisi data CO2 reduction dan energy output,
2. `Financial_Dataset.xlsx` – berisi biaya investasi tiap proyek,
3. `Social_Dataset.xlsx` – berisi status lahan dan konflik sosial,
4. `Economic_Dataset.xlsx` – berisi GDP growth dan daya tarik investasi,
5. `Geospatial_Dataset.xlsx` – berisi efisiensi lokasi,

Setiap dataset dihubungkan melalui kolom `Project_ID`.

---

## 5. Hasil Output dari Jawaban

Soal 1: If-Else dan Aritmatika

```import pandas as pd

env = pd.read_excel("Environmental_Dataset.xlsx")
fin = pd.read_excel("Financial_Dataset.xlsx")
df = env.merge(fin, on="Project_ID")
plts = df[df["Project_ID"].str.startswith("PLTS")]

for _, row in plts.iterrows():
    ratio = row["CO2_Reduction"] / (row["Investment_Cost"] * 1_000_000)
    status = "High" if ratio >= 0.5 else "Low"
    print(f"{row['Project_ID']}: {ratio:.2f} ({status})")

Soal 2: For Loop dan List

env = pd.read_excel("Environmental_Dataset.xlsx")

co2_list = []
for _, row in env.iterrows():
    if row["Project_ID"].startswith("PLTM"):
        co2_list.append(row["CO2_Reduction"])

if co2_list:
    avg = sum(co2_list) / len(co2_list)
    print(f"Average CO2 Reduction for PLTM Projects: {avg:.0f} tons CO2e")
else:
    print("No PLTM projects found.")

Soal 3: While Loop dan Input Pengguna

social = pd.read_excel("Social_Dataset.xlsx")
social_dict = social.set_index("Project_ID")[["Land_Status", "Tingkat_Konflik"]].to_dict("index")

while True:
    pid = input("Enter Project_ID (or 'DONE' to finish): ")
    if pid == "DONE":
        break
    if pid in social_dict:
        land = social_dict[pid]["Land_Status"]
        conflict = social_dict[pid]["Tingkat_Konflik"]
        print(f"{pid} - Land Status: {land}, Tingkat Konflik: {conflict}")
    else:
        print("Project not found")

Soal 4: Dictionary dan Filter Kondisional

econ = pd.read_excel("Economic_Dataset.xlsx")
social = pd.read_excel("Social_Dataset.xlsx")
df = econ.merge(social, on="Project_ID")

project_dict = {
    row["Project_ID"]: (row["Daya_Tarik_Investasi"], row["Tingkat_Konflik"])
    for _, row in df.iterrows()
}

for pid, (daya, konflik) in project_dict.items():
    if daya == "High" and konflik == "Low":
        print(pid)

Soal 5: Fungsi dan Aritmatika

geo = pd.read_excel("Geospatial_Dataset.xlsx")
fin = pd.read_excel("Financial_Dataset.xlsx")
df = geo.merge(fin, on="Project_ID")

def calculate_total_investment(df):
    total = 0
    for _, row in df.iterrows():
        if row["Efisiensi_Lokasi"] == "High":
            total += row["Investment_Cost"]
    return total

total = calculate_total_investment(df)
print(f"Total Investment for High-Efficiency Locations: {total:.2f} billion Rp")

Soal 6: Modul dan Error Handling

def compute_co2_efficiency(co2, cost):
    try:
        return co2 / (cost * 1_000_000)
    except ZeroDivisionError:
        return "Cannot compute"

projects = [
    ("PLTS-NTT-001", 50000, 100),
    ("PLTS-JATIM-001", 45000, 100),
    ("PLTM-PAPU-001", 40000, 0)
]

for pid, co2, cost in projects:
    result = compute_co2_efficiency(co2, cost)
    print(f"{pid}: {result}")

Soal 7: Error Handling dalam Loop

env = pd.read_excel("Environmental_Dataset.xlsx")
energy_dict = env.set_index("Project_ID")["Energy_Output"].to_dict()

project_ids = ["PLTS-NTT-001", "PLTM-PAPU-001", "INVALID-ID"]
total_output = 0
count = 0

for pid in project_ids:
    try:
        total_output += energy_dict[pid]
        count += 1
    except KeyError:
        print(f"{pid} not found.")

if count > 0:
    average_output = total_output / count
    print(f"Average Energy Output: {average_output:.0f} kWh")
else:
    print("No valid projects found.")
```
Hasil dari seluruh jawaban menunjukkan keberhasilan peserta dalam mengolah data real-world menggunakan Python. Pada soal bonus, model Decision Tree yang dilatih dengan tiga fitur utama menghasilkan **akurasi sekitar 85–90%**, dan mampu memprediksi proyek baru dengan `GDP_Growth=5.0`, `CO2_Reduction=70000`, dan `Investment_Cost=150` sebagai proyek dengan **daya tarik investasi tinggi**. Visualisasi pohon keputusan juga menunjukkan bagaimana fitur berpengaruh terhadap klasifikasi akhir.

--- 

## 6. Kesimpulan
Tugas ini menunjukkan pentingnya integrasi antara keterampilan teknis Python dan pemahaman terhadap isu keberlanjutan. 

Dari eksplorasi data dasar hingga penerapan machine learning, peserta mampu mengolah informasi kompleks menjadi wawasan yang dapat digunakan untuk mendukung keputusan pendanaan hijau di Indonesia. 

Tugas ini juga menunjukkan bahwa pendekatan data-driven sangat relevan dalam menilai kelayakan proyek energi bersih.
