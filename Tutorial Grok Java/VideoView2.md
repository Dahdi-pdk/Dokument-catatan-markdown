Berikut adalah kode Java lengkap untuk membuat video player menggunakan **VideoView** (tanpa ExoPlayer) di Android, dengan layout yang dibuat secara **programmatic** (tanpa file XML). Kode ini mencakup pemutaran video, kontrol buffering, dan penanganan lifecycle, dioptimalkan untuk video berukuran besar seperti yang dibahas sebelumnya. Saya juga akan menyertakan logika untuk menangani subtitle sederhana menggunakan `TextView` untuk menampilkan subtitle secara manual, serta `ProgressBar` untuk indikator buffering.

---

### Fitur Kode
- **VideoView**: Untuk memutar video dari URL (streaming) atau penyimpanan lokal.
- **Layout Programmatic**: Membuat `VideoView`, `ProgressBar`, dan `TextView` (untuk subtitle) secara dinamis.
- **Buffering**: Menampilkan `ProgressBar` saat buffering.
- **Subtitle**: Parsing file `.srt` sederhana dan sinkronisasi dengan waktu video.
- **Lifecycle**: Mengelola pemutaran saat pause, resume, dan destroy.
- **Optimasi Video Besar**: Pengaturan untuk menangani video beresolusi tinggi atau ukuran besar.

---

### Prasyarat
- **Izin di AndroidManifest.xml**:
  ```xml
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
  <!-- Untuk Android 13+ -->
  <uses-permission android:name="android.permission.READ_MEDIA_VIDEO" />
  ```
- **File Video**: Video dalam format MP4 atau 3GP (misalnya, URL streaming atau file lokal di `/sdcard/large_video.mp4`).
- **File Subtitle** (opsional): File `.srt` untuk subtitle (misalnya, di `/sdcard/subtitle.srt`).

---

### Kode Java Lengkap
Berikut adalah kode di `MainActivity.java` untuk membuat video player dengan layout programmatic:

