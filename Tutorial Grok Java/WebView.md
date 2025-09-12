Berikut adalah contoh cara menggunakan WebView untuk menampilkan atau merender HTML dalam aplikasi Android menggunakan bahasa Java:

1. Pertama, tambahkan izin internet di AndroidManifest.xml:
```xml
<uses-permission android:name="android.permission.INTERNET" />
```

2. Tambahkan WebView ke layout XML (contoh: res/layout/activity_main.xml):
```xml
<?xml version="1.0" 0="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <WebView
        android:id="@+id/webView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>
```

3. Berikut adalah contoh kode Java untuk Activity (contoh: MainActivity.java):

```java
package com.example.mywebviewapp;

import android.os.Bundle;
import android.webkit.WebSettings;
import android.webkit.WebView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    
    private WebView webView;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        // Inisialisasi WebView
        webView = findViewById(R.id.webView);
        
        // Mengaktifkan JavaScript (opsional)
        WebSettings webSettings = webView.getSettings();
        webSettings.setJavaScriptEnabled(true);
        
        // Contoh 1: Memuat HTML dari string
        String htmlContent = "<html>" +
                "<head>" +
                "<title>Contoh HTML</title>" +
                "</head>" +
                "<body>" +
                "<h1>Selamat Datang!</h1>" +
                "<p>Ini adalah contoh HTML yang dirender di WebView.</p>" +
                "<button onclick=\"alert('Halo dari JavaScript!')\">Klik Saya</button>" +
                "</body>" +
                "</html>";
        
        webView.loadData(htmlContent, "text/html", "UTF-8");
        
        // Contoh 2: Memuat URL (uncomment untuk mencoba)
        // webView.loadUrl("https://www.example.com");
        
        // Contoh 3: Memuat file HTML dari assets (uncomment untuk mencoba)
        // webView.loadUrl("file:///android_asset/sample.html");
    }
    
    // Menangani tombol back
    @Override
    public void onBackPressed() {
        if (webView.canGoBack()) {
            webView.goBack();
        } else {
            super.onBackPressed();
        }
    }
}
```

4. Jika Anda ingin memuat file HTML dari folder assets:
   - Buat folder `assets` di direktori `app/src/main`
   - Tambahkan file HTML (contoh: sample.html) di folder assets
   - Gunakan `webView.loadUrl("file:///android_asset/sample.html");`

Penjelasan kode:
- `WebView` diinisialisasi dari layout XML menggunakan `findViewById`
- `WebSettings` digunakan untuk mengatur konfigurasi seperti mengaktifkan JavaScript
- `loadData()` digunakan untuk memuat konten HTML dari string
- `loadUrl()` digunakan untuk memuat URL atau file dari assets
- Method `onBackPressed()` di-override untuk mendukung navigasi mundur di WebView

Catatan penting:
1. Tambahkan izin INTERNET di AndroidManifest.xml jika memuat konten dari web
2. Aktifkan JavaScript hanya jika diperlukan karena alasan keamanan
3. Untuk API level 30+ (Android 11+), tambahkan `android:usesCleartextTraffic="true"` di AndroidManifest.xml jika memuat konten HTTP (bukan HTTPS)
4. Pastikan untuk menangani error loading dan keamanan konten dengan WebViewClient jika diperlukan

Contoh penanganan error dengan WebViewClient:
```java
webView.setWebViewClient(new WebViewClient() {
    @Override
    public void onReceivedError(WebView view, int errorCode, String description, String failingUrl) {
        super.onReceivedError(view, errorCode, description, failingUrl);
        // Handle error here
        Toast.makeText(MainActivity.this, "Error: " + description, Toast.LENGTH_SHORT).show();
    }
});
```

Ini adalah contoh dasar penggunaan WebView. Anda dapat menyesuaikan sesuai kebutuhan aplikasi Anda.

Tentu! Berikut adalah penjelasan ulang tentang cara memuat file HTML dari folder `assets` di aplikasi Android menggunakan WebView dengan Java:

### Langkah-langkah Memuat File HTML dari Folder Assets

1. **Buat File HTML di Folder Assets**:
   - Buat folder `assets` di direktori proyek Anda, tepatnya di `app/src/main/assets`. Jika folder ini belum ada, Anda bisa membuatnya.
   - Tambahkan file HTML (misalnya, `sample.html`) di dalam folder `assets`. Contoh isi file `sample.html`:
     ```html
     <html>
     <head>
         <title>Contoh HTML</title>
     </head>
     <body>
         <h1>Selamat Datang!</h1>
         <p>Ini adalah contoh HTML yang dimuat dari folder assets.</p>
         <button onclick="Android.showToast('Halo dari JavaScript!')">Klik Saya</button>
     </body>
     </html>
     ```

