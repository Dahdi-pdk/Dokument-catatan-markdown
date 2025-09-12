Solusi: Komunikasi Dua Arah antara WebView dan Kode Java Native Android
Solusi ini akan memberikan panduan langkah demi langkah yang komprehensif dan dapat diimplementasikan untuk membangun komunikasi dua arah antara konten web (HTML/JavaScript) yang ditampilkan di WebView dan kode Java native Android Anda. Solusi ini mencakup konfigurasi WebView, pembuatan JavaScript Interface, dan contoh kode lengkap.
1. Gambaran Umum
Proses komunikasi ini melibatkan beberapa komponen utama:
 * WebView: Komponen UI yang menampilkan konten web.
 * WebSettings: Digunakan untuk mengaktifkan JavaScript di dalam WebView.
 * JavaScript Interface: Sebuah kelas Java yang berfungsi sebagai "jembatan" antara JavaScript dan kode Java. Method dalam kelas ini akan dapat dipanggil dari JavaScript.
 * @JavascriptInterface Annotation: Digunakan untuk menandai method Java yang ingin diekspos ke JavaScript. Ini adalah fitur keamanan penting untuk mencegah eksploitasi.
 * Kode HTML/JavaScript: Konten web yang akan memanggil method Java.
2. Implementasi
Mari kita mulai dengan implementasi langkah demi langkah.
Langkah 1: Tambahkan Izin Internet ke AndroidManifest.xml
Untuk dapat memuat konten web, aplikasi Anda memerlukan izin untuk mengakses internet. Tambahkan baris berikut di dalam tag <manifest> dari file AndroidManifest.xml:
<uses-permission android:name="android.permission.INTERNET" />

Langkah 2: Buat JavaScript Interface
Buat sebuah kelas Java yang akan bertindak sebagai JavaScript Interface. Kelas ini akan berisi method yang ingin Anda panggil dari JavaScript. Pastikan untuk menggunakan anotasi @JavascriptInterface pada setiap method yang ingin Anda panggil.
Contoh JavaScript Interface:
import android.content.Context;
import android.widget.Toast;
import android.webkit.JavascriptInterface;
import android.util.Log;

public class WebAppInterface {
    Context mContext;

    /** Instantiate the interface and set the context */
    WebAppInterface(Context c) {
        mContext = c;
    }

    /** Show a toast from the web page */
    @JavascriptInterface
    public void showToast(String toast) {
        // Method ini akan berjalan di background thread.
        // Untuk mengakses UI, gunakan Handler atau runOnUiThread.
        // Contoh ini langsung menunjukkan Toast, yang aman dari background thread
        Toast.makeText(mContext, toast, Toast.LENGTH_SHORT).show();
    }

    /** Log a message from the web page */
    @JavascriptInterface
    public void logMessage(String message) {
        Log.d("WebViewApp", message);
    }
}

Penjelasan:
 * Kelas WebAppInterface memiliki dua method: showToast dan logMessage.
 * Kedua method tersebut diberi anotasi @JavascriptInterface. Ini menandakan bahwa method ini aman untuk dipanggil dari JavaScript.
 * Method showToast akan menampilkan Toast di Android, sementara logMessage akan menulis pesan ke Logcat.
 * Ingat, method yang dipanggil dari JavaScript tidak berjalan di UI thread, sehingga jika Anda perlu memperbarui UI (misalnya, mengubah teks TextView), Anda harus menggunakan Handler atau Activity.runOnUiThread().
Langkah 3: Konfigurasi WebView di Activity
Sekarang, konfigurasikan WebView di Activity Anda. Ini melibatkan:
 * Mengaktifkan JavaScript melalui WebSettings.
 * Menambahkan JavaScript Interface ke WebView.
 * Memuat file HTML.
Contoh kode di MainActivity.java:
import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.webkit.WebViewClient;
import android.os.Handler;
import android.os.Looper;

public class MainActivity extends AppCompatActivity {

    private WebView myWebView;
    private Handler mainHandler = new Handler(Looper.getMainLooper());

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        myWebView = (WebView) findViewById(R.id.webview);

        // 1. Aktifkan JavaScript
        WebSettings webSettings = myWebView.getSettings();
        webSettings.setJavaScriptEnabled(true);

        // 2. Tambahkan JavaScript Interface
        // Objek 'WebAppInterface' akan diekspos ke JavaScript dengan nama 'Android'
        myWebView.addJavascriptInterface(new WebAppInterface(this), "Android");

        // Opsional: Atur WebViewClient untuk mengontrol navigasi
        myWebView.setWebViewClient(new WebViewClient() {
            // Ini akan memastikan link di WebView tetap terbuka di WebView,
            // tidak membuka browser eksternal.
            @Override
            public boolean shouldOverrideUrlLoading(WebView view, String url) {
                view.loadUrl(url);
                return true;
            }
        });

        // 3. Muat file HTML dari direktori 'assets'
        myWebView.loadUrl("file:///android_asset/my_web_page.html");
    }

    // Metode untuk memanggil JavaScript dari Java (jika diperlukan)
    public void callJavaScriptFunction() {
        // Contoh: Memanggil fungsi JavaScript bernama 'updateStatus'
        // Jalankan di UI thread karena memanipulasi UI
        mainHandler.post(new Runnable() {
            @Override
            public void run() {
                myWebView.evaluateJavascript("javascript:updateStatus('Hello from Android!');", null);
            }
        });
    }

    // Pastikan untuk menangani back button dengan benar
    @Override
    public void onBackPressed() {
        if (myWebView.canGoBack()) {
            myWebView.goBack();
        } else {
            super.onBackPressed();
        }
    }
}