```java
import android.media.MediaPlayer;
import android.net.Uri;
import android.os.Bundle;
import android.os.Handler;
import android.view.ViewGroup;
import android.widget.MediaController;
import android.widget.ProgressBar;
import android.widget.RelativeLayout;
import android.widget.TextView;
import android.widget.VideoView;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.content.ContextCompat;
import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {
    private VideoView videoView;
    private ProgressBar progressBar;
    private TextView subtitleText;
    private Handler handler = new Handler();
    private List<Subtitle> subtitles = new ArrayList<>();
    private int currentSubtitleIndex = 0;

    // Kelas untuk menyimpan subtitle
    private static class Subtitle {
        long startTime;
        long endTime;
        String text;

        Subtitle(long startTime, long endTime, String text) {
            this.startTime = startTime;
            this.endTime = endTime;
            this.text = text;
        }
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // Buat layout utama (RelativeLayout)
        RelativeLayout mainLayout = new RelativeLayout(this);
        mainLayout.setLayoutParams(new ViewGroup.LayoutParams(
                ViewGroup.LayoutParams.MATCH_PARENT,
                ViewGroup.LayoutParams.MATCH_PARENT));
        mainLayout.setBackgroundColor(ContextCompat.getColor(this, android.R.color.black));

        // Buat VideoView
        videoView = new VideoView(this);
        RelativeLayout.LayoutParams videoParams = new RelativeLayout.LayoutParams(
                ViewGroup.LayoutParams.MATCH_PARENT,
                ViewGroup.LayoutParams.WRAP_CONTENT);
        videoParams.addRule(RelativeLayout.CENTER_IN_PARENT);
        videoView.setLayoutParams(videoParams);

        // Buat ProgressBar untuk buffering
        progressBar = new ProgressBar(this);
        RelativeLayout.LayoutParams progressParams = new RelativeLayout.LayoutParams(
                ViewGroup.LayoutParams.WRAP_CONTENT,
                ViewGroup.LayoutParams.WRAP_CONTENT);
        progressParams.addRule(RelativeLayout.CENTER_IN_PARENT);
        progressBar.setLayoutParams(progressParams);
        progressBar.setVisibility(ViewGroup.GONE);

        // Buat TextView untuk subtitle
        subtitleText = new TextView(this);
        RelativeLayout.LayoutParams subtitleParams = new RelativeLayout.LayoutParams(
                ViewGroup.LayoutParams.MATCH_PARENT,
                ViewGroup.LayoutParams.WRAP_CONTENT);
        subtitleParams.addRule(RelativeLayout.ALIGN_BOTTOM, videoView.getId());
        subtitleParams.setMargins(10, 10, 10, 10);
        subtitleText.setLayoutParams(subtitleParams);
        subtitleText.setBackgroundColor(0x80000000); // Semi-transparan
        subtitleText.setTextColor(ContextCompat.getColor(this, android.R.color.white));
        subtitleText.setTextSize(16);
        subtitleText.setGravity(android.view.Gravity.CENTER);
        subtitleText.setPadding(10, 10, 10, 10);

        // Tambahkan view ke layout
        mainLayout.addView(videoView);
        mainLayout.addView(progressBar);
        mainLayout.addView(subtitleText);

        // Set layout sebagai konten activity
        setContentView(mainLayout);

        // Tambahkan MediaController
        MediaController mediaController = new MediaController(this);
        mediaController.setAnchorView(videoView);
        videoView.setMediaController(mediaController);

        // Tentukan sumber video
        String videoUrl = "https://archive.org/download/BigBuckBunny_124/Content/big_buck_bunny_720p_surround.mp4"; // Video besar (~440MB, 720p)
        // Untuk lokal: "file:///sdcard/large_video.mp4"
        Uri uri = Uri.parse(videoUrl);
        videoView.setVideoURI(uri);

        // Load subtitle dari file .srt (opsional)
        loadSubtitles("/sdcard/subtitle.srt"); // Ganti dengan path subtitle

        // Listener untuk persiapan video
        videoView.setOnPreparedListener(new MediaPlayer.OnPreparedListener() {
            @Override
            public void onPrepared(MediaPlayer mp) {
                // Optimasi untuk video besar
                mp.setVideoScalingMode(MediaPlayer.VIDEO_SCALING_MODE_SCALE_TO_FIT);
                mp.setScreenOnWhilePlaying(true); // Jaga layar menyala
                progressBar.setVisibility(ViewGroup.GONE);
                videoView.start();
                startSubtitleSync();
            }
        });

        // Listener untuk buffering
        videoView.setOnInfoListener(new MediaPlayer.OnInfoListener() {
            @Override
            public boolean onInfo(MediaPlayer mp, int what, int extra) {
                switch (what) {
                    case MediaPlayer.MEDIA_INFO_BUFFERING_START:
                        progressBar.setVisibility(ViewGroup.VISIBLE);
                        System.out.println("Buffering dimulai");
                        return true;
                    case MediaPlayer.MEDIA_INFO_BUFFERING_END:
                        progressBar.setVisibility(ViewGroup.GONE);
                        System.out.println("Buffering selesai");
                        return true;
                    case MediaPlayer.MEDIA_INFO_VIDEO_RENDERING_START:
                        System.out.println("Rendering video dimulai");
                        return true;
                }
                return false;
            }
        });

        // Listener untuk error
        videoView.setOnErrorListener(new MediaPlayer.OnErrorListener() {
            @Override
            public boolean onError(MediaPlayer mp, int what, int extra) {
                progressBar.setVisibility(ViewGroup.GONE);
                System.out.println("Error: " + what + ", Extra: " + extra);
                // Coba ulang jika diperlukan
                retryPlayback(uri);
                return true;
            }
        });

        // Tampilkan ProgressBar saat memulai buffering
        progressBar.setVisibility(ViewGroup.VISIBLE);
    }

    // Parsing file .srt
    private void loadSubtitles(String srtPath) {
        try {
            File file = new File(srtPath);
            if (!file.exists()) {
                System.out.println("File subtitle tidak ditemukan");
                return;
            }
            BufferedReader reader = new BufferedReader(new FileReader(file));
            String line;
            StringBuilder text = new StringBuilder();
            long startTime = 0, endTime = 0;

            while ((line = reader.readLine()) != null) {
                if (line.matches("\\d+")) { // Nomor subtitle
                    line = reader.readLine(); // Timestamp
                    String[] times = line.split(" --> ");
                    startTime = parseTime(times[0]);
                    endTime = parseTime(times[1]);

                    // Baca teks subtitle
                    text = new StringBuilder();
                    while ((line = reader.readLine()) != null && !line.isEmpty()) {
                        text.append(line).append("\n");
                    }
                    subtitles.add(new Subtitle(startTime, endTime, text.toString().trim()));
                }
            }
            reader.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // Konversi timestamp (00:00:01,000) ke milidetik
    private long parseTime(String time) {
        String[] parts = time.replace(",", ":").split(":");
        int hours = Integer.parseInt(parts[0]);
        int minutes = Integer.parseInt(parts[1]);
        int seconds = Integer.parseInt(parts[2]);
        int millis = Integer.parseInt(parts[3]);
        return (hours * 3600000L) + (minutes * 60000L) + (seconds * 1000L) + millis;
    }

    // Sinkronisasi subtitle dengan video
    private void startSubtitleSync() {
        handler.post(new Runnable() {
            @Override
            public void run() {
                if (videoView.isPlaying()) {
                    long currentPosition = videoView.getCurrentPosition();
                    subtitleText.setText("");

                    for (int i = 0; i < subtitles.size(); i++) {
                        Subtitle subtitle = subtitles.get(i);
                        if (currentPosition >= subtitle.startTime && currentPosition <= subtitle.endTime) {
                            subtitleText.setText(subtitle.text);
                            currentSubtitleIndex = i;
                            break;
                        }
                    }
                }
                handler.postDelayed(this, 100); // Perbarui setiap 100ms
            }
        });
    }

    // Fungsi untuk mencoba ulang
    private void retryPlayback(Uri uri) {
        videoView.setVideoURI(uri);
        progressBar.setVisibility(ViewGroup.VISIBLE);
    }

    // Kelola lifecycle
    @Override
    protected void onPause() {
        super.onPause();
        if (videoView != null && videoView.isPlaying()) {
            videoView.pause();
            handler.removeCallbacksAndMessages(null);
        }
    }

    @Override
    protected void onResume() {
        super.onResume();
        if (videoView != null && !videoView.isPlaying()) {
            videoView.start();
            startSubtitleSync();
        }
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        if (videoView != null) {
            videoView.stopPlayback();
        }
        handler.removeCallbacksAndMessages(null);
    }
}
```

