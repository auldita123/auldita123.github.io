# ========== Presentasi besok ==========
## Code
# File 1 — `1_rgb_to_grayscale.py`

```python
# 1_rgb_to_grayscale.py
# Baca "apel.png", konversi RGB -> Grayscale (dua metode: rata-rata atau luminosity)
# Tampilkan citra asli + grayscale, serta histogram (diagram kartesius)

import cv2
import numpy as np
import matplotlib.pyplot as plt

# --- konfigurasi ---
img_path = "apel.png"
method = "lum"   # "avg" untuk rata-rata, "lum" untuk luminosity (0.3R + 0.6G + 0.1B)

# --- baca gambar (OpenCV baca BGR) ---
img = cv2.imread(img_path)
if img is None:
    raise FileNotFoundError(f"Tidak dapat menemukan file: {img_path}")

b, g, r = cv2.split(img)          # gunakan split seperti diminta

# --- konversi ke grayscale ---
if method == "avg":
    gray = ((r.astype(np.float32) + g + b) / 3.0).round().astype(np.uint8)
else:
    # luminosity
    gray = (0.3 * r + 0.6 * g + 0.1 * b).round().astype(np.uint8)

# --- histogram data ---
hist_r, _ = np.histogram(r.flatten(), bins=256, range=(0,255))
hist_g, _ = np.histogram(g.flatten(), bins=256, range=(0,255))
hist_b, _ = np.histogram(b.flatten(), bins=256, range=(0,255))
hist_gray, bins = np.histogram(gray.flatten(), bins=256, range=(0,255))
bins_centers = (bins[:-1] + bins[1:]) / 2

# --- tampilkan hasil ---
plt.figure(figsize=(12,6))

plt.subplot(2,3,1)
plt.imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
plt.title("Asli (RGB)")
plt.axis('off')

plt.subplot(2,3,2)
plt.imshow(gray, cmap='gray')
plt.title(f"Grayscale ({'rata-rata' if method=='avg' else 'luminosity'})")
plt.axis('off')

plt.subplot(2,3,4)
plt.plot(bins_centers, hist_r, label='R')
plt.plot(bins_centers, hist_g, label='G')
plt.plot(bins_centers, hist_b, label='B')
plt.title("Histogram Kanal Warna (R,G,B)")
plt.xlabel("Intensitas")
plt.ylabel("Frekuensi")
plt.legend()

plt.subplot(2,3,5)
plt.plot(bins_centers, hist_gray, label='Gray')
plt.title("Histogram Grayscale")
plt.xlabel("Intensitas")
plt.ylabel("Frekuensi")
plt.tight_layout()
plt.show()

# simpan hasil grayscale
cv2.imwrite("grayscale_1.png", gray)
print("Selesai: 'grayscale_1.png' tersimpan.")
```

---

# File 2 — `2_negasi_k.py`

```python
# 2_negasi_k.py
# Lakukan negasi pada citra: out = k - I
# Catatan: sebelum negasi, ubah dulu ke grayscale (luminosity)
# Tampilkan citra asli (grayscale), hasil negasi, dan histogram masing-masing

import cv2
import numpy as np
import matplotlib.pyplot as plt

img_path = "apel.png"
k = 255   # ubah nilai k secara manual sesuai kebutuhan

img = cv2.imread(img_path)
if img is None:
    raise FileNotFoundError(f"Tidak dapat menemukan file: {img_path}")

b, g, r = cv2.split(img)
# grayscale (luminosity)
gray = (0.3 * r + 0.6 * g + 0.1 * b).round().astype(np.uint8)

# negasi dengan k
neg = (k - gray).astype(np.int16)   # sementara agar tidak overflow
neg = np.clip(neg, 0, 255).astype(np.uint8)

# histogram
hist_gray, bins = np.histogram(gray.flatten(), bins=256, range=(0,255))
hist_neg, _ = np.histogram(neg.flatten(), bins=256, range=(0,255))
bins_centers = (bins[:-1] + bins[1:]) / 2

plt.figure(figsize=(10,5))
plt.subplot(2,2,1)
plt.imshow(gray, cmap='gray')
plt.title("Grayscale (sebelum negasi)")
plt.axis('off')

plt.subplot(2,2,2)
plt.imshow(neg, cmap='gray')
plt.title(f"Negasi (k - I)  k={k}")
plt.axis('off')

plt.subplot(2,2,3)
plt.plot(bins_centers, hist_gray)
plt.title("Histogram Grayscale")
plt.xlabel("Intensitas")
plt.ylabel("Frekuensi")

plt.subplot(2,2,4)
plt.plot(bins_centers, hist_neg)
plt.title("Histogram Negasi")
plt.xlabel("Intensitas")
plt.ylabel("Frekuensi")

plt.tight_layout()
plt.show()

cv2.imwrite("negasi_k.png", neg)
print("Selesai: 'negasi_k.png' tersimpan.")
```

