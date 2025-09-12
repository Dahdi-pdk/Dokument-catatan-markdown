Untuk menggunakan **TextView** di Android dengan Java, Anda perlu mengetahui langkah-langkah dasar untuk mengatur TextView baik melalui XML maupun kode Java, serta memahami atribut-atribut yang sering digunakan. Berikut adalah penjelasan lengkapnya:

---

### **1. Cara Menggunakan TextView di Android dengan Java**

#### **a. Menambahkan TextView di Layout XML**
TextView biasanya didefinisikan dalam file XML layout (misalnya, `res/layout/activity_main.xml`). Contoh:

```xml
<TextView
    android:id="@+id/textView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Hello, World!"
    android:textSize="18sp"
    android:textColor="#000000"
    android:gravity="center" />
```

- **`android:id`**: Memberikan ID unik untuk TextView agar bisa diakses di kode Java.
- **`android:layout_width` dan `android:layout_height`**: Menentukan lebar dan tinggi TextView.
- **`android:text`**: Menentukan teks yang ditampilkan.

#### **b. Mengakses dan Memanipulasi TextView di Java**
Di file Java (misalnya, `MainActivity.java`), Anda dapat mengakses TextView menggunakan ID dan memodifikasi propertinya. Contoh:

```java
import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Mengakses TextView berdasarkan ID
        TextView textView = findViewById(R.id.textView);

        // Mengubah teks
        textView.setText("Selamat datang di Android!");

        // Mengubah ukuran teks
        textView.setTextSize(20);

        // Mengubah warna teks
        textView.setTextColor(getResources().getColor(R.color.black));

        // Mengubah gravitasi teks
        textView.setGravity(Gravity.CENTER);
    }
}
```

- **`findViewById(R.id.textView)`**: Mengambil referensi TextView dari layout XML.
- **`setText()`**: Mengatur teks baru.
- **`setTextSize()`**: Mengatur ukuran teks (dalam sp).
- **`setTextColor()`**: Mengatur warna teks.
- **`setGravity()`**: Mengatur perataan teks.

#### **c. Membuat TextView Secara Programmatic**
Anda juga bisa membuat TextView sepenuhnya melalui kode Java tanpa XML:

```java
import android.os.Bundle;
import android.widget.LinearLayout;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // Membuat LinearLayout sebagai container
        LinearLayout layout = new LinearLayout(this);
        layout.setOrientation(LinearLayout.VERTICAL);
        layout.setLayoutParams(new LinearLayout.LayoutParams(
                LinearLayout.LayoutParams.MATCH_PARENT,
                LinearLayout.LayoutParams.MATCH_PARENT));

        // Membuat TextView
        TextView textView = new TextView(this);
        textView.setText("TextView Programmatic");
        textView.setTextSize(18);
        textView.setTextColor(getResources().getColor(R.color.black));
        textView.setLayoutParams(new LinearLayout.LayoutParams(
                LinearLayout.LayoutParams.WRAP_CONTENT,
                LinearLayout.LayoutParams.WRAP_CONTENT));

        // Menambahkan TextView ke layout
        layout.addView(textView);

        // Mengatur layout sebagai konten activity
        setContentView(layout);
    }
}
```

---

### **2. Atribut dan Value TextView di XML**
Berikut adalah beberapa atribut TextView yang umum digunakan di XML beserta nilai (value) yang mungkin:

