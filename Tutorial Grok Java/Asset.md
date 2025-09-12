Dalam pengembangan aplikasi Android, ada beberapa cara untuk mengakses atau mengimpor *asset* (seperti file gambar, JSON, font, atau file lainnya) yang disimpan dalam folder `assets` di proyek Anda. Berikut adalah langkah-langkah dan penjelasan singkat:

### 1. **Memahami Folder Assets**
- Folder `assets` terletak di dalam direktori proyek Android, biasanya di `app/src/main/assets/`.
- Berbeda dengan folder `res` (resources), file di folder `assets` tidak dikompilasi menjadi ID sumber daya (tidak memiliki `R.id`). File ini disimpan dalam bentuk aslinya dan dapat diakses sebagai file mentah.
- Folder `assets` cocok untuk menyimpan file seperti JSON, file konfigurasi, font kustom, atau file media yang tidak perlu diakses melalui ID sumber daya.

### 2. **Menambahkan Asset**
- Buat folder `assets` di `app/src/main/` jika belum ada. Di Android Studio:
  - Klik kanan pada modul `app` → `New` → `Folder` → `Assets Folder`.
- Tambahkan file (misalnya, `data.json`, `image.png`, atau `font.ttf`) ke dalam folder `assets`. Anda juga bisa membuat subfolder di dalamnya (misalnya, `assets/images/`).

### 3. **Mengakses Asset di Kode**
Untuk mengakses file di folder `assets`, Anda menggunakan `AssetManager`. Berikut adalah cara melakukannya:

#### a. **Mengakses AssetManager**
```java
AssetManager assetManager = context.getAssets();
```

- `context` bisa berupa `Activity` (gunakan `this` di dalam Activity) atau `getApplicationContext()`.

#### b. **Membaca File sebagai String**
Jika Anda ingin membaca file teks (misalnya, JSON atau txt):
```java
public String readAssetFile(String fileName) {
    try {
        InputStream inputStream = getAssets().open(fileName);
        BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream));
        StringBuilder stringBuilder = new StringBuilder();
        String line;
        while ((line = reader.readLine()) != null) {
            stringBuilder.append(line).append("\n");
        }
        inputStream.close();
        return stringBuilder.toString();
    } catch (IOException e) {
        e.printStackTrace();
        return null;
    }
}
```
- Contoh penggunaan: `String json = readAssetFile("data.json");`

#### c. **Membaca File sebagai Bitmap (untuk Gambar)**
Jika Anda ingin memuat gambar dari folder `assets`:
```java
public Bitmap loadImageFromAsset(String fileName) {
    try {
        InputStream inputStream = getAssets().open(fileName);
        return BitmapFactory.decodeStream(inputStream);
    } catch (IOException e) {
        e.printStackTrace();
        return null;
    }
}
```
- Contoh penggunaan: `Bitmap bitmap = loadImageFromAsset("images/photo.png");`

#### d. **Mengakses File dalam Subfolder**
Jika file berada di subfolder (misalnya, `assets/images/photo.png`), gunakan path relatif:
```java
InputStream inputStream = getAssets().open("images/photo.png");
```

#### e. **Menggunakan File untuk Font Kustom**
Untuk memuat font dari `assets`:
```java
Typeface typeface = Typeface.createFromAsset(getAssets(), "fonts/custom_font.ttf");
textView.setTypeface(typeface);
```

### 4. **Mengimpor Asset secara Dinamis**
Jika Anda perlu mengakses semua file dalam folder tertentu:
```java
public String[] listAssets(String folderPath) {
    try {
        return getAssets().list(folderPath);
    } catch (IOException e) {
        e.printStackTrace();
        return null;
    }
}
```
- Contoh: `String[] files = listAssets("images");` akan mengembalikan daftar nama file di folder `assets/images`.

### 5. **Catatan Penting**
- **Perbedaan dengan `res/raw`**: File di `res/raw` diakses melalui ID sumber daya (`R.raw.filename`), sedangkan file di `assets` diakses melalui path string.
- **Ukuran File**: Folder `assets` cocok untuk file besar, tetapi perhatikan ukuran aplikasi karena file di `assets` disertakan apa adanya dalam APK.
- **Error Handling**: Selalu tangani `IOException` saat mengakses file untuk mencegah crash.
- **Kotlin**: Jika menggunakan Kotlin, Anda bisa menggunakan fungsi serupa dengan sintaks yang lebih ringkas:
  ```kotlin
  fun readAssetFile(fileName: String): String? {
      return try {
          assets.open(fileName).bufferedReader().use { it.readText() }
      } catch (e: IOException) {
          e.printStackTrace()
          null
      }
  }
  ```

