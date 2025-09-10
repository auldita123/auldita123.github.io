#========== Presentasi besok ==========
##Code
Oke, saya akan buatkan kode program Python sederhana sesuai permintaan Anda.
Setiap nomor akan saya buat **per file** dengan nama file berbeda. Semua menggunakan gambar `apel.png`.

> Catatan:
>
> * Semua operasi dasar akan menggunakan **OpenCV (`cv2`)**, **NumPy**, dan **Matplotlib**.
> * Tidak menggunakan toolbox canggih, hanya rumus dasar dan manipulasi array.
> * Histogram akan ditampilkan dengan diagram kartesius untuk setiap citra (asli dan hasil olahan).
> * Untuk keperluan manipulasi channel, akan saya gunakan `cv2.split(img)`.

---

## 📂 File 1: `1_rgb_to_grayscale.py`

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Baca gambar
img = cv2.imread("apel.png")
b, g, r = cv2.split(img)

# Grayscale dengan rata-rata
gray_avg = ((r + g + b) / 3).astype(np.uint8)

# Grayscale dengan luminositas
gray_lum = (0.3 * r + 0.6 * g + 0.1 * b).astype(np.uint8)

# Tampilkan hasil
plt.subplot(2, 2, 1), plt.imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB)), plt.title("Citra Asli")
plt.subplot(2, 2, 2), plt.imshow(gray_avg, cmap='gray'), plt.title("Grayscale Rata-rata")
plt.subplot(2, 2, 3), plt.imshow(gray_lum, cmap='gray'), plt.title("Grayscale Luminositas")

# Histogram
plt.figure()
plt.hist(gray_avg.ravel(), bins=256, range=[0,256], color='gray')
plt.title("Histogram Grayscale (Rata-rata)")

plt.figure()
plt.hist(gray_lum.ravel(), bins=256, range=[0,256], color='gray')
plt.title("Histogram Grayscale (Luminositas)")

plt.show()
```

---

## 📂 File 2: `2_negasi.py`

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Baca gambar
img = cv2.imread("apel.png")
b, g, r = cv2.split(img)

k = 255  # bisa diganti manual
neg = k - img

# Tampilkan hasil
plt.subplot(1, 2, 1), plt.imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB)), plt.title("Citra Asli")
plt.subplot(1, 2, 2), plt.imshow(cv2.cvtColor(neg, cv2.COLOR_BGR2RGB)), plt.title("Negasi")

# Histogram
plt.figure()
plt.hist(img.ravel(), bins=256, range=[0,256])
plt.title("Histogram Asli")

plt.figure()
plt.hist(neg.ravel(), bins=256, range=[0,256])
plt.title("Histogram Negasi")

plt.show()
```

---

## 📂 File 3: `3_brightness.py`

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Baca gambar
img = cv2.imread("apel.png")

k = 50  # nilai brightness (bisa diganti manual)
bright = cv2.add(img, k*np.ones(img.shape, dtype=np.uint8))

# Tampilkan hasil
plt.subplot(1, 2, 1), plt.imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB)), plt.title("Citra Asli")
plt.subplot(1, 2, 2), plt.imshow(cv2.cvtColor(bright, cv2.COLOR_BGR2RGB)), plt.title("Brightness +k")

# Histogram
plt.figure()
plt.hist(img.ravel(), bins=256, range=[0,256])
plt.title("Histogram Asli")

plt.figure()
plt.hist(bright.ravel(), bins=256, range=[0,256])
plt.title("Histogram Brightness")

plt.show()
```

---

## 📂 File 4: `4_contrast_stretching.py`

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Baca gambar grayscale
img = cv2.imread("apel.png", 0)

# Rumus Contrast Stretching
min_val = np.min(img)
max_val = np.max(img)
cs = ((img - min_val) / (max_val - min_val) * 255).astype(np.uint8)

# Tampilkan hasil
plt.subplot(1, 2, 1), plt.imshow(img, cmap='gray'), plt.title("Citra Asli")
plt.subplot(1, 2, 2), plt.imshow(cs, cmap='gray'), plt.title("Contrast Stretching")

# Histogram
plt.figure()
plt.hist(img.ravel(), bins=256, range=[0,256], color='gray')
plt.title("Histogram Asli")

plt.figure()
plt.hist(cs.ravel(), bins=256, range=[0,256], color='gray')
plt.title("Histogram Contrast Stretching")

plt.show()
```