---

### Penjelasan Kode

#### 1. **Layout Programmatic**
- **RelativeLayout**: Digunakan sebagai layout utama untuk menampung `VideoView`, `ProgressBar`, dan `TextView`.
- **VideoView**: Ditempatkan di tengah dengan `MATCH_PARENT` untuk lebar dan `WRAP_CONTENT` untuk tinggi agar sesuai dengan rasio video.
- **ProgressBar**: Ditempatkan di tengah untuk indikator buffering, disembunyikan saat video siap.
- **TextView**: Digunakan untuk menampilkan subtitle, diletakkan di bagian bawah `VideoView` dengan latar belakang semi-transparan.

#### 2. **Pemutaran Video**
- **Sumber Video**: Mendukung streaming (contoh: URL MP4 besar) atau file lokal (`file:///sdcard/large_video.mp4`).
- **MediaController**: Menyediakan kontrol UI standar (play, pause, seek bar).
- **Optimasi Video Besar**:
  - `setVideoScalingMode`: Menggunakan `SCALE_TO_FIT` untuk menghindari scaling berlebihan.
  - `setScreenOnWhilePlaying(true)`: Menjaga layar menyala selama pemutaran.

#### 3. **Buffering**
- **OnInfoListener**: Mendeteksi status buffering (`MEDIA_INFO_BUFFERING_START` dan `MEDIA_INFO_BUFFERING_END`) untuk menampilkan/sembunyikan `ProgressBar`.
- **Indikator**: `ProgressBar` ditampilkan saat buffering untuk memberikan umpan balik kepada pengguna.