2. **Tambahkan WebView di Layout XML**:
   - Pastikan Anda memiliki `WebView` di file layout XML (misalnya, `res/layout/activity_main.xml`):
     ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         android:layout_width="match_parent"
         android:layout_height="match_parent"
         android:orientation="vertical">

         <WebView
             android:id="@+id/webView"
             android:layout_width="match_parent"
             android:layout_height="match_parent" />

     </LinearLayout>
     ```

3. **Konfigurasi WebView di Activity**:
   - Gunakan metode `loadUrl` dengan prefiks `file:///android_asset/` untuk memuat file HTML dari folder `assets`.
   - Berikut adalah contoh kode di `MainActivity.java`:

     ```java
     package com.example.mywebviewapp;

     import android.content.Context;
     import android.os.Bundle;
     import android.webkit.JavascriptInterface;
     import android.webkit.WebSettings;
     import android.webkit.WebView;
     import android.widget.Toast;
     import androidx.appcompat.app.AppCompatActivity;

     public class MainActivity extends AppCompatActivity {

         private WebView webView;

         @Override
         protected void onCreate(Bundle savedInstanceState) {
             super.onCreate(savedInstanceState);
             setContentView(R.layout.activity_main);

             // Inisialisasi WebView
             webView = findViewById(R.id.webView);

             // Aktifkan JavaScript (opsional, jika HTML mengandung JavaScript)
             WebSettings webSettings = webView.getSettings();
             webSettings.setJavaScriptEnabled(true);

             // Tambahkan JavaScript interface (opsional, untuk komunikasi dengan Java)
             webView.addJavascriptInterface(new WebAppInterface(this), "Android");

             // Memuat file HTML dari folder assets
             webView.loadUrl("file:///android_asset/sample.html");
         }

         // Interface untuk komunikasi dengan JavaScript
         public class WebAppInterface {
             Context mContext;

             WebAppInterface(Context context) {
                 mContext = context;
             }

             @JavascriptInterface
             public void showToast(String message) {
                 Toast.makeText(mContext, message, Toast.LENGTH_SHORT).show();
             }
         }

         // Menangani tombol back
         @Override
         public void onBackPressed() {
             if (webView.canGoBack()) {
                 webView.goBack();
             } else {
                 super.onBackPressed();
             }
         }
     }
     ```

### Penjelasan
- **Folder Assets**: File di folder `assets` diakses menggunakan URL dengan format `file:///android_asset/nama_file.html`. Folder ini bersifat read-only dan cocok untuk menyimpan aset statis seperti HTML, CSS, atau JavaScript.
- **loadUrl**: Metode `webView.loadUrl("file:///android_asset/sample.html")` memuat file HTML dari folder `assets`. Pastikan nama file sesuai dengan yang ada di folder `assets`.
- **JavaScript (Opsional)**: Jika file HTML mengandung JavaScript, aktifkan `setJavaScriptEnabled(true)`. Dalam contoh di atas, JavaScript dapat memanggil fungsi Java (`Android.showToast`) melalui `addJavascriptInterface`.
- **Keamanan**: Memuat file dari `assets` lebih aman dibandingkan memuat dari internet karena kontennya berada di dalam aplikasi dan tidak memerlukan koneksi jaringan.
- **Izin**: Tidak diperlukan izin khusus seperti `INTERNET` untuk memuat file dari `assets`, berbeda dengan memuat konten dari web.

### Catatan Tambahan
- **Struktur Folder**: Pastikan file HTML (dan aset terkait seperti CSS atau gambar) ditempatkan di folder `assets` atau subfoldernya (misalnya, `assets/html/sample.html`). Untuk subfolder, gunakan `file:///android_asset/html/sample.html`.
- **Aset Terkait**: Jika file HTML merujuk ke file lain (misalnya, CSS atau gambar), pastikan file tersebut juga ada di folder `assets` dan jalur relatifnya benar. Misalnya, `<img src="images/example.jpg">` akan mencari file di `assets/images/example.jpg`.
- **Error Handling**: Tambahkan `WebViewClient` untuk menangani error pemuatan, meskipun error jarang terjadi pada file lokal:
  ```java
  webView.setWebViewClient(new WebViewClient() {
      @Override
      public void onReceivedError(WebView view, int errorCode, String description, String failingUrl) {
          super.onReceivedError(view, errorCode, description, failingUrl);
          Toast.makeText(MainActivity.this, "Error: " + description, Toast.LENGTH_LONG).show();
      }
  });
  ```