Penjelasan:
 * Baris webSettings.setJavaScriptEnabled(true); sangat penting untuk mengaktifkan JavaScript.
 * myWebView.addJavascriptInterface(new WebAppInterface(this), "Android"); adalah inti dari komunikasi ini. Di sini, kita membuat instance dari WebAppInterface dan menamakannya "Android". Nama ini ("Android") akan digunakan di JavaScript untuk memanggil method Java.
 * myWebView.loadUrl("file:///android_asset/my_web_page.html"); memuat file HTML lokal dari direktori assets.
Langkah 4: Buat File HTML dengan Kode JavaScript
Buatlah file HTML yang akan ditampilkan di WebView. Letakkan file ini di dalam direktori app/src/main/assets. Jika direktori assets belum ada, buatlah terlebih dahulu.
Contoh my_web_page.html:
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Web App with Android Interface</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body { font-family: sans-serif; padding: 20px; }
        button { font-size: 16px; margin: 10px 0; padding: 10px; width: 100%; }
    </style>
</head>
<body>

    <h1>WebView & Android Interface</h1>
    <p>Klik tombol di bawah untuk memanggil method Java.</p>

    <button onclick="showToastFromJs()">Tampilkan Toast</button>
    <button onclick="logMessageFromJs()">Kirim Log ke Android</button>

    <p>Status: <span id="status">Menunggu interaksi...</span></p>

    <script>
        // Fungsi ini akan memeriksa apakah objek "Android" ada
        // sebelum mencoba memanggil method-nya. Ini adalah praktik terbaik.
        function checkAndroidInterface() {
            return typeof Android !== 'undefined' && Android !== null;
        }

        // Memanggil method showToast() dari Java
        function showToastFromJs() {
            if (checkAndroidInterface()) {
                // Panggil method Java 'showToast' dengan parameter string
                Android.showToast("Ini adalah Toast dari JavaScript!");
            } else {
                alert("Interface Android tidak ditemukan!");
            }
        }

        // Memanggil method logMessage() dari Java
        function logMessageFromJs() {
            if (checkAndroidInterface()) {
                // Panggil method Java 'logMessage' dengan parameter string
                Android.logMessage("Pesan dari web page telah dikirim ke Logcat.");
            } else {
                alert("Interface Android tidak ditemukan!");
            }
        }

        // Fungsi ini bisa dipanggil dari kode Java (komunikasi dua arah)
        function updateStatus(message) {
            document.getElementById('status').innerText = message;
        }

    </script>
</body>
</html>

Penjelasan:
 * Di JavaScript, kita memanggil method Java melalui objek yang kita berikan di addJavascriptInterface. Dalam kasus ini, namanya adalah Android.
 * Sintaks pemanggilannya adalah Android.namaMethodJava(parameter).
 * Contoh Android.showToast("Ini adalah Toast dari JavaScript!"); akan memanggil method showToast di kelas WebAppInterface dengan string "Ini adalah Toast dari JavaScript!" sebagai argumen.
 * Kami juga menambahkan fungsi updateStatus di JavaScript. Fungsi ini dapat dipanggil dari kode Java menggunakan myWebView.evaluateJavascript(), memungkinkan komunikasi dua arah.
3. Keamanan dan Pertimbangan Penting
 * Anotasi @JavascriptInterface: Ini adalah fitur keamanan vital yang diperkenalkan di API level 17 (Android 4.2). Tanpa anotasi ini, semua method publik dari kelas JavaScript Interface akan diekspos ke JavaScript, berpotensi menimbulkan kerentanan. Selalu gunakan anotasi ini.
 * XSS (Cross-Site Scripting): Pastikan Anda hanya memuat konten web dari sumber yang tepercaya. Jika Anda memuat konten dari internet, ada risiko konten tersebut mencoba menjalankan kode berbahaya.
 * Thread Safety: Ingat bahwa method yang dipanggil dari JavaScript akan berjalan di background thread. Jika method Anda perlu memperbarui UI, Anda harus beralih ke UI thread menggunakan Handler atau Activity.runOnUiThread(). Contoh:
   @JavascriptInterface
public void updateUIFromWeb(final String message) {
    // Beralih ke UI thread untuk memperbarui TextView
    mainHandler.post(new Runnable() {
        @Override
        public void run() {
            myTextView.setText(message);
        }
    });
}

 * API Level 17 (Android 4.2) dan di bawahnya: Jika Anda mendukung versi Android di bawah 4.2, semua method publik akan terekspos. Ini adalah risiko keamanan yang signifikan. Untuk aplikasi modern, ini tidak lagi menjadi masalah besar karena mayoritas perangkat sudah di atas API level 17.
Dengan mengikuti panduan di atas, Anda sekarang memiliki fondasi yang kuat untuk membangun aplikasi Android yang dapat berkomunikasi secara efektif dengan konten web yang disajikan melalui WebView.
