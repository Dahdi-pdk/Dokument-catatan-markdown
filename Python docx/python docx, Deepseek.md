Berikut adalah daftar komprehensif method dalam library `python-docx`, diorganisir berdasarkan kelas utama:

---

## **1. Document Class (`docx.document.Document`)**
Kelas utama untuk membuat/memodifikasi dokumen Word.

### **Konstruktor**
- `Document(docx=None)`  
  Membuat dokumen baru atau membuka dokumen yang ada.  
  **Return**: Objek `Document`.  
  **Parameter**:  
  - `docx`: Path ke file .docx (opsional).

### **Method Utama**
- `add_paragraph(text="", style=None)`  
  Menambahkan paragraf baru.  
  **Return**: Objek `Paragraph`.  
  **Parameter**:  
  - `text`: Teks paragraf.  
  - `style`: Nama gaya (misal, "Heading 1").  

- `add_heading(text="", level=1)`  
  Menambahkan heading dengan level tertentu.  
  **Return**: Objek `Paragraph`.  

- `add_page_break()`  
  Menambahkan pemisah halaman.  
  **Return**: Objek `Paragraph` kosong dengan page break.  

- `add_table(rows, cols, style=None)`  
  Menambahkan tabel baru.  
  **Return**: Objek `Table`.  

- `add_picture(image_path, width=None, height=None)`  
  Menyisipkan gambar.  
  **Return**: Objek `InlineShape`.  

- `save(path)`  
  Menyimpan dokumen ke path tertentu.  

### **Atribut**
- `paragraphs`: Daftar semua paragraf.  
- `tables`: Daftar semua tabel.  
- `sections`: Daftar semua section.  
- `styles`: Daftar gaya yang tersedia.  

---

## **2. Paragraph Class (`docx.text.paragraph.Paragraph`)**
Mewakili satu paragraf dalam dokumen.

### **Method Utama**
- `add_run(text="", style=None)`  
  Menambahkan teks dengan format tertentu.  
  **Return**: Objek `Run`.  

- `insert_paragraph_before(text="", style=None)`  
  Menyisipkan paragraf sebelum paragraf saat ini.  

- `clear()`  
  Menghapus semua konten paragraf.  

### **Atribut**
- `text`: Teks dalam paragraf.  
- `style`: Gaya paragraf.  
- `runs`: Daftar objek `Run` dalam paragraf.  
- `alignment`: Perataan (kiri/kanan/tengah).  

---

## **3. Run Class (`docx.text.run.Run`)**
Mewakili bagian teks dengan format tertentu dalam paragraf.

### **Method Utama**
- `add_text(text)`  
  Menambahkan teks ke run.  

- `add_break(break_type=WD_BREAK.LINE)`  
  Menambahkan line break/page break.  

- `add_picture(image_path, width=None, height=None)`  
  Menyisipkan gambar dalam run.  

### **Atribut (Formatting)**
- `bold`: Teks tebal (`True`/`False`).  
- `italic`: Teks miring.  
- `underline`: Garis bawah.  
- `font.name`: Jenis font.  
- `font.size`: Ukuran font (dalam pt).  
- `font.color.rgb`: Warna teks (format RGB).  

---

## **4. Table Class (`docx.table.Table`)**
Mewakili tabel dalam dokumen.

### **Method Utama**
- `add_row()`  
  Menambahkan baris baru.  
  **Return**: Objek `Row`.  

- `add_column(width)`  
  Menambahkan kolom baru.  

- `cell(row_idx, col_idx)`  
  Mengakses sel tertentu.  
  **Return**: Objek `Cell`.  

### **Atribut**
- `rows`: Daftar baris.  
- `columns`: Daftar kolom.  
- `style`: Gaya tabel.  

---

## **5. Cell Class (`docx.table.Cell`)**
Mewakili sel dalam tabel.

### **Method Utama**
- `add_paragraph(text="", style=None)`  
  Menambahkan paragraf dalam sel.  

- `merge(other_cell)`  
  Menggabungkan sel dengan sel lain.  

### **Atribut**
- `text`: Teks dalam sel.  
- `paragraphs`: Daftar paragraf dalam sel.  

---

## **6. Section Class (`docx.section.Section`)**
Mengatur pengaturan halaman (margin, orientasi).

### **Method/Atribut Penting**
- `start_type`: Jenis section (misal, `WD_SECTION.NEW_PAGE`).  
- `orientation`: Orientasi halaman (`WD_ORIENT.PORTRAIT`/`LANDSCAPE`).  
- `page_width`, `page_height`: Ukuran halaman.  
- `left_margin`, `right_margin`: Margin.  

---

## **7. Style Classes (`docx.styles.style.Style`)**
Mengelola gaya dokumen.

### **Method Utama**
- `font`: Mengatur font (nama, ukuran, warna).  
- `paragraph_format`: Mengatur spasi, alignment.  

### **Contoh Gaya Built-in**
- "Normal"  
- "Heading 1"  
- "Title"  
- "List Bullet"  

---

## **8. Font Class (`docx.text.run.Font`)**
Mengatur properti font dalam `Run`.

### **Atribut Penting**
- `name`: Nama font (misal, "Arial").  
- `size`: Ukuran font (dalam `Pt`).  
- `bold`, `italic`, `underline`: Format teks.  
- `color.rgb`: Warna teks (misal, `RGBColor(255, 0, 0)` untuk merah).  

---

## **Catatan Khusus**
1. Untuk mengakses semua paragraf:  
   ```python
   for paragraph in document.paragraphs:
       print(paragraph.text)
   ```
2. Untuk mengubah margin:  
   ```python
   section = document.sections[0]
   section.left_margin = Inches(1.5)
   ```
3. Daftar lengkap gaya tersedia di:  
   ```python
   print(document.styles)
   ```

Dokumentasi resmi: [python-docx Documentation](https://python-docx.readthedocs.io/)