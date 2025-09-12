# Panduan Komprehensif Modul `subprocess` Python

## Daftar Isi
1. [Pengenalan Modul Subprocess](#pengenalan-modul-subprocess)
2. [Perbandingan dengan `os.system()`](#perbandingan-dengan-ossystem)
3. [Metode Utama](#metode-utama)
4. [Parameter Penting](#parameter-penting)
5. [Penanganan Input/Output](#penanganan-inputoutput)
6. [Penanganan Error](#penanganan-error)
7. [Praktik Keamanan](#praktik-keamanan)
8. [Shell=True vs Shell=False](#shelltrue-vs-shellfalse)
9. [Timeout dan Pembatalan Proses](#timeout-dan-pembatalan-proses)
10. [Komunikasi dengan Proses Berjalan](#komunikasi-dengan-proses-berjalan)
11. [Pertimbangan Platform](#pertimbangan-platform)
12. [Best Practices](#best-practices)

## Pengenalan Modul Subprocess

Modul `subprocess` dalam Python memungkinkan Anda untuk membuat proses baru, terhubung ke pipa input/output/error mereka, dan mendapatkan kode return mereka. Ini adalah pengganti yang lebih kuat untuk beberapa fungsi lama seperti `os.system()` dan `os.spawn*()`.

Kegunaan utama:
- Menjalankan perintah sistem eksternal
- Berinteraksi dengan proses lain
- Mengotomatisasi tugas-tugas sistem
- Mengintegrasikan alat eksternal ke dalam aplikasi Python

Contoh dasar:
```python
import subprocess

# Menjalankan perintah sederhana
result = subprocess.run(['ls', '-l'], capture_output=True, text=True)
print(result.stdout)
```

## Perbandingan dengan `os.system()`

`os.system()` adalah cara lama untuk menjalankan perintah shell dengan beberapa keterbatasan:
- Tidak bisa menangkap output
- Bergantung pada shell sistem
- Tidak fleksibel dalam penanganan proses

Keunggulan `subprocess`:
- Kontrol lebih besar atas proses
- Bisa menangkap output/stdin/stderr
- Tidak bergantung pada shell (kecuali jika diinginkan)
- Fleksibilitas dalam penanganan pipa

Contoh perbandingan:
```python
# Cara lama dengan os.system()
import os
os.system('ls -l > output.txt')  # Tidak bisa membaca output langsung

# Cara baru dengan subprocess
import subprocess
result = subprocess.run(['ls', '-l'], capture_output=True, text=True)
print(result.stdout)  # Output langsung tersedia
```

## Metode Utama

### `subprocess.run()`
Metode level tinggi yang direkomendasikan untuk kebanyakan kasus penggunaan.

```python
result = subprocess.run(['echo', 'Hello World'], capture_output=True, text=True)
print(result.returncode)  # 0 berarti sukses
print(result.stdout)      # 'Hello World\n'
```

### `subprocess.call()`
Mirip dengan `run()` tetapi hanya mengembalikan kode return (Python 3.5+, lebih baik gunakan `run()`).

```python
return_code = subprocess.call(['ls', '-l'])
print(return_code)
```

### `subprocess.check_call()`
Seperti `call()` tetapi memunculkan exception jika return code bukan 0.

```python
try:
    subprocess.check_call(['false'])
except subprocess.CalledProcessError as e:
    print(f"Command failed with return code {e.returncode}")
```

### `subprocess.check_output()`
Seperti `check_call()` tetapi mengembalikan output.

```python
output = subprocess.check_output(['echo', 'Hello World'], text=True)
print(output)  # 'Hello World\n'
```

### `subprocess.Popen()`
API level rendah untuk kontrol lebih besar atas proses.

```python
process = subprocess.Popen(['ping', '-c', '4', 'google.com'], stdout=subprocess.PIPE, text=True)
output, errors = process.communicate()
print(output)
```

## Parameter Penting

### `shell`
Menentukan apakah perintah dijalankan melalui shell.

```python
# Tanpa shell (direkomendasikan)
subprocess.run(['ls', '-l'])

# Dengan shell (hati-hati dengan security!)
subprocess.run('ls -l', shell=True)
```

### `capture_output`
Menangkap stdout dan stderr.

```python
result = subprocess.run(['ls'], capture_output=True, text=True)
print(result.stdout)
```

### `text` (alias `universal_newlines`)
Menangani input/output sebagai string bukan bytes.

```python
result = subprocess.run(['echo', 'hello'], capture_output=True, text=True)
print(result.stdout)  # 'hello\n'
```

### `cwd`
Menjalankan perintah di direktori tertentu.

```python
subprocess.run(['ls'], cwd='/tmp')
```

### `env`
Menentukan environment variables untuk proses.

```python
custom_env = {'MY_VAR': 'value', 'PATH': '/usr/bin'}
subprocess.run(['env'], env=custom_env)
```

### `timeout`
Membatalkan proses setelah waktu tertentu.

```python
try:
    subprocess.run(['sleep', '10'], timeout=5)
except subprocess.TimeoutExpired:
    print("Process timed out!")
```

## Penanganan Input/Output

### Redirect stdin, stdout, stderr

```python
# Menulis ke stdin proses
result = subprocess.run(['grep', 'python'], input='python is great\njava is ok\n', capture_output=True, text=True)
print(result.stdout)  # 'python is great\n'

# Redirect stdout ke file
with open('output.txt', 'w') as f:
    subprocess.run(['ls', '-l'], stdout=f)

# Menggabungkan stderr ke stdout
result = subprocess.run(['ls', 'nonexistent'], stdout=subprocess.PIPE, stderr=subprocess.STDOUT, text=True)
print(result.stdout)
```

### Pipes antara proses

```python
# Pipe output dari satu proses ke proses lain
ps = subprocess.Popen(['ps', '-A'], stdout=subprocess.PIPE)
grep = subprocess.Popen(['grep', 'python'], stdin=ps.stdout, stdout=subprocess.PIPE, text=True)
ps.stdout.close()  # Memungkinkan ps menerima SIGPIPE jika grep keluar
output = grep.communicate()[0]
print(output)
```

## Penanganan Error

### Exception yang umum

```python
try:
    subprocess.run(['false'], check=True)
except subprocess.CalledProcessError as e:
    print(f"Error: {e}")

try:
    subprocess.run(['sleep', '10'], timeout=1)
except subprocess.TimeoutExpired:
    print("Timeout!")

try:
    subprocess.run(['nonexistent_command'])
except FileNotFoundError as e:
    print(f"Command not found: {e}")
```

### Memeriksa return code

```python
result = subprocess.run(['ls', 'nonexistent_file'])
if result.returncode != 0:
    print(f"Command failed with code {result.returncode}")
```

## Praktik Keamanan

### Command injection

**Tidak aman:**
```python
user_input = '; rm -rf /'  # Input berbahaya
subprocess.run(f'echo {user_input}', shell=True)  # Bencana!
```

**Aman:**
```python
user_input = '; rm -rf /'
subprocess.run(['echo', user_input])  # Aman karena tidak melalui shell
```

### Rekomendasi keamanan

1. Hindari `shell=True` ketika memungkinkan
2. Jika harus menggunakan `shell=True`, gunakan `shlex.quote()` untuk input
3. Validasi semua input pengguna
4. Gunakan daftar argumen bukan string tunggal

Contoh aman dengan shell:
```python
import shlex

user_input = '; rm -rf /'
safe_input = shlex.quote(user_input)
subprocess.run(f'echo {safe_input}', shell=True)  # Sekarang aman
```

## Shell=True vs Shell=False

### `shell=False` (default, direkomendasikan)
- Lebih aman
- Tidak memerlukan escaping argumen
- Lebih efisien
- Langsung menjalankan program tanpa shell perantara

```python
subprocess.run(['ls', '-l', '*.py'])  # Globbing tidak bekerja
```

### `shell=True`
- Diperlukan untuk fitur shell seperti wildcard, pipes, dll.
- Lebih berisiko untuk command injection
- Lebih lambat karena melibatkan shell

```python
subprocess.run('ls -l *.py', shell=True)  # Globbing bekerja
```

## Timeout dan Pembatalan Proses

### Timeout dengan `run()`

```python
try:
    subprocess.run(['sleep', '10'], timeout=2)
except subprocess.TimeoutExpired:
    print("Process took too long!")
```

### Membatalkan proses dengan `Popen()`

```python
process = subprocess.Popen(['ping', 'google.com'])
try:
    process.wait(timeout=5)
except subprocess.TimeoutExpired:
    process.terminate()  # Mengirim SIGTERM
    process.wait()       # Pastikan proses benar-benar berhenti
    print("Process terminated")
```

## Komunikasi dengan Proses Berjalan

### Menggunakan `Popen.communicate()`

```python
process = subprocess.Popen(['grep', 'python'], stdin=subprocess.PIPE, stdout=subprocess.PIPE, text=True)
output, errors = process.communicate(input='python is great\njava is ok\n')
print(output)  # 'python is great\n'
```

### Interaktif dengan proses

```python
import time

process = subprocess.Popen(['python3', '-i'], stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)

# Kirim perintah
process.stdin.write('print(2+2)\n')
process.stdin.flush()
time.sleep(0.1)  # Beri waktu untuk pemrosesan

# Baca output
output = process.stdout.read(1024)
print(output)  # '4\n>>> '
```

## Pertimbangan Platform

### Perbedaan Windows vs Unix

1. **Path executables**:
   ```python
   # Cross-platform
   subprocess.run(['python', 'script.py'])
   
   # Platform-specific
   if os.name == 'nt':
       subprocess.run(['python.exe', 'script.py'])
   else:
       subprocess.run(['python3', 'script.py'])
   ```

2. **Perintah shell**:
   ```python
   # Windows
   subprocess.run('dir', shell=True)
   
   # Unix
   subprocess.run('ls', shell=True)
   ```

3. **Environment variables**:
   ```python
   # Cross-platform
   env = {'PATH': '/usr/bin' if os.name != 'nt' else 'C:\\Windows\\System32'}
   subprocess.run(['command'], env=env)
   ```

### Contoh cross-platform

```python
import os
import subprocess

def open_file_explorer(path):
    if os.name == 'nt':
        subprocess.run(['explorer', path])
    elif os.name == 'posix':
        subprocess.run(['xdg-open', path])
    else:
        subprocess.run(['open', path])  # macOS
```

## Best Practices

1. **Gunakan `subprocess.run()` untuk kebanyakan kasus**:
   ```python
   # Direkomendasikan
   result = subprocess.run(['ls', '-l'], capture_output=True, text=True)
   ```

2. **Hindari `shell=True` kecuali diperlukan**:
   ```python
   # Buruk
   subprocess.run('ls -l', shell=True)
   
   # Baik
   subprocess.run(['ls', '-l'])
   ```

3. **Tangkap dan proses output dengan benar**:
   ```python
   result = subprocess.run(['ls'], capture_output=True, text=True)
   if result.returncode == 0:
       print(result.stdout)
   else:
       print(result.stderr)
   ```

4. **Selalu gunakan timeout untuk proses panjang**:
   ```python
   try:
       subprocess.run(['long_running_task'], timeout=3600)
   except subprocess.TimeoutExpired:
       print("Process took too long")
   ```

5. **Gunakan `check=True` untuk memastikan sukses**:
   ```python
   try:
       subprocess.run(['important_command'], check=True)
   except subprocess.CalledProcessError:
       print("Command failed - handle appropriately")
   ```

6. **Bersihkan proses yang menggantung**:
   ```python
   process = subprocess.Popen(['some_process'])
   try:
       process.wait(timeout=10)
   except subprocess.TimeoutExpired:
       process.terminate()
       process.kill()  # Jika terminate tidak cukup
   ```

7. **Gunakan `text=True` untuk string bukan bytes**:
   ```python
   result = subprocess.run(['echo', 'hello'], capture_output=True, text=True)
   print(result.stdout)  # String, bukan bytes
   ```

8. **Untuk banyak proses, pertimbangkan `asyncio` atau threading**:
   ```python
   import asyncio
   
   async def run_commands():
       proc1 = await asyncio.create_subprocess_exec('cmd1')
       proc2 = await asyncio.create_subprocess_exec('cmd2')
       await asyncio.gather(proc1.wait(), proc2.wait())
   
   asyncio.run(run_commands())
   ```

## Contoh Praktis

### 1. Menjalankan pipa shell dengan aman

```python
import subprocess

# Tanpa shell (direkomendasikan)
ps = subprocess.Popen(['ps', '-A'], stdout=subprocess.PIPE)
grep = subprocess.Popen(['grep', 'python'], stdin=ps.stdout, stdout=subprocess.PIPE, text=True)
ps.stdout.close()  # Memungkinkan ps menerima SIGPIPE jika grep keluar
output = grep.communicate()[0]
print(output)

# Dengan shell (jika benar-benar diperlukan)
cmd = 'ps -A | grep python'
safe_cmd = 'ps -A | grep python'  # Asumsikan sudah divalidasi
subprocess.run(safe_cmd, shell=True, check=True)
```

### 2. Backup database dengan timestamp

```python
import subprocess
import time
import os

timestamp = time.strftime('%Y%m%d-%H%M%S')
backup_file = f'db_backup_{timestamp}.sql'

try:
    subprocess.run(
        ['pg_dump', '-U', 'user', 'mydb', '-f', backup_file],
        check=True,
        env={**os.environ, 'PGPASSWORD': 'secret'}
    )
    print(f"Backup successful: {backup_file}")
except subprocess.CalledProcessError as e:
    print(f"Backup failed: {e}")
```

### 3. Monitor proses dengan timeout

```python
import subprocess
import shlex

def run_with_timeout(cmd, timeout):
    try:
        result = subprocess.run(
            shlex.split(cmd),
            capture_output=True,
            text=True,
            timeout=timeout
        )
        return (True, result.stdout)
    except subprocess.TimeoutExpired:
        return (False, f"Command timed out after {timeout} seconds")
    except subprocess.Called