| **Atribut**                  | **Deskripsi**                                                                 | **Nilai yang Mungkin**                                                                 |
|------------------------------|-------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|
| `android:id`                 | ID unik untuk TextView                                                       | `@+id/nama_id`                                                                       |
| `android:layout_width`       | Lebar TextView                                                               | `match_parent`, `wrap_content`, atau nilai dalam dp (misalnya, `100dp`)              |
| `android:layout_height`      | Tinggi TextView                                                              | `match_parent`, `wrap_content`, atau nilai dalam dp (misalnya, `50dp`)               |
| `android:text`               | Teks yang ditampilkan                                                        | String (misalnya, `"Hello World"`) atau referensi `@string/nama_string`              |
| `android:textSize`           | Ukuran teks                                                                  | Nilai dalam `sp` (misalnya, `18sp`)                                                  |
| `android:textColor`          | Warna teks                                                                   | Kode warna (misalnya, `#FF0000`), `@color/nama_warna`, atau `@android:color/black`   |
| `android:textStyle`          | Gaya teks                                                                    | `normal`, `bold`, `italic`                                                           |
| `android:gravity`            | Perataan teks dalam TextView                                                 | `center`, `start`, `end`, `top`, `bottom`, dll.                                      |
| `android:layout_gravity`     | Perataan TextView dalam layout                                               | `center`, `start`, `end`, `top`, `bottom`, dll.                                      |
| `android:background`         | Warna atau drawable untuk latar belakang                                     | Kode warna, `@drawable/nama_drawable`, atau `@color/nama_warna`                      |
| `android:padding`            | Jarak dalam TextView                                                         | Nilai dalam dp (misalnya, `10dp`)                                                    |
| `android:ellipsize`          | Menangani teks yang terlalu panjang                                          | `none`, `start`, `middle`, `end`, `marquee`                                          |
| `android:maxLines`           | Jumlah baris maksimum                                                        | Angka (misalnya, `2`)                                                                |
| `android:singleLine`         | Membatasi teks ke satu baris (deprecated, gunakan `maxLines="1"`)            | `true`, `false`                                                                      |
| `android:textAppearance`     | Gaya teks bawaan                                                             | `?android:attr/textAppearanceSmall`, `?android:attr/textAppearanceMedium`, dll.       |
| `android:typeface`           | Jenis font                                                                   | `normal`, `sans`, `serif`, `monospace`                                               |
| `android:fontFamily`         | Font kustom                                                                  | Nama font (misalnya, `"sans-serif"`) atau referensi font kustom                      |
| `android:lineSpacingExtra`   | Jarak tambahan antar baris                                                   | Nilai dalam dp (misalnya, `4dp`)                                                     |
| `android:visibility`         | Visibilitas TextView                                                         | `visible`, `invisible`, `gone`                                                       |
| `android:onClick`            | Method yang dipanggil saat TextView diklik                                   | Nama method di Activity (misalnya, `"onTextViewClick"`)                              |

---

### **3. Atribut dan Value TextView di Java**
Atribut-atribut di atas juga dapat diatur melalui metode Java. Berikut beberapa metode yang sesuai dengan atribut XML:

| **Atribut XML**              | **Metode Java**                          | **Contoh Penggunaan**                                |
|------------------------------|------------------------------------------|-----------------------------------------------------|
| `android:text`               | `setText(String)`                       | `textView.setText("Hello World");`                  |
| `android:textSize`           | `setTextSize(float)`                    | `textView.setTextSize(18);`                        |
| `android:textColor`          | `setTextColor(int)`                     | `textView.setTextColor(Color.BLACK);`              |
| `android:textStyle`          | `setTypeface(Typeface, int)`            | `textView.setTypeface(null, Typeface.BOLD);`       |
| `android:gravity`            | `setGravity(int)`                       | `textView.setGravity(Gravity.CENTER);`             |
| `android:background`         | `setBackgroundColor(int)`               | `textView.setBackgroundColor(Color.BLUE);`         |
| `android:padding`            | `setPadding(int, int, int, int)`        | `textView.setPadding(10, 10, 10, 10);`            |
| `android:ellipsize`          | `setEllipsize(TextUtils.TruncateAt)`    | `textView.setEllipsize(TextUtils.TruncateAt.END);` |
| `android:maxLines`           | `setMaxLines(int)`                      | `textView.setMaxLines(2);`                         |
| `android:visibility`         | `setVisibility(int)`                    | `textView.setVisibility(View.VISIBLE);`            |

---

### **4. Contoh Praktis: TextView dengan Aksi Klik**
Berikut adalah contoh TextView yang berubah teks saat diklik:

```xml
<!-- res/layout/activity_main.xml -->
<TextView
    android:id="@+id/textView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Klik saya!"
    android:padding="10dp"
    android:textSize="18sp"
    android:onClick="onTextViewClick" />
```

```java
// MainActivity.java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    // Method yang dipanggil saat TextView diklik
    public void onTextViewClick(View view) {
        TextView textView = (TextView) view;
        textView.setText("Teks telah diubah!");
        textView.setTextColor(Color.RED);
    }
}
```

---

### **5. Catatan Penting**
- **Unit Ukuran**: Gunakan `sp` untuk ukuran teks agar mendukung skalabilitas font berdasarkan pengaturan perangkat pengguna.
- **Performance**: Hindari terlalu banyak manipulasi TextView secara programmatic di loop atau operasi berat, karena dapat memengaruhi performa.
- **Kustomisasi Font**: Untuk font kustom, letakkan file font di folder `res/font` dan gunakan `android:fontFamily="@font/nama_font"` atau `setTypeface()` di Java.
- **Aksesibilitas**: Pastikan teks memiliki kontras yang cukup dengan latar belakang dan ukuran teks yang cukup besar untuk aksesibilitas.
- 
- 
- 
- 
- 
- 
---






# Menampilkan teks txt
---


Untuk menampilkan teks dari file eksternal seperti file `.txt` di **TextView** pada aplikasi Android menggunakan Java, Anda perlu membaca isi file tersebut dan kemudian mengatur teksnya ke TextView. File `.txt` dapat disimpan di beberapa lokasi, seperti folder `assets`, `res/raw`, atau penyimpanan eksternal perangkat. Berikut adalah penjelasan langkah-langkah untuk melakukannya, dengan fokus pada pendekatan paling umum (menggunakan folder `assets` atau `res/raw`) dan contoh kode.

---

### **1. Menyimpan File Teks**
Ada dua lokasi umum untuk menyimpan file `.txt` di proyek Android:
- **Folder `assets`**: File di sini bersifat read-only dan cocok untuk file statis yang disertakan dalam APK.
- **Folder `res/raw`**: File di sini juga read-only, diakses sebagai resource, dan memiliki ID resource unik.

#### **a. Menyimpan di Folder `assets`**
1. Buat folder `assets` di root proyek (jika belum ada). Klik kanan pada proyek di Android Studio → New → Folder → Assets Folder.
2. Salin file teks (misalnya, `mytext.txt`) ke folder `assets` atau subfolder (misalnya, `assets/texts/mytext.txt`).

#### **b. Menyimpan di Folder `res/raw`**
1. Buat folder `raw` di dalam folder `res` (klik kanan `res` → New → Android Resource Directory, pilih `raw` sebagai Resource Type).
2. Salin file teks (misalnya, `mytext.txt`) ke `res/raw`. Nama file harus huruf kecil dan tanpa spasi.

---

### **2. Membaca File Teks dan Menampilkannya di TextView**

#### **a. Membaca dari Folder `assets`**
File di folder `assets` diakses menggunakan `AssetManager`. Berikut adalah contoh kode untuk membaca file `.txt` dan menampilkannya di TextView:

```java
import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Mengakses TextView
        TextView textView = findViewById(R.id.textView);

        try {
            // Mengakses file dari assets
            InputStream inputStream = getAssets().open("mytext.txt");
            BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream));
            StringBuilder stringBuilder = new StringBuilder();
            String line;

            // Membaca file baris per baris
            while ((line = reader.readLine()) != null) {
                stringBuilder.append(line).append("\n");
            }

            // Menutup stream
            reader.close();
            inputStream.close();

            // Menampilkan teks di TextView
            textView.setText(stringBuilder.toString());

        } catch (IOException e) {
            e.printStackTrace();
            textView.setText("Error: Tidak dapat membaca file!");
        }
    }
}
```