### 6. **Contoh Praktis**
Misalkan Anda memiliki file `config.json` di folder `assets`:
```java
try {
    InputStream inputStream = getAssets().open("config.json");
    String json = new BufferedReader(new InputStreamReader(inputStream))
            .lines()
            .collect(Collectors.joining("\n"));
    // Parse JSON atau gunakan sesuai kebutuhan
    inputStream.close();
} catch (IOException e) {
    e.printStackTrace();
}
```

### 7. **Alternatif: Menggunakan Library**
Untuk mempermudah pengelolaan asset, Anda bisa menggunakan library seperti Gson untuk parsing JSON atau Glide untuk memuat gambar:
- **Gson**:
  ```java
  Gson gson = new Gson();
  InputStream inputStream = getAssets().open("data.json");
  MyData data = gson.fromJson(new InputStreamReader(inputStream), MyData.class);
  ```
- **Glide** (untuk gambar):
  ```java
  Glide.with(context)
       .load("file:///android_asset/images/photo.png")
       .into(imageView);
  ```

### 8. **Praktik Terbaik**
- Gunakan folder `assets` hanya untuk file yang tidak memerlukan ID sumber daya.
- Jika file dapat dikelompokkan sebagai resource (misalnya, drawable, string), pertimbangkan menggunakan folder `res`.
- Simpan file besar di server jika memungkinkan untuk mengurangi ukuran APK.
- Selalu tutup `InputStream` setelah digunakan untuk mencegah kebocoran memori.

Jika Anda memiliki pertanyaan lebih spesifik, misalnya, tentang jenis file tertentu atau penggunaan di framework tertentu (seperti Jetpack Compose), beri tahu saya!

Mengelola *asset* besar dalam pengembangan aplikasi Android memerlukan strategi khusus untuk menjaga performa aplikasi, mengurangi ukuran APK/AAB, dan memastikan pengalaman pengguna yang optimal. Berikut adalah panduan lengkap untuk mengelola *asset* besar (seperti file video, audio, gambar beresolusi tinggi, atau dataset besar) yang disimpan di folder `assets` atau sumber lainnya:

### 1. **Memahami Tantangan Asset Besar**
- **Ukuran APK/AAB**: Asset besar meningkatkan ukuran file aplikasi, yang dapat memengaruhi waktu unduh dan penyimpanan di perangkat pengguna.
- **Kinerja**: Memuat asset besar ke memori (misalnya, gambar atau video) dapat menyebabkan masalah performa atau *out-of-memory* (OOM) errors.
- **Keterbatasan Perangkat**: Perangkat dengan spesifikasi rendah mungkin kesulitan menangani asset besar.
- **Distribusi**: Google Play memiliki batasan ukuran APK (100 MB untuk APK, 150 MB untuk AAB tanpa ekspansi). Asset besar mungkin memerlukan solusi tambahan seperti *App Bundle* atau pengunduhan dinamis.

### 2. **Strategi Mengelola Asset Besar**
Berikut adalah beberapa pendekatan untuk mengelola asset besar secara efektif:

#### a. **Optimasi Asset**
- **Kompresi File**:
  - **Gambar**: Gunakan format seperti WebP atau JPEG dengan tingkat kompresi yang sesuai. Alat seperti *ImageOptim* atau *TinyPNG* dapat membantu mengurangi ukuran file tanpa mengorbankan kualitas.
  - **Video**: Kompres video menggunakan codec seperti H.264 atau H.265 (HEVC). Gunakan alat seperti *FFmpeg* untuk mengoptimalkan ukuran video.
  - **Audio**: Gunakan format seperti MP3 atau AAC dengan bitrate rendah untuk audio yang tidak memerlukan kualitas tinggi.
