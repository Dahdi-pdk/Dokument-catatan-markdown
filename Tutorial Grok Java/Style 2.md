
# **Tema Varian 1**
---

Berikut variasi tema yang hanya menggunakan resource bawaan Android (tidak memerlukan resource eksternal seperti `@color/...`):

```xml
<resources>
    <!-- Tema dasar dengan DarkActionBar -->
    <style name="AppTheme" parent="@android:style/Theme.Material.Light.DarkActionBar">
        <!-- Menggunakan warna bawaan Material -->
        <item name="android:colorPrimary">@android:color/holo_blue_light</item>
        <item name="android:colorPrimaryDark">@android:color/holo_blue_dark</item>
        <item name="android:colorAccent">@android:color/holo_green_light</item>
    </style>

    <!-- Tema Fullscreen -->
    <style name="FullScreenTheme" parent="@android:style/Theme.Material.Light.NoActionBar.Fullscreen">
        <item name="android:colorPrimary">@android:color/holo_purple</item>
        <item name="android:colorPrimaryDark">@android:color/holo_purple_dark</item>
        <item name="android:colorAccent">@android:color/holo_orange_light</item>
    </style>

    <!-- Tema tanpa ActionBar -->
    <style name="NoActionBarTheme" parent="@android:style/Theme.Material.Light.NoActionBar">
        <item name="android:colorPrimary">@android:color/holo_red_light</item>
        <item name="android:colorPrimaryDark">@android:color/holo_red_dark</item>
        <item name="android:colorAccent">@android:color/holo_blue_light</item>
    </style>

    <!-- Tema dengan windowFullscreen (status bar hidden) -->
    <style name="NoStatusBarTheme" parent="AppTheme">
        <item name="android:windowFullscreen">true</item>
    </style>

    <!-- Tema gelap (Dark Theme) -->
    <style name="DarkTheme" parent="@android:style/Theme.Material">
        <item name="android:colorPrimary">@android:color/holo_blue_dark</item>
        <item name="android:colorPrimaryDark">@android:color/black</item>
        <item name="android:colorAccent">@android:color/holo_green_light</item>
    </style>
</resources>
```

Warna yang digunakan adalah warna bawaan Android dari `@android:color/...` seperti:
- `holo_blue_light`, `holo_blue_dark`
- `holo_green_light`
- `holo_purple`, `holo_purple_dark`
- `holo_orange_light`
- `holo_red_light`, `holo_red_dark`
- `black`, dll.

Semua warna ini tersedia di semua versi Android modern tanpa perlu mendefinisikan warna custom.

---


# **Varian 2**
---
Berikut variasi tema yang hanya menggunakan resource bawaan Android **tanpa androidx** (menggunakan support library lama atau framework default Android):

```xml
<resources>
    <!-- Base Theme dengan DarkActionBar -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Menggunakan warna bawaan Android -->
        <item name="colorPrimary">@android:color/holo_blue_light</item>
        <item name="colorPrimaryDark">@android:color/holo_blue_dark</item>
        <item name="colorAccent">@android:color/holo_green_light</item>
    </style>

    <!-- Fullscreen Theme (No ActionBar & No Status Bar) -->
    <style name="FullScreenTheme" parent="Theme.AppCompat.Light.NoActionBar.Fullscreen">
        <item name="colorPrimary">@android:color/holo_purple</item>
        <item name="colorPrimaryDark">@android:color/holo_purple_dark</item>
        <item name="colorAccent">@android:color/holo_orange_light</item>
    </style>

    <!-- No ActionBar Theme -->
    <style name="NoActionBarTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <item name="colorPrimary">@android:color/holo_red_light</item>
        <item name="colorPrimaryDark">@android:color/holo_red_dark</item>
        <item name="colorAccent">@android:color/holo_blue_light</item>
    </style>

    <!-- Hidden Status Bar (tetap ada ActionBar) -->
    <style name="NoStatusBarTheme" parent="AppTheme">
        <item name="android:windowFullscreen">true</item>
    </style>

    <!-- Dark Theme (AppCompat Dark) -->
    <style name="DarkTheme" parent="Theme.AppCompat">
        <item name="colorPrimary">@android:color/holo_blue_dark</item>
        <item name="colorPrimaryDark">@android:color/black</item>
        <item name="colorAccent">@android:color/holo_green_light</item>
    </style>

    <!-- Dialog Theme -->
    <style name="DialogTheme" parent="Theme.AppCompat.Light.Dialog">
        <item name="colorPrimary">@android:color/holo_orange_dark</item>
        <item name="colorAccent">@android:color/holo_blue_light</item>
    </style>
</resources>
```