- **Optimasi**: Gunakan pengaturan seperti `setDomStorageEnabled(true)` atau `setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK)` jika file HTML memerlukan penyimpanan lokal atau caching (lihat respons sebelumnya tentang optimasi).

Dengan langkah-langkah ini, Anda dapat dengan mudah memuat dan menampilkan file HTML dari folder `assets` di WebView. Jika ada pertanyaan lebih lanjut atau Anda membutuhkan contoh spesifik, silakan beri tahu!


#  **Cara Integrasi dengan Javascript**
---

Untuk mengintegrasikan JavaScript dengan WebView di aplikasi Android menggunakan Java, Anda dapat menggunakan `addJavascriptInterface` untuk menghubungkan kode JavaScript dengan kode Java di aplikasi. Berikut adalah contoh langkah-langkah dan kode untuk mengintegrasikan JavaScript dengan WebView:

### Langkah-langkah:
1. **Aktifkan JavaScript** di WebView.
2. **Buat interface Java** yang akan dipanggil dari JavaScript.
3. **Tambahkan interface tersebut** ke WebView menggunakan `addJavascriptInterface`.
4. **Buat HTML dengan JavaScript** yang memanggil fungsi Java melalui interface.
5. **Tangani komunikasi dua arah** (opsional) antara JavaScript dan Java.

### Contoh Kode:

#### 1. Layout XML (res/layout/activity_main.xml)
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <WebView
        android:id="@+id/webView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>
```

#### 2. Buat Interface Java untuk JavaScript
Buat kelas Java yang akan digunakan sebagai jembatan antara JavaScript dan aplikasi Android.

```java
package com.example.mywebviewapp;

import android.content.Context;
import android.webkit.JavascriptInterface;
import android.widget.Toast;

public class WebAppInterface {
    Context mContext;

    WebAppInterface(Context context) {
        mContext = context;
    }

    // Fungsi ini akan dipanggil dari JavaScript
    @JavascriptInterface
    public void showToast(String message) {
        Toast.makeText(mContext, message, Toast.LENGTH_SHORT).show();
    }

    // Contoh fungsi lain
    @JavascriptInterface
    public String getDataFromAndroid() {
        return "Data dari Android!";
    }
}
```

#### 3. Kode Activity (MainActivity.java)
```java
package com.example.mywebviewapp;

import android.os.Bundle;
import android.webkit.WebSettings;
import android.webkit.WebView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private WebView webView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Inisialisasi WebView
        webView = findViewById(R.id.webView);

        // Aktifkan JavaScript
        WebSettings webSettings = webView.getSettings();
        webSettings.setJavaScriptEnabled(true);

        // Tambahkan interface JavaScript
        webView.addJavascriptInterface(new WebAppInterface(this), "Android");

        // Contoh HTML dengan JavaScript
        String htmlContent = "<html>" +
                "<head>" +
                "<title>Integrasi JavaScript</title>" +
                "</head>" +
                "<body>" +
                "<h1>Integrasi JavaScript dengan WebView</h1>" +
                "<button onclick=\"callAndroidFunction()\">Panggil Fungsi Android</button>" +
                "<p id=\"result\">Menunggu data...</p>" +
                "<script>" +
                "function callAndroidFunction() {" +
                "    Android.showToast('Halo dari JavaScript!');" + // Memanggil fungsi Java
                "    document.getElementById('result').innerText = Android.getDataFromAndroid();" + // Mengambil data dari Java
                "}" +
                "</script>" +
                "</body>" +
                "</html>";

        // Memuat konten HTML
        webView.loadData(htmlContent, "text/html", "UTF-8");
    }

    // Menangani tombol back
    @Override
    public void onBackPressed() {
        if (webView.canGoBack()) {
            webView.goBack();
        } else {
            super.onBackPressed();
        }
    }
}
```

#### 4. Tambahkan Izin di AndroidManifest.xml
Jika memuat konten dari internet, pastikan izin berikut ada:
```xml
<uses-permission android:name="android.permission.INTERNET" />
```

### Penjelasan Kode:
- **`WebAppInterface`**: Kelas ini berfungsi sebagai jembatan antara JavaScript dan Java. Anotasi `@JavascriptInterface` diperlukan agar fungsi dapat diakses dari JavaScript.
- **`addJavascriptInterface`**: Menghubungkan objek `WebAppInterface` ke WebView dengan nama "Android" (nama ini akan digunakan di JavaScript).
- **HTML dengan JavaScript**: Tombol dalam HTML memanggil fungsi `callAndroidFunction`, yang:
  - Memanggil `Android.showToast` untuk menampilkan Toast di Android.
  - Memanggil `Android.getDataFromAndroid` untuk mengambil data dari Java dan menampilkannya di halaman web.
- **`setJavaScriptEnabled(true)`**: Mengaktifkan JavaScript di WebView (pastikan hanya diaktifkan jika diperlukan karena alasan keamanan).

### Contoh File HTML di Assets (opsional)
Jika Anda ingin memuat HTML dari folder `assets`, buat file (misalnya: `sample.html`) di `app/src/main/assets`:
```html
<html>
<head>
    <title>Integrasi JavaScript</title>