- **Resolusi yang Sesuai**: Sesuaikan resolusi asset dengan kebutuhan aplikasi. Misalnya, gunakan gambar dengan resolusi lebih rendah untuk thumbnail.
- **Subset Asset**: Jika menggunakan dataset besar (misalnya, file JSON atau database), pertimbangkan untuk hanya menyertakan data yang diperlukan.

#### b. **Menyimpan Asset di Server (Download On-Demand)**
- Daripada menyimpan asset besar di folder `assets`, simpan di server dan unduh saat dibutuhkan:
  - **Keuntungan**: Mengurangi ukuran aplikasi dan memungkinkan pembaruan asset tanpa memperbarui aplikasi.
  - **Cara Implementasi**:
    - Gunakan layanan cloud seperti Firebase Storage, AWS S3, atau server khusus.
    - Unduh file menggunakan library seperti *OkHttp* atau *Retrofit*:
      ```java
      OkHttpClient client = new OkHttpClient();
      Request request = new Request.Builder()
          .url("https://your-server.com/large_file.mp4")
          .build();
      client.newCall(request).enqueue(new Callback() {
          @Override
          public void onResponse(Call call, Response response) throws IOException {
              InputStream inputStream = response.body().byteStream();
              // Simpan ke penyimpanan lokal atau proses
          }
          @Override
          public void onFailure(Call call, IOException e) {
              e.printStackTrace();
          }
      });
      ```
    - Simpan file yang diunduh di *cache* atau *internal storage* perangkat untuk penggunaan offline.
  - **Catatan**: Pastikan menangani kasus tanpa koneksi internet dengan menyediakan fallback atau pesan error yang jelas.

#### c. **Menggunakan Android App Bundle (AAB) dengan Dynamic Delivery**
- Dengan *Android App Bundle*, Anda dapat memisahkan asset besar ke dalam modul yang diunduh secara dinamis melalui *Play Feature Delivery* atau *Play Asset Delivery*:
  - **Play Asset Delivery (PAD)**: Cocok untuk asset besar seperti tekstur game atau file media.
    - **Jenis Pengiriman**:
      - **Install-time**: Asset diunduh saat instalasi (cocok untuk asset <150 MB).
      - **Fast-follow**: Asset diunduh segera setelah instalasi.
      - **On-demand**: Asset diunduh saat dibutuhkan (misalnya, saat level game tertentu dimainkan).
    - **Cara Implementasi**:
      1. Tambahkan asset ke *asset pack* di `app/src/main/assetpacks/`.
      2. Konfigurasikan `build.gradle`:
         ```gradle
         android {
             ...
             assetPacks = [":assetpack1"]
         }
         ```
      3. Gunakan *Play Asset Delivery API* untuk mengakses asset:
         ```java
         AssetPackManager assetPackManager = AssetPackManagerFactory.getInstance(context);
         assetPackManager.registerListener(stateUpdate -> {
             if (stateUpdate.status() == AssetPackStatus.COMPLETED) {
                 // Asset siap digunakan
                 String assetPath = assetPackManager.getAssetPackPath("assetpack1") + "/file.mp4";
             }
         });
         ```
  - **Keuntungan**: Mengurangi ukuran aplikasi awal dan mendukung pengiriman asset yang fleksibel.

#### d. **Mengelola Memori Saat Memuat Asset**
- **Streaming untuk File Besar**: Untuk file seperti video atau audio, gunakan streaming alih-alih memuat seluruh file ke memori:
  - Gunakan `MediaPlayer` untuk audio/video:
    ```java
    MediaPlayer mediaPlayer = new MediaPlayer();
    AssetFileDescriptor afd = getAssets().openFd("large_video.mp4");
    mediaPlayer.setDataSource(afd.getFileDescriptor(), afd.getStartOffset(), afd.getLength());
    mediaPlayer.prepare();
    mediaPlayer.start();
    ```
  - Untuk gambar besar, gunakan `BitmapFactory.Options` untuk mengurangi konsumsi memori:
    ```java
    BitmapFactory.Options options = new BitmapFactory.Options();
    options.inSampleSize = 2; // Kurangi resolusi (misalnya, 1/2 ukuran asli)
    Bitmap bitmap = BitmapFactory.decodeStream(getAssets().open("large_image.png"), null, options);
    ```