### Perbedaan dengan versi sebelumnya:
1. **Menggunakan `Theme.AppCompat`** (Support Library lama) alih-alih `Theme.Material`.
2. **Tidak memakai prefix `android:`** pada atribut warna (karena AppCompat memiliki namespace sendiri).
3. **Tetap menggunakan warna bawaan Android** (`@android:color/...`), sehingga tidak perlu resource eksternal.

### Catatan:
- Jika Anda benar-benar ingin **tanpa Support Library sama sekali**, ganti parent dengan `@android:style/Theme.Holo.Light` (untuk Android 4.x) atau `@android:style/Theme.DeviceDefault.Light`.
- Contoh tema pure Android framework (tanpa AppCompat):
  ```xml
  <style name="PureAndroidTheme" parent="@android:style/Theme.Holo.Light.DarkActionBar">
      <item name="android:colorPrimary">@android:color/holo_blue_light</item>
  </style>
  ```


---



# **Tema untuk widget**
yang hanya menggunakan **resource bawaan Android** tanpa memerlukan *AndroidX* atau library eksternal:

---

### **1. Tema Dasar untuk Widget (Menggunakan `Theme.Holo` - API 14+)**
```xml
<style name="WidgetButtonStyle" parent="@android:style/Widget.Holo.Button">
    <item name="android:background">@android:drawable/btn_default_holo_light</item>
    <item name="android:textColor">@android:color/primary_text_light</item>
</style>
```

---

### **2. Tema Tombol dengan Warna Bawaan Android**
```xml
<style name="ColoredHoloButton" parent="@android:style/Widget.Holo.Button">
    <!-- Warna dari @android:color/holo_* -->
    <item name="android:background">@android:color/holo_blue_light</item>
    <item name="android:textColor">@android:color/white</item>
    <item name="android:textStyle">bold</item>
</style>
```

---

### **3. Tema EditText (TextInput) Holo**
```xml
<style name="HoloEditText" parent="@android:style/Widget.Holo.EditText">
    <item name="android:background">@android:drawable/editbox_background_normal</item>
    <item name="android:textColorHint">@android:color/darker_gray</item>
</style>
```

---

### **4. Tema CheckBox/RadioButton Holo**
```xml
<style name="HoloCheckBox" parent="@android:style/Widget.Holo.CompoundButton.CheckBox">
    <item name="android:button">@android:drawable/btn_check_holo_light</item>
</style>
```

---

### **5. Tema Switch Holo (API 14+)**
```xml
<style name="HoloSwitch" parent="@android:style/Widget.Holo.CompoundButton.Switch">
    <item name="android:thumb">@android:drawable/switch_thumb_holo_light</item>
    <item name="android:track">@android:drawable/switch_track_holo_light</item>
</style>
```

---

### **6. Tema ProgressBar Holo**
```xml
<style name="HoloProgressBar" parent="@android:style/Widget.Holo.ProgressBar.Horizontal">
    <item name="android:progressDrawable">@android:drawable/progress_horizontal_holo_light</item>
</style>
```

---

### **7. Tema Spinner (Dropdown) Holo**
```xml
<style name="HoloSpinner" parent="@android:style/Widget.Holo.Spinner">
    <item name="android:background">@android:drawable/spinner_background_holo_light</item>
    <item name="android:popupBackground">@android:drawable/spinner_dropdown_background_holo_light</item>
</style>
```

---

### **8. Tema Custom untuk CardView (Menggunakan `android:background`)**
```xml
<style name="HoloCard" parent="@android:style/Widget.Holo.Light">
    <item name="android:background">@android:drawable/dialog_holo_light_frame</item>
    <item name="android:elevation">4dp</item> <!-- Hanya bekerja di API 21+ -->
</style>
```

---

### **Catatan Penting:**
1. **Semua resource (`@android:drawable/...`, `@android:color/...`) adalah bawaan Android** dan tidak memerlukan library tambahan.
2. **Parent style (`Widget.Holo.*`) tersedia sejak API 14 (Android 4.0+)**.
3. Jika Anda menargetkan **Android versi lama (API 10 atau di bawahnya)**, ganti `Holo` dengan `DeviceDefault` atau `Theme.Light`.
   Contoh:
   ```xml
   <style name="LegacyButton" parent="@android:style/Widget.DeviceDefault.Button">
       <item name="android:background">@android:drawable/btn_default</item>
   </style>
   ```

4. **Untuk Android 5.0+ (API 21+)** tanpa AndroidX, Anda bisa menggunakan `Theme.Material`:
   ```xml
   <style name="MaterialButton" parent="@android:style/Widget.Material.Button">
       <item name="android:background">@android:color/holo_blue_dark</item>
   </style>
   ```

---