</head>
<body>
    <h1>Integrasi JavaScript dengan WebView</h1>
    <button onclick="callAndroidFunction()">Panggil Fungsi Android</button>
    <p id="result">Menunggu data...</p>
    <script>
        function callAndroidFunction() {
            Android.showToast('Halo dari JavaScript!');
            document.getElementById('result').innerText = Android.getDataFromAndroid();
        }
    </script>
</body>
</html>
```
Kemudian, muat file tersebut di `MainActivity.java`:
```java
webView.loadUrl("file:///android_asset/sample.html");
```

### Catatan Penting:
1. **Keamanan**: Penggunaan `addJavascriptInterface` memiliki risiko keamanan, terutama jika memuat konten dari sumber yang tidak tepercaya. Gunakan hanya dengan konten yang Anda kontrol.
2. **API Level**: Untuk API level 17+ (Android 4.2+), hanya metode dengan anotasi `@JavascriptInterface` yang dapat diakses dari JavaScript.
3. **Error Handling**: Tambahkan `WebViewClient` untuk menangani error atau memantau proses pemuatan halaman.
4. **JavaScript ke Android**: Panggilan dari JavaScript ke Java bersifat sinkronus, pastikan logika di sisi Java ringan untuk menghindari ANR (Application Not Responding).
5. **Android ke JavaScript**: Untuk memanggil JavaScript dari Java, gunakan `webView.evaluateJavascript("javascriptFunction()", null);`.

Contoh memanggil JavaScript dari Java:
```java
webView.evaluateJavascript("javascript:document.getElementById('result').innerText = 'Data dari Android';", null);
```

Dengan kode di atas, Anda dapat membuat komunikasi dua arah antara JavaScript dan aplikasi Android melalui WebView. Sesuaikan sesuai kebutuhan aplikasi Anda!


#  **Penangan Error**
---

Untuk menangani error pada WebView di aplikasi Android menggunakan Java, Anda dapat menggunakan `WebViewClient` untuk mendeteksi dan menangani berbagai jenis error seperti kegagalan pemuatan halaman, masalah koneksi, atau error SSL. Berikut adalah contoh implementasi penanganan error WebView yang komprehensif:

### Langkah-langkah:
1. **Gunakan WebViewClient**: Override metode seperti `onReceivedError`, `onReceivedHttpError`, dan `onReceivedSslError` untuk menangani error spesifik.
2. **Tampilkan pesan error**: Gunakan Toast, Snackbar, atau UI khusus untuk memberi tahu pengguna tentang error.
3. **Tangani navigasi**: Pastikan aplikasi tetap responsif, misalnya dengan opsi untuk mencoba ulang atau kembali.

### Contoh Kode:

#### 1. Layout XML (res/layout/activity_main.xml)
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <WebView
        android:id="@+id/webView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>
```