---

## 📂 File 5: `5_histogram_equalization.py`

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Baca gambar grayscale
img = cv2.imread("apel.png", 0)

# Histogram Equalization
heq = cv2.equalizeHist(img)

# Histogram Specification (ke citra referensi)
ref = cv2.imread("apel.png", 0)  # contoh: pakai citra itu sendiri
hist_ref, bins = np.histogram(ref.flatten(), 256, [0,256])
cdf_ref = hist_ref.cumsum()
cdf_ref = 255 * cdf_ref / cdf_ref[-1]

hist_img, _ = np.histogram(img.flatten(), 256, [0,256])
cdf_img = hist_img.cumsum()
cdf_img = 255 * cdf_img / cdf_img[-1]

mapping = np.zeros(256, dtype=np.uint8)
for i in range(256):
    diff = np.abs(cdf_img[i] - cdf_ref)
    mapping[i] = np.argmin(diff)

spec = mapping[img]

# Tampilkan hasil
plt.subplot(2, 2, 1), plt.imshow(img, cmap='gray'), plt.title("Citra Asli")
plt.subplot(2, 2, 2), plt.imshow(heq, cmap='gray'), plt.title("Histogram Equalization")
plt.subplot(2, 2, 3), plt.imshow(spec, cmap='gray'), plt.title("Histogram Specification")

# Histogram
plt.figure()
plt.hist(img.ravel(), bins=256, range=[0,256], color='gray')
plt.title("Histogram Asli")

plt.figure()
plt.hist(heq.ravel(), bins=256, range=[0,256], color='gray')
plt.title("Histogram Equalization")

plt.figure()
plt.hist(spec.ravel(), bins=256, range=[0,256], color='gray')
plt.title("Histogram Specification")

