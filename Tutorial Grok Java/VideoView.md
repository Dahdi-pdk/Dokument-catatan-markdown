Untuk menggunakan **VideoView** di Android dalam pengembangan aplikasi, berikut adalah panduan langkah demi langkah yang ringkas dan jelas:

### 1. **Tambahkan VideoView ke Layout XML**
Tambahkan `VideoView` ke file layout XML (misalnya, `activity_main.xml`):

```xml
<VideoView
    android:id="@+id/videoView"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_gravity="center" />
```

### 2. **Izin yang Diperlukan**
Jika video diambil dari internet, tambahkan izin di `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Untuk menyimpan atau mengakses file video di penyimpanan perangkat (Android 10+), tambahkan izin penyimpanan:

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

Untuk Android 13+, gunakan izin spesifik seperti `READ_MEDIA_VIDEO`:

```xml
<uses-permission android:name="android.permission.READ_MEDIA_VIDEO" />
```

### 3. **Konfigurasi VideoView di Activity/Fragment**
Di file Java/Kotlin (misalnya, `MainActivity`), atur `VideoView` untuk memutar video.

#### **Kotlin**
```kotlin
import android.net.Uri
import android.os.Bundle
import android.widget.MediaController
import androidx.appcompat.app.AppCompatActivity
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Inisialisasi VideoView
        val videoView = videoView

        // Tambahkan MediaController untuk kontrol pemutaran (play, pause, dll.)
        val mediaController = MediaController(this)
        mediaController.setAnchorView(videoView)
        videoView.setMediaController(mediaController)

        // Tentukan sumber video (lokal atau online)
        val videoUri = Uri.parse("android.resource://" + packageName + "/" + R.raw.sample_video) // Video lokal
        // atau
        // val videoUri = Uri.parse("https://example.com/sample.mp4") // Video online

        // Set URI ke VideoView
        videoView.setVideoURI(videoUri)

        // Mulai pemutaran video
        videoView.start()
    }
}
```

#### **Java**
```java
import android.net.Uri;
import android.os.Bundle;
import android.widget.MediaController;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Inisialisasi VideoView
        VideoView videoView = findViewById(R.id.videoView);

        // Tambahkan MediaController untuk kontrol pemutaran
        MediaController mediaController = new MediaController(this);
        mediaController.setAnchorView(videoView);
        videoView.setMediaController(mediaController);

        // Tentukan sumber video (lokal atau online)
        Uri videoUri = Uri.parse("android.resource://" + getPackageName() + "/" + R.raw.sample_video); // Video lokal
        // atau
        // Uri videoUri = Uri.parse("https://example.com/sample.mp4"); // Video online

        // Set URI ke VideoView
        videoView.setVideoURI(videoUri);

        // Mulai pemutaran video
        videoView.start();
    }
}
```

### 4. **Menambahkan Video ke Proyek**
- **Video Lokal**: Simpan file video (misalnya, `sample_video.mp4`) di folder `res/raw`. Jika folder `raw` belum ada, buat di `res/raw`.
- **Video Online**: Pastikan URL video valid dan perangkat terhubung ke internet.

### 5. **Menangani Izin di Android 6.0+**
Untuk izin penyimpanan, tambahkan kode untuk meminta izin saat runtime:

#### **Kotlin**
```kotlin
import android.Manifest
import android.content.pm.PackageManager
import androidx.core.app.ActivityCompat
import androidx.core.content.ContextCompat

private fun checkStoragePermission() {
    if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_EXTERNAL_STORAGE)
        != PackageManager.PERMISSION_GRANTED) {
        ActivityCompat.requestPermissions(this,
            arrayOf(Manifest.permission.READ_EXTERNAL_STORAGE), 100)
    }
}
```

#### **Java**
```java
import android.Manifest;
import android.content.pm.PackageManager;

private void checkStoragePermission() {
    if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_EXTERNAL_STORAGE)
            != PackageManager.PERMISSION_GRANTED) {
        ActivityCompat.requestPermissions(this,
                new String[]{Manifest.permission.READ_EXTERNAL_STORAGE}, 100);
    }
}
```

### 6. **Menangani Event Pemutaran (Opsional)**
Anda bisa menambahkan listener untuk menangani event seperti selesai pemutaran:

#### **Kotlin**
```kotlin
videoView.setOnCompletionListener {
    // Tindakan saat video selesai
    println("Video selesai diputar")
}
```

#### **Java**
```java
videoView.setOnCompletionListener(mp -> {
    // Tindakan saat video selesai
    System.out.println("Video selesai diputar");
});
```

### 7. **Catatan Penting**
- **Format Video**: Pastikan format video didukung (MP4, 3GP, dll.).
- **Error Handling**: Tambahkan `setOnErrorListener` untuk menangani error pemutaran.
- **Performa**: Untuk video kompleks atau streaming, pertimbangkan menggunakan **ExoPlayer** sebagai alternatif yang lebih canggih.
- **Testing**: Uji di emulator atau perangkat fisik, karena pemutaran video bisa berbeda antar perangkat.

### Contoh Sumber Video
- **Lokal**: `android.resource://<package_name>/raw/sample_video`
- **Eksternal**: `file:///sdcard/sample_video.mp4`
- **Online**: `https://www.example.com/sample.mp4`

Jika Anda membutuhkan contoh lebih spesifik atau fitur tambahan (seperti buffering untuk streaming), beri tahu saya!




---

Dalam penggunaan **VideoView** di Android, ada banyak aspek yang dapat diatur melalui kode untuk mengontrol perilaku dan tampilan pemutaran video. Berikut adalah daftar hal-hal yang bisa diatur melalui kode beserta penjelasan singkat:

### 1. **Sumber Video**
   - **Mengatur URI Video**: Tentukan sumber video, baik dari lokal (res/raw, penyimpanan internal/eksternal) maupun online (URL).
     ```kotlin
     videoView.setVideoURI(Uri.parse("android.resource://" + packageName + "/" + R.raw.sample_video))
     // atau
     videoView.setVideoURI(Uri.parse("https://example.com/sample.mp4"))
     ```

### 2. **Kontrol Pemutaran**
   - **Memulai Video**: Memulai pemutaran video.
     ```kotlin
     videoView.start()
     ```
   - **Menjeda Video**: Menjeda pemutaran.
     ```kotlin
     videoView.pause()
     ```
   - **Menghentikan Video**: Menghentikan pemutaran (VideoView tidak memiliki stop langsung, tetapi bisa diatur ulang).
     ```kotlin
     videoView.stopPlayback()
     ```
   - **Melanjutkan Pemutaran**: Melanjutkan dari posisi jeda.
     ```kotlin
     videoView.resume()
     ```
   - **Mengatur Posisi Pemutaran**: Mengatur waktu mulai pemutaran (dalam milidetik).
     ```kotlin
     videoView.seekTo(10000) // Mulai dari detik ke-10
     ```

### 3. **MediaController**
   - **Mengaktifkan MediaController**: Menambahkan kontrol UI seperti play, pause, dan scrubber.
     ```kotlin
     val mediaController = MediaController(this)
     mediaController.setAnchorView(videoView)
     videoView.setMediaController(mediaController)
     ```
   - **Menyesuaikan Posisi MediaController**: Mengatur posisi tampilan kontrol (misalnya, di bawah video).
     ```kotlin
     mediaController.setAnchorView(videoView)
     ```

### 4. **Event Listener**
   - **OnPreparedListener**: Dipanggil saat video siap diputar, berguna untuk menyesuaikan pengaturan awal.
     ```kotlin
     videoView.setOnPreparedListener { mediaPlayer ->
         // Contoh: Atur volume
         mediaPlayer.setVolume(0.5f, 0.5f)
     }
     ```
   - **OnCompletionListener**: Dipanggil saat video selesai diputar.
     ```kotlin
     videoView.setOnCompletionListener {
         // Tindakan setelah selesai, misalnya ulang video
         videoView.start()
     }
     ```
   - **OnErrorListener**: Menangani error selama pemutaran.
     ```kotlin
     videoView.setOnErrorListener { mp, what, extra ->
         println("Error: $what, Extra: $extra")
         true // Kembalikan true untuk menandakan error ditangani
     }
     ```

### 5. **Pengaturan Audio dan Video**
   - **Volume**: Mengatur volume audio video.
     ```kotlin
     videoView.setOnPreparedListener { mediaPlayer ->
         mediaPlayer.setVolume(0.0f, 0.0f) // Bisukan
     }
     ```
   - **Looping**: Mengatur video untuk diputar ulang secara otomatis.
     ```kotlin
     videoView.setOnPreparedListener { mediaPlayer ->
         mediaPlayer.isLooping = true
     }
     ```
   - **Kecepatan Pemutaran**: Mengatur kecepatan (hanya didukung di API 23+).
     ```kotlin
     videoView.setOnPreparedListener { mediaPlayer ->
         mediaPlayer.playbackParams = mediaPlayer.playbackParams.setSpeed(1.5f) // 1.5x lebih cepat
     }
     ```

### 6. **Tampilan dan Ukuran**
   - **Skala Video**: Menyesuaikan skala video agar sesuai dengan VideoView (misalnya, stretch, fit, atau crop).
     ```kotlin
     videoView.setOnPreparedListener { mediaPlayer ->
         mediaPlayer.setVideoScalingMode(MediaPlayer.VIDEO_SCALING_MODE_SCALE_TO_FIT)
     }
     ```
   - **Ukuran VideoView**: Mengatur ukuran VideoView secara dinamis.
     ```kotlin
     videoView.layoutParams = ViewGroup.LayoutParams(500, 300) // Lebar 500px, tinggi 300px
     ```

### 7. **Buffering untuk Video Online**
   - **Menangani Buffering**: Gunakan `OnInfoListener` untuk mendeteksi status buffering.
     ```kotlin
     videoView.setOnInfoListener { mp, what, extra ->
         when (what) {
             MediaPlayer.MEDIA_INFO_BUFFERING_START -> println("Buffering dimulai")
             MediaPlayer.MEDIA_INFO_BUFFERING_END -> println("Buffering selesai")
         }
         true
     }
     ```

### 8. **Z-Order dan Lapisan Tampilan**
   - **Z-Order**: Mengatur apakah VideoView berada di atas atau di bawah elemen UI lain.
     ```kotlin
     videoView.setZOrderOnTop(true) // VideoView di atas elemen lain
     ```

### 9. **Atribut Lain**
   - **Background Color**: Mengatur warna latar belakang VideoView.
     ```kotlin
     videoView.setBackgroundColor(Color.BLACK)
     ```
   - **Visibilitas**: Mengatur visibilitas VideoView.
     ```kotlin
     videoView.visibility = View.VISIBLE // atau View.GONE
     ```