---

# File 3 — `3_brightness_k.py`

```python
# 3_brightness_k.py
# Ubah brightness: out = I + k  (k bisa negatif untuk meredupkan)
# Gunakan grayscale terlebih dahulu (luminosity)
# Tampilkan citra asli, hasil, dan histogram

import cv2
import numpy as np
import matplotlib.pyplot as plt

img_path = "apel.png"
k = 40   # ubah manual; bisa negatif

img = cv2.imread(img_path)
if img is None:
    raise FileNotFoundError(f"Tidak dapat menemukan file: {img_path}")

b, g, r = cv2.split(img)
gray = (0.3 * r + 0.6 * g + 0.1 * b).round().astype(np.uint8)

# brightness change
bright = gray.astype(np.int16) + int(k)   # sementara tipe int untuk mencegah overflow
bright = np.clip(bright, 0, 255).astype(np.uint8)

# histogram
hist_gray, bins = np.histogram(gray.flatten(), bins=256, range=(0,255))
hist_bright, _ = np.histogram(bright.flatten(), bins=256, range=(0,255))
bins_centers = (bins[:-1] + bins[1:]) / 2

plt.figure(figsize=(10,5))
plt.subplot(2,2,1)
plt.imshow(gray, cmap='gray')
plt.title("Grayscale (sebelum)")
plt.axis('off')

plt.subplot(2,2,2)
plt.imshow(bright, cmap='gray')
plt.title(f"Brightness I + k (k={k})")
plt.axis('off')

plt.subplot(2,2,3)
plt.plot(bins_centers, hist_gray)
plt.title("Histogram Sebelum")
plt.xlabel("Intensitas")
plt.ylabel("Frekuensi")

plt.subplot(2,2,4)
plt.plot(bins_centers, hist_bright)
plt.title("Histogram Sesudah")
plt.xlabel("Intensitas")
plt.ylabel("Frekuensi")

plt.tight_layout()
plt.show()

cv2.imwrite("brightness_k.png", bright)
print("Selesai: 'brightness_k.png' tersimpan.")
```

---

# File 4 — `4_contrast_stretching.py`

```python
# 4_contrast_stretching.py
# Contrast stretching sederhana: mapping linear dari [min, max] ke [0,255]
# Ubah ke grayscale dulu (luminosity)
# Tampilkan citra asli, hasil, dan histogram

import cv2
import numpy as np
import matplotlib.pyplot as plt

img_path = "apel.png"

img = cv2.imread(img_path)
if img is None:
    raise FileNotFoundError(f"Tidak dapat menemukan file: {img_path}")

b, g, r = cv2.split(img)
gray = (0.3 * r + 0.6 * g + 0.1 * b).round().astype(np.uint8)

minI = gray.min()
maxI = gray.max()

if maxI == minI:
    stretched = gray.copy()
else:
    stretched = ((gray.astype(np.float32) - float(minI)) / (float(maxI - minI)) * 255.0)
    stretched = np.clip(stretched.round(), 0, 255).astype(np.uint8)

# histogram
hist_gray, bins = np.histogram(gray.flatten(), bins=256, range=(0,255))
hist_stretch, _ = np.histogram(stretched.flatten(), bins=256, range=(0,255))
bins_centers = (bins[:-1] + bins[1:]) / 2

plt.figure(figsize=(10,5))
plt.subplot(2,2,1)
plt.imshow(gray, cmap='gray')
plt.title(f"Grayscale (min={minI}, max={maxI})")
plt.axis('off')

plt.subplot(2,2,2)
plt.imshow(stretched, cmap='gray')
plt.title("Contrast Stretching")
plt.axis('off')

plt.subplot(2,2,3)
plt.plot(bins_centers, hist_gray)
plt.title("Histogram Sebelum")
plt.xlabel("Intensitas")
plt.ylabel("Frekuensi")

plt.subplot(2,2,4)
plt.plot(bins_centers, hist_stretch)
plt.title("Histogram Sesudah")
plt.xlabel("Intensitas")
plt.ylabel("Frekuensi")

plt.tight_layout()
plt.show()

cv2.imwrite("contrast_stretching.png", stretched)
print("Selesai: 'contrast_stretching.png' tersimpan.")
```