- **Pembersihan Memori**: Pastikan untuk melepaskan sumber daya setelah digunakan (misalnya, `bitmap.recycle()` atau `inputStream.close()`).

#### e. **Menyimpan Asset di Penyimpanan Eksternal**
- Jika asset besar diunduh atau diakses, simpan di penyimpanan eksternal atau internal perangkat:
  ```java
  File file = new File(context.getExternalFilesDir(null), "large_file.mp4");
  try (InputStream inputStream = getAssets().open("large_file.mp4");
       FileOutputStream outputStream = new FileOutputStream(file)) {
      byte[] buffer = new byte[1024];
      int bytesRead;
      while ((bytesRead = inputStream.read(buffer)) != -1) {
          outputStream.write(buffer, 0, bytesRead);
      }
  } catch (IOException e) {
      e.printStackTrace();
  }
  ```
- **Izin**: Jika menggunakan penyimpanan eksternal, pastikan menambahkan izin di `AndroidManifest.xml` (untuk Android 9 ke bawah):
  ```xml
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
  ```

#### f. **Menggunakan Kompresi di Assets**
- Jika asset harus disimpan di folder `assets`, pertimbangkan untuk mengompresnya sebagai file ZIP dan dekompresi saat runtime:
  ```java
  public void unzipAsset(String assetFileName, File destination) {
      try (InputStream inputStream = getAssets().open(assetFileName);
           ZipInputStream zis = new ZipInputStream(inputStream)) {
          ZipEntry entry;
          while ((entry = zis.getNextEntry()) != null) {
              File newFile = new File(destination, entry.getName());
              try (FileOutputStream fos = new FileOutputStream(newFile)) {
                  byte[] buffer = new byte[1024];
                  int len;
                  while ((len = zis.read(buffer)) > 0) {
                      fos.write(buffer, 0, len);
                  }
              }
          }
      } catch (IOException e) {
          e.printStackTrace();
      }
  }
  ```

### 3. **Praktik Terbaik**
- **Gunakan Format Efisien**: Pilih format file yang hemat ruang (WebP untuk gambar, MP4 dengan H.265 untuk video).
- **Tes di Berbagai Perangkat**: Uji performa aplikasi di perangkat dengan spesifikasi rendah untuk memastikan kompatibilitas.
- **Caching**: Cache asset yang diunduh untuk mengurangi kebutuhan pengunduhan berulang.
- **Monitoring Ukuran**: Gunakan alat seperti *Android Studio Profiler* atau *APK Analyzer* untuk memantau kontribusi asset terhadap ukuran aplikasi.
- **Dokumentasi dan Organisasi**: Atur asset dalam subfolder yang jelas (misalnya, `assets/videos/`, `assets/images/`) untuk memudahkan pengelolaan.

### 4. **Alat dan Library Pendukung**
- **Glide/Picasso**: Untuk memuat dan mengelola gambar besar dengan efisien.
- **FFmpeg**: Untuk mengompresi atau memproses video/audio.
- **OkHttp/Retrofit**: Untuk mengunduh asset dari server.
- **Room/SQlite**: Jika asset berupa data besar, pertimbangkan menyimpannya sebagai database setelah diproses.

### 5. **Contoh Praktis**
Misalkan Anda memiliki video besar (`large_video.mp4`) di server:
1. Unduh video saat aplikasi pertama kali dijalankan:
   ```java
   OkHttpClient client = new OkHttpClient();
   Request request = new Request.Builder()
       .url("https://your-server.com/large_video.mp4")
       .build();
   Response response = client.newCall(request).execute();
   File file = new File(getExternalFilesDir(null), "large_video.mp4");
   FileOutputStream fos = new FileOutputStream(file);
   fos.write(response.body().bytes());
   fos.close();
   ```
2. Putar video menggunakan `ExoPlayer`:
   ```java
   ExoPlayer player = new ExoPlayer.Builder(context).build();
   MediaItem mediaItem = MediaItem.fromUri(Uri.fromFile(file));
   player.setMediaItem(mediaItem);
   player.prepare();
   player.play();
   ```