### 10. **Interaksi dengan Lifecycle**
   - **Menjeda saat Activity Dijeda**: Menghentikan atau menjeda video saat aplikasi tidak aktif.
     ```kotlin
     override fun onPause() {
         super.onPause()
         videoView.pause()
     }
     override fun onResume() {
         super.onResume()
         videoView.start()
     }
     ```

### Catatan Penting
- **Batasan VideoView**: VideoView cocok untuk pemutaran video sederhana. Untuk fitur lanjutan seperti streaming adaptif (HLS/DASH), subtitle, atau kontrol lebih kompleks, gunakan **ExoPlayer**.
- **Format Video**: Pastikan format video didukung (MP4, 3GP, dll.) untuk menghindari error.
- **Izin Runtime**: Untuk akses penyimpanan atau internet, pastikan izin diminta sesuai kebutuhan (lihat contoh sebelumnya).

Jika Anda ingin contoh kode untuk fitur spesifik di atas atau penjelasan lebih mendalam, beri tahu saya!


---


Berikut adalah contoh kode lengkap dalam **Java** untuk menggunakan **VideoView** di Android, mencakup pengaturan sumber video, kontrol pemutaran, MediaController, event listener, dan beberapa fitur lain yang disebutkan sebelumnya:

### Contoh Kode Java
File: `MainActivity.java`

```java
import android.media.MediaPlayer;
import android.net.Uri;
import android.os.Bundle;
import android.view.View;
import android.widget.MediaController;
import android.widget.VideoView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    private VideoView videoView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Inisialisasi VideoView
        videoView = findViewById(R.id.videoView);

        // Tambahkan MediaController untuk kontrol pemutaran
        MediaController mediaController = new MediaController(this);
        mediaController.setAnchorView(videoView);
        videoView.setMediaController(mediaController);

        // Tentukan sumber video (lokal atau online)
        Uri videoUri = Uri.parse("android.resource://" + getPackageName() + "/" + R.raw.sample_video); // Video lokal
        // atau
        // Uri videoUri = Uri.parse("https://example.com/sample.mp4"); // Video online

        // Set URI ke VideoView
        videoView.setVideoURI(videoUri);

        // Pengaturan tambahan saat video siap diputar
        videoView.setOnPreparedListener(new MediaPlayer.OnPreparedListener() {
            @Override
            public void onPrepared(MediaPlayer mp) {
                // Atur volume (0.0f = mute, 1.0f = full)
                mp.setVolume(0.5f, 0.5f);
                // Atur looping
                mp.setLooping(true);
                // Atur skala video
                mp.setVideoScalingMode(MediaPlayer.VIDEO_SCALING_MODE_SCALE_TO_FIT);
                // Mulai pemutaran
                videoView.start();
            }
        });

        // Listener untuk event selesai pemutaran
        videoView.setOnCompletionListener(new MediaPlayer.OnCompletionListener() {
            @Override
            public void onCompletion(MediaPlayer mp) {
                // Tindakan saat video selesai, misalnya ulang
                videoView.start();
            }
        });

        // Listener untuk menangani error
        videoView.setOnErrorListener(new MediaPlayer.OnErrorListener() {
            @Override
            public boolean onError(MediaPlayer mp, int what, int extra) {
                System.out.println("Error: " + what + ", Extra: " + extra);
                return true; // True berarti error ditangani
            }
        });

        // Listener untuk menangani buffering (khususnya untuk video online)
        videoView.setOnInfoListener(new MediaPlayer.OnInfoListener() {
            @Override
            public boolean onInfo(MediaPlayer mp, int what, int extra) {
                switch (what) {
                    case MediaPlayer.MEDIA_INFO_BUFFERING_START:
                        System.out.println("Buffering dimulai");
                        break;
                    case MediaPlayer.MEDIA_INFO_BUFFERING_END:
                        System.out.println("Buffering selesai");
                        break;
                }
                return true;
            }
        });

        // Mengatur Z-Order (opsional)
        videoView.setZOrderOnTop(true);

        // Mengatur warna latar belakang (opsional)
        videoView.setBackgroundColor(getResources().getColor(android.R.color.black));
    }

    // Menangani lifecycle untuk pause/resume video
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
            videoView.resume();
        }
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        if (videoView != null) {
            videoView.stopPlayback(); // Hentikan pemutaran saat activity dihancurkan
        }
    }
}
```

### File Layout XML
File: `res/layout/activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <VideoView
        android:id="@+id/videoView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true" />

</RelativeLayout>
```

### Izin di Manifest
File: `AndroidManifest.xml`

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<!-- Untuk Android 13+ -->
<uses-permission android:name="android.permission.READ_MEDIA_VIDEO" />
```

### Penjelasan Kode
1. **Inisialisasi VideoView**: Mengambil referensi VideoView dari layout.
2. **MediaController**: Menambahkan kontrol UI (play, pause, seek bar).
3. **Sumber Video**: Mengatur URI untuk video lokal (`res/raw/sample_video.mp4`) atau online.
4. **OnPreparedListener**: Mengatur volume, looping, dan skala video saat video siap.
5. **OnCompletionListener**: Menangani aksi setelah video selesai (misalnya, ulang).
6. **OnErrorListener**: Menangani error selama pemutaran.
7. **OnInfoListener**: Menangani status buffering untuk video online.
8. **Lifecycle Management**: Menjeda, melanjutkan, atau menghentikan video sesuai siklus hidup Activity.
9. **Z-Order dan Background**: Mengatur posisi tampilan dan warna latar belakang.

### Menangani Izin Runtime (Opsional)
Untuk video dari penyimpanan eksternal, tambahkan kode untuk meminta izin:

```java
import android.Manifest;
import android.content.pm.PackageManager;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;

private void checkStoragePermission() {
    if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_EXTERNAL_STORAGE)
            != PackageManager.PERMISSION_GRANTED) {
        ActivityCompat.requestPermissions(this,
                new String[]{Manifest.permission.READ_EXTERNAL_STORAGE}, 100);
    } else {
        // Izin sudah diberikan, lanjutkan inisialisasi video
        setupVideoView();
    }
}

@Override
public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
    super.onRequestPermissionsResult(requestCode, permissions, grantResults);
    if (requestCode == 100 && grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
        // Izin diberikan, inisialisasi video
        setupVideoView();
    } else {
        // Izin ditolak, tampilkan pesan
        System.out.println("Izin penyimpanan diperlukan");
    }
}

private void setupVideoView() {
    // Kode inisialisasi VideoView seperti di onCreate
    Uri videoUri = Uri.parse("file:///sdcard/sample_video.mp4");
    videoView.setVideoURI(videoUri);
    videoView.start();
}
```

### Catatan
- **Format Video**: Pastikan video dalam format yang didukung (MP4, 3GP).
- **Video Lokal**: Simpan di `res/raw` (huruf kecil, tanpa ekstensi di kode).
- **Alternatif**: Untuk fitur lebih kompleks (streaming HLS/DASH, subtitle), gunakan **ExoPlayer**.
- **Testing**: Uji di perangkat fisik untuk memastikan kompatibilitas.

Jika Anda ingin contoh kode untuk fitur spesifik (misalnya, kecepatan pemutaran atau buffering), beri tahu saya!


---


Berikut adalah panduan langkah demi langkah untuk menggunakan **ExoPlayer** dalam pengembangan aplikasi Android menggunakan **Java**, sebagai alternatif yang lebih canggih dibandingkan **VideoView** untuk memutar video. ExoPlayer mendukung fitur seperti streaming adaptif (HLS/DASH), kustomisasi, dan buffering yang lebih baik.

---

### 1. **Tambahkan Dependensi ExoPlayer**
Tambahkan dependensi ExoPlayer ke file `build.gradle` (Module: app):

```gradle
implementation 'androidx.media3:media3-exoplayer:1.3.1'
implementation 'androidx.media3:media3-exoplayer-dash:1.3.1' // Untuk DASH streaming
implementation 'androidx.media3:media3-exoplayer-hls:1.3.1' // Untuk HLS streaming
implementation 'androidx.media3:media3-ui:1.3.1' // Untuk UI PlayerView
```

Sinkronkan proyek setelah menambahkan dependensi.

---

### 2. **Tambahkan Izin di AndroidManifest.xml**
Untuk memutar video dari internet, tambahkan izin berikut:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Untuk video dari penyimpanan eksternal (opsional):

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<!-- Untuk Android 13+ -->
<uses-permission android:name="android.permission.READ_MEDIA_VIDEO" />
```

---

### 3. **Buat Layout XML**
Tambahkan `PlayerView` (komponen UI ExoPlayer) ke file layout, misalnya `activity_main.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.media3.ui.PlayerView
        android:id="@+id/player_view"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        app:use_controller="true"
        app:resize_mode="fit" />

</RelativeLayout>
```

- `app:use_controller="true"`: Mengaktifkan kontrol UI (play, pause, seek bar).
- `app:resize_mode="fit"`: Menyesuaikan video agar sesuai dengan ukuran PlayerView.

---

### 4. **Inisialisasi ExoPlayer di Activity**
Berikut adalah kode Java untuk mengatur ExoPlayer di `MainActivity.java`:

```java
import android.net.Uri;
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
import androidx.media3.common.MediaItem;
import androidx.media3.exoplayer.ExoPlayer;
import androidx.media3.ui.PlayerView;

public class MainActivity extends AppCompatActivity {
    private ExoPlayer player;
    private PlayerView playerView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Inisialisasi PlayerView
        playerView = findViewById(R.id.player_view);

        // Inisialisasi ExoPlayer
        player = new ExoPlayer.Builder(this).build();
        playerView.setPlayer(player);

        // Tentukan sumber video
        String videoUrl = "https://media.geeksforgeeks.org/wp-content/uploads/20201217163353/Screenrecorder-2020-12-17-16-32-03-350.mp4";
        // atau untuk video lokal:
        // String videoUrl = "android.resource://" + getPackageName() + "/" + R.raw.sample_video;
        Uri uri = Uri.parse(videoUrl);
        MediaItem mediaItem = MediaItem.fromUri(uri);

        // Set media item ke player
        player.setMediaItem(mediaItem);

        // Persiapkan dan mulai pemutaran
        player.prepare();
        player.setPlayWhenReady(true); // Mulai otomatis saat siap
    }

    // Kelola lifecycle
    @Override
    protected void onStop() {
        super.onStop();
        if (player != null) {
            player.release(); // Bebaskan sumber daya
            player = null;
        }
    }

    @Override
    protected void onPause() {
        super.onPause();
        if (player != null) {
            player.pause(); // Jeda pemutaran
        }
    }

    @Override
    protected void onResume() {
        super.onResume();
        if (player != null && !player.isPlaying()) {
            player.play(); // Lanjutkan pemutaran
        }
    }
}
```

