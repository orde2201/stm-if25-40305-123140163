# 🎵 Analisis Audio & Eksperimen Resampling Signal Processing

Dokumentasi projek analisis pemprosesan isyarat digital (*Digital Signal Processing*) untuk menganalisis sifat domain masa dan frekuensi pada fail audio, mengenalpasti *noise floor*, serta membuktikan fenomena *aliasing* secara visual akibat proses *downsampling*.

---

## 📌 Ringkasan Eksperimen

Projek ini terbahagi kepada dua bahagian utama:
1. **Visualisasi Audio 4 Dimensi**: Menganalisis isyarat suara asal menerusi empat perspektif visual (*Waveform*, *FFT Spectrum*, *STFT Spectrogram*, dan *Mel-Spectrogram*)[cite: 1, 2, 3].
2. **Eksperimen Resampling & Aliasing**: Membandingkan kesan *downsampling* daripada kadar sampel asal ($44.1\text{ kHz}$) kepada $8\text{ kHz}$ menggunakan senario *Naive Decimation* (tanpa penapis) berbanding *Filtered Resampling* (menggunakan *Low-Pass Filter* anti-aliasing)[cite: 4].

---

## 📊 1. Visualisasi Audio 4 Dimensi

Eksperimen pertama mengekstrak sifat audio menerusi 4 reka bentuk visual:

### 1.1 Waveform (Amplitud vs Masa)
* **Penerangan**: Menunjukkan variasi amplitud isyarat suara mengikut domain masa.
* **Analisis**: Menampilkan bahagian senyap (*silence*) serta kawasan sebutan pertuturan manusia.

### 1.2 Spektrum FFT (Magnitud dBFS vs Frekuensi)
* **Penerangan**: Transformasi Fast Fourier untuk melihat taburan tenaga dalam domain frekuensi ($20 \log_{10}$)[cite: 1].
* **Dapatan Frekuensi Dominan Hingar**:
  * **Dominasi Utama ($0 - 500\text{ Hz}$)**: Tenaga puncak menghampiri $0\text{ dBFS}$ berada pada frekuensi sangat rendah yang didominasi oleh deruman fizikal (*rumble* mesin/kipas/penyaman udara) bersama frekuensi asas vokal manusia[cite: 1].
  * **Jalur Vokal ($1.000 - 7.500\text{ Hz}$)**: Berada pada julat $-25\text{ dBFS}$ hingga $-50\text{ dBFS}$[cite: 1].
  * **Pemotongan Mikrofon ($\approx 7.500\text{ Hz}$)**: Tenaga jatuh mendadak ke bawah $-90\text{ dBFS}$, menandakan had fizikal mikrofon perakam[cite: 1].

### 1.3 Spektrogram STFT (Masa-Frekuensi)
* **Penerangan**: Pemetaan masa dan frekuensi (*Short-Time Fourier Transform*) untuk mengenalpasti kestabilan hingar (*noise*)[cite: 1, 2].
* **Analisis Garisan Horizontal**:
  * **Stationary Noise**: Kelihatan garisan horizontal membujur lurus dan konsisten pada julat $0 - 7.500\text{ Hz}$ dari awal hingga akhir durasi (saat $0 - 12$)[cite: 1, 2]. Ini membuktikan kewujudan hingar statik latar belakang yang sentiasa berterusan[cite: 1, 2].
  * **Isyarat Vokal**: Kelihatan sebagai corak menegak/blok harmonik yang terputus-putus pada frekuensi rendah-sederhana ($0 - 2.000\text{ Hz}$)[cite: 1, 2].

### 1.4 Mel-Spektrogram (Skala Perseptual Mel)
* **Penerangan**: Penukaran skala frekuensi linear kepada skala perseptual Mel yang meniru pendengaran logaritmik manusia[cite: 1, 3].
* **Kelebihan Bentuk Vokal (*Formants*)**:
  * Skala Mel melebarkan (*zoom-in*) julat frekuensi rendah ($0 - 4.000\text{ Hz}$) di mana tenaga utama artikulasi vokal ($F_1, F_2, F_3$) berada[cite: 1, 3].
  * Jalur vokal yang dulunya padat berhimpit di bahagian bawah pada spektrogram linear kini terurai jelas, lebih padat, dan tampak kontras di atas warna latar belakang hingar statik[cite: 1, 2, 3].

---

## 🔄 2. Eksperimen Resampling & Pembuktian Aliasing

Eksperimen kedua melakukan *downsampling* kadar sampel isyarat daripada **$44.100\text{ Hz}$** kepada **$8.000\text{ Hz}$** (faktor desimasi $M = 5$), sekali gus memotong had Nyquist baharu ($f_N$) kepada **$4.000\text{ Hz}$**[cite: 4].

### Perbandingan Senario
1. **Senario A (Naive Decimation - `x[::M]`)**: Mengambil sampel secara mentah tanpa penapis LPF anti-aliasing[cite: 4].
2. **Senario B (Clean Resampling - `librosa.resample`)**: Menggunakan *Low-Pass Filter* anti-aliasing sebelum pemotongan sampel dibuat[cite: 4].

### 📝 Laporan Analisis Aliasing
* **Kewujudan Aliasing (Senario A)**:
  Disebabkan frekuensi tinggi asal ($4.000\text{ Hz} - 7.500\text{ Hz}$) tidak diredam terlebih dahulu, tenaga tersebut terpantul semula (*folding effect*) ke dalam jalur frekuensi baharu $0 - 4.000\text{ Hz}$ akibat pelanggaran Teorem Nyquist-Shannon[cite: 4].
* **Bukti Visual Spektrogram**:
  * Latar belakang frekuensi $2.000\text{ Hz} - 4.000\text{ Hz}$ pada Senario A kelihatan jauh lebih terang dan padat dengan tumpukan warna ungu-kemerahan tiruan[cite: 4].
  * Muncul garisan *phantom* hingar statik tambahan dan kekaburan harmonik vokal akibat pantulan tenaga frekuensi tinggi[cite: 4].
* **Penyelesaian Anti-Aliasing (Senario B)**:
  Penapis LPF memotong seluruh tenaga di atas $4.000\text{ Hz}$ terlebih dahulu[cite: 4]. Hasilnya, spektrogram Senario B mengekalkan bentuk isyarat asal $0 - 4.000\text{ Hz}$ secara tepat tanpa sebarang artefak distorsi atau tenaga palsu[cite: 4].

---

## 🛠️ Keperluan Sistem & Pustaka

### Keperluan Pustaka (*Dependencies*)
```bash
pip install numpy matplotlib librosa scipy