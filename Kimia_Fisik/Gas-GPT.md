# Pengantar singkat

Di bawah ini saya akan menjelaskan secara deduktif dan terstruktur — dimulai dari model paling fundamental (Gas Ideal), lalu menambahkan kompleksitas (Hukum Gas, Gas Sempurna vs Gas Nyata), menuju konsep campuran (Fraksi mol, Tekanan parsial, Tekanan total) dan akhirnya perbaikan kuantitatif untuk gas nyata (Koefisien virial). Setiap bagian berisi definisi operasional, persamaan utama (dengan penjelasan variabel), asumsi/batasan, kondisi penerapan, contoh aplikasi, dan bagaimana tiap konsep saling terkait.

---

# 1. Gas Ideal (model paling fundamental)

**Definisi operasional:** Gas ideal adalah model teoretis di mana molekul dianggap sebagai partikel titik tanpa volume sendiri dan tanpa gaya antar-molekul. Semua interaksi selain tumbukan sempurna-elastis diabaikan.

**Persamaan utama (Hukum Gas Ideal):**

pV=nRTpV = nRT

atau per-mol (menggunakan volume molar Vm=V/nV_m = V/n):

pVm=RTpV_m = RT

di mana

- pp = tekanan (Pa),
    
- VV = volume (m³),
    
- nn = jumlah mol,
    
- RR = konstanta gas universal (8.3144621 J mol−1K−18{.}3144621\ \mathrm{J\,mol^{-1}K^{-1}}),
    
- TT = temperatur mutlak (K).
    

**Asumsi & batasan:**

- Molekul tak berdimensi (volume nol).
    
- Tidak ada gaya antar-molekul (potensial nol kecuali saat tumbukan).
    
- Energi internal bergantung hanya pada temperatur (untuk gas monoatomik U=CvTU = C_v T).
    
- Berlaku baik pada tekanan rendah dan temperatur tinggi (rendah kerapatan).
    

**Kondisi penerapan:** ρ→0\rho \to 0 (kepadatan rendah), pp rendah relatif terhadap gaya antar-molekul, TT bukan mendekati titik kondensasi.

**Contoh aplikasi praktis:** Perhitungan cepat perbandingan volume, hukum termodinamika dasar (cycle ideal), estimasi awal desain sistem gas (balon, pipa) sebelum koreksi nyata.

---

# 2. Hukum Gas (empiris: Boyle, Charles, Gay-Lussac)

**Definisi operasional:** Kumpulan hukum empiris yang mengamati hubungan sederhana antara p,V,Tp, V, T pada kondisi tertentu.

**Persamaan ringkas:**

- Boyle (T konstan): pV=konstanpV = \text{konstan}.
    
- Charles (p konstan): V/T=konstanV/T = \text{konstan}.
    
- Gay-Lussac (V konstan): p/T=konstanp/T = \text{konstan}.
    

**Asumsi & batasan:** Setiap hukum hanya berlaku ketika variabel lain dijaga konstan; ini adalah batasan empiris yang diinkorporasi ke persamaan umum pV=nRTpV = nRT.

**Kondisi penerapan:** Percobaan pada rentang di mana gas berperilaku hampir ideal.

**Contoh aplikasi praktis:** Kalibrasi instrumen laboratorium, ilustrasi dasar hukum termodinamika untuk mahasiswa.

---

# 3. Gas Sempurna

**Definisi operasional:** Istilah yang kadang dipertukarkan dengan “gas ideal”, tetapi dalam literatur termodinamika “gas sempurna” sering didefinisikan lebih kuat: gas yang memenuhi persamaan keadaan ideal **dan** memiliki energi internal UU dan entalpi HH yang merupakan fungsi tunggal temperatur (tidak bergantung pada volume). Dengan kata lain, selain persamaan pV=nRTpV = nRT, juga berlaku U=U(T)U=U(T).

**Persamaan terkait:**

- pV=nRTpV = nRT (sebagai bagian dari definisi)
    
- dU=Cv(T) dTdU = C_v(T)\,dT (tanpa kontribusi volumetrik)
    

**Asumsi & batasan:** Sama dengan gas ideal + sifat termodinamika internal tergantung hanya pada TT.

**Kondisi penerapan:** Ketika kontribusi potensial antar-molekul terhadap energi dapat diabaikan (biasanya gas monoatomik atau pada kondisi sangat jarang).

**Contoh aplikasi praktis:** Analisis siklus termodinamika ideal (Carnot, Otto, Brayton) dan perhitungan energi untuk gas ideal di teknik mesin.