#### 4. **Subtitle**
- **Parsing .srt**: File subtitle diparsing secara manual untuk mengekstrak waktu mulai, waktu selesai, dan teks.
- **Sinkronisasi**: `Handler` digunakan untuk memperbarui subtitle setiap 100ms berdasarkan posisi video saat ini.
- **TextView**: Menampilkan subtitle dengan latar belakang semi-transparan untuk visibilitas.

#### 5. **Error Handling**
- **OnErrorListener**: Menangani error seperti URL tidak valid atau kegagalan buffering, dengan opsi untuk mencoba ulang.

#### 6. **Lifecycle**
- **onPause**: Menjeda video dan menghentikan sinkronisasi subtitle.
- **onResume**: Melanjutkan pemutaran dan sinkronisasi subtitle.
- **onDestroy**: Menghentikan pemutaran dan membersihkan `Handler`.

---

### Menangani Izin Runtime (Untuk Video/Subtitle Lokal)
Jika video atau subtitle disimpan di penyimpanan eksternal, tambahkan kode untuk meminta izin:

```java
import android.content.pm.PackageManager;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;

private void checkStoragePermission() {
    if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_EXTERNAL_STORAGE)
            != PackageManager.PERMISSION_GRANTED) {
        ActivityCompat.requestPermissions(this,
                new String[]{Manifest.permission.READ_EXTERNAL_STORAGE}, 100);
    } else {
        setupVideoView();
    }
}

@Override
public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
    super.onRequestPermissionsResult(requestCode, permissions, grantResults);
    if (requestCode == 100 && grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
        setupVideoView();
    } else {
        System.out.println("Izin penyimpanan diperlukan");
    }
}

private void setupVideoView() {
    String videoUrl = "file:///sdcard/large_video.mp4";
    Uri uri = Uri.parse(videoUrl);
    videoView.setVideoURI(uri);
    loadSubtitles("/sdcard/subtitle.srt");
    progressBar.setVisibility(ViewGroup.VISIBLE);
}
```

Tambahkan pemanggilan `checkStoragePermission()` di awal `onCreate` sebelum mengatur `videoView.setVideoURI()`.

---

### Contoh File Subtitle (.srt)
Simpan file di `/sdcard/subtitle.srt`:
```srt
1
00:00:01,000 --> 00:00:03,000
Halo, ini adalah subtitle pertama.

2
00:00:04,000 --> 00:00:06,000
Ini subtitle kedua untuk video.
```

---

### Optimasi untuk Video Berukuran Besar
- **Kompresi**: Gunakan codec H.264/H.265 dengan bitrate lebih rendah untuk mengurangi ukuran file:
  ```bash
  ffmpeg -i input.mp4 -c:v libx264 -b:v 4000k -vf scale=1920:1080 output.mp4
  ```
- **Skala Video**: `SCALE_TO_FIT` membantu mengurangi beban rendering pada perangkat low-end.
- **Koneksi Jaringan**: Untuk streaming, pastikan server mendukung **range requests** (HTTP 206) agar buffering lebih efisien.
- **Hardware Acceleration**: Pastikan di `AndroidManifest.xml`:
  ```xml
  <application android:hardwareAccelerated="true">
  ```

---

### Keterbatasan VideoView
- **Buffering**: Tidak ada kontrol tingkat rendah atas ukuran buffer, sehingga performa bergantung pada MediaPlayer bawaan.
- **Subtitle**: Parsing `.srt` manual bisa memengaruhi performa, terutama untuk video besar.
- **Streaming Adaptif**: Tidak mendukung HLS/DASH, hanya streaming progresif (MP4, 3GP).
- **Alternatif**: Untuk video besar dengan subtitle, **ExoPlayer** lebih disarankan karena dukungan subtitle bawaan dan buffering canggih.