---

# File 5 — `5_hist_equal_spec.py`

```python
# 5_hist_equal_spec.py
# Histogram equalization (manual) dan histogram specification (manual)
# Gunakan grayscale terlebih dahulu (luminosity)
# Tampilkan original, equalized, specified + histogram masing-masing

import cv2
import numpy as np
import matplotlib.pyplot as plt

img_path = "apel.png"

img = cv2.imread(img_path)
if img is None:
    raise FileNotFoundError(f"Tidak dapat menemukan file: {img_path}")

b, g, r = cv2.split(img)
gray = (0.3 * r + 0.6 * g + 0.1 * b).round().astype(np.uint8)

# --- Histogram Equalization (manual) ---
hist, bins = np.histogram(gray.flatten(), bins=256, range=(0,255))
pdf = hist.astype(np.float64) / hist.sum()
cdf = pdf.cumsum()
map_eq = np.floor(255 * cdf).astype(np.uint8)   # mapping 0..255

equalized = map_eq[gray]   # apply mapping

# --- Histogram Specification (matching) ---
# Kita buat target PDF secara programatik (contoh: Gaussian tertempat)
x = np.arange(256)
mu = 150.0
sigma = 30.0
target_pdf = np.exp(-0.5 * ((x - mu)/sigma)**2)
target_pdf = target_pdf / target_pdf.sum()
target_cdf = target_pdf.cumsum()

# Sumber cdf (dari grayscale asli)
src_hist, _ = np.histogram(gray.flatten(), bins=256, range=(0,255))
src_pdf = src_hist.astype(np.float64) / src_hist.sum()
src_cdf = src_pdf.cumsum()

# mapping spesifikasi: untuk setiap level r cari s sehingga src_cdf[r] ~ target_cdf[s]
map_spec = np.zeros(256, dtype=np.uint8)
for r in range(256):
    # cari s minimal dimana target_cdf[s] >= src_cdf[r]
    s = np.searchsorted(target_cdf, src_cdf[r])
    if s > 255:
        s = 255
    map_spec[r] = s

specified = map_spec[gray]

# --- histogram data ---
bins_centers = (bins[:-1] + bins[1:]) / 2
hist_orig, _ = np.histogram(gray.flatten(), bins=256, range=(0,255))
hist_eq, _ = np.histogram(equalized.flatten(), bins=256, range=(0,255))
hist_spec, _ = np.histogram(specified.flatten(), bins=256, range=(0,255))

# --- tampilkan ---
plt.figure(figsize=(12,8))

plt.subplot(3,3,1)
plt.imshow(gray, cmap='gray')
plt.title("Original Grayscale")
plt.axis('off')

plt.subplot(3,3,2)
plt.imshow(equalized, cmap='gray')
plt.title("Histogram Equalization")
plt.axis('off')

plt.subplot(3,3,3)
plt.imshow(specified, cmap='gray')
plt.title("Histogram Specification (target Gaussian)")
plt.axis('off')

plt.subplot(3,3,4)
plt.plot(bins_centers, hist_orig)
plt.title("Histogram Original")
plt.xlabel("Intensitas")
plt.ylabel("Frekuensi")

plt.subplot(3,3,5)
plt.plot(bins_centers, hist_eq)
plt.title("Histogram Equalized")
plt.xlabel("Intensitas")
plt.ylabel("Frekuensi")

plt.subplot(3,3,6)
plt.plot(bins_centers, hist_spec)
plt.title("Histogram Specified")
plt.xlabel("Intensitas")
plt.ylabel("Frekuensi")

# tampilkan juga PDF target sebagai referensi
plt.subplot(3,3,8)
plt.plot(x, target_pdf)
plt.title("Target PDF (normalisasi)")
plt.xlabel("Intensitas")
plt.ylabel("Prob.")

plt.tight_layout()
plt.show()

cv2.imwrite("hist_equalized.png", equalized)
cv2.imwrite("hist_specified.png", specified)
print("Selesai: 'hist_equalized.png' dan 'hist_specified.png' tersimpan.")
```

---

## Catatan singkat (penting & sesuai permintaan)