### **Contoh Penggunaan di Layout XML:**
```xml
<Button
    style="@style/ColoredHoloButton"
    android:text="Tombol Holo" />

<EditText
    style="@style/HoloEditText"
    android:hint="Input text" />

<CheckBox
    style="@style/HoloCheckBox"
    android:text="Centang saya" />
```

Dengan pendekatan ini, Anda tidak perlu menambahkan *AndroidX*, *Support Library*, atau resource eksternal.

---




# Default Style 

Jika Anda ingin menggunakan **Material Design tanpa AndroidX** (murni menggunakan resource bawaan Android framework), berikut adalah contoh tema dan style untuk widget berbasis **Theme.Material** (tersedia sejak API 21/Lollipop).  

---

### **1. Tema Dasar Material (Tanpa AndroidX)**  
```xml
<!-- themes.xml -->
<style name="AppTheme" parent="@android:style/Theme.Material.Light.DarkActionBar">
    <!-- Warna bawaan Android (Material) -->
    <item name="android:colorPrimary">@android:color/holo_blue_dark</item>
    <item name="android:colorPrimaryDark">@android:color/holo_blue_dark</item>
    <item name="android:colorAccent">@android:color/holo_green_light</item>
    <item name="android:colorControlHighlight">@android:color/holo_blue_light</item>
</style>
```

---

### **2. Style untuk Widget Material**  
#### **Tombol (Button)**  
```xml
<style name="MaterialButton" parent="@android:style/Widget.Material.Button">
    <item name="android:background">@android:color/holo_blue_light</item>
    <item name="android:textColor">@android:color/white</item>
    <item name="android:elevation">4dp</item>
</style>
```

#### **Tombol Borderless (Flat Button)**  
```xml
<style name="MaterialButton.Borderless" parent="@android:style/Widget.Material.Button.Borderless">
    <item name="android:textColor">@android:color/holo_blue_dark</item>
</style>
```

#### **EditText (TextInput)**  
```xml
<style name="MaterialEditText" parent="@android:style/Widget.Material.EditText">
    <item name="android:background">@android:drawable/edit_text_holo_light</item>
    <item name="android:textColorHint">@android:color/darker_gray</item>
</style>
```

#### **CheckBox & RadioButton**  
```xml
<style name="MaterialCheckBox" parent="@android:style/Widget.Material.CompoundButton.CheckBox">
    <item name="android:buttonTint">@android:color/holo_green_light</item>
</style>

<style name="MaterialRadioButton" parent="@android:style/Widget.Material.CompoundButton.RadioButton">
    <item name="android:buttonTint">@android:color/holo_purple</item>
</style>
```

#### **Switch (Material Style)**  
```xml
<style name="MaterialSwitch" parent="@android:style/Widget.Material.CompoundButton.Switch">
    <item name="android:thumbTint">@android:color/holo_orange_light</item>
    <item name="android:trackTint">@android:color/holo_blue_light</item>
</style>
```

#### **ProgressBar (Material Circular)**  
```xml
<style name="MaterialProgressBar" parent="@android:style/Widget.Material.ProgressBar">
    <item name="android:indeterminateTint">@android:color/holo_blue_light</item>
</style>
```

#### **CardView (Menggunakan FrameLayout dengan Elevation)**  
```xml
<style name="MaterialCard" parent="@android:style/Widget.Material.Light">
    <item name="android:background">@android:drawable/dialog_holo_light_frame</item>
    <item name="android:elevation">8dp</item>
    <item name="android:outlineProvider">bounds</item>
</style>
```

---

### **3. Contoh Penggunaan dalam Layout XML**  
```xml
<Button
    style="@style/MaterialButton"
    android:text="Tombol Material" />

<CheckBox
    style="@style/MaterialCheckBox"
    android:text="CheckBox Material" />

<EditText
    style="@style/MaterialEditText"
    android:hint="Input Material" />

<FrameLayout
    style="@style/MaterialCard"
    android:layout_width="match_parent"
    android:layout_height="100dp">
    <!-- Konten Card -->
</FrameLayout>
```

---

### **Catatan Penting:**  
1. **Hanya bekerja di API 21+ (Android 5.0+)** karena `Theme.Material` tidak tersedia di versi sebelumnya.  
2. **Warna (`@android:color/...`) dan drawable (`@android:drawable/...`) adalah bawaan Android**, tidak perlu library tambahan.  
3. **Jika butuh kompatibilitas dengan Android versi lama (pre-Lollipop), Anda harus menggunakan `Theme.Holo` atau `Theme.DeviceDefault`.**  
4. **Elevation & efek bayangan hanya bekerja di API 21+.**  

