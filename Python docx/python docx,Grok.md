Berikut adalah daftar komprehensif dari method-method utama dalam library `python-docx`, yang diorganisir berdasarkan kelas utama seperti yang diminta. Library ini digunakan untuk membuat dan memodifikasi dokumen Microsoft Word (.docx) secara programatik. Saya akan mencakup kelas-kelas utama seperti `Document`, `Paragraph`, `Run`, `Table`, `Cell`, `Section`, `Style`, dan `Font`, dengan penjelasan singkat, parameter, nilai return, dan catatan khusus jika relevan. Daftar ini akan dikelompokkan berdasarkan kelas untuk memudahkan pemahaman.

> **Catatan**: Library `python-docx` memiliki banyak method internal atau properti yang tidak dimaksudkan untuk penggunaan langsung oleh pengguna akhir. Saya akan fokus pada method yang relevan untuk pengguna umum berdasarkan dokumentasi resmi dan penggunaan praktis. Jika ada method tertentu yang tidak tercakup, Anda dapat meminta detail tambahan.

---

### 1. Document Class
Kelas `Document` adalah titik masuk utama untuk membuat atau memodifikasi dokumen Word. Objek ini merepresentasikan seluruh dokumen.

#### Method Utama
1. **Document.add_heading(text: str, level: int = 1) -> Paragraph**
   - **Deskripsi**: Menambahkan heading (judul) dengan level tertentu (misalnya, Heading 1, Heading 2).
   - **Return**: Objek `Paragraph` yang dibuat.
   - **Catatan**: Level berkisar dari 0 (Title) hingga 9, dengan 1-6 paling umum digunakan untuk heading standar.
   - **Contoh**: `doc.add_heading("Judul Dokumen", level=1)`

2. **Document.add_paragraph(text: str = '', style: str = None) -> Paragraph**
   - **Deskripsi**: Menambahkan paragraf baru dengan teks opsional dan gaya tertentu.
   - **Return**: Objek `Paragraph`.
   - **Catatan**: Gaya dapat berupa nama seperti `'Normal'` atau `'BodyText'`.
   - **Contoh**: `doc.add_paragraph("Ini adalah paragraf.", style='Normal')`

3. **Document.add_table(rows: int, cols: int, style: str = None) -> Table**
   - **Deskripsi**: Menambahkan tabel dengan jumlah baris dan kolom yang ditentukan.
   - **Return**: Objek `Table`.
   - **Catatan**: Tabel kosong dibuat; isi sel harus ditambahkan secara terpisah.
   - **Contoh**: `table = doc.add_table(rows=2, cols=3)`

4. **Document.add_page_break() -> None**
   - **Deskripsi**: Menambahkan pemisah halaman (page break).
   - **Return**: Tidak ada.
   - **Catatan**: Digunakan untuk memulai konten pada halaman baru.
   - **Contoh**: `doc.add_page_break()`

5. **Document.add_picture(image_path_or_stream, width: Inches = None, height: Inches = None) -> InlineShape**
   - **Deskripsi**: Menambahkan gambar ke dokumen pada posisi saat ini.
   - **Return**: Objek `InlineShape` yang mewakili gambar.
   - **Catatan**: Parameter `width` dan `height` menggunakan satuan seperti `Inches` atau `Cm` dari `docx.shared`.
   - **Contoh**: `doc.add_picture('image.png', width=Inches(2))`

6. **Document.add_section(start_type: WD_SECTION_START = WD_SECTION_START.NEW_PAGE) -> Section**
   - **Deskripsi**: Menambahkan bagian baru dengan jenis awal tertentu (misalnya, halaman baru).
   - **Return**: Objek `Section`.
   - **Catatan**: Berguna untuk mengatur properti seperti orientasi atau margin per bagian.
   - **Contoh**: `section = doc.add_section(WD_SECTION_START.NEW_PAGE)`

7. **Document.save(path_or_stream: str | file-like) -> None**
   - **Deskripsi**: Menyimpan dokumen ke file atau stream.
   - **Return**: Tidak ada.
   - **Catatan**: Path bisa berupa string (misalnya, `'output.docx'`) atau objek file.
   - **Contoh**: `doc.save('output.docx')`

#### Properti Penting (Bukan Method, tetapi Relevan)
- **`Document.paragraphs`**: Mengembalikan daftar semua paragraf dalam dokumen (list of `Paragraph`).
- **`Document.tables`**: Mengembalikan daftar semua tabel dalam dokumen (list of `Table`).
- **`Document.sections`**: Mengembalikan daftar semua bagian dalam dokumen (list of `Section`).
- **`Document.styles`**: Mengembalikan koleksi gaya yang tersedia (objek `Styles`).

---

### 2. Paragraph Class
Kelas `Paragraph` merepresentasikan paragraf dalam dokumen.