---

### Contoh URL untuk Pengujian
- **Streaming**: `https://archive.org/download/BigBuckBunny_124/Content/big_buck_bunny_720p_surround.mp4` (~440MB, 720p).
- **Lokal**: `file:///sdcard/large_video.mp4`.

Jika Anda ingin menambahkan fitur spesifik (misalnya, tombol retry, kontrol subtitle on/off, atau transisi ke ExoPlayer), beri tahu saya!


---

Berikut adalah kode Java lengkap untuk membuat video player menggunakan **VideoView** (tanpa ExoPlayer) di Android, dengan layout yang dibuat secara **programmatic** (tanpa file XML). Kode ini mencakup pemutaran video, kontrol buffering, dan penanganan lifecycle, dioptimalkan untuk video berukuran besar, tanpa fitur subtitle. Saya akan memastikan kode tetap ringkas, jelas, dan mendukung streaming serta video lokal.

---

### Fitur Kode
- **VideoView**: Memutar video dari URL (streaming) atau penyimpanan lokal.
- **Layout Programmatic**: Membuat `VideoView` dan `ProgressBar` secara dinamis.
- **Buffering**: Menampilkan `ProgressBar` saat buffering.
- **Lifecycle**: Mengelola pemutaran saat pause, resume, dan destroy.
- **Optimasi Video Besar**: Pengaturan untuk video beresolusi tinggi atau ukuran besar.

---

### Prasyarat
- **Izin di AndroidManifest.xml**:
  ```xml
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
  <!-- Untuk Android 13+ -->
  <uses-permission android:name="android.permission.READ_MEDIA_VIDEO" />
  ```
- **File Video**: Video dalam format MP4 atau 3GP (misalnya, URL streaming atau file lokal di `/sdcard/large_video.mp4`).
- **Hardware Acceleration** (disarankan untuk video besar):
  ```xml
  <application android:hardwareAccelerated="true">
  ```

---

### Kode Java Lengkap
File: `MainActivity.java`