---

### 5. **Fitur yang Dapat Diatur di ExoPlayer**
Berikut adalah beberapa hal yang dapat Anda atur melalui kode Java dengan ExoPlayer:

#### a. **Kontrol Pemutaran**
- **Play/Pause**:
  ```java
  player.setPlayWhenReady(true); // Mainkan
  player.setPlayWhenReady(false); // Jeda
  ```
- **Seek ke Posisi Tertentu**:
  ```java
  player.seekTo(10000); // Seek ke detik ke-10 (dalam milidetik)
  ```
- **Looping**:
  ```java
  player.setRepeatMode(Player.REPEAT_MODE_ONE); // Ulangi satu video
  // atau Player.REPEAT_MODE_ALL untuk playlist
  ```

#### b. **Mengatur Volume**
```java
player.setVolume(0.5f); // Set volume (0.0f = mute, 1.0f = penuh)
```

#### c. **Menangani Buffering**
Tambahkan listener untuk mendeteksi status buffering:
```java
player.addListener(new Player.Listener() {
    @Override
    public void onPlayerStateChanged(boolean playWhenReady, int playbackState) {
        if (playbackState == Player.STATE_BUFFERING) {
            System.out.println("Buffering...");
        } else if (playbackState == Player.STATE_READY) {
            System.out.println("Siap diputar");
        }
    }
});
```

#### d. **Mengatur Header untuk Streaming**
Untuk menambahkan header (misalnya, token autentikasi):
```java
import androidx.media3.datasource.DefaultHttpDataSource;

DefaultHttpDataSource.Factory dataSourceFactory = new DefaultHttpDataSource.Factory()
    .setDefaultRequestProperties(new HashMap<String, String>() {{
        put("Authorization", "Bearer your_token_here");
    }});
player.setMediaSource(new ProgressiveMediaSource.Factory(dataSourceFactory)
    .createMediaSource(MediaItem.fromUri(uri)));
```

#### e. **Mengatur Kecepatan Pemutaran**
```java
player.setPlaybackParameters(new PlaybackParameters(1.5f)); // 1.5x kecepatan
```

#### f. **Menangani Error**
```java
player.addListener(new Player.Listener() {
    @Override
    public void onPlayerError(PlaybackException error) {
        System.out.println("Error: " + error.getMessage());
    }
});
```

#### g. **Streaming Adaptif (HLS/DASH)**
Untuk HLS:
```java
import androidx.media3.exoplayer.hls.HlsMediaSource;

HlsMediaSource hlsMediaSource = new HlsMediaSource.Factory(dataSourceFactory)
    .createMediaSource(MediaItem.fromUri(uri));
player.setMediaSource(hlsMediaSource);
```

#### h. **Menambahkan Subtitle**
Jika video memiliki subtitle:
```java
MediaItem.SubtitleConfiguration subtitle = new MediaItem.SubtitleConfiguration.Builder()
    .setUri(Uri.parse("https://example.com/subtitle.srt"))
    .setMimeType(MimeTypes.APPLICATION_SUBRIP)
    .setLanguage("en")
    .build();
MediaItem mediaItem = new MediaItem.Builder()
    .setUri(videoUrl)
    .setSubtitleConfigurations(Collections.singletonList(subtitle))
    .build();
player.setMediaItem(mediaItem);
```

---

### 6. **Menangani Izin Runtime (Opsional)**
Jika video diakses dari penyimpanan eksternal, tambahkan kode untuk meminta izin:

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
        initializePlayer();
    }
}

@Override
public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
    super.onRequestPermissionsResult(requestCode, permissions, grantResults);
    if (requestCode == 100 && grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
        initializePlayer();
    } else {
        System.out.println("Izin penyimpanan diperlukan");
    }
}
```

---

### 7. **Keunggulan ExoPlayer Dibandingkan VideoView**
- **Streaming Adaptif**: Mendukung HLS dan DASH untuk menyesuaikan kualitas video berdasarkan jaringan.[](https://www.geeksforgeeks.org/android/exoplayer-in-android-with-example/)
- **Kustomisasi**: Memungkinkan pengaturan UI, subtitle, dan header HTTP.[](https://stackoverflow.com/questions/75080206/how-to-use-exoplayer-in-android)
- **Buffering**: Penanganan buffering lebih baik untuk streaming online.[](https://developer.android.com/media/media3/exoplayer)
- **Format Media**: Mendukung lebih banyak format (MP4, WebM, MKV, dll.).[](https://www.technetexperts.com/using-exoplayer-media-player-for-android/)
- **Lifecycle-Aware**: Integrasi lebih baik dengan siklus hidup aplikasi.[](https://www.droidcon.com/2025/07/14/getting-started-with-exo-player-in-jetpack-compose/)

---

### 8. **Catatan Penting**
- **Format Video**: Pastikan format video didukung (lihat dokumentasi ExoPlayer untuk daftar lengkap).
- **Lifecycle**: Selalu panggil `player.release()` saat Activity dihancurkan untuk mencegah kebocoran memori.
- **Alternatif UI**: Gunakan Jetpack Compose dengan `androidx.media3.ui.compose` untuk UI modern.[](https://www.droidcon.com/2025/07/14/getting-started-with-exo-player-in-jetpack-compose/)
- **Testing**: Uji di perangkat fisik untuk memastikan kompatibilitas.

Jika Anda ingin contoh kode untuk fitur spesifik seperti playlist, DRM, atau integrasi dengan Jetpack Compose, beri tahu saya![](https://www.udemy.com/course/android-media-playback-with-media3-exoplayer/)



---


Berikut adalah panduan menggunakan **ExoPlayer** di Android dengan **Java** untuk menangani **buffering** dan **streaming** (termasuk HLS/DASH untuk streaming adaptif). ExoPlayer sangat cocok untuk streaming karena mendukung pengelolaan buffer yang canggih dan format streaming seperti HLS (HTTP Live Streaming) atau DASH (Dynamic Adaptive Streaming over HTTP). Saya akan menjelaskan langkah-langkahnya secara ringkas dan memberikan contoh kode.

---

### 1. **Prasyarat**
Pastikan Anda telah menambahkan dependensi ExoPlayer di `build.gradle` (Module: app):

```gradle
implementation 'androidx.media3:media3-exoplayer:1.3.1'
implementation 'androidx.media3:media3-exoplayer-hls:1.3.1' // Untuk HLS
implementation 'androidx.media3:media3-exoplayer-dash:1.3.1' // Untuk DASH
implementation 'androidx.media3:media3-ui:1.3.1' // Untuk UI PlayerView
```

Tambahkan izin di `AndroidManifest.xml` untuk streaming:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

---

### 2. **Layout XML**
Gunakan `PlayerView` untuk menampilkan video dan kontrol UI di `res/layout/activity_main.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.media3.ui.PlayerView
        android:id="@+id/player_view"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        app:use_controller="true"
        app:resize_mode="fit"
        app:buffering_spinner="true" /> <!-- Menampilkan spinner saat buffering -->

</RelativeLayout>
```

- `app:buffering_spinner="true"`: Menampilkan indikator loading saat buffering.

---

### 3. **Konfigurasi ExoPlayer untuk Streaming dan Buffering**
Berikut adalah kode Java untuk mengatur ExoPlayer dengan fokus pada streaming (HLS/DASH) dan pengelolaan buffering di `MainActivity.java`:

```java
import android.net.Uri;
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
import androidx.media3.common.MediaItem;
import androidx.media3.common.Player;
import androidx.media3.datasource.DefaultHttpDataSource;
import androidx.media3.exoplayer.ExoPlayer;
import androidx.media3.exoplayer.hls.HlsMediaSource;
import androidx.media3.exoplayer.source.MediaSource;
import androidx.media3.ui.PlayerView;

public class MainActivity extends AppCompatActivity {
    private ExoPlayer player;
    private PlayerView playerView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Inisialisasi PlayerView
        playerView = findViewById(R.id.player_view);

        // Konfigurasi ExoPlayer dengan pengaturan buffering
        player = new ExoPlayer.Builder(this)
                .setLoadControl(
                        new DefaultLoadControl.Builder()
                                .setBufferDurationsMs(
                                        5000, // Minimum buffer (ms) sebelum mulai
                                        30000, // Maksimum buffer (ms)
                                        2500, // Buffer untuk mulai playback (ms)
                                        5000 // Buffer untuk melanjutkan setelah jeda (ms)
                                )
                                .build()
                )
                .build();

        // Hubungkan PlayerView dengan ExoPlayer
        playerView.setPlayer(player);

        // Tentukan sumber video streaming (contoh: HLS)
        String videoUrl = "https://bitdash-a.akamaihd.net/content/sintel/hls/playlist.m3u8"; // URL HLS
        // atau DASH: "https://dash.akamai.com/akamai/bbb_30fps/bbb_30fps.mpd"
        Uri uri = Uri.parse(videoUrl);

        // Buat DataSource untuk streaming
        DefaultHttpDataSource.Factory dataSourceFactory = new DefaultHttpDataSource.Factory()
                .setUserAgent("ExoPlayerApp")
                .setConnectTimeoutMs(5000)
                .setReadTimeoutMs(5000);

        // Buat MediaSource untuk HLS
        MediaSource mediaSource = new HlsMediaSource.Factory(dataSourceFactory)
                .createMediaSource(MediaItem.fromUri(uri));
        // Untuk DASH, gunakan:
        // MediaSource mediaSource = new DashMediaSource.Factory(dataSourceFactory)
        //        .createMediaSource(MediaItem.fromUri(uri));

        // Set media source ke player
        player.setMediaSource(mediaSource);

        // Tambahkan listener untuk memantau status buffering dan pemutaran
        player.addListener(new Player.Listener() {
            @Override
            public void onPlayerStateChanged(boolean playWhenReady, int playbackState) {
                switch (playbackState) {
                    case Player.STATE_BUFFERING:
                        System.out.println("Buffering...");
                        break;
                    case Player.STATE_READY:
                        System.out.println("Siap diputar");
                        break;
                    case Player.STATE_ENDED:
                        System.out.println("Pemutaran selesai");
                        break;
                    case Player.STATE_IDLE:
                        System.out.println("Player idle");
                        break;
                }
            }

            @Override
            public void onPlayerError(PlaybackException error) {
                System.out.println("Error: " + error.getMessage());
            }
        });