**Penjelasan**:
- **`getAssets().open("mytext.txt")`**: Membuka file `mytext.txt` dari folder `assets`.
- **`BufferedReader`**: Membaca file baris per baris untuk efisiensi.
- **`StringBuilder`**: Menggabungkan semua baris teks.
- **`textView.setText()`**: Menampilkan teks ke TextView.
- **Penanganan error**: Menggunakan `try-catch` untuk menangani kemungkinan file tidak ditemukan atau error lainnya.

**Layout XML** (`res/layout/activity_main.xml`):
```xml
<TextView
    android:id="@+id/textView"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:textSize="16sp"
    android:padding="16dp" />
```

---

#### **b. Membaca dari Folder `res/raw`**
File di folder `res/raw` diakses sebagai resource menggunakan `Resources`. Berikut adalah contoh kodenya:

```java
import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Mengakses TextView
        TextView textView = findViewById(R.id.textView);

        try {
            // Mengakses file dari res/raw
            InputStream inputStream = getResources().openRawResource(R.raw.mytext);
            BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream));
            StringBuilder stringBuilder = new StringBuilder();
            String line;

            // Membaca file baris per baris
            while ((line = reader.readLine()) != null) {
                stringBuilder.append(line).append("\n");
            }

            // Menutup stream
            reader.close();
            inputStream.close();

            // Menampilkan teks di TextView
            textView.setText(stringBuilder.toString());

        } catch (IOException e) {
            e.printStackTrace();
            textView.setText("Error: Tidak dapat membaca file!");
        }
    }
}
```

**Penjelasan**:
- **`getResources().openRawResource(R.raw.mytext)`**: Mengakses file `mytext.txt` dari folder `res/raw`. Nama file tanpa ekstensi (misalnya, `mytext` bukan `mytext.txt`).
- **Sisanya** mirip dengan pendekatan `assets`, menggunakan `BufferedReader` untuk membaca file.

**Catatan**:
- Pastikan file di `res/raw` memiliki nama huruf kecil dan tanpa spasi (misalnya, `mytext.txt`, bukan `MyText.txt`).
- File di `res/raw` diakses dengan ID resource (`R.raw.nama_file`).

---

### **3. Membaca dari Penyimpanan Eksternal (Opsional)**
Jika file `.txt` berada di penyimpanan eksternal (misalnya, di folder Download perangkat), Anda perlu izin dan kode tambahan untuk membacanya. Namun, ini lebih kompleks karena melibatkan izin seperti `READ_EXTERNAL_STORAGE`.

**Contoh Kode (Penyimpanan Eksternal)**:
```java
import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;
import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.IOException;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Mengakses TextView
        TextView textView = findViewById(R.id.textView);

        // Path ke file di penyimpanan eksternal (misalnya, folder Download)
        File file = new File(Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DOWNLOADS), "mytext.txt");

        try {
            BufferedReader reader = new BufferedReader(new FileReader(file));
            StringBuilder stringBuilder = new StringBuilder();
            String line;

            // Membaca file baris per baris
            while ((line = reader.readLine()) != null) {
                stringBuilder.append(line).append("\n");
            }

            // Menutup reader
            reader.close();

            // Menampilkan teks di TextView
            textView.setText(stringBuilder.toString());

        } catch (IOException e) {
            e.printStackTrace();
            textView.setText("Error: Tidak dapat membaca file!");
        }
    }
}
```