```java
import android.media.MediaPlayer;
import android.net.Uri;
import android.os.Bundle;
import android.view.ViewGroup;
import android.widget.MediaController;
import android.widget.ProgressBar;
import android.widget.RelativeLayout;
import android.widget.VideoView;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.content.ContextCompat;

public class MainActivity extends AppCompatActivity {
    private VideoView videoView;
    private ProgressBar progressBar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // Buat layout utama (RelativeLayout)
        RelativeLayout mainLayout = new RelativeLayout(this);
        mainLayout.setLayoutParams(new ViewGroup.LayoutParams(
                ViewGroup.LayoutParams.MATCH_PARENT,
                ViewGroup.LayoutParams.MATCH_PARENT));
        mainLayout.setBackgroundColor(ContextCompat.getColor(this, android.R.color.black));

        // Buat VideoView
        videoView = new VideoView(this);
        RelativeLayout.LayoutParams videoParams = new RelativeLayout.LayoutParams(
                ViewGroup.LayoutParams.MATCH_PARENT,
                ViewGroup.LayoutParams.WRAP_CONTENT);
        videoParams.addRule(RelativeLayout.CENTER_IN_PARENT);
        videoView.setLayoutParams(videoParams);

        // Buat ProgressBar untuk buffering
        progressBar = new ProgressBar(this);
        RelativeLayout.LayoutParams progressParams = new RelativeLayout.LayoutParams(
                ViewGroup.LayoutParams.WRAP_CONTENT,
                ViewGroup.LayoutParams.WRAP_CONTENT);
        progressParams.addRule(RelativeLayout.CENTER_IN_PARENT);
        progressBar.setLayoutParams(progressParams);
        progressBar.setVisibility(ViewGroup.GONE);

        // Tambahkan view ke layout
        mainLayout.addView(videoView);
        mainLayout.addView(progressBar);

        // Set layout sebagai konten activity
        setContentView(mainLayout);

        // Tambahkan MediaController
        MediaController mediaController = new MediaController(this);
        mediaController.setAnchorView(videoView);
        videoView.setMediaController(mediaController);

        // Tentukan sumber video
        String videoUrl = "https://archive.org/download/BigBuckBunny_124/Content/big_buck_bunny_720p_surround.mp4"; // Video besar (~440MB, 720p)
        // Untuk lokal: "file:///sdcard/large_video.mp4"
        Uri uri = Uri.parse(videoUrl);
        videoView.setVideoURI(uri);

        // Listener untuk persiapan video
        videoView.setOnPreparedListener(new MediaPlayer.OnPreparedListener() {
            @Override
            public void onPrepared(MediaPlayer mp) {
                // Optimasi untuk video besar
                mp.setVideoScalingMode(MediaPlayer.VIDEO_SCALING_MODE_SCALE_TO_FIT);
                mp.setScreenOnWhilePlaying(true); // Jaga layar menyala
                progressBar.setVisibility(ViewGroup.GONE);
                videoView.start();
            }
        });

        // Listener untuk buffering
        videoView.setOnInfoListener(new MediaPlayer.OnInfoListener() {
            @Override
            public boolean onInfo(MediaPlayer mp, int what, int extra) {
                switch (what) {
                    case MediaPlayer.MEDIA_INFO_BUFFERING_START:
                        progressBar.setVisibility(ViewGroup.VISIBLE);
                        System.out.println("Buffering dimulai");
                        return true;
                    case MediaPlayer.MEDIA_INFO_BUFFERING_END:
                        progressBar.setVisibility(ViewGroup.GONE);
                        System.out.println("Buffering selesai");
                        return true;
                    case MediaPlayer.MEDIA_INFO_VIDEO_RENDERING_START:
                        System.out.println("Rendering video dimulai");
                        return true;
                }
                return false;
            }
        });

        // Listener untuk error
        videoView.setOnErrorListener(new MediaPlayer.OnErrorListener() {
            @Override
            public boolean onError(MediaPlayer mp, int what, int extra) {
                progressBar.setVisibility(ViewGroup.GONE);
                System.out.println("Error: " + what + ", Extra: " + extra);
                // Coba ulang jika diperlukan
                retryPlayback(uri);
                return true;
            }
        });

        // Tampilkan ProgressBar saat memulai buffering
        progressBar.setVisibility(ViewGroup.VISIBLE);
    }

    // Fungsi untuk mencoba ulang
    private void retryPlayback(Uri uri) {
        videoView.setVideoURI(uri);
        progressBar.setVisibility(ViewGroup.VISIBLE);
    }

    // Kelola lifecycle
    @Override
    protected void onPause() {
        super.onPause();
        if (videoView != null && videoView.isPlaying()) {
            videoView.pause();
        }
    }

    @Override
    protected void onResume() {
        super.onResume();
        if (videoView != null && !videoView.isPlaying()) {
            videoView.start();
        }
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        if (videoView != null) {
            videoView.stopPlayback();
        }
    }
}
```

---

### Penjelasan Kode

#### 1. **Layout Programmatic**
- **RelativeLayout**: Digunakan sebagai layout utama untuk menampung `VideoView` dan `ProgressBar`.
- **VideoView**: Ditempatkan di tengah dengan `MATCH_PARENT` untuk lebar dan `WRAP_CONTENT` untuk tinggi agar sesuai dengan rasio video.
- **ProgressBar**: Ditempatkan di tengah untuk indikator buffering, disembunyikan saat video siap.
- **Warna Latar**: Latar belakang hitam (`android.R.color.black`) untuk estetika.

#### 2. **Pemutaran Video**
- **Sumber Video**: Mendukung streaming (contoh: URL MP4 besar) atau file lokal (`file:///sdcard/large_video.mp4`).
- **MediaController**: Menyediakan kontrol UI standar (play, pause, seek bar).
- **Optimasi Video Besar**:
  - `setVideoScalingMode(MediaPlayer.VIDEO_SCALING_MODE_SCALE_TO_FIT)`: Mengurangi beban rendering untuk video beresolusi tinggi.
  - `setScreenOnWhilePlaying(true)`: Menjaga layar menyala selama pemutaran.