Jika Anda ingin **Material Design yang kompatibel dengan Android lama (API < 21) tanpa AndroidX**, Anda harus menggunakan **AppCompat (Support Library v7)**. Namun, jika benar-benar ingin menghindari library eksternal, batasi penggunaan hanya untuk **API 21+**.


---

Berikut adalah **style default bawaan Android** (tanpa AndroidX atau library eksternal) yang akan digunakan jika tidak ada custom style yang diatur:

---

### **1. Tema Default Berdasarkan Versi Android**
#### **Android 5.0+ (API 21+, Material Design)**
- **Light Theme:**  
  ```xml
  @android:style/Theme.Material.Light
  ```
- **Dark Theme:**  
  ```xml
  @android:style/Theme.Material
  ```
- **Light with Dark Action Bar:**  
  ```xml
  @android:style/Theme.Material.Light.DarkActionBar
  ```

#### **Android 4.0–4.4 (API 14–20, Holo Theme)**
- **Light Theme:**  
  ```xml
  @android:style/Theme.Holo.Light
  ```
- **Dark Theme:**  
  ```xml
  @android:style/Theme.Holo
  ```
- **Light with Dark Action Bar:**  
  ```xml
  @android:style/Theme.Holo.Light.DarkActionBar
  ```

#### **Android 3.0–3.2 (API 11–13, Early Holo)**
  ```xml
  @android:style/Theme.DeviceDefault
  ```

---

### **2. Style Default Widget (Tanpa Custom Style)**
#### **Button**
```xml
@android:style/Widget.Material.Button          (API 21+)
@android:style/Widget.Holo.Button              (API 14+)
@android:style/Widget.Button                   (API 1+)
```

#### **TextView**
```xml
@android:style/Widget.Material.TextView        (API 21+)
@android:style/Widget.Holo.TextView            (API 14+)
@android:style/Widget.TextView                 (API 1+)
```

#### **EditText**
```xml
@android:style/Widget.Material.EditText        (API 21+)
@android:style/Widget.Holo.EditText            (API 14+)
@android:style/Widget.EditText                 (API 1+)
```

#### **CheckBox/RadioButton**
```xml
@android:style/Widget.Material.CompoundButton.CheckBox  (API 21+)
@android:style/Widget.Holo.CompoundButton.CheckBox      (API 14+)
@android:style/Widget.CompoundButton.CheckBox           (API 1+)
```

#### **Switch**
```xml
@android:style/Widget.Material.CompoundButton.Switch    (API 21+)
@android:style/Widget.Holo.CompoundButton.Switch        (API 14+)
```

---

### **3. Warna Default Bawaan Android**
- **Primary Color:**  
  ```xml
  @android:color/background_light (Light Theme)  
  @android:color/background_dark (Dark Theme)
  ```
- **Accent Color:**  
  ```xml
  @android:color/holo_blue_light (umum digunakan)
  ```
- **Text Colors:**  
  ```xml
  @android:color/primary_text_light  
  @android:color/primary_text_dark
  ```

---

### **4. Contoh Tampilan Default**
#### **Tombol (Button)**
- **Material (API 21+):**  
  - Latar belakang: `?android:attr/selectableItemBackground` (ripple effect)  
  - Warna teks: `@android:color/primary_text_light/dark`  

- **Holo (API 14+):**  
  - Latar belakang: `@android:drawable/btn_default_holo_light/dark`  
  - Warna teks: `@android:color/primary_text_holo_light/dark`  

#### **EditText**
- **Material:**  
  - Garis bawah (underline) dengan animasi.  
- **Holo:**  
  - Kotak input dengan border (`@android:drawable/editbox_background_normal`).  

---

### **5. Cara Mengecek Style Default**
Jika Anda tidak mengatur style sama sekali di `themes.xml` atau `styles.xml`, Android akan menggunakan **parent theme sesuai versi OS**:
```xml
<!-- Di AndroidManifest.xml -->
<application android:theme="@android:style/Theme.Material.Light">
    <!-- Akan menggunakan style Material Light default -->
</application>
```

---

### **6. Catatan Penting**
1. **Tidak perlu definisi style manual** jika ingin menggunakan bawaan Android.  
2. **Perbedaan tampilan tergantung versi OS** (contoh: tombol di Android 5.0 akan punya ripple effect, sedangkan di Android 4.4 flat).  
3. **Jika ingin konsisten di semua versi tanpa AndroidX**, Anda harus menggunakan `Theme.DeviceDefault` (akan mengikuti tema sistem perangkat).  

Contoh penggunaan `DeviceDefault`:
```xml
<style name="AppTheme" parent="@android:style/Theme.DeviceDefault.Light.DarkActionBar">
    <!-- Tidak perlu definisi item, biarkan default -->
</style>
```