**Izin di `AndroidManifest.xml`**:
```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

**Catatan**:
- Untuk Android 6.0+ (API 23+), Anda perlu meminta izin runtime untuk `READ_EXTERNAL_STORAGE`.
- Untuk Android 10+ (API 29+), akses penyimpanan eksternal mungkin memerlukan scoped storage atau izin khusus.

---

### **4. Catatan Penting**
- **Pilih Lokasi File**:
  - Gunakan `assets` untuk file statis yang disertakan dalam aplikasi dan tidak akan berubah.
  - Gunakan `res/raw` jika Anda ingin file diakses sebagai resource dengan ID.
  - Gunakan penyimpanan eksternal hanya jika file dihasilkan atau diunduh oleh pengguna.
- **Penanganan Error**:
  - Selalu gunakan `try-catch` untuk menangani `IOException` atau file tidak ditemukan.
  - Tampilkan pesan error di TextView jika gagal membaca file.
- **Ukuran File**:
  - Untuk file besar, pertimbangkan membaca secara bertahap (streaming) agar tidak membebani memori.
- **Encoding**:
  - Pastikan file teks menggunakan encoding yang sesuai (misalnya, UTF-8). `BufferedReader` secara default mendukung UTF-8.
- **Aksesibilitas**:
  - Pastikan teks yang ditampilkan di TextView memiliki ukuran dan kontras yang baik untuk keterbacaan.

---

### **5. Contoh File Teks**
Misalkan isi file `mytext.txt`:
```txt
Selamat datang di aplikasi Android!
Ini adalah contoh teks dari file eksternal.
```

Hasilnya akan ditampilkan di TextView persis seperti isi file, dengan tambahan baris baru (`\n`) sesuai kode di atas.

---

### **6. Contoh Lengkap dengan XML**
**res/layout/activity_main.xml**:
```xml
<TextView
    android:id="@+id/textView"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:textSize="16sp"
    android:padding="16dp"
    android:fontFamily="@font/myfont" />
```

**MainActivity.java (Menggunakan Assets)**:
```Java

import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.content.res.ResourcesCompat;
import android.graphics.Typeface;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Mengakses TextView
        TextView textView = findViewById(R.id.textView);

        // Mengatur font kustom (opsional)

```

Jika Anda memiliki pertanyaan lebih spesifik atau ingin contoh lain, silakan beri tahu!


---
# Versi Sederhana 

```Java

import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;
import java.io.*;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        TextView textView = findViewById(R.id.textView);

        try (BufferedReader reader = new BufferedReader(
                new InputStreamReader(getAssets().open("mytext.txt")))) {

            StringBuilder text = new StringBuilder();
            String line;
            while ((line = reader.readLine()) != null)
                text.append(line).append("\n");

            textView.setText(text.toString());

        } catch (IOException e) {
            textView.setText("Error: Tidak dapat membaca file!");
        }
    }
}

```
# Lebih Sederhana

```Java

try (BufferedReader r = new BufferedReader(new InputStreamReader(getAssets().open("mytext.txt")))) {
    StringBuilder s = new StringBuilder();
    String l;
    while ((l = r.readLine()) != null) s.append(l).append("\n");
    textView.setText(s.toString());
} catch (IOException e) {
    textView.setText("Error!");
}

```


---


# Versi Professional 

Versi lebih profesional dari kode MainActivity berarti:

✅ Lebih bersih dan terstruktur
✅ Menggunakan fungsi terpisah (prinsip separation of concerns)
✅ Konsisten dalam penamaan dan format
✅ Mudah untuk diuji dan dikembangkan
✅ Menangani error dengan baik dan tidak menampilkan stack trace secara langsung

Berikut versi lebih profesional dari kode kamu:

```Java

import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

public class MainActivity extends AppCompatActivity {

    private TextView textView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        textView = findViewById(R.id.textView);

        String fileContent = readAssetFile("mytext.txt");
        if (fileContent != null) {
            textView.setText(fileContent);
        } else {
            textView.setText("Terjadi kesalahan saat membaca file.");
        }
    }

    /**
     * Membaca isi file dari folder assets dan mengembalikannya sebagai String.
     *
     * @param filename nama file di folder assets
     * @return isi file sebagai String, atau null jika terjadi kesalahan
     */
    private String readAssetFile(String filename) {
        StringBuilder content = new StringBuilder();

        try (InputStream inputStream = getAssets().open(filename);
             BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream))) {

            String line;
            while ((line = reader.readLine()) != null) {
                content.append(line).append("\n");
            }

            return content.toString();

        } catch (IOException e) {
            // Logging bisa ditambahkan di sini, contoh: Log.e("MainActivity", "Gagal membaca file", e);
            return null;
        }
    }
}