        // Persiapkan dan mulai pemutaran
        player.prepare();
        player.setPlayWhenReady(true); // Mulai otomatis saat siap
    }

    // Kelola lifecycle
    @Override
    protected void onPause() {
        super.onPause();
        if (player != null) {
            player.pause();
        }
    }

    @Override
    protected void onResume() {
        super.onResume();
        if (player != null && !player.isPlaying()) {
            player.play();
        }
    }

    @Override
    protected void onStop() {
        super.onStop();
        if (player != null) {
            player.release();
            player = null;
        }
    }
}
```

---

### 4. **Penjelasan Komponen Penting**

#### a. **Pengaturan Buffering**
ExoPlayer menggunakan `DefaultLoadControl` untuk mengatur ukuran buffer. Parameter yang dapat disesuaikan:
- `minBufferMs`: Waktu minimum buffer sebelum mulai pemutaran (contoh: 5000 ms).
- `maxBufferMs`: Kapasitas maksimum buffer (contoh: 30000 ms).
- `bufferForPlaybackMs`: Buffer minimum untuk memulai pemutaran (contoh: 2500 ms).
- `bufferForPlaybackAfterRebufferMs`: Buffer untuk melanjutkan setelah rebuffering (contoh: 5000 ms).

Contoh di atas menggunakan nilai yang cukup untuk streaming stabil. Sesuaikan nilai ini berdasarkan kebutuhan aplikasi (misalnya, kurangi untuk latensi rendah atau tingkatkan untuk koneksi lambat).

#### b. **Streaming Adaptif (HLS/DASH)**
- **HLS**: Gunakan `HlsMediaSource` untuk memutar stream `.m3u8`. ExoPlayer secara otomatis menyesuaikan kualitas berdasarkan bandwidth.
- **DASH**: Gunakan `DashMediaSource` untuk stream `.mpd`.
- ExoPlayer mendukung adaptasi bitrate, sehingga video beralih ke resolusi lebih rendah/tinggi sesuai kecepatan jaringan.

#### c. **DataSource untuk Streaming**
- `DefaultHttpDataSource.Factory` mengatur koneksi HTTP untuk streaming.
- Anda dapat menyesuaikan:
  - `setUserAgent`: Identifikasi aplikasi Anda ke server.
  - `setConnectTimeoutMs` dan `setReadTimeoutMs`: Timeout untuk koneksi dan pembacaan data.
  - `setDefaultRequestProperties`: Menambahkan header seperti autentikasi.

Contoh menambahkan header:
```java
dataSourceFactory.setDefaultRequestProperties(new HashMap<String, String>() {{
    put("Authorization", "Bearer your_token_here");
}});
```

#### d. **Memantau Status Buffering**
Listener `onPlayerStateChanged` memungkinkan Anda mendeteksi:
- `STATE_BUFFERING`: Sedang mengisi buffer.
- `STATE_READY`: Buffer cukup, pemutaran siap.
- Gunakan informasi ini untuk menampilkan UI khusus (misalnya, spinner loading).

#### e. **Menangani Error**
Gunakan `onPlayerError` untuk menangani error seperti URL tidak valid atau kegagalan jaringan:
```java
@Override
public void onPlayerError(PlaybackException error) {
    System.out.println("Error: " + error.getMessage());
    // Tampilkan pesan error ke pengguna atau coba ulang
}
```

---

### 5. **Menangani Izin Runtime (Opsional)**
Jika streaming dari penyimpanan lokal, tambahkan izin runtime:

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
        initializePlayer();
    }
}

@Override
public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
    super.onRequestPermissionsResult(requestCode, permissions, grantResults);
    if (requestCode == 100 && grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
        initializePlayer();
    } else {
        System.out.println("Izin penyimpanan diperlukan");
    }
}

private void initializePlayer() {
    // Kode inisialisasi player seperti di onCreate
    Uri uri = Uri.parse("file:///sdcard/sample_video.mp4");
    MediaSource mediaSource = new ProgressiveMediaSource.Factory(new DefaultDataSource.Factory(this))
            .createMediaSource(MediaItem.fromUri(uri));
    player.setMediaSource(mediaSource);
    player.prepare();
    player.setPlayWhenReady(true);
}
```

---

### 6. **Tips Tambahan**
- **Optimasi Buffering**: Sesuaikan `DefaultLoadControl` untuk skenario tertentu:
  - Untuk latensi rendah (misalnya, live streaming), kurangi `minBufferMs` dan `bufferForPlaybackMs`.
  - Untuk video on-demand, gunakan buffer lebih besar untuk mencegah rebuffering.
- **UI Kustom**: Anda dapat menyembunyikan `PlayerView` bawaan dan membuat kontrol UI sendiri dengan tombol kustom.
- **Retry Mechanism**: Jika streaming gagal, tambahkan logika untuk mencoba ulang dengan `player.prepare()` setelah menangani error.
- **URL Testing**: Gunakan URL HLS/DASH publik untuk pengujian, seperti:
  - HLS: `https://bitdash-a.akamaihd.net/content/sintel/hls/playlist.m3u8`
  - DASH: `https://dash.akamai.com/akamai/bbb_30fps/bbb_30fps.mpd`

---

### 7. **Keunggulan ExoPlayer untuk Streaming**
- **Adaptasi Bitrate**: Secara otomatis menyesuaikan kualitas video berdasarkan kecepatan jaringan.
- **Buffering Efisien**: Kontrol buffer yang fleksibel untuk mengurangi jeda.
- **Format Streaming**: Mendukung HLS, DASH, dan SmoothStreaming.
- **Kustomisasi**: Mendukung header HTTP, DRM, dan subtitle.

Jika Anda ingin contoh kode untuk fitur spesifik seperti live streaming, DRM, atau menampilkan progres buffering di UI, beri tahu saya!



---

Untuk menangani **buffering** dan **streaming** video di Android **tanpa ExoPlayer**, Anda dapat menggunakan **VideoView** dengan kombinasi **MediaPlayer** bawaan Android. Namun, perlu dicatat bahwa **VideoView** memiliki keterbatasan dibandingkan ExoPlayer, terutama dalam hal streaming adaptif (HLS/DASH) dan kontrol buffering yang canggih. VideoView lebih cocok untuk streaming video progresif (misalnya, MP4 langsung) atau file lokal. Berikut adalah panduan menggunakan VideoView untuk streaming dan buffering dalam **Java**.

---

### 1. **Prasyarat**
- **Izin**: Tambahkan izin di `AndroidManifest.xml` untuk streaming dari internet:
  ```xml
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  ```
  Untuk video dari penyimpanan eksternal (opsional):
  ```xml
  <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
  <!-- Untuk Android 13+ -->
  <uses-permission android:name="android.permission.READ_MEDIA_VIDEO" />
  ```

- **Format Video**: VideoView hanya mendukung format seperti MP4 atau 3GP untuk streaming progresif. Untuk HLS/DASH, VideoView kurang optimal dan sering gagal tanpa konfigurasi tambahan.

---

### 2. **Layout XML**
Tambahkan `VideoView` ke file layout, misalnya `res/layout/activity_main.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <VideoView
        android:id="@+id/videoView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true" />

    <!-- Opsional: ProgressBar untuk menunjukkan buffering -->
    <ProgressBar
        android:id="@+id/progressBar"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:visibility="gone" />

</RelativeLayout>
```

---

### 3. **Konfigurasi VideoView untuk Streaming dan Buffering**
Berikut adalah kode Java untuk mengatur **VideoView** agar dapat melakukan streaming video dan menangani buffering di `MainActivity.java`:

```java
import android.media.MediaPlayer;
import android.net.Uri;
import android.os.Bundle;
import android.view.View;
import android.widget.MediaController;
import android.widget.ProgressBar;
import android.widget.VideoView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    private VideoView videoView;
    private ProgressBar progressBar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Inisialisasi VideoView dan ProgressBar
        videoView = findViewById(R.id.videoView);
        progressBar = findViewById(R.id.progressBar);

        // Tambahkan MediaController untuk kontrol pemutaran
        MediaController mediaController = new MediaController(this);
        mediaController.setAnchorView(videoView);
        videoView.setMediaController(mediaController);

        // Tentukan sumber video streaming (contoh: MP4 progresif)
        String videoUrl = "https://media.geeksforgeeks.org/wp-content/uploads/20201217163353/Screenrecorder-2020-12-17-16-32-03-350.mp4";
        // atau video lokal:
        // String videoUrl = "android.resource://" + getPackageName() + "/" + R.raw.sample_video;
        Uri uri = Uri.parse(videoUrl);

        // Set URI ke VideoView
        videoView.setVideoURI(uri);

        // Listener untuk menangani persiapan video
        videoView.setOnPreparedListener(new MediaPlayer.OnPreparedListener() {
            @Override
            public void onPrepared(MediaPlayer mp) {
                // Sembunyikan ProgressBar saat video siap
                progressBar.setVisibility(View.GONE);
                // Mulai pemutaran
                videoView.start();
            }
        });

        // Listener untuk menangani buffering
        videoView.setOnInfoListener(new MediaPlayer.OnInfoListener() {
            @Override
            public boolean onInfo(MediaPlayer mp, int what, int extra) {
                switch (what) {
                    case MediaPlayer.MEDIA_INFO_BUFFERING_START:
                        // Tampilkan ProgressBar saat buffering dimulai
                        progressBar.setVisibility(View.VISIBLE);
                        System.out.println("Buffering dimulai");
                        return true;
                    case MediaPlayer.MEDIA_INFO_BUFFERING_END:
                        // Sembunyikan ProgressBar saat buffering selesai
                        progressBar.setVisibility(View.GONE);
                        System.out.println("Buffering selesai");
                        return true;
                }
                return false;
            }
        });

        // Listener untuk menangani error
        videoView.setOnErrorListener(new MediaPlayer.OnErrorListener() {
            @Override
            public boolean onError(MediaPlayer mp, int what, int extra) {
                progressBar.setVisibility(View.GONE);
                System.out.println("Error: " + what + ", Extra: " + extra);
                // Tampilkan pesan error ke pengguna atau coba ulang
                return true; // True berarti error ditangani
            }
        });
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

### 4. **Penjelasan Komponen Penting**

#### a. **Streaming dengan VideoView**
- **Jenis Streaming**: VideoView mendukung **progressive streaming** (unduh file video secara bertahap, seperti MP4) tetapi **tidak mendukung streaming adaptif** seperti HLS/DASH secara langsung. Untuk HLS/DASH, Anda memerlukan ExoPlayer atau library lain.
- **URL Video**: Gunakan URL yang menunjuk ke file video (misalnya, `.mp4`). Contoh:
  - `https://media.geeksforgeeks.org/wp-content/uploads/20201217163353/Screenrecorder-2020-12-17-16-32-03-350.mp4`