plt.show()
```

---

👉 Semua file di atas sederhana, sesuai aturan:

* Tidak pakai toolbox khusus, hanya `cv2`, `numpy`, `matplotlib`.
* Histogram ditampilkan untuk setiap citra.
* Path selalu `"apel.png"`.
* Variabel `k` bisa diganti manual untuk negasi & brightness.

Mau saya satukan semua kode dalam **1 file dengan menu pilihan (1–5)** supaya lebih praktis, atau Anda lebih suka **terpisah per file** seperti di atas?
##Penjelasan
# Penjelasan lengkap — per poin, jelas dan terstruktur

Bagus — di bawah ini saya jelaskan **dengan lengkap dan poin-per-poin** mengenai setiap operasi pada kode yang saya berikan sebelumnya. Saya akan menjelaskan **apa yang dilakukan**, **rumus matematisnya**, **catatan implementasi (dtype, overflow, OpenCV BGR, dll.)**, **cara menyesuaikan variabel `k`**, **cara menampilkan histogram**, dan **tips/edge cases**.

---

# 1) RGB → Grayscale (dua metode: rata-rata & luminositas)

**Tujuan:** mengubah citra berwarna (RGB/BGR) menjadi citra intensitas tunggal (grayscale).

**Metode & rumus:**

* **Rata-rata (Average):**
  $I = \frac{R + G + B}{3}$
  artinya, rata-rata ketiga kanal.
* **Luminositas (weighted):**
  $I = 0.3R + 0.6G + 0.1B$
  (bobot umum yang memperhitungkan sensitivitas mata manusia terhadap hijau lebih kuat).

**Implementasi (poin penting):**

* Gunakan `b, g, r = cv2.split(img)` untuk memisahkan channel. (`split()` sesuai permintaan Anda)
* Jika `img` dibaca oleh OpenCV, format channel adalah **BGR**, bukan RGB. Saat menampilkan dengan Matplotlib, konversi `cv2.cvtColor(img, cv2.COLOR_BGR2RGB)` untuk tampilan warna yang benar.
* Lakukan operasi pada array `numpy` lalu ubah hasil ke `np.uint8` dengan `.astype(np.uint8)`.
* Hindari pembulatan tak sengaja: lakukan perhitungan sebagai `float` dulu, baru cast ke `uint8`.
* Jika ingin hasil lebih akurat gunakan `np.round(...)` sebelum cast.

**Histogram:**

* Untuk grayscale gunakan `plt.hist(gray.ravel(), bins=256, range=[0,256])`.
* `ravel()` meratakan array 2D menjadi 1D (satu histogram intensitas).

**Catatan praktis:**

* Metode luminositas biasanya menghasilkan kontras dan persepsi visual lebih baik.
* Kompleksitas: O(N)—memproses setiap piksel sekali.

---

# 2) Operasi Negasi (dengan variabel `k` yang bisa diubah)

**Tujuan:** menghasilkan citra negatif (invers warna) dengan parameter `k` sehingga bisa diubah-ubah.

**Rumus umum:**

* Negasi standar: $I' = 255 - I$ (untuk `uint8`).
* Versi umum dengan k: $I' = k - I$ (di mana `k` biasanya 255 atau bisa diatur).

**Implementasi (poin penting):**

* Di kode: `k = 255` (default) → `neg = k - img`
* Pastikan `img` bertipe `uint8`. Operasi `k - img` dengan `k` integer akan menghasilkan tipe `uint8` (hasil diklatkan sesuai tipe).
* Jika `k` < 255, akan menghasilkan range intensitas baru (mis. `k=200` → nilai negatif mungkin tidak ada karena `uint8` wrap). Untuk menghindari wrap/underflow, gunakan `np.clip` atau konversi ke `int16` lalu clip:

  ```python
  neg = np.clip(k - img.astype(np.int16), 0, 255).astype(np.uint8)
  ```
* Untuk gambar warna, operasi berlaku per-channel (B, G, R) — `k - img` dioperasikan pada seluruh array 3-kanal.

**Histogram:**

* Bandingkan histogram asli vs histogram negasi. Histogram negasi akan terlihat “dibalik” posisi intensitasnya.

**Catatan praktis:**

* `k=255` adalah standar. Jika ingin efek berbeda, ubah `k` bebas; perhatikan clipping.
* Negasi tidak menambah detail—hanya membalik intensitas.

---

# 3) Operasi Brightness (menggunakan variabel `k`)

**Tujuan:** menaikkan atau menurunkan kecerahan citra.

**Rumus sederhana:**

* Brightness additif: $I' = I + k$
  Jika `k` positif → lebih terang; `k` negatif → lebih gelap.

**Implementasi (poin penting):**

* Disarankan pakai `cv2.add(img, scalar)` karena `cv2.add` melakukan clipping otomatis ke 0..255 pada `uint8`.

  ```python
  bright = cv2.add(img, np.ones(img.shape, dtype=np.uint8) * k)
  ```
* Alternatif: manual pakai `np.clip(img.astype(int) + k, 0, 255).astype(np.uint8)`
* `k` bisa diubah manual di kode. Jika ingin nilai negatif, lebih aman gunakan `np.int16` untuk operasi lalu clip.
* Untuk warna, penambahan dilakukan per-channel sehingga warna relatif dipertahankan.

**Histogram:**

* Menambahkan brightness menggeser histogram ke kanan (untuk kenaikan k) atau kiri (untuk penurunan k).

**Catatan praktis:**

* Jangan gunakan `img + k` langsung pada `uint8` tanpa clip karena bisa terjadi overflow modulus.
* Kompleksitas O(N).

---

# 4) Contrast Stretching (Linear contrast stretching)

**Tujuan:** memperbesar rentang intensitas citra sehingga memanfaatkan full 0–255 dan memperbaiki kontras.

**Rumus dasar (linear scaling):**

* Ambil nilai minimum `min_val` dan maksimum `max_val` dari citra (biasanya per channel atau untuk grayscale):
  $I' = \frac{I - \text{min\_val}}{\text{max\_val} - \text{min\_val}} \times 255$
* Hasil di-clip ke \[0,255] lalu di-cast ke `uint8`.

**Implementasi (poin penting):**

* Jika `max_val == min_val`, hindari pembagian nol — kembalikan citra kosong atau salin citra asal.

  ```python
  if max_val == min_val:
      cs = img.copy()
  else:
      cs = ((img - min_val) / (max_val - min_val) * 255).astype(np.uint8)
  ```
* Untuk gambar warna, bisa diterapkan pada tiap channel secara terpisah (split → stretch tiap channel → merge), atau konversi ke ruang warna yang memisahkan luminans (YCrCb/HSV) dan stretch hanya pada Y/V.

**Histogram:**

* Setelah stretching, histogram akan menyebar lebih merata di rentang \[0,255], meningkatkan kontras.

**Catatan praktis:**

* Tidak mengubah distribusi relatif intensitas—hanya skala linear.
* Biasanya bagus untuk citra yang memiliki rentang intensitas sempit.

---

# 5) Histogram Equalization & Histogram Specification (Matching)

**Tujuan:**

* **Histogram Equalization (HE):** membuat histogram citra lebih merata (meningkatkan kontras lokal/global) dengan memetakan intensitas melalui CDF.
* **Histogram Specification (Matching):** mengubah histogram citra sehingga menyerupai histogram referensi.

**Histogram Equalization (intuitif):**

* Hitung histogram `h(i)` untuk i=0..255.
* Hitung CDF: `cdf(i) = sum_{j=0..i} h(j)`.
* Normalisasi: `cdf_norm(i) = round(255 * cdf(i) / cdf(255))`.
* Mapping: setiap pixel dengan nilai `i` → `cdf_norm(i)`.

**Implementasi (sederhana):**

* Untuk grayscale cepat: `heq = cv2.equalizeHist(img)` (OpenCV built-in, tapi tetap dasar, bukan toolbox).
* Untuk warna: pakai `YCrCb` atau `HSV` → equalize channel Y/V lalu konversi balik untuk menghindari shifting warna.

**Histogram Specification (Matching) — langkah demi langkah:**

1. Hitung CDF sumber: `cdf_s(i)` (dinormalisasi ke \[0,255]).
2. Hitung CDF referensi: `cdf_r(j)` (dinormalisasi).
3. Untuk setiap nilai intensitas `i` pada sumber, temukan nilai `j` pada referensi yang **meminimalkan |cdf\_s(i) - cdf\_r(j)|**. Itu mapping `M[i] = j`.
4. Terapkan mapping: `out = M[src]`.

**Kode ringkas mapping:**

```python
# asumsi hist dan cdf sudah ada dan ternormalisasi ke 0..255
mapping = np.zeros(256, dtype=np.uint8)
for i in range(256):
    diff = np.abs(cdf_s[i] - cdf_r)
    mapping[i] = np.argmin(diff)