* Semua skrip membaca file bernama `"apel.png"` dari direktori kerja. Pastikan file ada di folder yang sama saat menjalankan.
* Saya memakai `cv2`, `numpy`, `matplotlib` sesuai saran. Tidak menggunakan toolbox eksternal lain.
* Untuk pemisahan kanal saya menggunakan `cv2.split()` seperti diminta.
* Nomor 2, 3, 4, 5 mengubah citra ke grayscale (luminosity) sebelum operasi sesuai perintah.
* Variabel `k` pada file 2 dan 3 berada di bagian konfigurasi atas sehingga mudah diubah manual.
* Kode dibuat sederhana, sedikit percabangan, tidak banyak definisi fungsi, mudah dibaca untuk praktikum.
* Jika Anda ingin metode grayscale berbeda (mis. rata-rata), ubah `method` di file 1 atau sesuaikan konversi di file lain.

Kalau mau, saya bisa gabungkan semuanya menjadi satu menu interaktif (opsional) — tapi kalau praktikum Anda menginginkan file per nomor, format sekarang sudah sesuai. Mau saya tambahkan komentar langkah demi langkah atau contoh nilai `k` yang direkomendasikan?

## Penjelasan
Berikut penjelasan lengkap, jelas, dan ber-point per operasi (1–5). Saya tulis dalam Bahasa Indonesia, setiap operasi dipisah agar mudah dibaca — termasuk tujuan, rumus, langkah implementasi (sesuai permintaan: pakai `cv2`, `numpy`, `matplotlib`, pakai `split`), catatan tipe data, batasan/kesalahan umum, dan saran parameter.

# 1. RGB → Grayscale

**Tujuan**

* Mengubah citra berwarna (3 kanal: R, G, B) menjadi citra intensitas tunggal (grayscale) untuk memudahkan operasi berikutnya.

**Metode / Rumus yang diminta**

* Rata-rata: `I = (R + G + B) / 3`
* Luminosity: `I = 0.3*R + 0.6*G + 0.1*B`
  (Luminosity menimbang komponen hijau lebih besar karena mata manusia lebih sensitif terhadap hijau.)

**Langkah implementasi (ringkas)**

1. Baca gambar dengan `cv2.imread("apel.png")` → format BGR.
2. Gunakan `b, g, r = cv2.split(img)` (diminta pakai `split`).
3. Terapkan rumus (pakai `numpy` vectorized):

   * rata-rata: `gray = ((r.astype(float) + g + b) / 3).round().astype(np.uint8)`
   * luminosity: `gray = (0.3*r + 0.6*g + 0.1*b).round().astype(np.uint8)`
4. Simpan: `cv2.imwrite("grayscale.png", gray)`.
5. Tampilkan histogram: `numpy.histogram(gray.flatten(), bins=256, range=(0,255))` dan plot dengan `matplotlib` (diagram kartesius) menggunakan `plt.plot(bins_centers, counts)`.

**Catatan tipe data**

* Lakukan operasi dalam `float` atau `float32` lalu `round()` dan `astype(np.uint8)` untuk menghindari pembulatan negatif dan overflow.

**Harapan visual / histogram**

* Grayscale terdistribusi menurut intensitas campuran kanal; luminosity biasanya menghasilkan gambar lebih natural dibanding rata-rata.

---

# 2. Operasi Negasi (dengan variabel `k`)

**Tujuan**

* Membalikkan intensitas citra: titik gelap → terang dan terang → gelap, dengan konstanta `k` yang bisa diubah manual.

**Rumus**

* `out = k - I`
  (Biasanya `k = 255` untuk citra 8-bit sehingga `out = 255 - I`.)

**Syarat awal**

* **Wajib** ubah ke grayscale dulu (sesuai permintaan). Operasikan pada array grayscale.

**Langkah implementasi**

1. Hitung `gray` sesuai langkah operasi 1.
2. Tetapkan `k` di bagian konfigurasi (mis. `k = 255`).
3. Hitung: `neg = k - gray.astype(np.int16)` (pakai tipe temp `int16` agar negatif tidak overflow).
4. Clip dan konversi: `neg = np.clip(neg, 0, 255).astype(np.uint8)`.
5. Simpan dan tampilkan citra dan histogram.

**Catatan / Pitfalls**

* Gunakan `int16` atau `int32` saat operasi aritmetika sebelum `clip` untuk menghindari wraparound pada `uint8`.
* Jika `k` lebih kecil dari 255 (mis. 200), hasil masih valid — intensitas akan ter-shift; namun visualnya berbeda (tidak “pembalikan penuh”).

