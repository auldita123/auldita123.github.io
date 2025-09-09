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