#### Method Utama
1. **Paragraph.add_run(text: str = None, style: str = None) -> Run**
   - **Deskripsi**: Menambahkan run (segmen teks dengan format sama) ke paragraf.
   - **Return**: Objek `Run`.
   - **Catatan**: Run digunakan untuk mengatur format teks seperti bold atau italic.
   - **Contoh**: `run = paragraph.add_run("Teks tebal")`

2. **Paragraph.clear() -> Paragraph**
   - **Deskripsi**: Menghapus semua konten (run) dari paragraf, tetapi mempertahankan properti paragraf.
   - **Return**: Objek `Paragraph` itu sendiri.
   - **Catatan**: Berguna untuk mengganti isi paragraf tanpa membuat paragraf baru.
   - **Contoh**: `paragraph.clear()`

3. **Paragraph.insert_paragraph_before(text: str = None, style: str = None) -> Paragraph**
   - **Deskripsi**: Menyisipkan paragraf baru sebelum paragraf saat ini.
   - **Return**: Objek `Paragraph` baru.
   - **Catatan**: Berguna untuk menambahkan konten di antara paragraf yang sudah ada.
   - **Contoh**: `new_para = paragraph.insert_paragraph_before("Paragraf baru")`

#### Properti Penting
- **`Paragraph.text`**: Mendapatkan atau mengatur teks lengkap paragraf (string).
- **`Paragraph.style`**: Mendapatkan atau mengatur gaya paragraf (objek `Style` atau string nama gaya).
- **`Paragraph.runs`**: Mengembalikan daftar semua run dalam paragraf (list of `Run`).

---

### 3. Run Class
Kelas `Run` merepresentasikan segmen teks dalam paragraf dengan format yang konsisten.

#### Method Utama
1. **Run.add_break(break_type: WD_BREAK = WD_BREAK.LINE) -> None**
   - **Deskripsi**: Menambahkan pemisah (break) seperti baris baru atau halaman baru ke run.
   - **Return**: Tidak ada.
   - **Catatan**: `WD_BREAK` mencakup opsi seperti `LINE`, `PAGE`, atau `COLUMN`.
   - **Contoh**: `run.add_break(WD_BREAK.PAGE)`

2. **Run.clear() -> Run**
   - **Deskripsi**: Menghapus teks dari run, tetapi mempertahankan formatnya.
   - **Return**: Objek `Run` itu sendiri.
   - **Catatan**: Berguna untuk mengganti teks dalam run.
   - **Contoh**: `run.clear()`

#### Properti Penting
- **`Run.text`**: Mendapatkan atau mengatur teks dalam run (string).
- **`Run.bold`**: Mengatur atau mendapatkan status tebal (boolean).
- **`Run.italic`**: Mengatur atau mendapatkan status miring (boolean).
- **`Run.underline`**: Mengatur atau mendapatkan status garis bawah (boolean atau konstanta seperti `WD_UNDERLINE.SINGLE`).
- **`Run.font`**: Mengembalikan objek `Font` untuk mengatur properti font seperti ukuran atau warna.

---

### 4. Table Class
Kelas `Table` merepresentasikan tabel dalam dokumen.

#### Method Utama
1. **Table.add_row() -> Row**
   - **Deskripsi**: Menambahkan baris baru ke tabel.
   - **Return**: Objek `Row`.
   - **Catatan**: Baris baru memiliki jumlah kolom yang sama dengan tabel.
   - **Contoh**: `row = table.add_row()`

2. **Table.add_column(width: Length) -> Column**
   - **Deskripsi**: Menambahkan kolom baru ke tabel dengan lebar tertentu.
   - **Return**: Objek `Column`.
   - **Catatan**: Lebar menggunakan satuan seperti `Inches` atau `Cm`.
   - **Contoh**: `column = table.add_column(Inches(1))`

#### Properti Penting
- **`Table.rows`**: Mengembalikan daftar semua baris dalam tabel (list of `Row`).
- **`Table.columns`**: Mengembalikan daftar semua kolom dalam tabel (list of `Column`).
- **`Table.cell(row_idx: int, col_idx: int)`**: Mengembalikan sel pada indeks baris dan kolom tertentu (objek `Cell`).
- **`Table.style`**: Mendapatkan atau mengatur gaya tabel (string atau objek `Style`).

---

### 5. Cell Class
Kelas `Cell` merepresentasikan sel dalam tabel.

#### Method Utama
1. **Cell.add_paragraph(text: str = '', style: str = None) -> Paragraph**
   - **Deskripsi**: Menambahkan paragraf baru ke sel.
   - **Return**: Objek `Paragraph`.
   - **Catatan**: Mirip dengan `Document.add_paragraph`, tetapi terbatas pada sel.
   - **Contoh**: `paragraph = cell.add_paragraph("Isi sel")`