### 6. **Jika Asset Harus di Folder `assets`**
Jika tidak memungkinkan menyimpan di server, gunakan *Play Asset Delivery* atau simpan di `assets` dengan kompresi maksimal. Namun, pastikan untuk:
- Memecah file besar menjadi bagian kecil jika memungkinkan.
- Menggunakan streaming untuk membaca file secara bertahap.
- Menguji dampaknya pada ukuran APK dan performa aplikasi.

Jika Anda memiliki kasus spesifik (misalnya, jenis asset tertentu atau kebutuhan khusus seperti game), beri tahu saya untuk solusi yang lebih disesuaikan!

Dalam pengembangan aplikasi Android, penggunaan **database lokal** sangat umum untuk menyimpan data secara offline, seperti data pengguna, cache, atau konfigurasi aplikasi. Database lokal memungkinkan aplikasi berfungsi tanpa koneksi internet dan meningkatkan performa dengan mengurangi ketergantungan pada server. Berikut adalah penjelasan tentang cara menggunakan database lokal di Android, dengan fokus pada solusi populer seperti **Room** (rekomendasi resmi dari Google) dan langkah-langkah praktisnya.

### 1. **Memahami Database Lokal di Android**
- **Tujuan**: Menyimpan data terstruktur (seperti daftar pengguna, transaksi, atau pengaturan) di perangkat pengguna.
- **Jenis Database**:
  - **SQLite**: Database ringan yang terintegrasi di Android, cocok untuk data relasional.
  - **Room**: Abstraksi di atas SQLite yang menyederhanakan pengelolaan database.
  - **SharedPreferences**: Untuk data sederhana (key-value), bukan database relasional.
  - **File Internal/Eksternal**: Untuk data tidak terstruktur, tetapi kurang efisien dibandingkan database.
- **Kapan Menggunakan Database Lokal**:
  - Menyimpan data sementara (cache) untuk akses offline.
  - Menyimpan data pengguna seperti preferensi atau riwayat.
  - Mengelola dataset besar dari asset (misalnya, JSON yang diimpor ke database).

### 2. **Menggunakan Room untuk Database Lokal**
Room adalah library resmi dari Android Jetpack yang direkomendasikan untuk pengelolaan database lokal karena kemudahan penggunaan, keamanan, dan integrasi dengan LiveData atau Kotlin Flow. Berikut langkah-langkahnya:

#### a. **Menambahkan Dependensi**
Tambahkan dependensi Room di file `build.gradle` (modul app):
```gradle
dependencies {
    implementation "androidx.room:room-runtime:2.6.1"
    annotationProcessor "androidx.room:room-compiler:2.6.1"
    // Jika menggunakan Kotlin
    implementation "androidx.room:room-ktx:2.6.1"
    kapt "androidx.room:room-compiler:2.6.1"
}
```
Jika menggunakan Kotlin, pastikan plugin `kapt` diaktifkan:
```gradle
apply plugin: 'kotlin-kapt'
```

#### b. **Membuat Komponen Room**
Room terdiri dari tiga komponen utama:
1. **Entity**: Kelas yang merepresentasikan tabel dalam database.
2. **DAO (Data Access Object)**: Interface untuk mendefinisikan operasi database (insert, update, delete, query).
3. **Database**: Kelas abstrak yang mengelola koneksi database dan menyediakan akses ke DAO.

**Contoh Entity** (misalnya, untuk tabel pengguna):
```java
@Entity(tableName = "users")
public class User {
    @PrimaryKey(autoGenerate = true)
    public int id;

    @ColumnInfo(name = "name")
    public String name;

    @ColumnInfo(name = "email")
    public String email;
}
```
**Kotlin**:
```kotlin
@Entity(tableName = "users")
data class User(
    @PrimaryKey(autoGenerate = true)
    val id: Int = 0,
    @ColumnInfo(name = "name")
    val name: String,
    @ColumnInfo(name = "email")
    val email: String
)
```

**Contoh DAO**:
```java
@Dao
public interface UserDao {
    @Insert
    void insert(User user);

    @Update
    void update(User user);

    @Delete
    void delete(User user);

    @Query("SELECT * FROM users")
    List<User> getAllUsers();
}
```
**Kotlin**:
```kotlin
@Dao
interface UserDao {
    @Insert
    suspend fun insert(user: User)

    @Update
    suspend fun update(user: User)

    @Delete
    suspend fun delete(user: User)

    @Query("SELECT * FROM users")
    fun getAllUsers(): List<User>
}
```