> Catatan: Untuk kebanyakan percakapan praktis, “gas sempurna” dan “gas ideal” sering dipakai bergantian; saya membedakan untuk menegaskan sifat U(T)U(T).

---

# 4. Gas Nyata

**Definisi operasional:** Gas nyata adalah gas yang menunjukkan deviasi dari hukum gas ideal karena volume molekul nyata dan interaksi antar-molekul (atraktif atau repulsif).

**Persamaan umum (contoh persamaan keadaan nyata):**

- Persamaan van der Waals:
    

(p+aVm2)(Vm−b)=RT\left(p + \frac{a}{V_m^2}\right)(V_m - b) = RT

dengan aa dan bb konstanta spesifik untuk tiap gas.

- Bentuk lain: Redlich–Kwong, Soave–Redlich–Kwong (SRK), Peng–Robinson (PR), dsb.
    

**Penjelasan variabel:**

- aa mengoreksi gaya tarik antar-molekul (satuan Pa m6 mol−2\mathrm{Pa\ m^6\ mol^{-2}} tergantung satuan),
    
- bb mengoreksi volume molekul sendiri (satuan m3 mol−1\mathrm{m^3\ mol^{-1}}).
    

**Asumsi & batasan:** Persamaan nyata adalah model; masing-masing berlaku lebih baik di range tekanan/temperatur tertentu. vdW sederhana menangkap esensi tetapi tidak akurat dekat kritikal.

**Kondisi penerapan:** Tekanan menengah hingga tinggi, temperatur mendekati titik kondensasi atau di bawahnya, proses industri (compressor, reaktor bertekanan).

**Contoh aplikasi praktis:** Perancangan kolom destilasi, perhitungan kinerja kompresor turbin, prediksi kondisi kondensasi dalam pipa, perhitungan sifat termodinamika untuk proses kimia.

---

# 5. Hukum Pembatas (Limit laws)

**Definisi operasional:** Prinsip matematis/physical bahwa perilaku gas nyata harus kembali ke perilaku ideal dalam limit tertentu (biasanya limit kerapatan → 0 atau p→0p \to 0 pada TT tetap). Ini juga mencakup ekstrapolasi hukum empiris ke kasus limit.

**Statement penting:**

lim⁡p→0Z=1ataulim⁡ρ→0pVm=RT\lim_{p\to 0} Z = 1 \quad\text{atau}\quad \lim_{\rho\to 0} pV_m = RT

di mana ZZ adalah faktor kompresibilitas (lihat di bawah).

**Asumsi & batasan:** Berlaku selama model yang digunakan konsisten dengan termodinamika pada limit tersebut.

**Kondisi penerapan:** Digunakan sebagai pengecekan konsistensi model persamaan keadaan; memandu ekspansi virial (lihat bagian koefisien virial).

**Aplikasi praktis:** Verifikasi model; dasar justifikasi untuk menggunakan hukum ideal pada kondisi vakum atau gas sangat encer.

---

# 6. Fraksi Mol

**Definisi operasional:** Proporsi mol dari suatu komponen dalam campuran gas.

xi=nintotalx_i = \frac{n_i}{n_\text{total}}

di mana nin_i = mol komponen ii, ntotal=∑jnjn_\text{total}=\sum_j n_j.

**Asumsi & batasan:** Fraksi mol tidak bergantung pada sifat non-idealitas langsung (definisi murni matematis), tapi hubungan tekanan parsialnya bergantung pada perilaku (ideality) campuran.

**Kondisi penerapan:** Semua analisis campuran gas, stoikiometri reaksi gas, perhitungan sifat campuran.

**Contoh aplikasi praktis:** Menentukan komposisi umpan reaktor, perhitungan titik didih parsial dalam destilasi uap (bersama Raoult’s law untuk uap-cair ideal).

---

# 7. Tekanan Parsial (Hukum Dalton)

**Definisi operasional:** Tekanan parsial adalah tekanan yang akan diberikan oleh komponen gas jika sendirian menempati seluruh volume pada temperatur sistem.

**Hukum Dalton (untuk gas ideal campuran):**

ptotal=∑ipi,pi=xiptotalp_{\text{total}} = \sum_i p_i,\qquad p_i = x_i p_{\text{total}}

di mana pip_i adalah tekanan parsial komponen ii.

**Asumsi & batasan:** Berlaku _secara tepat_ hanya jika gas berperilaku ideal atau jika komponen tidak saling berinteraksi (campuran ideal). Untuk gas nyata, tekanan parsial lebih rumit dan perlu fugacity.

**Kondisi penerapan:** Campuran gas pada tekanan rendah; desain sistem gas (mis. distribusi gas medis), analisis atmosfer.