**Saran nilai `k`**

* Umumnya `k = 255`. Eksperimen: `k` di range `[0, 255]`. Jika `k` < 255, hasil tidak sepenuhnya invers penuh (tingkat kegelapan berubah).

**Harapan histogram**

* Histogram akan terbalik secara horizontal relatif terhadap histogram asli (frekuensi intensitas `i` dipindah ke `k-i`).

---

# 3. Operasi Brightness (dengan variabel `k`, bisa negatif)

**Tujuan**

* Mengubah kecerahan citra: `k` positif → mencerahkan, `k` negatif → menggelapkan.

**Rumus**

* `out = I + k`
  (I: intensitas grayscale)

**Syarat awal**

* Konversi ke grayscale terlebih dahulu (seperti disyaratkan).

**Langkah implementasi**

1. Hitung `gray`.
2. Tetapkan `k` (mis. `k = 40` untuk mencerahkan, `k = -40` untuk menggelapkan).
3. Operasi dengan tipe yang aman: `bright = gray.astype(np.int16) + int(k)`.
4. `bright = np.clip(bright, 0, 255).astype(np.uint8)`.
5. Simpan dan tampilkan citra serta histogram.

**Catatan penting**

* Harus `clip` supaya tidak keluar dari rentang 0–255.
* Jika `k` sangat besar (mis. >255) maka hasil hampir seluruhnya putih; jika sangat negatif (< -255) hampir seluruhnya hitam.
* Pastikan `k` integer (atau cast ke `int`) untuk operasi array.

**Saran nilai praktis**

* `k` di kisaran `-100` sampai `+100` biasanya memberikan efek yang terlihat tanpa merusak detail; tapi tergantung citra asli.

**Harapan histogram**

* Seluruh histogram bergeser ke kanan jika `k` positif, ke kiri jika `k` negatif, bentuknya relatif sama kecuali karena clipping.

---

# 4. Contrast Stretching (Linear Contrast Stretch)

**Tujuan**

* Memperluas rentang intensitas citra agar memanfaatkan full range 0–255. Membuat citra tampak lebih kontras.

**Rumus (linear mapping)**

* Jika `minI = min(I)` dan `maxI = max(I)`, maka:
  `out = (I - minI) / (maxI - minI) * 255`
* Jika `maxI == minI` → citra seragam, tidak ada perubahan.

**Langkah implementasi**

1. Konversi ke grayscale.
2. Hitung `minI = gray.min()` dan `maxI = gray.max()`.
3. Jika `maxI == minI`, hasil = copy(gray) (tidak ada stretching).
4. Jika tidak, lakukan per-pixel:
   `stretched = ((gray.astype(float) - minI) / (maxI - minI) * 255).round().astype(np.uint8)`
5. Simpan dan tampilkan hasil + histogram.

**Catatan**

* Lakukan operasi dalam `float` agar tidak kehilangan presisi sebelum rounding.
* Setelah mapping, histogram biasanya “tersebar” lebih luas, sehingga detail di bayangan/terang muncul.
* Jika ada outlier sangat gelap/terang, `minI` atau `maxI` ekstrem bisa membuat sebagian besar piksel hanya sedikit berubah — pertimbangkan clipping/persentil (opsional) jika ingin mengabaikan outlier (tetapi user minta sederhana jadi gunakan min/max).

**Harapan histogram**

* Histogram setelah stretching akan menempati hampir seluruh rentang 0–255; bentuk menjadi lebih melebar.

---

# 5. Histogram Equalization & Histogram Specification (Matching)

**Tujuan**

* **Histogram equalization**: redistribusi intensitas agar histogram menjadi lebih merata, meningkatkan kontras lokal di area dengan konsentrasi intensitas.
* **Histogram specification (matching)**: ubah histogram citra sumber agar menyerupai histogram target yang ditentukan (mis. Gaussian, uniform, atau histogram citra lain).

**A. Histogram equalization (manual) — langkah**

1. Konversi ke grayscale.
2. Hitung histogram: `hist, _ = np.histogram(gray.flatten(), bins=256, range=(0,255))`.
3. Hitung PDF: `pdf = hist / hist.sum()`.
4. Hitung CDF: `cdf = pdf.cumsum()`.
5. Mapping (discrete): `map_eq = floor(255 * cdf).astype(np.uint8)`.
   Ini menghasilkan pemetaan nilai sumber `r` → nilai baru `s = map_eq[r]`.
