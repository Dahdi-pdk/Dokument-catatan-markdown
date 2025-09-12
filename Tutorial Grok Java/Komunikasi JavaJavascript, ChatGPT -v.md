Berikut adalah panduan lengkap dan terstruktur untuk memanggil method Java dari HTML/JavaScript yang ditampilkan di WebView Android, dengan penjelasan langkah demi langkah dan kode lengkap siap pakai.


---

✅ 1. Konsep Umum

Android menyediakan cara komunikasi dua arah antara JavaScript di WebView dan kode Java native menggunakan fitur JavaScript Interface.

Untuk memanggil method Java dari JavaScript:

Aktifkan JavaScript di WebView.

Buat kelas Java dengan method yang ingin dipanggil.

Tandai method dengan anotasi @JavascriptInterface.

Tambahkan kelas tersebut ke WebView melalui addJavascriptInterface.



---

✅ 2. Struktur File
```
MyApp/
├── app/
│   └── src/
│       └── main/
│           ├── java/com/example/myapp/MainActivity.java
│           ├── assets/webpage.html
│           └── res/layout/activity_main.xml


```


---

✅ 3. Langkah Implementasi

📌 a. HTML/JavaScript (assets/webpage.html)

```
<!DOCTYPE html>
```

<html>
<head>
    <title>Test Interface</title>
    <script type="text/javascript">
        function callAndroidMethod() {
            AndroidInterface.showToast("Halo dari JavaScript!");
        }
    </script>
</head>
<body>
    <h1>WebView - JavaScript to Java</h1>
    <button onclick="callAndroidMethod()">Panggil Java Method</button>
</body>
</html>


---

📌 b. Layout (res/layout/activity_main.xml)

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <WebView
        android:id="@+id/webview"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
</LinearLayout>


---

📌 c. JavaScript Interface (MyJavaScriptInterface.java)

package com.example.myapp;

import android.content.Context;
import android.webkit.JavascriptInterface;
import android.widget.Toast;

public class MyJavaScriptInterface {
    Context mContext;

    MyJavaScriptInterface(Context context) {
        this.mContext = context;
    }

    @JavascriptInterface
    public void showToast(String message) {
        // Gunakan UI thread untuk tampilkan Toast
        Toast.makeText(mContext, message, Toast.LENGTH_SHORT).show();
    }
}


---

📌 d. MainActivity (MainActivity.java)

package com.example.myapp;

import android.os.Bundle;
import android.webkit.WebView;
import android.webkit.WebSettings;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    WebView webView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        webView = findViewById(R.id.webview);

        // Aktifkan JavaScript
        WebSettings webSettings = webView.getSettings();
        webSettings.setJavaScriptEnabled(true);

        // Tambahkan interface
        webView.addJavascriptInterface(new MyJavaScriptInterface(this), "AndroidInterface");

        // Muat HTML dari assets
        webView.loadUrl("file:///android_asset/webpage.html");
    }
}


---

✅ 4. Catatan Keamanan

Aspek	Penjelasan

@JavascriptInterface	WAJIB untuk Android 4.2+ agar method bisa dipanggil dari JS.
addJavascriptInterface	HANYA tambahkan ke konten trusted seperti file:///android_asset. Jangan pada konten eksternal.
Validasi Input	Validasi semua input dari JS untuk menghindari injeksi.



---

✅ 5. Jika Ingin Update UI (View) dari Method Java

Method yang dipanggil JS dijalankan di background thread. Untuk mengakses UI, gunakan:

@JavascriptInterface
public void showToast(final String message) {
    ((Activity) mContext).runOnUiThread(() -> 
        Toast.makeText(mContext, message, Toast.LENGTH_SHORT).show()
    );
}


---

✅ 6. Uji Coba

Setelah aplikasi dijalankan:

1. Aplikasi menampilkan halaman HTML.


2. Klik tombol Panggil Java Method.


3. Android akan menampilkan Toast dengan pesan dari HTML.




---

✅ 7. Bonus: Panggil JavaScript dari Java

Jika ingin sebaliknya (Java → JavaScript):

webView.evaluateJavascript("javascript:callAndroidMethod()", null);


---

🎯 Ringkasan

Langkah	Detail

1	Aktifkan JavaScript di WebView
2	Buat class Java dengan @JavascriptInterface
3	Tambahkan interface ke WebView
4	Panggil method dari HTML
5	Pastikan keamanan & UI thread handling



---

Kalau kamu butuh versi dengan komunikasi dua arah penuh (JS → Java → JS) atau integrasi dengan Flask API lokal, SQLite, atau lainnya — tinggal bilang saja, aku siap bantu 💡