#### 2. Kode Activity (MainActivity.java)
```java
package com.example.mywebviewapp;

import android.os.Bundle;
import android.webkit.WebResourceError;
import android.webkit.WebResourceRequest;
import android.webkit.WebView;
import android.webkit.WebViewClient;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private WebView webView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Inisialisasi WebView
        webView = findViewById(R.id.webView);

        // Aktifkan JavaScript (opsional)
        webView.getSettings().setJavaScriptEnabled(true);

        // Set WebViewClient untuk menangani error
        webView.setWebViewClient(new WebViewClient() {
            // Menangani error umum (API level < 23)
            @Override
            public void onReceivedError(WebView view, int errorCode, String description, String failingUrl) {
                super.onReceivedError(view, errorCode, description, failingUrl);
                handleError(errorCode, description);
            }

            // Menangani error untuk API level 23+ (Android 6.0+)
            @Override
            public void onReceivedError(WebView view, WebResourceRequest request, WebResourceError error) {
                super.onReceivedError(view, request, error);
                handleError(error.getErrorCode(), error.getDescription().toString());
            }

            // Menangani HTTP error (contoh: 404, 500)
            @Override
            public void onReceivedHttpError(WebView view, WebResourceRequest request, WebResourceResponse errorResponse) {
                super.onReceivedHttpError(view, request, errorResponse);
                String message = "HTTP Error: " + errorResponse.getStatusCode() + " - " + errorResponse.getReasonPhrase();
                Toast.makeText(MainActivity.this, message, Toast.LENGTH_LONG).show();
            }

            // Menangani SSL error
            @Override
            public void onReceivedSslError(WebView view, android.webkit.SslErrorHandler handler, android.net.http.SslError error) {
                super.onReceivedSslError(view, handler, error);
                Toast.makeText(MainActivity.this, "SSL Error: " + error.getPrimaryError(), Toast.LENGTH_LONG).show();
                // handler.proceed(); // Hati-hati: hanya gunakan untuk debugging, tidak disarankan untuk produksi
                handler.cancel(); // Batalkan pemuatan jika SSL error
            }

            // Dipanggil saat halaman selesai dimuat
            @Override
            public void onPageFinished(WebView view, String url) {
                super.onPageFinished(view, url);
                Toast.makeText(MainActivity.this, "Halaman selesai dimuat", Toast.LENGTH_SHORT).show();
            }
        });

        // Memuat URL atau HTML
        webView.loadUrl("https://example.com"); // Ganti dengan URL atau HTML yang diinginkan
        // Contoh memuat HTML:
        // String htmlContent = "<html><body><h1>Contoh WebView</h1></body></html>";
        // webView.loadData(htmlContent, "text/html", "UTF-8");
    }

    // Fungsi untuk menangani error
    private void handleError(int errorCode, String description) {
        String errorMessage;
        switch (errorCode) {
            case WebViewClient.ERROR_HOST_LOOKUP:
                errorMessage = "Gagal menemukan host: " + description;
                break;
            case WebViewClient.ERROR_CONNECT:
                errorMessage = "Gagal koneksi ke server: " + description;
                break;
            case WebViewClient.ERROR_TIMEOUT:
                errorMessage = "Koneksi timeout: " + description;
                break;
            case WebViewClient.ERROR_UNKNOWN:
                errorMessage = "Error tidak dikenal: " + description;
                break;
            default:
                errorMessage = "Error: " + description + " (Code: " + errorCode + ")";
                break;
        }
        Toast.makeText(MainActivity.this, errorMessage, Toast.LENGTH_LONG).show();

        // Opsional: Tampilkan halaman error kustom
        String errorHtml = "<html><body><h1>Error</h1><p>" + errorMessage + "</p></body></html>";
        webView.loadData(errorHtml, "text/html", "UTF-8");
    }

    // Menangani tombol back
    @Override
    public void onBackPressed() {
        if (webView.canGoBack()) {
            webView.goBack();
        } else {
            super.onBackPressed();
        }
    }
}
```

#### 3. Tambahkan Izin di AndroidManifest.xml
Jika memuat konten dari internet, tambahkan izin berikut:
```xml
<uses-permission android:name="android.permission.INTERNET" />
```

### Penjelasan Kode:
1. **`WebViewClient`**:
   - `onReceivedError`: Menangani error umum seperti kegagalan koneksi atau timeout. Ada dua versi:
     - Untuk API < 23 (gunakan parameter `int errorCode, String description`).
     - Untuk API ≥ 23 (gunakan `WebResourceRequest` dan `WebResourceError`).
   - `onReceivedHttpError`: Menangani error HTTP seperti 404 (Not Found) atau 500 (Server Error).
   - `onReceivedSslError`: Menangani error SSL, seperti sertifikat tidak valid. **Catatan**: Jangan gunakan `handler.proceed()` di produksi karena dapat membahayakan keamanan.
   - `onPageFinished`: Dipanggil saat halaman selesai dimuat, dapat digunakan untuk memberi tahu pengguna bahwa pemuatan berhasil.