- **Batasan**: VideoView tidak secara otomatis menyesuaikan bitrate berdasarkan kecepatan jaringan, sehingga performa streaming bergantung pada koneksi yang stabil.

#### b. **Pengelolaan Buffering**
- **OnInfoListener**: Digunakan untuk mendeteksi status buffering:
  - `MEDIA_INFO_BUFFERING_START`: Buffering dimulai, tampilkan indikator (misalnya, ProgressBar).
  - `MEDIA_INFO_BUFFERING_END`: Buffering selesai, sembunyikan indikator.
- **UI Buffering**: Dalam contoh, `ProgressBar` ditampilkan/sembunyikan berdasarkan status buffering.
- **Kontrol Buffering**: VideoView tidak memberikan kontrol tingkat rendah atas ukuran buffer (seperti `DefaultLoadControl` di ExoPlayer). Buffering dikelola secara internal oleh MediaPlayer, sehingga opsi kustomisasi terbatas.

#### c. **Menangani Error**
- Gunakan `OnErrorListener` untuk mendeteksi masalah seperti URL tidak valid atau kegagalan koneksi.
- Anda dapat menambahkan logika untuk mencoba ulang dengan memanggil `setVideoURI` kembali jika diperlukan:
  ```java
  videoView.setVideoURI(uri);
  videoView.start();
  ```

#### d. **MediaController**
- `MediaController` menyediakan kontrol UI standar (play, pause, seek bar) untuk streaming.
- Tidak ada opsi kustomisasi lanjutan seperti di ExoPlayer.

---

### 5. **Menangani Izin Runtime (Opsional)**
Jika video diakses dari penyimpanan eksternal, tambahkan kode untuk meminta izin:

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
    Uri uri = Uri.parse("file:///sdcard/sample_video.mp4");
    videoView.setVideoURI(uri);
    videoView.start();
}
```

---

### 6. **Keterbatasan VideoView untuk Streaming dan Buffering**
- **Tidak Mendukung HLS/DASH**: VideoView tidak dapat menangani streaming adaptif seperti HLS atau DASH, yang membutuhkan penyesuaian bitrate otomatis.
- **Buffering Terbatas**: Anda tidak dapat mengatur ukuran buffer atau waktu buffering secara eksplisit.
- **Performa**: Streaming di VideoView rentan terhadap lag atau error pada koneksi jaringan yang tidak stabil.
- **UI Terbatas**: Kontrol UI terbatas pada `MediaController` bawaan tanpa banyak opsi kustomisasi.

---

### 7. **Tips untuk Streaming dengan VideoView**
- **Gunakan URL Progresif**: Pastikan URL menunjuk ke file MP4 atau 3GP yang dapat diunduh secara progresif.
- **Koneksi Stabil**: VideoView bekerja lebih baik dengan koneksi internet stabil karena kurangnya adaptasi bitrate.
- **Indikator Buffering**: Tambahkan `ProgressBar` atau UI kustom untuk meningkatkan pengalaman pengguna saat buffering.
- **Error Handling**: Selalu sediakan logika cadangan (misalnya, coba ulang atau pesan error) untuk menangani kegagalan streaming.
- **Alternatif**: Untuk streaming HLS/DASH atau kontrol buffering yang lebih baik, **ExoPlayer** sangat disarankan.

---

### 8. **Contoh URL untuk Pengujian**
- **MP4 Progresif**: `https://media.geeksforgeeks.org/wp-content/uploads/20201217163353/Screenrecorder-2020-12-17-16-32-03-350.mp4`
- **Lokal**: `android.resource://<package_name>/raw/sample_video`

Jika Anda ingin contoh kode untuk fitur spesifik (misalnya, menambahkan tombol coba ulang saat error atau menangani video lokal), beri tahu saya! Alternatifnya, jika Anda ingin beralih ke ExoPlayer untuk streaming yang lebih andal, saya bisa memberikan kode tambahan.



---



Memutar video berukuran besar di Android menggunakan **VideoView** (tanpa ExoPlayer) memerlukan perhatian khusus karena keterbatasan **VideoView** dalam mengelola buffering dan memori. Video berukuran besar (baik dari segi ukuran file maupun resolusi) dapat menyebabkan masalah seperti lag, crash karena kehabisan memori, atau buffering lama saat streaming. Berikut adalah panduan dalam **Java** untuk menangani video berukuran besar dengan **VideoView**, termasuk streaming dan buffering, serta tips untuk mengoptimalkan performa.

---

### 1. **Tantangan Memutar Video Berukuran Besar**
- **Memori**: Video beresolusi tinggi (misalnya, 4K) atau durasi panjang membutuhkan memori besar untuk buffering dan decoding.
- **Buffering**: Untuk streaming, video besar membutuhkan waktu buffering lebih lama, terutama pada koneksi lambat.
- **Performa VideoView**: VideoView tidak mendukung streaming adaptif (HLS/DASH) atau pengelolaan buffer tingkat lanjut, sehingga performa bisa menurun.
- **Penyimpanan Lokal**: Jika video disimpan di perangkat, akses file besar memerlukan izin dan manajemen yang tepat.

---

### 2. **Prasyarat**
- **Izin di AndroidManifest.xml**:
  ```xml
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
  <!-- Untuk Android 13+ -->
  <uses-permission android:name="android.permission.READ_MEDIA_VIDEO" />
  ```

- **Format Video**: Pastikan video dalam format yang didukung VideoView (MP4, 3GP). Untuk video besar, kompresi H.264 atau H.265 (HEVC) disarankan untuk mengurangi ukuran file tanpa mengorbankan kualitas.

- **Resolusi dan Bitrate**: Video beresolusi tinggi (misalnya, 1080p atau 4K) dengan bitrate tinggi akan lebih sulit diputar di perangkat low-end.

---

### 3. **Layout XML**
Gunakan `VideoView` dengan `ProgressBar` untuk menunjukkan buffering di `res/layout/activity_main.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <VideoView
        android:id="@+id/videoView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true" />

    <ProgressBar
        android:id="@+id/progressBar"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:visibility="gone" />

</RelativeLayout>
```

---

### 4. **Kode Java untuk Memutar Video Berukuran Besar**
Berikut adalah kode di `MainActivity.java` untuk menangani video besar, baik lokal maupun streaming, dengan fokus pada buffering dan pengelolaan memori:

```java
import android.media.MediaPlayer;
import android.net.Uri;
import android.os.Bundle;
import android.view.View;
import android.widget.MediaController;
import android.widget.ProgressBar;
import android.widget.VideoView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    private VideoView videoView;
    private ProgressBar progressBar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Inisialisasi VideoView dan ProgressBar
        videoView = findViewById(R.id.videoView);
        progressBar = findViewById(R.id.progressBar);

        // Tambahkan MediaController untuk kontrol pemutaran
        MediaController mediaController = new MediaController(this);
        mediaController.setAnchorView(videoView);
        videoView.setMediaController(mediaController);

        // Tentukan sumber video
        String videoUrl = "https://archive.org/download/BigBuckBunny_124/Content/big_buck_bunny_720p_surround.mp4"; // Video besar (contoh)
        // atau video lokal:
        // String videoUrl = "file:///sdcard/large_video.mp4";
        Uri uri = Uri.parse(videoUrl);

        // Set URI ke VideoView
        videoView.setVideoURI(uri);

        // Listener untuk persiapan video
        videoView.setOnPreparedListener(new MediaPlayer.OnPreparedListener() {
            @Override
            public void onPrepared(MediaPlayer mp) {
                // Optimasi untuk video besar
                mp.setVideoScalingMode(MediaPlayer.VIDEO_SCALING_MODE_SCALE_TO_FIT_WITH_CROPPING);
                mp.setScreenOnWhilePlaying(true); // Jaga layar tetap menyala
                mp.setLooping(false); // Atur looping jika diperlukan

                // Sembunyikan ProgressBar saat video siap
                progressBar.setVisibility(View.GONE);
                videoView.start();
            }
        });

        // Listener untuk buffering
        videoView.setOnInfoListener(new MediaPlayer.OnInfoListener() {
            @Override
            public boolean onInfo(MediaPlayer mp, int what, int extra) {
                switch (what) {
                    case MediaPlayer.MEDIA_INFO_BUFFERING_START:
                        progressBar.setVisibility(View.VISIBLE);
                        System.out.println("Buffering dimulai");
                        return true;
                    case MediaPlayer.MEDIA_INFO_BUFFERING_END:
                        progressBar.setVisibility(View.GONE);
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
                progressBar.setVisibility(View.GONE);
                System.out.println("Error: " + what + ", Extra: " + extra);
                // Tampilkan pesan error atau coba ulang
                return true;
            }
        });

        // Tampilkan ProgressBar saat memulai buffering
        progressBar.setVisibility(View.VISIBLE);
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

### 5. **Menangani Izin Runtime (Untuk Video Lokal)**
Jika video besar disimpan di penyimpanan eksternal, tambahkan kode untuk meminta izin:

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
    Uri uri = Uri.parse("file:///sdcard/large_video.mp4");
    videoView.setVideoURI(uri);
    progressBar.setVisibility(View.VISIBLE);
}
```

---

### 6. **Tips Mengelola Video Berukuran Besar dengan VideoView**

#### a. **Optimasi Buffering**
- **Koneksi Jaringan**: Pastikan koneksi stabil untuk streaming. VideoView tidak mendukung adaptasi bitrate, jadi gunakan URL dengan bitrate sedang (misalnya, 2-5 Mbps untuk 720p).
- **ProgressBar**: Gunakan `ProgressBar` untuk memberi tahu pengguna saat buffering, seperti dalam kode di atas.
- **Preload**: VideoView secara otomatis mengelola buffering, tetapi Anda tidak dapat mengatur ukuran buffer seperti di ExoPlayer. Untuk streaming, pastikan server mendukung **range requests** (HTTP 206 Partial Content).

#### b. **Optimasi Memori**
- **Skala Video**: Gunakan `setVideoScalingMode(MediaPlayer.VIDEO_SCALING_MODE_SCALE_TO_FIT)` untuk mengurangi beban rendering pada video beresolusi tinggi.
- **Hardware Acceleration**: Pastikan `android:hardwareAccelerated="true"` di `AndroidManifest.xml` untuk aplikasi atau activity:
  ```xml
  <application android:hardwareAccelerated="true">
  ```
- **Hapus Cache**: Panggil `videoView.stopPlayback()` saat tidak digunakan untuk membebaskan memori.