out = mapping[src]  # src adalah citra grayscale
```

**Histogram (visualisasi):**

* Tampilkan histogram sebelum & sesudah untuk melihat efek.
* Untuk histogram specification, bandingkan histogram hasil vs histogram referensi.

**Catatan praktis & caveat:**

* `cv2.equalizeHist` hanya untuk single channel (grayscale). Untuk warna, equalize luminance.
* Histogram matching bisa membuat noise/artefak jika distribusi referensi sangat berbeda.
* Mapping yang dibuat diskret → intensitas yang dihasilkan integer 0..255.
* Jika referensi sama dengan sumber, mapping hampir identitas.

---

# Umum: Penanganan channel & `split(img)`

**Mengapa `split(img)`?**

* `cv2.split(img)` memudahkan manipulasi tiap channel (B, G, R) tanpa toolbox lanjutan — sesuai permintaan Anda.
* Contoh ringkas:

  ```python
  b,g,r = cv2.split(img)
  # manipulasi tiap channel, lalu:
  result = cv2.merge([b_new, g_new, r_new])
  ```

**Catatan:** setelah `split` dan manipulasi, gunakan `cv2.merge` untuk menyusun kembali.

---

# Histogram: detail plotting agar jelas perbedaan intensitas

**Rekomendasi plotting:**

* Untuk **citra warna (RGB/BGR)** — tampilkan histogram 3 channel terpisah (R,G,B) agar terlihat perbedaan:

  ```python
  plt.hist(b.ravel(), bins=256, range=[0,256], color='b', alpha=0.5)
  plt.hist(g.ravel(), bins=256, range=[0,256], color='g', alpha=0.5)
  plt.hist(r.ravel(), bins=256, range=[0,256], color='r', alpha=0.5)
  ```
* Untuk **grayscale**, cukup satu histogram (bins=256).
* Saat menampilkan citra asli dan hasil, selalu tampilkan juga histogram masing-masing agar perbandingan lebih informatif.

---

# Praktikal: dependencies & cara menjalankan

**Instalasi:**

```bash
pip install opencv-python numpy matplotlib
```

**Menjalankan file contoh:**

```bash
python 1_rgb_to_grayscale.py
python 2_negasi.py
# dst...
```

**Sumber citra:** semua file mengakses `"apel.png"` di folder yang sama dengan script.

---

# Tips debugging & edge case testing

* **Periksa path**: `apel.png` harus berada di direktori kerja atau gunakan path absolut.
* **Cek `img is None`** setelah `cv2.imread()` untuk memastikan file terbaca.
* **Periksa dtype**: `img.dtype` harus `uint8`. Jika bukan, adjust casting.
* **Gunakan `cv2.imwrite()`** untuk menyimpan hasil jika ingin membandingkan secara offline.
* **Gunakan `np.clip`** untuk mencegah overflow/underflow saat operasi aritmetik.
* **Untuk warna saat equalizing**, prefer equalize pada channel luminance (contoh: Y pada YCrCb) supaya warna tidak berubah abnormal.
* **Performa:** semua operasi linear O(N). Untuk gambar besar, proses akan memakan waktu linear terhadap jumlah piksel.

---

# Saran perbaikan opsional (jika mau ditingkatkan nanti)

* Buat fungsi umum untuk histogram plotting agar kode tidak duplikat.
* Tambah GUI sederhana atau argparser sehingga `k` dapat dioper ke script via command line (`python 2_negasi.py --k 200`).
* Implementasi contrast stretching per-channel atau memakai CLAHE (Contrast Limited AHE) untuk hasil lokal yang lebih baik (`cv2.createCLAHE()` — opsional, lebih advance).
* Tambah visualisasi CDF (plot CDF) ketika menampilkan HE / matching agar pemahaman lebih baik.

---

# Contoh ringkas mapping nilai `k` (penggunaan praktis)

* **Negasi standar:** `k = 255`
* **Negasi khusus (kurangi intensitas maksimum):** `k = 200` → akan membuat nilai terbalik relatif terhadap 200
* **Brightness bertambah 50:** `k = 50` pada `cv2.add(img, k)`
* **Brightness dikurangi 30:** `k = -30` → lakukan arithmetic dengan tipe signed + `np.clip`

---

Kalau Anda mau, saya bisa:

* 1. **Satukan semua operasi ke satu file** dengan menu (pilih operasi), atau
* 2. **Tambahkan argumen command line** sehingga `k`, nama file input/output, dan pilihan metode bisa diatur tanpa edit kode, atau
* 3. **Tambahkan contoh output (gambar + histogram) untuk tiap operasi** (saya bisa bantu buatkan script yang menyimpan hasil).

Mau saya lanjut ke salah satu opsi itu?


---

# ==========library:==========
Dalam praktikum **pengolahan citra dan computer vision**, Python punya banyak library populer yang sering dipakai. Berikut beberapa yang paling umum:

---

### 🔹 **Library Utama untuk Pengolahan Citra**

1. **OpenCV (`cv2`)**

   * Paling populer untuk *image processing* dan *computer vision*.
   * Fitur: filtering, edge detection, face detection, object tracking, transformasi geometris, dll.
   * Cocok untuk praktikum dasar sampai lanjut.

2. **scikit-image**

   * Bagian dari ekosistem *SciPy*.
   * Fokus pada algoritma analisis citra: segmentasi, denoising, fitur, morfologi, dll.
   * Lebih "pythonic" dan mudah dipahami dibanding OpenCV untuk pemula.

3. **Pillow (PIL Fork)**

   * Library sederhana untuk membaca, menulis, dan manipulasi gambar dasar (resize, crop, rotate, dll).
   * Biasanya dipakai untuk tugas awal sebelum masuk ke OpenCV atau scikit-image.

---

### 🔹 **Library Pendukung**

4. **NumPy**

   * Hampir semua citra direpresentasikan sebagai array.
   * Dipakai untuk operasi matematis cepat (misalnya filtering manual, konvolusi).

5. **Matplotlib**

   * Untuk menampilkan gambar, membuat histogram, dan visualisasi hasil olah citra.

6. **SciPy**

   * Ada modul `scipy.ndimage` untuk operasi filtering, morfologi, dan transformasi citra.

---

### 🔹 **Untuk Computer Vision Tingkat Lanjut**

7. **TensorFlow** / **PyTorch**

   * Digunakan kalau praktikum sudah masuk ke *deep learning* (CNN untuk klasifikasi/segmentasi).

8. **Keras**

   * High-level API di atas TensorFlow, mempermudah implementasi *deep learning*.

---

📌 **Kesimpulan untuk praktikum pengolahan citra dasar:**
Biasanya dipakai kombinasi **OpenCV + NumPy + Matplotlib**, atau **scikit-image + Matplotlib**.

Mau saya buatin contoh kode sederhana (misalnya membaca gambar, ubah jadi grayscale, lalu tampilkan hasilnya) pakai **OpenCV** dan **scikit-image** biar kelihatan perbedaan penggunaannya?



Oke 👍, kita bahas **rumus operasi kontras** dalam pengolahan citra digital.  

---

## 🔹 1. Definisi Singkat
Operasi kontras bertujuan untuk **meregangkan (stretching)** atau **meningkatkan perbedaan** antar nilai intensitas piksel supaya citra terlihat lebih jelas.  

---

## 🔹 2. Rumus Linear Contrast Stretching
Rumus umum:  

\[
f'(x,y) = \alpha \cdot f(x,y) + \beta
\]

dengan:
- \( f(x,y) \) = intensitas piksel asli  
- \( f'(x,y) \) = intensitas piksel hasil  
- \( \alpha \) = faktor kontras (slope)  
- \( \beta \) = faktor kecerahan (brightness, biasanya ditambah/dikurangi)  

👉 Kalau \(\alpha > 1\), kontras meningkat.  
👉 Kalau \(0 < \alpha < 1\), kontras menurun.  

---

## 🔹 3. Rumus Contrast Stretching (Normalisasi)
Jika ingin merenggangkan intensitas dari \([f_{min}, f_{max}]\) ke rentang \([0, L-1]\):  

\[
f'(x,y) = \frac{(f(x,y) - f_{min})}{(f_{max} - f_{min})} \cdot (L-1)
\]

dengan:
- \( f_{min} \) = intensitas minimum dalam citra  
- \( f_{max} \) = intensitas maksimum dalam citra  
- \( L \) = jumlah tingkat keabuan (misal 256 untuk 8-bit)  

👉 Hasilnya: citra akan lebih kontras karena nilai piksel dipetakan ke seluruh rentang 0–255.

---

## 🔹 4. Contoh
Misal citra punya intensitas hanya di rentang [50–150], maka:
\[
f'(x,y) = \frac{(f(x,y) - 50)}{(150 - 50)} \times 255
\]
→ Intensitas akan diregangkan ke [0–255].

---

📌 **Kesimpulan:**  
- Rumus sederhana: \( f'(x,y) = \alpha f(x,y) + \beta \)  
- Rumus **contrast stretching (normalisasi):**  
  \[
  f'(x,y) = \frac{(f(x,y) - f_{min})}{(f_{max} - f_{min})} \cdot (L-1)
  \]

---

Mau saya bikinkan contoh **citra kecil (4x4)** lalu dihitung hasil contrast stretching-nya biar lebih gampang dipahami?



Oke 👍, kita bahas **rumus operasi kontras** dalam pengolahan citra digital.  

---

## 🔹 1. Definisi Singkat
Operasi kontras bertujuan untuk **meregangkan (stretching)** atau **meningkatkan perbedaan** antar nilai intensitas piksel supaya citra terlihat lebih jelas.  

---

## 🔹 2. Rumus Linear Contrast Stretching
Rumus umum:  

\[
f'(x,y) = \alpha \cdot f(x,y) + \beta
\]

dengan:
- \( f(x,y) \) = intensitas piksel asli  
- \( f'(x,y) \) = intensitas piksel hasil  
- \( \alpha \) = faktor kontras (slope)  
- \( \beta \) = faktor kecerahan (brightness, biasanya ditambah/dikurangi)  

👉 Kalau \(\alpha > 1\), kontras meningkat.  
👉 Kalau \(0 < \alpha < 1\), kontras menurun.  

---

## 🔹 3. Rumus Contrast Stretching (Normalisasi)
Jika ingin merenggangkan intensitas dari \([f_{min}, f_{max}]\) ke rentang \([0, L-1]\):  

\[
f'(x,y) = \frac{(f(x,y) - f_{min})}{(f_{max} - f_{min})} \cdot (L-1)
\]

dengan:
- \( f_{min} \) = intensitas minimum dalam citra  
- \( f_{max} \) = intensitas maksimum dalam citra  
- \( L \) = jumlah tingkat keabuan (misal 256 untuk 8-bit)  

👉 Hasilnya: citra akan lebih kontras karena nilai piksel dipetakan ke seluruh rentang 0–255.

---

## 🔹 4. Contoh
Misal citra punya intensitas hanya di rentang [50–150], maka:
\[
f'(x,y) = \frac{(f(x,y) - 50)}{(150 - 50)} \times 255
\]
→ Intensitas akan diregangkan ke [0–255].

---

📌 **Kesimpulan:**  
- Rumus sederhana: \( f'(x,y) = \alpha f(x,y) + \beta \)  
- Rumus **contrast stretching (normalisasi):**  
  \[
  f'(x,y) = \frac{(f(x,y) - f_{min})}{(f_{max} - f_{min})} \cdot (L-1)
  \]

---

Mau saya bikinkan contoh **citra kecil (4x4)** lalu dihitung hasil contrast stretching-nya biar lebih gampang dipahami?




Oke, kita fokus ke **rumus operasi contrast stretching** 📌

---

## 🔹 Rumus Operasi Contrast Stretching

Jika $f(x,y)$ adalah intensitas piksel asli, maka hasil transformasi $f'(x,y)$ adalah:

$$
f'(x,y) = \frac{f(x,y) - f_{\min}}{f_{\max} - f_{\min}} \times (L-1)
$$

dengan keterangan:

* $f(x,y)$ = intensitas piksel pada koordinat $(x,y)$
* $f'(x,y)$ = intensitas hasil setelah stretching
* $f_{\min}$ = nilai intensitas terkecil pada citra
* $f_{\max}$ = nilai intensitas terbesar pada citra
* $L$ = jumlah tingkat keabuan (misal 256 untuk citra 8-bit → $L-1 = 255$)

---

## 🔹 Inti dari Rumus

* **Normalisasi linear**: menggeser nilai intensitas agar $f_{\min}$ jadi 0, dan $f_{\max}$ jadi 255 (untuk citra 8-bit).
* Rentang sempit $[f_{\min}, f_{\max}]$ diperluas ke $[0, 255]$.
* Hasilnya: **kontras citra meningkat** karena distribusi intensitas lebih merata.

---

## 🔹 Contoh

Misal citra grayscale kecil punya rentang intensitas $[50, 150]$.
Untuk $L=256$, rumus jadi:

$$
f'(x,y) = \frac{f(x,y) - 50}{150 - 50} \times 255
$$

👉 Piksel 50 → 0,
👉 Piksel 150 → 255,
👉 Nilai di antaranya diregangkan secara proporsional.

---

📌 **Kesimpulan:**
Rumus contrast stretching pada citra adalah:

$$
f'(x,y) = \frac{f(x,y) - f_{\min}}{f_{\max} - f_{\min}} \times (L-1)
$$

untuk memetakan intensitas asli ke rentang penuh \[0, 255] sehingga **kontras meningkat**.

---

Mau saya kasih contoh **perhitungan manual dengan matriks citra 4×4** supaya lebih jelas proses stretching-nya?



Oke 👍 mari kita bahas dua teknik penting dalam pengolahan citra: **Histogram Equalization** dan **Histogram Specification (Matching)**.

---

## 🔹 1. Histogram Equalization (Perataan Histogram)

### 📌 Rumus

Jika $r$ adalah intensitas asli dan $s$ adalah intensitas hasil, maka transformasi berbasis fungsi distribusi kumulatif (CDF):

$$
s = T(r) = (L-1) \cdot \sum_{k=0}^{r} p_r(r_k)
$$

dengan:

* $L$ = jumlah tingkat keabuan (misal 256)
* $p_r(r_k)$ = probabilitas kemunculan intensitas $r_k$
* $T(r)$ = fungsi transformasi kumulatif

👉 Intinya: intensitas dipetakan menggunakan **CDF** sehingga histogram hasil lebih **seragam (uniform)**.

### 📌 Tujuan

* Menyebarkan distribusi intensitas agar lebih merata.
* Meningkatkan **kontras** citra, khususnya jika citra awal terlalu gelap/terang.
* Membuat detail tersembunyi jadi lebih terlihat.

---

## 🔹 2. Histogram Specification (Histogram Matching)

### 📌 Rumus

Langkah-langkah:

1. Hitung transformasi histogram equalization untuk citra input:

   $$
   s = T(r) = \sum_{k=0}^{r} p_r(r_k)
   $$

2. Tentukan histogram target (spesifikasi) dengan distribusi $p_z(z)$ dan hitung fungsi kumulatifnya:

   $$
   G(z) = \sum_{k=0}^{z} p_z(z_k)
   $$

3. Cari inverse dari fungsi target:

   $$
   z = G^{-1}(s)
   $$

👉 Sehingga intensitas hasil $z$ didapat dari pemetaan antara distribusi input ke distribusi target.

### 📌 Tujuan

* Mengubah histogram citra agar mengikuti bentuk histogram tertentu yang diinginkan.
* Bisa dipakai untuk:

  * Menyamakan pencahayaan antar citra (misal pada video frame).
  * Membuat citra mirip dengan "gaya" pencahayaan citra lain (contoh citra referensi).
  * Penyesuaian citra medis agar sesuai standar tertentu.

---

## 🔹 Kesimpulan

* **Histogram Equalization**:

  $$
  s = (L-1) \cdot \sum_{k=0}^{r} p_r(r_k)
  $$

  → **Tujuan:** meratakan distribusi intensitas → meningkatkan kontras.

* **Histogram Specification**:

  $$
  z = G^{-1}(T(r))
  $$

  → **Tujuan:** memodifikasi histogram agar sesuai dengan distribusi target (ditentukan pengguna).

---

Mau saya buatkan **contoh implementasi Python** (Equalization vs Specification) biar lebih jelas bedanya secara visual?