```
---

Penjelasan Profesionalisme Kodenya:

Aspek	Penjelasan

🔧 Modularitas	Fungsi readAssetFile() terpisah agar mudah digunakan ulang dan dites.
📄 Dokumentasi	Ada komentar Javadoc pada fungsi – ini penting di dunia profesional.
🚫 No Stack Trace	Stack trace e.printStackTrace() dihilangkan agar tidak ditampilkan ke user akhir.
✅ Null-safe	Cek null saat file gagal dibaca, dan tampilkan pesan yang sopan.
📦 Clean Code	Konsisten dalam indentasi, penamaan, dan struktur kode.


Jika kamu ingin menambahkan logging untuk debugging tanpa menampilkannya ke user:

```Java
import android.util.Log;
// ...
Log.e("MainActivity", "Gagal membaca file: " + filename, e);

```
Letakkan itu di dalam blok catch. Tapi jangan tampilkan exception langsung ke user, karena itu buruk untuk UX dan keamanan.


---

Kalau kamu pakai MVVM atau arsitektur lain, kita juga bisa refactor lebih lanjut (misalnya memindahkan logic ke ViewModel). Tapi untuk MainActivity yang sederhana, versi di atas sudah cukup profesional.


---



# Versi Bergaya 

Versi yang lebih bergaya dengan animasi artinya kamu ingin menambahkan efek visual agar aplikasi terlihat lebih hidup dan menarik secara UI/UX, sambil tetap menjaga struktur kode yang profesional.

Berikut adalah versi MainActivity yang:

✅ Bersih dan profesional

💫 Menambahkan animasi fade-in saat teks ditampilkan

✨ Memiliki sedikit sentuhan gaya untuk efek visual

💬 Masih bisa dikembangkan untuk UI lebih interaktif (misalnya tombol refresh, dsb.)



---

📱 Hasil yang Akan Kamu Dapatkan

Teks dari file akan muncul dengan animasi memudar masuk (fade-in) di TextView.


---

🧑‍💻 Kode: MainActivity.java
```Java
import android.os.Bundle;
import android.view.animation.AlphaAnimation;
import android.view.animation.Animation;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

public class MainActivity extends AppCompatActivity {

    private TextView textView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        textView = findViewById(R.id.textView);
        textView.setAlpha(0f); // Mulai dari transparan

        String fileContent = readAssetFile("mytext.txt");
        if (fileContent != null) {
            textView.setText(fileContent);
            applyFadeInAnimation(textView);
        } else {
            textView.setText("Terjadi kesalahan saat membaca file.");
        }
    }

    /**
     * Membaca isi file dari folder assets.
     */
    private String readAssetFile(String filename) {
        StringBuilder content = new StringBuilder();

        try (InputStream inputStream = getAssets().open(filename);
             BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream))) {

            String line;
            while ((line = reader.readLine()) != null) {
                content.append(line).append("\n");
            }
            return content.toString();

        } catch (IOException e) {
            return null;
        }
    }

    /**
     * Memberikan animasi fade-in ke elemen TextView.
     */
    private void applyFadeInAnimation(TextView view) {
        Animation fadeIn = new AlphaAnimation(0f, 1f);
        fadeIn.setDuration(1000); // 1 detik
        fadeIn.setFillAfter(true); // Tetap tampil setelah animasi selesai
        view.startAnimation(fadeIn);
    }
}

```
---


🖼️ activity_main.xml (tambahkan padding/styling jika belum)
```Xml

<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="24dp"
    android:background="#FFFFFF">

    <TextView
        android:id="@+id/textView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="18sp"
        android:textColor="#333333"
        android:textStyle="normal"
        android:lineSpacingExtra="8dp"/>
</RelativeLayout>

```
---

✨ Ingin Lebih Lanjut?

Tambahkan tombol refresh dan beri animasi slide.

Gunakan Material Design Components (com.google.android.material).

Tampilkan animasi progress bar sebelum file dimuat.

Tambahkan transition fragment atau gunakan MotionLayout.



---

Kalau kamu ingin versi dengan animasi yang lebih kompleks seperti slide-in, scaling, atau rotation, tinggal bilang saja — saya bisa bantu tambahkannya juga!