**Contoh aplikasi praktis:** Menghitung kandungan O₂ pada campuran udara, perhitungan partial pressures dalam respirator/penyimpan gas.

---

# 8. Tekanan Total

**Definisi operasional:** Tekanan total adalah jumlah tekanan parsial semua komponen dalam campuran gas.

ptotal=∑ipip_{\text{total}} = \sum_i p_i

**Penjelasan lanjutan:** Untuk gas nyata perlu mengganti tekanan parsial pip_i dengan fugacity fif_i dan menggunakan konsep fugacity coefficient ϕi\phi_i, sehingga keadaan ekilib dan perhitungan termodinamika benar.

**Aplikasi praktis:** Penentuan kondisi operasi pipa, tangki, dan reaktor; perhitungan kestabilan fasa (termal & fasa gas-cair).

---

# 9. Koefisien Virial (Ekspansi Virial)

**Definisi operasional:** Virial expansion adalah cara sistematik untuk menulis deviasi dari gas ideal sebagai deret dalam fungsi kerapatan (atau 1/VmV_m). Koefisien virial fungsi temperatur dan mengandung informasi tentang interaksi multi-molekul.

**Ekspansi dalam volume molar VmV_m:**

pVmRT=Z=1+B(T)Vm+C(T)Vm2+D(T)Vm3+⋯\frac{pV_m}{RT} = Z = 1 + \frac{B(T)}{V_m} + \frac{C(T)}{V_m^2} + \frac{D(T)}{V_m^3} + \cdots

atau dalam kerapatan molar ρ=1/Vm\rho = 1/V_m:

Z=1+B(T) ρ+C(T) ρ2+⋯Z = 1 + B(T)\,\rho + C(T)\,\rho^2 + \cdots

di mana

- ZZ = faktor kompresibilitas,
    
- B(T),C(T),…B(T), C(T), \ldots = koefisien virial berurutan (fungsi TT).
    

**Interpretasi fisik:**

- B(T)B(T) mencerminkan kontribusi pasangan interaksi (dua-molekul),
    
- C(T)C(T) kontribusi tiga-molekul, dan seterusnya.
    

**Hubungan dengan persamaan van der Waals (contoh konkret):** Untuk vdW,

B(T)=b−aRTB(T) = b - \frac{a}{RT}

yang memberikan gambaran bagaimana parameter a,ba,b memengaruhi deviasi pada urutan pertama.

**Asumsi & batasan:** Ekspansi konvergen pada kepadatan rendah hingga menengah; tidak cocok sangat dekat titik kritikal atau fase cair.

**Kondisi penerapan:** Ketika deviasi kecil sampai sedang — ekspansi virial memberikan cara kuantitatif memperbaiki hukum gas ideal.

**Contoh aplikasi praktis:** Menyesuaikan data tekanan-volume pada eksperimen, ekstraksi koefisien interaksi dari data eksperimental, menghitung koreksi densitas untuk termodinamik proses.

---

# Hubungan logis & kuantitatif antar konsep (deduktif)

1. **Mulai dari Gas Ideal:** asumsi dasar → pV=nRTpV = nRT. Ini prediksi nol untuk deviasi; cocok pada limit p→0p\to 0, ρ→0\rho\to0.  
    → **Hukum Pembatas**: menegaskan bahwa model nyata harus mengembalikan hukum ini pada limit tersebut.
    
2. **Hukum Gas (empiris)** memberi bentuk-bentuk khusus pemeriksaan terpisah (Boyle, Charles...). Semua itu dijelaskan secara umum oleh persamaan gas ideal.
    
3. **Gas Sempurna** menambahkan sifat termodinamika: energi internal hanya fungsi TT. Ini penting untuk analisis energi pada siklus termodinamika (mengizinkan pemisahan pengaruh tekanan/volume).
    
4. **Pengamatan deviasi** (eksperimen: PV/T ≠ konst pada tekanan menengah) → diperlukan _model gas nyata_. Van der Waals adalah langkah pertama yang memperkenalkan parameter a,ba,b untuk menangani atraksi dan volume sendiri. Ini adalah model fenomenologis.
    
5. **Koefisien Virial** muncul sebagai pendekatan teratur: ekspansi dalam kepadatan. Pada densitas rendah, hanya B(T)B(T) yang signifikan sehingga:
    

Z≈1+B(T)Vm.Z \approx 1 + \frac{B(T)}{V_m}.

Untuk van der Waals, B(T)=b−aRTB(T)= b-\frac{a}{RT}, menunjukkan hubungan kuantitatif antara parameter vdW dan koefisien virial.