2. **Cell.merge(other_cell: Cell) -> None**
   - **Deskripsi**: Menggabungkan sel saat ini dengan sel lain untuk membuat sel yang lebih besar.
   - **Return**: Tidak ada.
   - **Catatan**: Sel yang digabungkan harus bersebelahan.
   - **Contoh**: `cell.merge(table.cell(0, 1))`

#### Properti Penting
- **`Cell.text`**: Mendapatkan atau mengatur teks dalam sel (string).
- **`Cell.paragraphs`**: Mengembalikan daftar semua paragraf dalam sel (list of `Paragraph`).

---

### 6. Section Class
Kelas `Section` mengatur properti bagian dokumen seperti margin dan orientasi.

#### Method Utama
1. **Section.header() -> Header**
   - **Deskripsi**: Mengembalikan header untuk bagian ini.
   - **Return**: Objek `Header`.
   - **Catatan**: Digunakan untuk menambahkan konten ke header.
   - **Contoh**: `header = section.header()`

2. **Section.footer() -> Footer**
   - **Deskripsi**: Mengembalikan footer untuk bagian ini.
   - **Return**: Objek `Footer`.
   - **Catatan**: Mirip dengan header, digunakan untuk menambahkan konten ke footer.
   - **Contoh**: `footer = section.footer()`

#### Properti Penting
- **`Section.page_height`**: Mendapatkan atau mengatur tinggi halaman (objek `Length`).
- **`Section.page_width`**: Mendapatkan atau mengatur lebar halaman (objek `Length`).
- **`Section.orientation`**: Mendapatkan atau mengatur orientasi (misalnya, `WD_ORIENT.PORTRAIT` atau `WD_ORIENT.LANDSCAPE`).
- **`Section.top_margin`**, **`Section.bottom_margin`**, **`Section.left_margin`**, **`Section.right_margin`**: Mengatur margin bagian (objek `Length`).

---

### 7. Style Class
Kelas `Style` mengelola gaya paragraf, karakter, atau tabel.

#### Method Utama
1. **Styles.add_style(name: str, style_type: WD_STYLE_TYPE, builtin: bool = False) -> Style**
   - **Deskripsi**: Menambahkan gaya kustom baru ke dokumen.
   - **Return**: Objek `Style`.
   - **Catatan**: `WD_STYLE_TYPE` mencakup `PARAGRAPH`, `CHARACTER`, atau `TABLE`.
   - **Contoh**: `style = doc.styles.add_style('CustomStyle', WD_STYLE_TYPE.PARAGRAPH)`

#### Properti Penting
- **`Style.font`**: Mengembalikan objek `Font` untuk gaya ini.
- **`Style.paragraph_format`**: Mengembalikan objek `ParagraphFormat` untuk mengatur properti seperti spasi atau indentasi.

---

### 8. Font Class
Kelas `Font` mengatur properti font seperti ukuran, warna, dan gaya.

#### Properti Penting (Tidak Ada Method Utama)
- **`Font.name`**: Mendapatkan atau mengatur nama font (misalnya, `'Arial'`).
- **`Font.size`**: Mendapatkan atau mengatur ukuran font (objek `Pt` dari `docx.shared`).
- **`Font.color.rgb`**: Mendapatkan atau mengatur warna font (objek `RGBColor`).
- **`Font.bold`**, **`Font.italic`**, **`Font.underline`**: Mengatur properti format teks (boolean atau konstanta).

---

### Catatan Tambahan
- **Modul Penting**: Library `python-docx` menggunakan modul seperti `docx.enum.text` (untuk `WD_BREAK`, `WD_STYLE_TYPE`), `docx.shared` (untuk `Inches`, `Pt`, `RGBColor`), dan `docx.enum.section` (untuk `WD_ORIENT`, `WD_SECTION_START`). Impor konstanta ini untuk penggunaan yang tepat.
- **Dokumentasi Resmi**: Untuk detail lebih lanjut, lihat dokumentasi `python-docx` di https://python-docx.readthedocs.io/.
- **Contoh Kode Umum**:
  ```python
  from docx import Document
  from docx.shared import Inches, Pt
  from docx.enum.text import WD_BREAK

  doc = Document()
  doc.add_heading('Judul Dokumen', level=1)
  para = doc.add_paragraph('Paragraf pertama.')
  run = para.add_run(' Teks tebal.')
  run.bold = True
  run.add_break(WD_BREAK.LINE)
  table = doc.add_table(rows=2, cols=2)
  cell = table.cell(0, 0)
  cell.text = 'Isi sel'
  doc.save('output.docx')
  ```

Jika Anda memerlukan penjelasan lebih mendalam tentang method tertentu atau contoh penggunaan spesifik, silakan beri tahu saya!