**Contoh Database**:
```java
@Database(entities = {User.class}, version = 1)
public abstract class AppDatabase extends RoomDatabase {
    public abstract UserDao userDao();
}
```
**Kotlin**:
```kotlin
@Database(entities = [User::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun userDao(): UserDao
}
```

#### c. **Menginisialisasi Database**
Inisialisasi database di aplikasi (biasanya dalam `Application` atau `Activity`):
```java
AppDatabase db = Room.databaseBuilder(
        getApplicationContext(),
        AppDatabase.class,
        "app_database"
).build();
```
**Kotlin**:
```kotlin
val db = Room.databaseBuilder(
    applicationContext,
    AppDatabase::class.java,
    "app_database"
).build()
```

#### d. **Menggunakan Database**
Contoh menyimpan dan mengambil data:
```java
// Menyimpan data
new Thread(() -> {
    User user = new User();
    user.name = "John Doe";
    user.email = "john@example.com";
    db.userDao().insert(user);
}).start();

// Mengambil data
List<User> users = db.userDao().getAllUsers();
```
**Kotlin dengan Coroutines**:
```kotlin
// Menyimpan data
lifecycleScope.launch {
    val user = User(name = "John Doe", email = "john@example.com")
    db.userDao().insert(user)
}

// Mengambil data
val users = db.userDao().getAllUsers()
```

#### e. **Menggunakan LiveData atau Flow (Opsional)**
Untuk pembaruan data secara reaktif:
```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users")
    fun getAllUsers(): LiveData<List<User>> // atau Flow<List<User>>
}
```
Gunakan di UI:
```kotlin
db.userDao().getAllUsers().observe(this) { users ->
    // Update UI dengan data
}
```

### 3. **Mengimpor Asset Besar ke Database Lokal**
Jika Anda ingin mengimpor data dari asset besar (misalnya, file JSON) ke database lokal:
1. **Baca File dari Assets**:
```kotlin
fun readJsonFromAssets(fileName: String): String? {
    return try {
        assets.open(fileName).bufferedReader().use { it.readText() }
    } catch (e: IOException) {
        e.printStackTrace()
        null
    }
}
```

2. **Parse JSON dan Masukkan ke Database**:
Misalkan JSON berisi daftar pengguna:
```kotlin
lifecycleScope.launch {
    val json = readJsonFromAssets("users.json")
    val users = Gson().fromJson(json, Array<User>::class.java).toList()
    db.userDao().insertAll(users)
}
```
**DAO Tambahan**:
```kotlin
@Dao
interface UserDao {
    @Insert
    suspend fun insertAll(users: List<User>)
}
```

### 4. **Mengelola Asset Besar dalam Database**
Jika asset besar berupa data (misalnya, dataset JSON atau CSV), berikut tipsnya:
- **Impor Bertahap**: Baca dan masukkan data secara bertahap (chunking) untuk menghindari kehabisan memori:
  ```kotlin
  fun importLargeJson(fileName: String) {
      assets.open(fileName).bufferedReader().useLines { lines ->
          lines.chunked(100) { chunk -> // Proses 100 baris sekaligus
              lifecycleScope.launch {
                  val users = chunk.map { Gson().fromJson(it, User::class.java) }
                  db.userDao().insertAll(users)
              }
          }
      }
  }
  ```
- **Gunakan Transaksi**: Untuk operasi masal, gunakan transaksi untuk meningkatkan performa:
  ```kotlin
  @Dao
  interface UserDao {
      @Transaction
      @Insert
      suspend fun insertAll(users: List<User>)
  }
  ```
- **Kompresi Data**: Simpan hanya data yang diperlukan di database untuk mengurangi ukuran.
- **Indexing**: Tambahkan indeks pada kolom yang sering di-query untuk mempercepat pencarian:
  ```kotlin
  @Entity(tableName = "users", indices = [Index(value = ["email"])])
  data class User(...)
  ```