#### c. **Kompresi Video**
- Jika memungkinkan, kompres video sebelum diunggah ke server atau disimpan di perangkat menggunakan alat seperti FFmpeg (contoh: `H.264` atau `H.265` dengan bitrate lebih rendah).
- Contoh perintah FFmpeg: `ffmpeg -i input.mp4 -vcodec libx264 -b:v 2000k -vf scale=1280:720 output.mp4`

#### d. **Menangani Streaming**
- **URL Progresif**: Gunakan URL video progresif (misalnya, `.mp4`). Contoh URL pengujian: `https://archive.org/download/BigBuckBunny_124/Content/big_buck_bunny_720p_surround.mp4`.
- **Timeout**: VideoView tidak mendukung pengaturan timeout secara langsung. Jika streaming lambat, pertimbangkan menggunakan server CDN yang cepat.
- **Retry Mechanism**: Jika buffering gagal, tambahkan logika untuk mencoba ulang:
  ```java
  private void retryPlayback() {
      Uri uri = Uri.parse(videoUrl);
      videoView.setVideoURI(uri);
      progressBar.setVisibility(View.VISIBLE);
  }
  ```

#### e. **Menangani Video Lokal**
- **Penyimpanan**: Simpan video di penyimpanan eksternal atau internal dan pastikan izin tersedia.
- **Path**: Gunakan `Uri.parse("file:///sdcard/large_video.mp4")` atau `Uri.fromFile(new File(path))`.
- **File Besar**: Pastikan perangkat memiliki ruang memori yang cukup untuk memproses file besar.

---

### 7. **Keterbatasan VideoView untuk Video Besar**
- **Tidak Ada Kontrol Buffer**: VideoView tidak memungkinkan pengaturan ukuran buffer atau waktu buffering seperti ExoPlayer.
- **Streaming Adaptif**: Tidak mendukung HLS/DASH, sehingga video besar dengan bitrate tinggi sering kali gagal pada koneksi lambat.
- **Memori**: Decoding video beresolusi tinggi (misalnya, 4K) dapat menyebabkan crash pada perangkat low-end.
- **Alternatif**: Untuk video besar, **ExoPlayer** sangat disarankan karena mendukung buffering canggih, streaming adaptif, dan manajemen memori yang lebih baik.

---

### 8. **Solusi Jika VideoView Tidak Cukup**
Jika Anda mengalami masalah seperti buffering lama, crash, atau performa buruk, pertimbangkan:
- **Gunakan ExoPlayer**: ExoPlayer mendukung HLS/DASH, pengaturan buffer kustom, dan optimasi untuk video besar. Contoh pengaturan buffer di ExoPlayer:
  ```java
  player = new ExoPlayer.Builder(this)
      .setLoadControl(new DefaultLoadControl.Builder()
          .setBufferDurationsMs(5000, 60000, 2500, 5000)
          .build())
      .build();
  ```
- **Kompresi Video**: Kurangi ukuran file atau resolusi sebelum streaming.
- **Chunked Streaming**: Jika memungkinkan, pecah video besar menjadi beberapa bagian kecil di server dan putar sebagai playlist (memerlukan server-side support).

---

### 9. **Contoh URL untuk Pengujian**
- **Video Besar (MP4)**: `https://archive.org/download/BigBuckBunny_124/Content/big_buck_bunny_720p_surround.mp4` (720p, ~440MB).
- **Video Lokal**: Simpan file di `/sdcard/large_video.mp4` atau `res/raw/large_video.mp4` (tanpa ekstensi di kode).

Jika Anda ingin kode tambahan untuk fitur spesifik (misalnya, menangani retry otomatis, menampilkan persentase buffering, atau beralih ke ExoPlayer untuk video besar), beri tahu saya!


---


Untuk menangani video berukuran besar (baik dari segi ukuran file, resolusi tinggi seperti 1080p/4K, atau bitrate tinggi) di Android, **ExoPlayer** adalah pilihan yang jauh lebih baik dibandingkan **VideoView** karena mendukung **streaming adaptif** (HLS/DASH), pengelolaan **buffering** yang canggih, dan optimasi memori. Berikut adalah panduan lengkap menggunakan **ExoPlayer** dalam **Java** untuk memutar video besar, dengan fokus pada streaming dan buffering.

---

### 1. **Keunggulan ExoPlayer untuk Video Besar**
- **Streaming Adaptif**: Mendukung HLS dan DASH, yang secara otomatis menyesuaikan bitrate berdasarkan kecepatan jaringan, mengurangi buffering pada video besar.
- **Kontrol Buffering**: Anda dapat mengatur ukuran buffer untuk mengoptimalkan performa pada koneksi lambat atau perangkat low-end.
- **Manajemen Memori**: Lebih efisien dalam menangani video beresolusi tinggi, dengan opsi untuk mengontrol decoding dan rendering.
- **Format Fleksibel**: Mendukung berbagai format (MP4, MKV, WebM) dan codec (H.264, H.265/HEVC).
- **Error Handling**: Memberikan kontrol lebih baik untuk menangani error jaringan atau file.

---

### 2. **Prasyarat**
Tambahkan dependensi ExoPlayer di `build.gradle` (Module: app):

```gradle
implementation 'androidx.media3:media3-exoplayer:1.3.1'
implementation 'androidx.media3:media3-exoplayer-hls:1.3.1' // Untuk HLS
implementation 'androidx.media3:media3-exoplayer-dash:1.3.1' // Untuk DASH
implementation 'androidx.media3:media3-ui:1.3.1' // Untuk UI PlayerView
```

Tambahkan izin di `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<!-- Untuk video lokal -->
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<!-- Untuk Android 13+ -->
<uses-permission android:name="android.permission.READ_MEDIA_VIDEO" />
```

Pastikan video dalam format yang didukung, seperti MP4 (H.264/H.265) untuk streaming progresif atau HLS/DASH untuk streaming adaptif.

---

### 3. **Layout XML**
Gunakan `PlayerView` untuk menampilkan video dan kontrol UI di `res/layout/activity_main.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.media3.ui.PlayerView
        android:id="@+id/player_view"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        app:use_controller="true"
        app:resize_mode="fit"
        app:buffering_spinner="true" />

    <!-- Opsional: ProgressBar kustom untuk buffering -->
    <ProgressBar
        android:id="@+id/progressBar"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:visibility="gone" />

</RelativeLayout>
```

- `app:buffering_spinner="true"`: Menampilkan indikator bawaan saat buffering.
- Tambahkan `ProgressBar` untuk UI kustom jika diperlukan.

---

### 4. **Kode Java untuk ExoPlayer dengan Video Besar**
Berikut adalah kode di `MainActivity.java` untuk memutar video besar (lokal atau streaming) dengan pengaturan buffering dan optimasi:

```java
import android.net.Uri;
import android.os.Bundle;
import android.view.View;
import android.widget.ProgressBar;
import androidx.appcompat.app.AppCompatActivity;
import androidx.media3.common.MediaItem;
import androidx.media3.common.Player;
import androidx.media3.datasource.DefaultDataSource;
import androidx.media3.datasource.DefaultHttpDataSource;
import androidx.media3.exoplayer.ExoPlayer;
import androidx.media3.exoplayer.hls.HlsMediaSource;
import androidx.media3.exoplayer.source.MediaSource;
import androidx.media3.exoplayer.source.ProgressiveMediaSource;
import androidx.media3.ui.PlayerView;

public class MainActivity extends AppCompatActivity {
    private ExoPlayer player;
    private PlayerView playerView;
    private ProgressBar progressBar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Inisialisasi PlayerView dan ProgressBar
        playerView = findViewById(R.id.player_view);
        progressBar = findViewById(R.id.progressBar);

        // Konfigurasi ExoPlayer dengan pengaturan buffering untuk video besar
        player = new ExoPlayer.Builder(this)
                .setLoadControl(new DefaultLoadControl.Builder()
                        .setBufferDurationsMs(
                                10000, // Minimum buffer (10 detik)
                                60000, // Maksimum buffer (60 detik)
                                5000,  // Buffer untuk mulai playback (5 detik)
                                10000  // Buffer untuk melanjutkan setelah rebuffering (10 detik)
                        )
                        .setTargetBufferBytes(50 * 1024 * 1024) // Target buffer 50MB untuk video besar
                        .build())
                .build();

        // Hubungkan PlayerView dengan ExoPlayer
        playerView.setPlayer(player);

        // Tentukan sumber video
        String videoUrl = "https://archive.org/download/BigBuckBunny_124/Content/big_buck_bunny_720p_surround.mp4"; // Video besar (~440MB, 720p)
        // Untuk HLS: "https://bitdash-a.akamaihd.net/content/sintel/hls/playlist.m3u8"
        // Untuk lokal: "file:///sdcard/large_video.mp4"
        Uri uri = Uri.parse(videoUrl);

        // Buat DataSource untuk streaming atau lokal
        DefaultDataSource.Factory dataSourceFactory = new DefaultDataSource.Factory(this,
                new DefaultHttpDataSource.Factory()
                        .setUserAgent("ExoPlayerApp")
                        .setConnectTimeoutMs(10000)
                        .setReadTimeoutMs(10000));

        // Gunakan ProgressiveMediaSource untuk MP4 atau file lokal
        MediaSource mediaSource = new ProgressiveMediaSource.Factory(dataSourceFactory)
                .createMediaSource(MediaItem.fromUri(uri));

        // Untuk HLS (streaming adaptif, lebih cocok untuk video besar)
        // MediaSource mediaSource = new HlsMediaSource.Factory(dataSourceFactory)
        //         .createMediaSource(MediaItem.fromUri(uri));

        // Set media source ke player
        player.setMediaSource(mediaSource);

        // Tambahkan listener untuk memantau buffering dan status pemutaran
        player.addListener(new Player.Listener() {
            @Override
            public void onPlayerStateChanged(boolean playWhenReady, int playbackState) {
                switch (playbackState) {
                    case Player.STATE_BUFFERING:
                        progressBar.setVisibility(View.VISIBLE);
                        System.out.println("Buffering...");
                        break;
                    case Player.STATE_READY:
                        progressBar.setVisibility(View.GONE);
                        System.out.println("Siap diputar");
                        break;
                    case Player.STATE_ENDED:
                        System.out.println("Pemutaran selesai");
                        break;
                    case Player.STATE_IDLE:
                        System.out.println("Player idle");
                        break;
                }
            }

            @Override
            public void onPlayerError(PlaybackException error) {
                progressBar.setVisibility(View.GONE);
                System.out.println("Error: " + error.getMessage());
                // Coba ulang jika diperlukan
                retryPlayback(uri);
            }
        });

        // Persiapkan dan mulai pemutaran
        player.prepare();
        player.setPlayWhenReady(true); // Mulai otomatis saat siap
    }

    // Fungsi untuk mencoba ulang jika gagal
    private void retryPlayback(Uri uri) {
        DefaultDataSource.Factory dataSourceFactory = new DefaultDataSource.Factory(this,
                new DefaultHttpDataSource.Factory());
        MediaSource mediaSource = new ProgressiveMediaSource.Factory(dataSourceFactory)
                .createMediaSource(MediaItem.fromUri(uri));
        player.setMediaSource(mediaSource);
        player.prepare();
        player.setPlayWhenReady(true);
        progressBar.setVisibility(View.VISIBLE);
    }

    // Kelola lifecycle
    @Override
    protected void onPause() {
        super.onPause();
        if (player != null) {
            player.pause();
        }
    }

    @Override
    protected void onResume() {
        super.onResume();
        if (player != null && !player.isPlaying()) {
            player.play();
        }
    }

    @Override
    protected void onStop() {
        super.onStop();
        if (player != null) {
            player.release();
            player = null;
        }
    }
}
```

