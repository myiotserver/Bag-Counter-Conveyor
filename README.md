# 🎒 Penghitung Karung di Ban Berjalan (Conveyor Bag Counter)

<div align="justify">
Aplikasi ini secara otomatis <strong>menghitung dan melacak karung semen</strong> yang melewati ban berjalan (conveyor belt) menggunakan kamera. Sistem ini menggunakan teknologi kecerdasan buatan (AI) berbasis YOLOv8 untuk mendeteksi karung, dan ByteTracker untuk melacaknya satu per satu. Cocok digunakan di pabrik atau gudang untuk memantau jumlah karung secara real-time tanpa perlu menghitung manual.
</div>

---

## 📋 Daftar Isi
1. [Contoh Hasil](#-contoh-hasil)
2. [Syarat Perangkat](#-syarat-perangkat)
3. [Cara Instalasi (Langkah demi Langkah)](#️-cara-instalasi-langkah-demi-langkah)
4. [Cara Menjalankan Program](#-cara-menjalankan-program)
5. [Cara Melatih Model AI Sendiri](#-cara-melatih-model-ai-sendiri-opsional)
6. [Masalah Umum](#-masalah-umum)
7. [Lisensi](#-lisensi)

---

## 🎬 Contoh Hasil

<div align="center">
    <img src="assets/conveyorvision_output.gif" width="800"/>
    <p><i>Contoh sistem sedang menghitung karung yang melewati garis detektor</i></p>
</div>

---

## 💻 Syarat Perangkat

Sebelum mulai, pastikan komputer kamu memiliki:

| Kebutuhan | Keterangan |
|-----------|-----------|
| **OS** | Windows 10/11 atau Linux |
| **GPU** | Kartu grafis NVIDIA (disarankan, bisa juga tanpa GPU tapi lebih lambat) |
| **RAM** | Minimal 8 GB |
| **Python** | Versi 3.8 ke atas |
| **Kamera / Video** | Webcam atau file video rekaman conveyor |

> ⚠️ **Catatan:** Jika komputer kamu **tidak punya GPU NVIDIA**, program tetap bisa berjalan menggunakan CPU, namun prosesnya akan lebih lambat.

---

## ⚙️ Cara Instalasi (Langkah demi Langkah)

> 🙋 **Buat yang belum pernah coding:** Ikuti langkah ini satu per satu. Jangan lewatkan satupun!

---

### Langkah 1 — Install Python

1. Buka halaman resmi Python: https://www.python.org/downloads/
2. Klik tombol **"Download Python"** (pilih versi terbaru)
3. Jalankan file installer yang sudah didownload
4. ✅ **PENTING:** Centang kotak **"Add Python to PATH"** sebelum klik Install
5. Klik **"Install Now"** dan tunggu sampai selesai

Untuk mengecek apakah Python sudah terpasang, buka **Command Prompt** (Windows) atau **Terminal** (Linux/Mac) lalu ketik:
```
python --version
```
Jika muncul angka versi Python, berarti berhasil ✅

---

### Langkah 2 — Download Project Ini

**Cara A — Jika kamu punya Git:**
```bash
git clone https://github.com/myiotserver/Bag-Counter-Conveyor.git
cd Bag-Counter-Conveyor
```

**Cara B — Tanpa Git:**
1. Klik tombol hijau **"Code"** di halaman GitHub ini
2. Pilih **"Download ZIP"**
3. Ekstrak file ZIP ke folder yang kamu inginkan (misalnya `D:\Bag-Counter-Conveyor`)
4. Buka folder hasil ekstrak tersebut

---

### Langkah 3 — Buat Lingkungan Python (Virtual Environment)

Virtual environment adalah ruang kerja terisolasi agar instalasi library tidak bertabrakan dengan program Python lain di komputermu.

Buka **Command Prompt**, masuk ke folder project, lalu jalankan:

```bash
python -m venv cv_env
```

Aktifkan virtual environment:

- **Windows:**
  ```bash
  cv_env\Scripts\activate
  ```
- **Linux / Mac:**
  ```bash
  source cv_env/bin/activate
  ```

> ✅ Jika berhasil, kamu akan melihat tulisan `(cv_env)` di awal baris Command Prompt

---

### Langkah 4 — Install Library yang Dibutuhkan

Pastikan virtual environment sudah aktif (ada tulisan `(cv_env)`), lalu jalankan perintah berikut satu per satu:

```bash
pip install ultralytics
pip install supervision==0.1.0
pip install onemetric
pip install opencv-python
pip install Pillow
```

Install ByteTracker (YOLOX):
```bash
pip install cython
pip install 'git+https://github.com/ifzhang/ByteTrack.git'
```

> ⏳ Proses instalasi mungkin memakan waktu beberapa menit tergantung kecepatan internet. Tunggu sampai selesai.

---

### Langkah 5 — Install PyTorch (Library AI)

Buka halaman resmi PyTorch: https://pytorch.org/get-started/locally/

Pilih sesuai dengan perangkat kamu:
- **Jika punya GPU NVIDIA:** pilih opsi CUDA yang sesuai
- **Jika tidak punya GPU:** pilih opsi CPU

Salin perintah yang diberikan di website tersebut dan jalankan di Command Prompt.

Contoh untuk CPU:
```bash
pip install torch torchvision torchaudio
```

---

## ▶️ Cara Menjalankan Program

### Persiapan

Buka file `scripts/counter.py` menggunakan teks editor (misalnya Notepad, VS Code, atau Notepad++).

Cari bagian ini di dalam file (sekitar baris 133–137):

```python
def main():
    # Gunakan '0' untuk webcam, atau masukkan path video
    video_path = 0
    # Gunakan model bawaan 'yolov8n.pt' untuk testing sementara
    model_path = "yolov8n.pt"
```

Ubah sesuai kebutuhan:

| Pengaturan | Nilai | Keterangan |
|-----------|-------|-----------|
| `video_path = 0` | `0` | Gunakan webcam pertama yang terhubung |
| `video_path = "D:\\video.mp4"` | path file | Gunakan file video rekaman |
| `model_path = "yolov8n.pt"` | `"yolov8n.pt"` | Model bawaan untuk percobaan |
| `model_path = "best.pt"` | path model | Model hasil pelatihan sendiri |

> 💡 **Tips:** Untuk percobaan pertama, biarkan saja `video_path = 0` (webcam) dan `model_path = "yolov8n.pt"` (model bawaan)

---

### Menjalankan

1. Buka Command Prompt
2. Masuk ke folder project:
   ```bash
   cd D:\Bag-Counter-Conveyor
   ```
3. Aktifkan virtual environment:
   ```bash
   cv_env\Scripts\activate
   ```
4. Masuk ke folder scripts:
   ```bash
   cd scripts
   ```
5. Jalankan program:
   ```bash
   python counter.py
   ```

6. Akan muncul jendela video dengan kotak deteksi di setiap karung yang terdeteksi
7. Tekan **tombol `Q`** pada keyboard untuk menutup program

---

## 🧠 Cara Melatih Model AI Sendiri (Opsional)

> ℹ️ Bagian ini untuk kamu yang ingin melatih AI agar lebih akurat mengenali jenis karung spesifik di tempat kamu.

### Langkah 1 — Siapkan Dataset Foto
- Ambil foto/video karung dari berbagai sudut dan kondisi cahaya
- Semakin banyak foto, semakin akurat model AI-nya (minimal 100 foto disarankan)

### Langkah 2 — Beri Label pada Foto
Gunakan salah satu tools berikut (gratis):
- **Roboflow:** https://roboflow.com — mudah digunakan, bisa online
- **VGG Image Annotator (VIA):** https://www.robots.ox.ac.uk/~vgg/software/via/

Gambar kotak (bounding box) di setiap karung pada foto, lalu export dengan format **YOLOv8**.

Hasil export akan menghasilkan folder dengan struktur seperti ini:
```
dataset/
├── train/
│   ├── images/
│   └── labels/
├── valid/
│   ├── images/
│   └── labels/
└── data.yaml
```

### Langkah 3 — Latih Model
```bash
yolo train data=dataset/data.yaml model=yolov8n.pt epochs=50 imgsz=640
```

Setelah selesai, file model akan tersimpan di folder `runs/detect/train/weights/best.pt`.

### Langkah 4 — Gunakan Model Hasil Pelatihan
Ubah `model_path` di `counter.py` menjadi:
```python
model_path = "runs/detect/train/weights/best.pt"
```

---

## 🔧 Masalah Umum

| Masalah | Solusi |
|---------|--------|
| `python` tidak dikenali | Pastikan Python sudah terinstall dan "Add to PATH" dicentang saat install |
| Kamera tidak terbuka | Cek apakah webcam terhubung, coba ganti `video_path = 0` ke `video_path = 1` |
| Error saat install library | Pastikan virtual environment sudah aktif (ada tulisan `cv_env` di Command Prompt) |
| Program berjalan sangat lambat | Komputer mungkin tidak punya GPU NVIDIA. Coba kurangi resolusi video |
| `ModuleNotFoundError` | Jalankan ulang perintah `pip install` untuk library yang disebutkan di error |

---

## 📄 Laporan Teknis

Untuk memahami cara kerja algoritma secara lebih mendalam, baca [LAPORAN TEKNIS](assets/ConveyorVision.pdf) *(dalam Bahasa Inggris)*.

---

## 🪪 Lisensi

Proyek ini dilisensikan di bawah ketentuan yang tercantum di file [LICENSE](LICENSE).

---

<div align="center">
  <i>Dibuat dengan ❤️ — Jika ada pertanyaan, silakan buka <a href="https://github.com/myiotserver/Bag-Counter-Conveyor/issues">Issues</a> di GitHub</i>
</div>