6. **Tekanan Parsial dan Fraksi Mol** (untuk campuran): Dalam limit ideal, Dalton berlaku (pi=xipp_i = x_i p). Untuk gas nyata, konsep _fugacity_ fif_i menggantikan pip_i:
    

μi=μi∘(T)+RTln⁡fif∘\mu_i = \mu_i^\circ(T) + RT\ln\frac{f_i}{f^\circ}

dengan fi=ϕipif_i = \phi_i p_i dan ϕi\phi_i = koefisien fugacity (dekorasi dari 1 menunjukkan non-idealitas). Koefisien fugacity sendiri bisa diekspresikan melalui integral ZZ yang terkait dengan koefisien virial.

7. **Keterkaitan praktis:**
    
    - Pada operasi industri: jika ZZ dekat 1, gunakan hukum ideal; jika tidak, pakai koreksi via virial atau persamaan keadaan nyata.
        
    - Untuk campuran: jika setiap komponen hampir ideal, pakai Dalton + xix_i; jika tidak, gunakan fugacity dan model campuran (koreksi virial campuran atau persamaan keadaan campuran SRK/PR).
        

---

# Ringkasan kuantitatif penting & rumus rujukan cepat

- Gas ideal: pV=nRTpV = nRT.
    
- Faktor kompresibilitas: Z=pVmRTZ = \dfrac{pV_m}{RT}. Ideal ⇒ Z=1Z=1.
    
- Virial expansion: Z=1+B(T)Vm+C(T)Vm2+⋯Z = 1 + \dfrac{B(T)}{V_m} + \dfrac{C(T)}{V_m^2} + \cdots.
    
- van der Waals: (p+aVm2)(Vm−b)=RT\left(p + \dfrac{a}{V_m^2}\right)(V_m - b)=RT → B(T)=b−aRTB(T)=b-\dfrac{a}{RT} (pertama).
    
- Dalton: ptot=∑pip_{\text{tot}}=\sum p_i dan ideal campuran: pi=xiptotp_i = x_i p_{\text{tot}}.
    
- Fugacity (untuk gas nyata): fi=ϕipif_i=\phi_i p_i, dengan ϕi\phi_i dihitung dari model keadaan nyata atau integrasi ZZ.
    

---

# Contoh aplikasi singkat (praktis)

1. **Desain tangki gas:** Jika operasi pada 1 bar dan temperatur jauh dari titik kritis → gunakan gas ideal untuk estimasi kapasitas; jika 100 bar → gunakan PR atau SRK atau koreksi virial.
    
2. **Analisis reaksi gas:** Gunakan fraksi mol dan tekanan parsial untuk menghitung laju reaksi, tetapi jika tekanan tinggi periksa fugacity.
    
3. **Penentuan sifat termodinamika dari data eksperimen:** Fit data pp vs VmV_m untuk mendapatkan B(T)B(T) (virial pertama) dan gunakan untuk mengekstrapolasi perilaku pada kerapatan rendah.
    
4. **Kromatografi gas / respirator medis:** Perhitungan partial pressures (idealis) untuk menentukan fraksi komponen; jika uap jenuh/kelembapan tinggi, koreksi non-ideal diperlukan.
    

---

# Penutup — Panduan praktek untuk pemilihan model

1. **Mulai dengan gas ideal** sebagai referensi dan cek faktor kompresibilitas ZZ dari data atau aturan praktis.
    
2. **Jika deviasi kecil (|Z−1| ≪ 0.01)**: koreksi kecil tidak perlu; gunakan gas ideal/sempurna.
    
3. **Jika deviasi moderat**: gunakan ekspansi virial (hanya beberapa koefisien pertama) jika kepadatan tidak terlalu tinggi.
    
4. **Jika tekanan/temperatur dekat kondisi kritikal atau tinggi**: gunakan persamaan keadaan nyata (vdW sederhana untuk pemahaman, SRK/PR untuk perhitungan industri).
    
5. **Untuk campuran**: mulai dengan Dalton + fraksi mol; pindah ke fugacity dan persamaan keadaan campuran bila diperlukan.
    

---

Kalau Anda ingin, saya bisa:

- Tunjukkan contoh numerik langkah-demi-langkah (mis. hitung B(T)B(T) dari data, bandingkan ZZ ideal vs vdW pada kondisi tertentu).
    
- Menyusun tabel perbandingan akurasi persamaan keadaan (vdW, SRK, PR) pada rentang T,pT,p tertentu.  
    Mau saya kerjakan contoh numerik atau aplikasi tertentu sekarang?