### 5. **Alternatif Selain Room**
- **SQLite Langsung**:
  - Gunakan `SQLiteOpenHelper` untuk mengelola database secara manual:
    ```java
    public class DatabaseHelper extends SQLiteOpenHelper {
        public DatabaseHelper(Context context) {
            super(context, "app_database.db", null, 1);
        }

        @Override
        public void onCreate(SQLiteDatabase db) {
            db.execSQL("CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT, email TEXT)");
        }

        @Override
        public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
            // Handle upgrade
        }
    }
    ```
  - Kurang direkomendasikan karena lebih kompleks dan rawan error dibandingkan Room.
- **Realm**:
  - Library database alternatif yang cepat untuk data kompleks, tetapi bukan bagian dari Jetpack.
  - Contoh penggunaan:
    ```kotlin
    val realm = Realm.getDefaultInstance()
    realm.executeTransaction { r ->
        val user = r.createObject(User::class.java)
        user.name = "John Doe"
        user.email = "john@example.com"
    }
    ```
- **SharedPreferences**:
  - Cocok untuk data sederhana (bukan asset besar):
    ```kotlin
    val prefs = getSharedPreferences("prefs", MODE_PRIVATE)
    prefs.edit().putString("key", "value").apply()
    ```

### 6. **Praktik Terbaik**
- **Gunakan Room untuk Data Relasional**: Room adalah pilihan terbaik untuk sebagian besar kasus karena aman, terintegrasi, dan mendukung fitur modern seperti Coroutines dan LiveData.
- **Hindari Penyimpanan Data Besar di Database**: Untuk file besar (gambar, video), simpan di penyimpanan lokal dan hanya simpan referensi (path) di database.
- **Backup dan Sinkronisasi**: Jika data penting, pertimbangkan sinkronisasi dengan server (misalnya, menggunakan WorkManager untuk upload periodik).
- **Keamanan**: Enkripsi database jika berisi data sensitif menggunakan `SQLCipher` dengan Room:
  ```gradle
  implementation "androidx.sqlite:sqlite:2.3.1"
  implementation "net.zetetic:android-database-sqlcipher:4.5.4"
  ```
- **Optimasi Performa**: Gunakan indeks, transaksi, dan query yang efisien untuk dataset besar.
- **Uji di Berbagai Perangkat**: Pastikan database berfungsi dengan baik di perangkat dengan penyimpanan terbatas.

### 7. **Contoh Praktis: Mengimpor JSON Besar ke Database**
Misalkan Anda memiliki file `users.json` di folder `assets`:
```json
[
    {"name": "John", "email": "john@example.com"},
    {"name": "Jane", "email": "jane@example.com"}
]
```
Langkah-langkah:
1. Baca JSON:
   ```kotlin
   val json = assets.open("users.json").bufferedReader().use { it.readText() }
   ```
2. Parse dan masukkan ke database:
   ```kotlin
   lifecycleScope.launch {
       val users = Gson().fromJson(json, Array<User>::class.java).toList()
       db.userDao().insertAll(users)
   }
   ```
3. Tampilkan data di UI:
   ```kotlin
   db.userDao().getAllUsers().observe(this) { users ->
       // Update RecyclerView atau UI lainnya
   }
   ```

### 8. **Mengatasi Masalah Umum**
- **Ukuran Database Besar**: Gunakan kompresi data atau hapus data yang tidak diperlukan secara periodik:
  ```kotlin
  @Query("DELETE FROM users WHERE id NOT IN (SELECT id FROM users ORDER BY id DESC LIMIT 100)")
  suspend fun deleteOldUsers()
  ```
- **Migrasi Database**: Jika skema database berubah, tambahkan migrasi di `RoomDatabase`:
  ```kotlin
  @Database(entities = [User::class], version = 2)
  abstract class AppDatabase : RoomDatabase() {
      companion object {
          val MIGRATION_1_2 = object : Migration(1, 2) {
              override fun migrate(database: SupportSQLiteDatabase) {
                  database.execSQL("ALTER TABLE users ADD COLUMN age INTEGER")
              }
          }
      }
  }
  ```
  Inisialisasi dengan migrasi:
  ```kotlin
  val db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "app_database")
      .addMigrations(AppDatabase.MIGRATION_1_2)
      .build()
  ```

Jika Anda memiliki kebutuhan spesifik (misalnya, format data tertentu, sinkronisasi dengan server, atau penggunaan database lain), beri tahu saya untuk solusi yang lebih terperinci!


