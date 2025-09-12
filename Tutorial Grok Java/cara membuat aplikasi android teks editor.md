Berikut adalah contoh kode untuk membuat aplikasi teks editor sederhana di Android dengan fitur simpan ke file .txt menggunakan Java. Aplikasi ini akan memiliki text editor dan tombol untuk menyimpan teks ke penyimpanan internal perangkat.

### Prasyarat
- Android Studio terinstall
- Familiar dengan dasar-dasar pengembangan Android (Activity, layout XML, permissions)

### Langkah-langkah

#### 1. Buat Proyek Baru di Android Studio
- Buat proyek baru dengan template "Empty Activity"
- Pilih bahasa Java
- Minimum SDK: API 21 (Lollipop) atau lebih tinggi

#### 2. Izin Penyimpanan
Tambahkan izin penyimpanan di `AndroidManifest.xml` untuk menyimpan file:
```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```

Untuk Android 6.0+ (API 23+), Anda perlu meminta izin saat runtime (dijelaskan di kode).

#### 3. Desain Layout
Edit file `res/layout/activity_main.xml` untuk membuat antarmuka:
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText
        android:id="@+id/editText"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:gravity="top"
        android:hint="Tulis teks di sini..."
        android:inputType="textMultiLine"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:id="@+id/saveButton"
            android:layout_width="0dp"
            android:layout_weight="1"
            android:layout_height="wrap_content"
            android:text="Simpan"/>

        <Button
            android:id="@+id/openButton"
            android:layout_width="0dp"
            android:layout_weight="1"
            android:layout_height="wrap_content"
            android:text="Buka"/>
    </LinearLayout>

</LinearLayout>
```

#### 4. Kode Activity
Edit file `MainActivity.java` untuk logika aplikasi:
```java
package com.example.texteditor; // Sesuaikan dengan package name Anda

import android.Manifest;
import android.content.pm.PackageManager;
import android.os.Bundle;
import android.os.Environment;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;
import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class MainActivity extends AppCompatActivity {
    private static final int STORAGE_PERMISSION_CODE = 100;
    private EditText editText;
    private Button saveButton, openButton;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        editText = findViewById(R.id.editText);
        saveButton = findViewById(R.id.saveButton);
        openButton = findViewById(R.id.openButton);

        saveButton.setOnClickListener(v -> checkPermissionAndSave());
        openButton.setOnClickListener(v -> checkPermissionAndOpen());
    }

    private void checkPermissionAndSave() {
        if (ContextCompat.checkSelfPermission(this, Manifest.permission.WRITE_EXTERNAL_STORAGE)
                != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions(this,
                    new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE},
                    STORAGE_PERMISSION_CODE);
        } else {
            saveFile();
        }
    }

    private void checkPermissionAndOpen() {
        if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_EXTERNAL_STORAGE)
                != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions(this,
                    new String[]{Manifest.permission.READ_EXTERNAL_STORAGE},
                    STORAGE_PERMISSION_CODE);
        } else {
            openFile();
        }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if (requestCode == STORAGE_PERMISSION_CODE) {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                if (permissions[0].equals(Manifest.permission.WRITE_EXTERNAL_STORAGE)) {
                    saveFile();
                } else if (permissions[0].equals(Manifest.permission.READ_EXTERNAL_STORAGE)) {
                    openFile();
                }
            } else {
                Toast.makeText(this, "Izin penyimpanan ditolak", Toast.LENGTH_SHORT).show();
            }
        }
    }

    private void saveFile() {
        String text = editText.getText().toString();
        String fileName = "catatan.txt"; // Nama file default
        File directory = new File(Environment.getExternalStoragePublicDirectory(
                Environment.DIRECTORY_DOCUMENTS), "TextEditor");
        if (!directory.exists()) {
            directory.mkdirs();
        }
        File file = new File(directory, fileName);

        try (FileWriter writer = new FileWriter(file)) {
            writer.write(text);
            Toast.makeText(this, "File disimpan di: " + file.getAbsolutePath(), Toast.LENGTH_LONG).show();
        } catch (IOException e) {
            Toast.makeText(this, "Error menyimpan file: " + e.getMessage(), Toast.LENGTH_SHORT).show();
        }
    }

    private void openFile() {
        File directory = new File(Environment.getExternalStoragePublicDirectory(
                Environment.DIRECTORY_DOCUMENTS), "TextEditor");
        File file = new File(directory, "catatan.txt");

        if (file.exists()) {
            try (BufferedReader reader = new BufferedReader(new FileReader(file))) {
                StringBuilder text = new StringBuilder();
                String line;
                while ((line = reader.readLine()) != null) {
                    text.append(line).append("\n");
                }
                editText.setText(text.toString());
                Toast.makeText(this, "File dibuka", Toast.LENGTH_SHORT).show();
            } catch (IOException e) {
                Toast.makeText(this, "Error membuka file: " + e.getMessage(), Toast.LENGTH_SHORT).show();
            }
        } else {
            Toast.makeText(this, "File tidak ditemukan", Toast.LENGTH_SHORT).show();
        }
    }
}
```

#### 5. Penjelasan Kode
- **Layout**:
  - `EditText` untuk menulis teks
  - Dua tombol: "Simpan" dan "Buka"
- **Izin Penyimpanan**:
  - Memeriksa dan meminta izin `WRITE_EXTERNAL_STORAGE` dan `READ_EXTERNAL_STORAGE` saat runtime
- **Simpan File**:
  - Menyimpan teks ke file `catatan.txt` di folder `Documents/TextEditor`
  - Membuat direktori jika belum ada
  - Menggunakan `FileWriter` untuk menulis teks
- **Buka File**:
  - Membaca file `catatan.txt` dari folder yang sama
  - Menampilkan isi file di `EditText`
  - Menggunakan `BufferedReader` untuk membaca teks
- **Error Handling**:
  - Menampilkan pesan `Toast` untuk sukses atau gagal
- **Lokasi Penyimpanan**:
  - File disimpan di penyimpanan eksternal (`Documents/TextEditor`), yang dapat diakses oleh pengguna

#### 6. Catatan Penting
- **Scoped Storage (Android 10+)**:
  - Untuk Android 10 ke atas, Anda mungkin ingin menggunakan `MediaStore` atau `Storage Access Framework` untuk pengelolaan file yang lebih modern. Kode di atas menggunakan penyimpanan eksternal tradisional untuk kesederhanaan.
- **Nama File**:
  - Kode ini menggunakan nama file statis (`catatan.txt`). Untuk fleksibilitas, Anda bisa menambahkan dialog untuk memasukkan nama file.
- **Fitur Tambahan**:
  - Tambahkan dialog untuk memilih file saat membuka
  - Tambahkan opsi untuk membuat file baru
  - Tambahkan konfirmasi sebelum menimpa file
- **Testing**:
  - Uji di emulator atau perangkat fisik
  - Pastikan izin penyimpanan diberikan
  - Periksa file di folder `Documents/TextEditor` menggunakan file manager

#### 7. Cara Menjalankan
1. Buat proyek di Android Studio
2. Salin kode XML ke `activity_main.xml`
3. Salin kode Java ke `MainActivity.java`
4. Tambahkan izin di `AndroidManifest.xml`
5. Jalankan aplikasi di emulator atau perangkat
6. Berikan izin penyimpanan saat diminta

Jika Anda ingin menambahkan fitur seperti dialog pemilihan file atau dukungan untuk Android modern, beri tahu, dan saya bisa bantu modifikasi kode!