#### 3. **Buffering**
- **OnInfoListener**: Mendeteksi status buffering:
  - `MEDIA_INFO_BUFFERING_START`: Menampilkan `ProgressBar`.
  - `MEDIA_INFO_BUFFERING_END`: Menyembunyikan `ProgressBar`.
- **Indikator**: `ProgressBar` memberikan umpan balik visual saat buffering.

#### 4. **Error Handling**
- **OnErrorListener**: Menangani error seperti URL tidak valid atau kegagalan buffering.
- **Retry**: Fungsi `retryPlayback` mencoba ulang pemutaran jika terjadi error.

#### 5. **Lifecycle**
- **onPause**: Menjeda video untuk menghemat sumber daya.
- **onResume**: Melanjutkan pemutaran jika diperlukan.
- **onDestroy**: Menghentikan pemutaran dan membebaskan sumber daya.

---

### Menangani Izin Runtime (Untuk Video Lokal)
Jika video disimpan di penyimpanan eksternal, tambahkan kode untuk meminta izin:

```java
import android.content.pm.PackageManager;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;

private void checkStoragePermission() {
    if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_EXTERNAL_STORAGE)
            != PackageManager.PERMISSION_GRANTED) {
        ActivityCompat.requestPermissions(this,
                new String[]{Manifest.permission.READ_EXTERNAL_STORAGE}, 100);
    } else {
        setupVideoView();
    }
}

@Override
public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
    super.onRequestPermissionsResult(requestCode, permissions, grantResults);
    if (requestCode == 100 && grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
        setupVideoView();
    } else {
        System.out.println("Izin penyimpanan diperlukan");
    }
}

private void setupVideoView() {
    String videoUrl = "file:///sdcard/large_video.mp4";
    Uri uri = Uri.parse(videoUrl);
    videoView.setVideoURI(uri);
    progressBar.setVisibility(ViewGroup.VISIBLE);
}
```

Tambahkan pemanggilan `checkStoragePermission()` di awal `onCreate` sebelum `videoView.setVideoURI()`.

---

### Optimasi untuk Video Berukuran Besar
- **Kompresi**: Gunakan codec H.264 atau H.265 (HEVC) untuk mengurangi ukuran file tanpa mengorbankan kualitas:
  ```bash
  ffmpeg -i input.mp4 -c:v libx264 -b:v 4000k -vf scale=1920:1080 output.mp4
  ```
- **Skala Video**: `SCALE_TO_FIT` membantu mengurangi beban rendering pada perangkat low-end.
- **Koneksi Jaringan**: Untuk streaming, pastikan server mendukung **range requests** (HTTP 206) untuk buffering progresif.
- **Hardware Acceleration**: Pastikan di `AndroidManifest.xml`:
  ```xml
  <application android:hardwareAccelerated="true">
  ```

---

### Keterbatasan VideoView
- **Buffering**: Tidak ada kontrol tingkat rendah atas ukuran atau waktu buffer, bergantung pada MediaPlayer bawaan.
- **Streaming Adaptif**: Tidak mendukung HLS/DASH, hanya streaming progresif (MP4, 3GP).
- **Performa**: Untuk video besar (misalnya, 4K atau file >500MB), performa bisa menurun pada perangkat low-end.
- **Alternatif**: Jika Anda membutuhkan streaming adaptif, buffering canggih, atau subtitle bawaan, pertimbangkan **ExoPlayer**.

---

### Contoh URL untuk Pengujian
- **Streaming**: `https://archive.org/download/BigBuckBunny_124/Content/big_buck_bunny_720p_surround.mp4` (~440MB, 720p).
- **Lokal**: `file:///sdcard/large_video.mp4`.

Jika Anda ingin menambahkan fitur spesifik (misalnya, tombol retry kustom, kontrol volume, atau transisi ke ExoPlayer), beri tahu saya!