---

### 5. **Menangani Izin Runtime (Untuk Video Lokal)**
Jika video besar disimpan di penyimpanan eksternal, tambahkan kode untuk meminta izin:

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
        initializePlayer();
    }
}

@Override
public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
    super.onRequestPermissionsResult(requestCode, permissions, grantResults);
    if (requestCode == 100 && grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
        initializePlayer();
    } else {
        System.out.println("Izin penyimpanan diperlukan");
    }
}

private void initializePlayer() {
    Uri uri = Uri.parse("file:///sdcard/large_video.mp4");
    DefaultDataSource.Factory dataSourceFactory = new DefaultDataSource.Factory(this);
    MediaSource mediaSource = new ProgressiveMediaSource.Factory(dataSourceFactory)
            .createMediaSource(MediaItem.fromUri(uri));
    player.setMediaSource(mediaSource);
    player.prepare();
    player.setPlayWhenReady(true);
    progressBar.setVisibility(View.VISIBLE);
}
```

---

### 6. **Optimasi untuk Video Berukuran Besar**

#### a. **Pengaturan Buffering**
- **DefaultLoadControl**: Kode di atas menggunakan buffer besar (60 detik, 50MB) untuk menangani video besar:
  ```java
  .setBufferDurationsMs(10000, 60000, 5000, 10000)
  .setTargetBufferBytes(50 * 1024 * 1024)
  ```
  - `minBufferMs`: 10 detik untuk memastikan buffering awal cukup.
  - `maxBufferMs`: 60 detik untuk menampung video besar.
  - `targetBufferBytes`: 50MB untuk mencegah kehabisan memori pada perangkat low-end.
- **Streaming Adaptif**: Gunakan HLS/DASH untuk menyesuaikan bitrate secara otomatis:
  ```java
  MediaSource mediaSource = new HlsMediaSource.Factory(dataSourceFactory)
          .createMediaSource(MediaItem.fromUri(uri));
  ```

#### b. **Optimasi Memori**
- **Hardware Decoding**: ExoPlayer secara default menggunakan hardware decoding untuk video besar, mengurangi beban CPU.
- **Resize Mode**: Gunakan `resize_mode="fit"` di XML untuk menghindari scaling berlebihan.
- **Release Player**: Selalu panggil `player.release()` di `onStop` untuk membebaskan memori.

#### c. **Streaming Adaptif**
- **HLS/DASH**: Untuk video besar, gunakan HLS atau DASH untuk mengurangi buffering dan menyesuaikan kualitas. Contoh URL HLS:
  - `https://bitdash-a.akamaihd.net/content/sintel/hls/playlist.m3u8`
- **Progressive Streaming**: Jika menggunakan MP4 progresif, pastikan server mendukung **range requests** (HTTP 206) untuk mengunduh video secara bertahap.

#### d. **Menangani Error**
- Tambahkan logika retry di `onPlayerError` untuk mencoba ulang jika streaming gagal.
- Pantau koneksi jaringan untuk mendeteksi kegagalan:
  ```java
  import android.net.ConnectivityManager;

  private boolean isNetworkAvailable() {
      ConnectivityManager cm = (ConnectivityManager) getSystemService(CONNECTIVITY_SERVICE);
      return cm.getActiveNetworkInfo() != null && cm.getActiveNetworkInfo().isConnected();
  }
  ```

#### e. **Kompresi Video**
- Jika memungkinkan, kompres video di server menggunakan codec H.265 (HEVC) untuk ukuran lebih kecil tanpa kehilangan kualitas. Contoh FFmpeg:
  ```bash
  ffmpeg -i input.mp4 -c:v libx265 -b:v 4000k -vf scale=1920:1080 output.mp4
  ```

#### f. **UI Buffering**
- Gunakan `ProgressBar` kustom atau indikator bawaan (`buffering_spinner`) untuk memberikan umpan balik saat buffering.
- Tambahkan informasi buffering (opsional):
  ```java
  player.addListener(new Player.Listener() {
      @Override
      public void onBufferedPositionChanged() {
          long bufferedMs = player.getBufferedPosition() - player.getCurrentPosition();
          System.out.println("Buffered: " + bufferedMs + "ms");
      }
  });
  ```

---

### 7. **Keunggulan ExoPlayer Dibandingkan VideoView untuk Video Besar**
- **Buffering Fleksibel**: Kontrol ukuran dan waktu buffer secara eksplisit.
- **Streaming Adaptif**: HLS/DASH memungkinkan penyesuaian bitrate, ideal untuk video besar pada koneksi bervariasi.
- **Memori Efisien**: Mendukung decoding hardware dan manajemen memori yang lebih baik.
- **Error Recovery**: Lebih mudah menangani error jaringan atau file dengan retry logic.

---

### 8. **Contoh URL untuk Pengujian**
- **MP4 Besar (Progresif)**: `https://archive.org/download/BigBuckBunny_124/Content/big_buck_bunny_720p_surround.mp4` (~440MB, 720p).
- **HLS (Adaptif)**: `https://bitdash-a.akamaihd.net/content/sintel/hls/playlist.m3u8`.
- **Lokal**: `file:///sdcard/large_video.mp4`.

---

### 9. **Catatan Penting**
- **Perangkat Low-End**: Untuk perangkat dengan RAM rendah, kurangi `maxBufferMs` dan `targetBufferBytes` untuk mencegah crash.
- **Koneksi Lambat**: Gunakan HLS/DASH untuk menyesuaikan kualitas secara otomatis.
- **Testing**: Uji di perangkat fisik dengan resolusi dan koneksi berbeda untuk memastikan performa.

Jika Anda ingin kode tambahan untuk fitur spesifik (misalnya, menangani playlist HLS, menampilkan persentase buffering, atau DRM untuk video besar), beri tahu saya!


---

Berikut adalah panduan untuk menambahkan subtitle pada video di Android menggunakan **VideoView** (tanpa ExoPlayer) dan **ExoPlayer** dalam bahasa **Java**. Karena video berukuran besar menjadi fokus dari pertanyaan sebelumnya, saya akan memastikan solusi ini juga mempertimbangkan performa untuk video besar. Saya akan menjelaskan kedua pendekatan secara terpisah, dengan langkah-langkah dan kode yang jelas.

---

## A. **Menambahkan Subtitle dengan VideoView (Tanpa ExoPlayer)**

**VideoView** tidak memiliki dukungan bawaan untuk menambahkan subtitle eksternal (misalnya, file `.srt` atau `.vtt`) secara langsung. Namun, Anda dapat menggunakan **MediaPlayer** yang mendasari VideoView untuk menangani subtitle dengan bantuan **TimedText** atau library tambahan seperti **SubtitleView**. Di bawah ini adalah solusi menggunakan **TimedText** untuk subtitle sederhana dan pendekatan alternatif dengan file `.srt`.

### 1. **Keterbatasan VideoView dengan Subtitle**
- **VideoView** hanya mendukung subtitle yang disematkan di dalam file video (embedded subtitles) atau melalui `TimedText` dengan pengaturan manual.
- Tidak mendukung file subtitle eksternal seperti `.srt` atau `.vtt` secara langsung tanpa library tambahan.
- Untuk video besar, pengelolaan subtitle dapat memengaruhi performa karena VideoView kurang efisien dalam rendering.

### 2. **Prasyarat**
- **File Subtitle**: Siapkan file subtitle dalam format `.srt` atau `.txt` dengan timestamp (contoh di bawah).
- **Izin**: Jika subtitle atau video disimpan di penyimpanan eksternal:
  ```xml
  <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
  <!-- Untuk Android 13+ -->
  <uses-permission android:name="android.permission.READ_MEDIA_VIDEO" />
  ```
- **Library Tambahan (Opsional)**: Untuk dukungan `.srt` yang lebih baik, gunakan library seperti `SubtitleView` atau parsing manual.

### 3. **Contoh File Subtitle (.srt)**
```srt
1
00:00:01,000 --> 00:00:03,000
Halo, ini adalah subtitle pertama.

2
00:00:04,000 --> 00:00:06,000
Ini subtitle kedua untuk video.
```

### 4. **Layout XML**
Tambahkan `TextView` untuk menampilkan subtitle di atas `VideoView` di `res/layout/activity_main.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <VideoView
        android:id="@+id/videoView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true" />

    <TextView
        android:id="@+id/subtitleText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignBottom="@id/videoView"
        android:background="#80000000" <!-- Latar belakang semi-transparan -->
        android:gravity="center"
        android:padding="10dp"
        android:textColor="@android:color/white"
        android:textSize="16sp" />

    <ProgressBar
        android:id="@+id/progressBar"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:visibility="gone" />

</RelativeLayout>
```

### 5. **Kode Java untuk VideoView dengan Subtitle**
Berikut adalah kode untuk memutar video dengan subtitle menggunakan parsing manual file `.srt`:

```java
import android.media.MediaPlayer;
import android.net.Uri;
import android.os.Bundle;
import android.os.Handler;
import android.view.View;
import android.widget.MediaController;
import android.widget.ProgressBar;
import android.widget.TextView;
import android.widget.VideoView;
import androidx.appcompat.app.AppCompatActivity;
import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {
    private VideoView videoView;
    private TextView subtitleText;
    private ProgressBar progressBar;
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
        setContentView(R.layout.activity_main);

        // Inisialisasi komponen
        videoView = findViewById(R.id.videoView);
        subtitleText = findViewById(R.id.subtitleText);
        progressBar = findViewById(R.id.progressBar);

        // Tambahkan MediaController
        MediaController mediaController = new MediaController(this);
        mediaController.setAnchorView(videoView);
        videoView.setMediaController(mediaController);

        // Tentukan sumber video
        String videoUrl = "https://archive.org/download/BigBuckBunny_124/Content/big_buck_bunny_720p_surround.mp4";
        // Untuk lokal: "file:///sdcard/large_video.mp4"
        Uri uri = Uri.parse(videoUrl);
        videoView.setVideoURI(uri);

        // Load subtitle dari file .srt
        loadSubtitles("/sdcard/subtitle.srt"); // Ganti dengan path subtitle

        // Listener untuk persiapan video
        videoView.setOnPreparedListener(new MediaPlayer.OnPreparedListener() {
            @Override
            public void onPrepared(MediaPlayer mp) {
                progressBar.setVisibility(View.GONE);
                mp.setScreenOnWhilePlaying(true);
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
                        progressBar.setVisibility(View.VISIBLE);
                        return true;
                    case MediaPlayer.MEDIA_INFO_BUFFERING_END:
                        progressBar.setVisibility(View.GONE);
                        return true;
                }
                return false;
            }
        });

        // Listener untuk error
        videoView.setOnErrorListener(new MediaPlayer.OnErrorListener() {
            @Override
            public boolean onError(MediaPlayer mp, int what, int extra) {
                progressBar.setVisibility(View.GONE);
                System.out.println("Error: " + what + ", Extra: " + extra);
                return true;
            }
        });
    }

    // Parsing file .srt
    private void loadSubtitles(String srtPath) {
        try {
            File file = new File(srtPath);
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

### 6. **Catatan untuk VideoView**
- **Keterbatasan**: Parsing `.srt` dilakukan secara manual karena VideoView tidak mendukung subtitle eksternal secara langsung. Ini bisa memengaruhi performa untuk video besar karena sinkronisasi subtitle bergantung pada `Handler`.
- **Video Besar**: Pastikan video dioptimalkan (misalnya, menggunakan H.264/H.265 dengan bitrate rendah) untuk mengurangi beban memori.
- **Alternatif Library**: Gunakan library seperti `SubtitleView` atau beralih ke ExoPlayer untuk pengelolaan subtitle yang lebih baik.

---

## B. **Menambahkan Subtitle dengan ExoPlayer**

**ExoPlayer** memiliki dukungan bawaan untuk subtitle eksternal (seperti `.srt`, `.vtt`, atau `.ass`) dan subtitle yang disematkan di dalam video. ExoPlayer juga lebih efisien untuk video besar karena mendukung streaming adaptif (HLS/DASH) dan pengelolaan memori yang lebih baik.

### 1. **Keunggulan ExoPlayer untuk Subtitle**
- Mendukung file subtitle eksternal (`.srt`, `.vtt`, dll.) tanpa parsing manual.
- Otomatis menangani sinkronisasi subtitle dengan video.
- Mendukung HLS/DASH, ideal untuk streaming video besar dengan subtitle.
- UI subtitle dapat dikustomisasi melalui `PlayerView`.

### 2. **Prasyarat**
- **Dependensi**: Pastikan dependensi ExoPlayer sudah ditambahkan di `build.gradle`:
  ```gradle
  implementation 'androidx.media3:media3-exoplayer:1.3.1'
  implementation 'androidx.media3:media3-exoplayer-hls:1.3.1'
  implementation 'androidx.media3:media3-exoplayer-dash:1.3.1'
  implementation 'androidx.media3:media3-ui:1.3.1'
  ```
- **File Subtitle**: Siapkan file `.srt` atau `.vtt` (lebih disarankan karena kompatibilitas luas).
- **Izin**: Sama seperti VideoView untuk penyimpanan atau internet.

### 3. **Layout XML**
Gunakan `PlayerView` dengan subtitle bawaan di `res/layout/activity_main.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.media3.ui.PlayerView
        android:id="@+id/player_view"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        app:use_controller="true"
        app:resize_mode="fit"
        app:buffering_spinner="true"
        app:show_subtitle_button="true" /> <!-- Aktifkan tombol subtitle -->

    <ProgressBar
        android:id="@+id/progressBar"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:visibility="gone" />

</RelativeLayout>
```

- `app:show_subtitle_button="true"`: Menampilkan tombol subtitle di kontrol UI (opsional).

### 4. **Kode Java untuk ExoPlayer dengan Subtitle**
Berikut adalah kode untuk memutar video besar dengan subtitle eksternal (`.srt`):

```java
import android.net.Uri;
import android.os.Bundle;
import android.view.View;
import android.widget.ProgressBar;
import androidx.appcompat.app.AppCompatActivity;
import androidx.media3.common.MediaItem;
import androidx.media3.common.MimeTypes;
import androidx.media3.common.Player;
import androidx.media3.datasource.DefaultDataSource;
import androidx.media3.datasource.DefaultHttpDataSource;
import androidx.media3.exoplayer.ExoPlayer;
import androidx.media3.exoplayer.hls.HlsMediaSource;
import androidx.media3.exoplayer.source.MediaSource;
import androidx.media3.ui.PlayerView;
import java.util.Collections;

public class MainActivity extends AppCompatActivity {
    private ExoPlayer player;
    private PlayerView playerView;
    private ProgressBar progressBar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Inisialisasi komponen
        playerView = findViewById(R.id.player_view);
        progressBar = findViewById(R.id.progressBar);

        // Konfigurasi ExoPlayer untuk video besar
        player = new ExoPlayer.Builder(this)
                .setLoadControl(new DefaultLoadControl.Builder()
                        .setBufferDurationsMs(10000, 60000, 5000, 10000)
                        .setTargetBufferBytes(50 * 1024 * 1024) // 50MB buffer
                        .build())
                .build();
        playerView.setPlayer(player);

        // Tentukan sumber video dan subtitle
        String videoUrl = "https://archive.org/download/BigBuckBunny_124/Content/big_buck_bunny_720p_surround.mp4";
        // Untuk HLS: "https://bitdash-a.akamaihd.net/content/sintel/hls/playlist.m3u8"
        // Untuk lokal: "file:///sdcard/large_video.mp4"
        String subtitleUrl = "file:///sdcard/subtitle.srt"; // Path subtitle lokal
        // Untuk online: "https://example.com/subtitle.srt"

        Uri videoUri = Uri.parse(videoUrl);
        Uri subtitleUri = Uri.parse(subtitleUrl);

        // Konfigurasi subtitle
        MediaItem.SubtitleConfiguration subtitle = new MediaItem.SubtitleConfiguration.Builder()
                .setUri(subtitleUri)
                .setMimeType(MimeTypes.APPLICATION_SUBRIP) // Untuk .srt
                // .setMimeType(MimeTypes.TEXT_VTT) // Untuk .vtt
                .setLanguage("en") // Bahasa subtitle
                .setSelectionFlags(MediaItem.SubtitleConfiguration.SELECTION_FLAG_DEFAULT)
                .build();

        // Buat MediaItem dengan video dan subtitle
        MediaItem mediaItem = new MediaItem.Builder()
                .setUri(videoUri)
                .setSubtitleConfigurations(Collections.singletonList(subtitle))
                .build();

        // Buat DataSource
        DefaultDataSource.Factory dataSourceFactory = new DefaultDataSource.Factory(this,
                new DefaultHttpDataSource.Factory()
                        .setUserAgent("ExoPlayerApp")
                        .setConnectTimeoutMs(10000)
                        .setReadTimeoutMs(10000));

        // Gunakan ProgressiveMediaSource untuk MP4 atau lokal
        MediaSource mediaSource = new ProgressiveMediaSource.Factory(dataSourceFactory)
                .createMediaSource(mediaItem);

        // Untuk HLS (streaming adaptif)
        // MediaSource mediaSource = new HlsMediaSource.Factory(dataSourceFactory)
        //         .createMediaSource(mediaItem);

        // Set media source
        player.setMediaSource(mediaSource);

        // Listener untuk buffering dan error
        player.addListener(new Player.Listener() {
            @Override
            public void onPlayerStateChanged(boolean playWhenReady, int playbackState) {
                switch (playbackState) {
                    case Player.STATE_BUFFERING:
                        progressBar.setVisibility(View.VISIBLE);
                        System.out.println("Buffering...");
                        break;
                    case Player.STATE_READY:
                        progressBar.setVisibility(View.GONE);
                        System.out.println("Siap diputar");
                        break;
                    case Player.STATE_ENDED:
                        System.out.println("Pemutaran selesai");
                        break;
                }
            }

            @Override
            public void onPlayerError(PlaybackException error) {
                progressBar.setVisibility(View.GONE);
                System.out.println("Error: " + error.getMessage());
                retryPlayback(videoUri, subtitleUri);
            }
        });

        // Persiapkan dan mulai pemutaran
        player.prepare();
        player.setPlayWhenReady(true);
        progressBar.setVisibility(View.VISIBLE);
    }

    // Fungsi untuk mencoba ulang
    private void retryPlayback(Uri videoUri, Uri subtitleUri) {
        MediaItem.SubtitleConfiguration subtitle = new MediaItem.SubtitleConfiguration.Builder()
                .setUri(subtitleUri)
                .setMimeType(MimeTypes.APPLICATION_SUBRIP)
                .setLanguage("en")
                .build();
        MediaItem mediaItem = new MediaItem.Builder()
                .setUri(videoUri)
                .setSubtitleConfigurations(Collections.singletonList(subtitle))
                .build();
        DefaultDataSource.Factory dataSourceFactory = new DefaultDataSource.Factory(this,
                new DefaultHttpDataSource.Factory());
        MediaSource mediaSource = new ProgressiveMediaSource.Factory(dataSourceFactory)
                .createMediaSource(mediaItem);
        player.setMediaSource(mediaSource);
        player.prepare();
        player.setPlayWhenReady(true);
        progressBar.setVisibility(View.VISIBLE);
    }

    // Kelola lifecycle
    @Override
    protected void onPause() {
        super.onPause();
        if (player != null) {
            player.pause();
        }
    }

    @Override
    protected void onResume() {
        super.onResume();
        if (player != null && !player.isPlaying()) {
            player.play();
        }
    }

    @Override
    protected void onStop() {
        super.onStop();
        if (player != null) {
            player.release();
            player = null;
        }
    }
}
```

### 5. **Menangani Izin Runtime (Untuk Subtitle/Video Lokal)**
Jika video atau subtitle disimpan di penyimpanan eksternal:

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
        initializePlayer();
    }
}

@Override
public void onReques