6. Terapkan: `equalized = map_eq[gray]`.
7. Simpan & tampilkan citra + histogram hasil.

**Penjelasan mengapa ini bekerja**

* CDF memberikan akumulasi probabilitas; mengalikan dengan 255 → pemetaan ke rentang penuh sehingga intensitas yang sering muncul dipindah agar tersebar.

**Catatan implementasi**

* Lakukan semua perhitungan numerik dalam `float64` atau `float32` untuk presisi.
* Pemetaan menggunakan array lookup sangat efisien: `map_eq[gray]` (vectorized).

**Harapan histogram**

* Histogram hasil cenderung lebih merata (tergantung distribusi asli), visualnya sering meningkatkan kontras pada rentang yang kurang digunakan.

---

**B. Histogram specification (matching) — langkah**

1. Konversi ke grayscale.
2. Hitung `src_hist` → `src_pdf` → `src_cdf`.
3. Tentukan `target_pdf` dan hitung `target_cdf` (target bisa dari citra lain atau di-generate, mis. Gaussian).
4. Buat mapping `map_spec` sedemikian sehingga untuk setiap level `r`:

   * cari `s` minimal yang memenuhi `target_cdf[s] >= src_cdf[r]`.
     Di `numpy`: `s = np.searchsorted(target_cdf, src_cdf[r])`
   * set `map_spec[r] = s` (clip `s` ke 0..255).
5. Terapkan: `specified = map_spec[gray]`.
6. Simpan & tampilkan.

**Kenapa ini bekerja**

* Kita melakukan transformasi sumber ke domain CDF (seperti equalization), lalu mereverse-map agar menyesuaikan distribusi dengan target. Intinya: padankan kuantiles.

**Catatan praktis**

* Jika target CDF memiliki plateau atau target PDF sangat sempit, beberapa `r` akan dipetakan ke nilai `s` yang sama → hasil posterisasi/tersegmentasi di nilai intensitas tertentu.
* Untuk target PDF yang dihasilkan (mis. Gaussian), pastikan normalisasi (`target_pdf = target_pdf / target_pdf.sum()`).

**Harapan histogram**

* Histogram akhir mengikuti bentuk target (mis. menyerupai Gaussian target); kontras dan tonal akan berubah sesuai target.

---

# Catatan umum untuk semua operasi (praktis & debugging)

* **Gunakan `cv2.split()`** saat memisahkan kanal (sesuai permintaan). Ingat OpenCV membaca sebagai BGR, jadi `b,g,r = split(img)` kemudian gunakan `r,g,b` untuk rumus berbasis R,G,B.
* **Tipe data**: konversi sementara ke `float`/`int16` saat operasi aritmetika, lalu `np.clip(...,0,255).astype(np.uint8)`.
* **Histogram plotting**:

  * `hist, bins = np.histogram(img.flatten(), bins=256, range=(0,255))`
  * `bins_centers = (bins[:-1] + bins[1:]) / 2`
  * `plt.plot(bins_centers, hist)` → diagram kartesius (bukan bar) seperti diminta.
* **Clipping** wajib untuk operasi aritmetika (brightness, negasi, dll) agar tidak overflow/wraparound.
* **Jangan banyak fungsi**: sesuai permintaan, lakukan langkah berurutan tanpa mendefinisikan banyak `def`.
* **Simpan hasil** tiap file sesuai nomor (`grayscale_1.png`, `negasi_k.png`, dll) agar praktikum rapi.
* **Performa**: semua operasi ini bersifat vektor dan O(N) terhadap jumlah piksel — cukup cepat untuk gambar biasa (ratusan KB–beberapa MB).
* **Uji**: jalankan tiap file/skrip dan coba beberapa nilai `k` (mis. `k=255`, `k=40`, `k=-40`) untuk melihat efeknya.
* **Edge cases**:

  * Kontras stretching: jika `minI == maxI` → lewati operator atau kembalikan salinan.
  * Histogram matching: jika `target_cdf` tidak naik monoton (seharusnya monoton non-decreasing) atau target tidak normal, normalisasikan dulu.

---

Kalau Anda mau, saya bisa:

* Beri contoh nilai `k` yang direkomendasikan untuk praktikum (daftar singkat),
* atau ringkas tiap langkah sebagai pseudocode singkat supaya mudah dimasukkan ke dalam skrip praktikum Anda.

Mau saya tambahkan contoh nilai `k` dan sedikit checklist praktikum (cek file ada, cek path, cek tipe)?


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