2. **`handleError` Method**:
   - Mengelompokkan logika penanganan error untuk memberikan pesan yang lebih deskriptif berdasarkan `errorCode`.
   - Menampilkan pesan error menggunakan `Toast`.
   - Opsional: Memuat halaman HTML kustom untuk menampilkan pesan error di WebView.

3. **Error Umum yang Ditangani**:
   - `ERROR_HOST_LOOKUP`: Gagal menemukan host (misalnya, domain tidak ditemukan).
   - `ERROR_CONNECT`: Gagal terhubung ke server.
   - `ERROR_TIMEOUT`: Pemuatan halaman timeout.
   - `ERROR_UNKNOWN`: Error tidak diketahui.

4. **Navigasi**: Method `onBackPressed` memungkinkan WebView kembali ke halaman sebelumnya jika memungkinkan.

### Catatan Penting:
1. **Keamanan SSL**: Jangan gunakan `handler.proceed()` untuk error SSL di aplikasi produksi, karena dapat membuat aplikasi rentan terhadap serangan man-in-the-middle.
2. **Debugging**: Gunakan Logcat atau Toast untuk mencatat error selama pengembangan.
3. **Custom Error Page**: Anda dapat memuat halaman HTML kustom atau menampilkan UI alternatif (misalnya, tombol "Coba Lagi") saat error terjadi.
4. **Koneksi Internet**: Tambahkan pemeriksaan koneksi internet sebelum memuat URL untuk memberikan pengalaman pengguna yang lebih baik.
   ```java
   private boolean isNetworkAvailable() {
       ConnectivityManager cm = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
       NetworkInfo networkInfo = cm.getActiveNetworkInfo();
       return networkInfo != null && networkInfo.isConnected();
   }
   ```

   Contoh penggunaan:
   ```java
   if (isNetworkAvailable()) {
       webView.loadUrl("https://example.com");
   } else {
       Toast.makeText(this, "Tidak ada koneksi internet", Toast.LENGTH_LONG).show();
       webView.loadData("<html><body><h1>Tidak ada koneksi</h1></body></html>", "text/html", "UTF-8");
   }
   ```

   Tambahkan izin berikut di AndroidManifest.xml untuk pemeriksaan koneksi:
   ```xml
   <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
   ```

5. **API Level**: Pastikan Anda menangani perbedaan versi API untuk `onReceivedError` seperti yang ditunjukkan di kode.

### Contoh Tambahan: Tombol Coba Lagi
Jika Anda ingin menambahkan tombol "Coba Lagi" pada halaman error:
```java
String errorHtml = "<html><body>" +
        "<h1>Error</h1>" +
        "<p>" + errorMessage + "</p>" +
        "<button onclick=\"window.location.reload()\">Coba Lagi</button>" +
        "</body></html>";
webView.loadData(errorHtml, "text/html", "UTF-8");
```

Dengan kode di atas, Anda dapat menangani berbagai jenis error WebView secara efektif dan memberikan pengalaman pengguna yang lebih baik. Sesuaikan logika penanganan error sesuai kebutuhan aplikasi Anda!



Untuk mengoptimalkan performa dan pengalaman pengguna WebView di aplikasi Android menggunakan Java, Anda perlu mempertimbangkan aspek seperti kecepatan pemuatan, penggunaan memori, efisiensi rendering, dan keamanan. Berikut adalah panduan optimasi WebView beserta contoh kode dalam bahasa Java:

### Strategi Optimasi WebView
1. **Aktifkan Caching**: Gunakan cache untuk mengurangi waktu pemuatan halaman.
2. **Optimalkan Pengaturan WebView**: Konfigurasi pengaturan seperti JavaScript, DOM storage, dan rendering untuk performa terbaik.
3. **Manajemen Memori**: Kurangi penggunaan memori dengan membatasi fitur yang tidak diperlukan.
4. **Tangani Pemuatan Konten**: Gunakan lazy loading atau preloading untuk konten berat.
5. **Keamanan**: Pastikan hanya konten tepercaya yang dimuat untuk menghindari risiko keamanan.
6. **Penanganan Error**: Tangani error dengan baik untuk meningkatkan UX (seperti yang telah dibahas sebelumnya).
7. **Optimasi JavaScript**: Minimalkan eksekusi JavaScript yang berat dan gunakan komunikasi efisien dengan Java.
8. **Hardware Acceleration**: Aktifkan akselerasi perangkat keras untuk rendering yang lebih cepat.

