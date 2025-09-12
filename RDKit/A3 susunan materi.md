# **Materi RDKit**

Berikut adalah ekstraksi, terjemahan, dan penyusunan teks sesuai dengan struktur di [RDKit Documentation - Getting Started in Python](https://www.rdkit.org/docs/GettingStartedInPython.html):

---

### **1. Memulai dengan RDKit dalam Python**  
- Catatan penting  
  - Apa ini?  
- **Membaca, Menggambar, dan Menulis Molekul**  
  - Membaca molekul tunggal  
  - Membaca kumpulan molekul  
  - Menulis molekul  
  - Menulis kumpulan molekul  

### **2. Bekerja dengan Molekul**  
- Iterasi melalui Atom dan Ikatan  
- Informasi cincin (Ring Information)  
- Memodifikasi molekul  
- Bekerja dengan molekul 2D:  
  - Menghasilkan penggambaran (Generating Depictions)  
- Bekerja dengan molekul 3D  
- Menyimpan molekul (Preserving Molecules)  
- Menggambar molekul  
- Metadata dalam gambar molekul  

### **3. Pencarian Substruktur (Substructure Searching)**  
- Stereokimia dalam pencocokan substruktur  
- Indeks Peta Atom dalam SMARTS  
- Pencocokan substruktur lanjutan  

### **4. Transformasi Kimia (Chemical Transformations)**  
- Transformasi berbasis substruktur  
- Dekomposisi Murcko  

### **5. Substruktur Umum Maksimum (Maximum Common Substructure)**  
- FMCS (Flexible Maximum Common Substructure)  
- RascalMCES  
- Pengelompokan dengan Rascal  

### **6. Fingerprinting dan Kesamaan Molekuler**  
- Fingerprint RDKit (Topologis)  
- Pasangan Atom dan Torsion Topologis  
- Fingerprint Morgan (Circular Fingerprints)  
- Kunci MACCS  
- Menjelaskan bit dari fingerprint  
- Fingerprint Morgan  
- Fingerprint RDKit  
- Menghasilkan gambar bit fingerprint  
- Memilih molekul beragam menggunakan fingerprint  

### **7. Perhitungan Deskriptor (Descriptor Calculation)**  
- Menghitung semua deskriptor  
- Menghitung muatan parsial  
- Visualisasi deskriptor  

### **8. Reaksi Kimia (Chemical Reactions)**  
- Menggambar reaksi kimia  
- Fungsi reaksi lanjutan:  
  - Melindungi atom (Protecting Atoms)  
  - Implementasi RECAP  
  - Implementasi BRICS  
  - Pendekatan fragmentasi lainnya  

### **9. Fitur Kimia dan Farmakofor**  
- Fitur kimia  
- Fingerprint farmakofor 2D  
- Fragmen molekul  
- Dekomposisi R-Group  

### **10. Mencari Ruang Synthon (Searching Synthon Spaces)**  
- Cara kerjanya:  
  - Persiapan basis data  
  - Penggunaan memori  
  - Pencarian substruktur  
  - Pencarian kesamaan  
  - Membatasi hasil (Limiting Hits)  

### **11. Fungsi Non-Kimia**  
- Vektor bit (Bit vectors)  
- Vektor nilai diskrit  
- Grid 3D  
- Titik (Points)  

### **12. Bantuan dan Topik Lanjutan**  
- Topik lanjutan/Peringatan:  
  - Mengedit molekul  
- Tips dan petunjuk tambahan:  
  - Perbandingan `Chem` vs `AllChem`  
  - Masalah SSSR (SSSR Problem)  

### **13. Daftar Alat Tersedia**  
- Daftar deskriptor yang tersedia  
- Daftar deskriptor 3D yang tersedia  
- Daftar fingerprint yang tersedia  
- Definisi fitur dalam fingerprint Morgan  

### **14. Penyaringan Dataset Molekuler**  
- Aturan Lipinski (Lipinski Rule of 5)  
- Menyaring substruktur yang tidak diinginkan  

### **15. Lisensi**  

---

### **Catatan Terjemahan**  
- Istilah teknis (misalnya "fingerprint", "substructure") dipertahankan dalam bahasa Inggris karena merupakan konsep spesifik dalam kimia komputasi.  
- Beberapa frasa diperbaiki untuk mencocokkan dokumentasi resmi RDKit (misalnya: "RascalMCES" bukan "Pascal").  
- Struktur mengikuti urutan dari [dokumentasi RDKit](https://www.rdkit.org/docs/GettingStartedInPython.html) dengan penyesuaian minor untuk konsistensi.  

Jika Anda memerlukan penjelasan lebih rinci tentang bagian tertentu, silakan merujuk langsung ke tautan di atas.