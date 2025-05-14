# HPCv2 Simulator 🖥️⚡

Simulasi penjadwalan dan manajemen energi untuk lingkungan High-Performance Computing (HPC) dengan dukungan kebijakan manajemen daya dan berbagai algoritma penjadwalan.

![HPC Simulation Visualization](https://via.placeholder.com/800x400.png?text=Sample+Gantt+Chart+Visualization)
_(Contoh visualisasi hasil simulasi)_

## Daftar Isi 📜

- [Instalasi](#-instalasi)
- [Panduan Penggunaan](#-panduan-penggunaan)
  - [Linux](#-linux-instructions)
  - [Windows](#-windows-instructions)
- [Kebijakan Manajemen Daya](#-kebijakan-manajemen-daya)
- [Algoritma Penjadwalan](#-algoritma-penjadwalan)
- [Visualisasi Hasil](#-visualisasi-hasil)
- [Dokumentasi Tambahan](#-dokumentasi-tambahan)

## 📥 Instalasi

### Prasyarat

- Python 3.8 (BatsimPy tidak kompatibel dengan python versi terbaru)
- Nix (untuk simulasi Batsim di Linux)

### Clone dan Setup Environment

```bash
git clone https://github.com/RakaSP/HPCv2.git
cd HPCv2
python3.8 -m venv myenv

# aktivasi virtual environment (linux)
source myenv/bin/activate

# aktivasi virtual environment (windows)
myenv/Scripts/activate

# install dependensi
pip install requirements.txt
```

## 🐧 Panduan Penggunaan Linux

1. **Simulasi Batsim**

   ```bash
   nix-shell env.nix
   python batsim_simulator_baseline.py
   python batsim_simulator_timeout.py  # dengan timeout policy
   ```

2. **Simulasi CSSP**

   ```bash
   source myenv/bin/activate
   python batsim_cssp_baseline.py
   python batsim_cssp_timeout.py  # dengan timeout policy
   ```

## 🪟 Panduan Penggunaan Windows

> ⚠️ Hanya simulasi CSSP yang bisa dijalankan. Batsim membutuhkan `nix-shell` yang tidak tersedia di Windows.

1. **Aktifkan virtual environment**:

   ```bash
   myenv\Scripts\activate
   ```

2. **Jalankan simulasi CSSP**:

   ```bash
   python batsim_cssp_baseline.py
   python batsim_cssp_timeout.py  # dengan timeout policy
   ```

## 💡 Baseline Policy

Pada simulasi dengan script `simulator_*_baseline.py`, semua node dalam sistem akan selalu aktif (idle/computing).

## ⏲️ Timeout Policy

Script `simulator_*_timeout.py` menggunakan kebijakan timeout, yang secara otomatis mengubah status nodes menjadi switch*off setelah node tersebut dalam status idle selama `X` detik. Nilai `X` adalah batas waktu yang ditentukan dalam timeout policy untuk mencegah node dalam status idle terlalu lama. Nilai `X` atau timeout dapat dimodifikasi pada script `simulator*\*\_timeout.py`

## 💻 Scheduling

Terdapat dua scheduler yang bisa digunakan dalam simulasi:

1. **Easy backfilling Scheduler**
2. **FCFS (First-Come, First-Served) Scheduler**

### Memilih Scheduler pada CSSP

Untuk memilih scheduler yang akan digunakan, cukup tentukan scheduler pada argumen `run_simulation()` seperti berikut:

```python
fcfs_scheduler = FCFSScheduler(None)
easy_scheduler = EasyScheduler(None)

# Untuk menjalankan simulator dengan penjadwalan easy backfilling
jobs_e, sim_e = run_simulation(easy_scheduler, platform_filepath, workload_filepath)
```

### Memilih Scheduler pada Batsim-Py

Untuk informasi lebih lanjut tentang bagaimana Batsim bekerja dengan penjadwalan dan cara mengonfigurasi BatsimPy, Anda dapat membaca tutorial lengkapnya di [Dokumentasi Scheduling BatsimPy](https://lccasagrande.github.io/batsim-py/tutorials/scheduling.html)

## 📊 Visualisasi Hasil

Gunakan `visualize_results.ipynb` untuk memvisualisasikan hasil penjadwalan dari file `.csv` yang dihasilkan oleh simulasi. Notebook ini akan menampilkan Gantt chart yang menunjukkan hasil penjadwalan dan alokasi node untuk mengeksekusi seluruh jobs.