### Contoh Kode dengan Optimasi

#### 1. Layout XML (res/layout/activity_main.xml)
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <WebView
        android:id="@+id/webView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>
```

#### 2. Kode Activity (MainActivity.java)
```java
package com.example.mywebviewapp;

import android.content.Context;
import android.net.ConnectivityManager;
import android.net.NetworkInfo;
import android.os.Bundle;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.webkit.WebViewClient;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private WebView webView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Inisialisasi WebView
        webView = findViewById(R.id.webView);

        // Optimasi pengaturan WebView
        WebSettings webSettings = webView.getSettings();
        webSettings.setJavaScriptEnabled(true); // Aktifkan hanya jika diperlukan
        webSettings.setDomStorageEnabled(true); // Aktifkan DOM storage untuk aplikasi web modern
        webSettings.setDatabaseEnabled(true); // Aktifkan database untuk penyimpanan lokal
        webSettings.setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK); // Gunakan cache jika tersedia
        webSettings.setAppCacheEnabled(true); // Aktifkan AppCache
        webSettings.setAppCachePath(getCacheDir().getAbsolutePath()); // Tentukan path cache
        webSettings.setLoadWithOverviewMode(true); // Skala konten agar sesuai layar
        webSettings.setUseWideViewPort(true); // Dukung viewport responsif
        webSettings.setBuiltInZoomControls(true); // Aktifkan zoom (opsional)
        webSettings.setDisplayZoomControls(false); // Sembunyikan kontrol zoom UI
        webSettings.setAllowFileAccess(false); // Tingkatkan keamanan dengan menonaktifkan akses file
        webSettings.setAllowContentAccess(false); // Nonaktifkan akses konten lokal

        // Aktifkan hardware acceleration (default di Android 4.4+)
        webView.setLayerType(View.LAYER_TYPE_HARDWARE, null);

        // Optimasi JavaScript Interface
        webView.addJavascriptInterface(new WebAppInterface(this), "Android");

        // Set WebViewClient untuk penanganan pemuatan dan error
        webView.setWebViewClient(new WebViewClient() {
            @Override
            public boolean shouldOverrideUrlLoading(WebView view, String url) {
                view.loadUrl(url); // Tangani pemuatan URL di WebView
                return true;
            }

            @Override
            public void onReceivedError(WebView view, int errorCode, String description, String failingUrl) {
                super.onReceivedError(view, errorCode, description, failingUrl);
                Toast.makeText(MainActivity.this, "Error: " + description, Toast.LENGTH_LONG).show();
                // Muat halaman error kustom
                String errorHtml = "<html><body><h1>Error</h1><p>" + description + "</p></body></html>";
                webView.loadData(errorHtml, "text/html", "UTF-8");
            }

            @Override
            public void onPageFinished(WebView view, String url) {
                super.onPageFinished(view, url);
                // Optimasi: Injeksi JavaScript setelah halaman selesai dimuat
                webView.evaluateJavascript("document.body.style.backgroundColor = '#f0f0f0';", null);
            }
        });

        // Cek koneksi sebelum memuat
        if (isNetworkAvailable()) {
            // Preload atau muat URL
            webView.loadUrl("https://example.com");
            // Contoh memuat HTML lokal:
            // String htmlContent = "<html><body><h1>Contoh WebView</h1><button onclick=\"Android.showToast('Halo!')\">Klik</button></body></html>";
            // webView.loadDataWithBaseURL(null, htmlContent, "text/html", "UTF-8", null);
        } else {
            Toast.makeText(this, "Tidak ada koneksi internet", Toast.LENGTH_LONG).show();
            webView.loadData("<html><body><h1>Tidak ada koneksi</h1></body></html>", "text/html", "UTF-8");
        }
    }

    // Interface untuk komunikasi dengan JavaScript
    public class WebAppInterface {
        Context mContext;

        WebAppInterface(Context context) {
            mContext = context;
        }

        @android.webkit.JavascriptInterface
        public void showToast(String message) {
            Toast.makeText(mContext, message, Toast.LENGTH_SHORT).show();
        }
    }

    // Cek koneksi internet
    private boolean isNetworkAvailable() {
        ConnectivityManager cm = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
        NetworkInfo networkInfo = cm.getActiveNetworkInfo();
        return networkInfo != null && networkInfo.isConnected();
    }

    // Menangani tombol back
    @Override
    public void onBackPressed() {
        if (webView.canGoBack()) {
            webView.goBack();
        } else {
            super.onBackPressed();
        }
    }

    // Optimasi memori: Bersihkan WebView saat Activity dihancurkan
    @Override
    protected void onDestroy() {
        super.onDestroy();
        if (webView != null) {
            webView.stopLoading(); // Hentikan pemuatan
            webView.clearCache(true); // Bersihkan cache
            webView.clearHistory(); // Bersihkan riwayat
            webView.destroy(); // Hancurkan WebView
            webView = null;
        }
    }
}
```

#### 3. Tambahkan Izin di AndroidManifest.xml
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

### Penjelasan Optimasi

1. **Caching**:
   - `setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK)`: Memanfaatkan cache jika tersedia, mengurangi penggunaan jaringan.
   - `setAppCacheEnabled(true)` dan `setAppCachePath`: Mengaktifkan AppCache untuk menyimpan aset web secara lokal.
   - `clearCache(true)` di `onDestroy`: Membersihkan cache saat tidak diperlukan untuk menghemat memori.

2. **Pengaturan WebView**:
   - `setDomStorageEnabled(true)`: Mengaktifkan DOM storage untuk aplikasi web modern.
   - `setLoadWithOverviewMode(true)` dan `setUseWideViewPort(true)`: Memastikan konten web responsif dan sesuai dengan layar.
   - `setAllowFileAccess(false)` dan `setAllowContentAccess(false)`: Meningkatkan keamanan dengan membatasi akses ke file lokal.
   - `setDisplayZoomControls(false)`: Menyembunyikan kontrol zoom bawaan untuk UX yang lebih bersih.

3. **Hardware Acceleration**:
   - `setLayerType(View.LAYER_TYPE_HARDWARE, null)`: Mengaktifkan akselerasi perangkat keras untuk rendering lebih cepat (default di Android 4.4+).
   - Catatan: Jika ada masalah rendering, Anda bisa beralih ke `LAYER_TYPE_SOFTWARE` untuk debugging.

4. **Manajemen Memori**:
   - Membersihkan WebView di `onDestroy` untuk mencegah kebocoran memori.
   - Menghentikan pemuatan (`stopLoading`) dan membersihkan riwayat (`clearHistory`) saat Activity dihancurkan.

5. **Pemeriksaan Koneksi**:
   - Menggunakan `isNetworkAvailable` untuk memeriksa koneksi sebelum memuat URL, mencegah error yang tidak perlu.
   - Menampilkan halaman error kustom jika tidak ada koneksi.

6. **Optimasi JavaScript**:
   - Menggunakan `evaluateJavascript` untuk eksekusi JavaScript yang lebih efisien dibandingkan `loadUrl("javascript:..")`.
   - Membatasi penggunaan `addJavascriptInterface` hanya untuk fungsi yang diperlukan, dengan anotasi `@JavascriptInterface` untuk keamanan.

7. **Penanganan Error**:
   - Menggunakan `WebViewClient` untuk menangani error (seperti dijelaskan di respons sebelumnya).
   - Menampilkan halaman error kustom untuk UX yang lebih baik.

8. **Preloading Konten**:
   - Anda dapat memuat aset statis dari folder `assets` untuk mengurangi ketergantungan pada jaringan:
     ```java
     webView.loadUrl("file:///android_asset/index.html");
     ```
   - Pastikan file HTML, CSS, dan JavaScript dioptimalkan (misalnya, minifikasi).

9. **Threading**:
   - Pastikan operasi berat seperti pemrosesan data dilakukan di thread terpisah untuk menghindari ANR (Application Not Responding).
   - Contoh: Gunakan AsyncTask atau Thread untuk memproses data sebelum memuat ke WebView.

### Tips Tambahan
1. **Minifikasi Aset Web**: Minifikasi file HTML, CSS, dan JavaScript untuk mengurangi ukuran dan waktu pemuatan.
2. **Lazy Loading**: Jika memuat konten berat (misalnya, gambar), gunakan teknik lazy loading di sisi JavaScript.
3. **Debugging WebView**:
   - Aktifkan debugging WebView untuk pengembangan:
     ```java
     if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
         WebView.setWebContentsDebuggingEnabled(true);
     }
     ```
   - Gunakan Chrome DevTools untuk memeriksa WebView melalui `chrome://inspect` di browser Chrome.
4. **Optimasi untuk Perangkat Low-End**:
