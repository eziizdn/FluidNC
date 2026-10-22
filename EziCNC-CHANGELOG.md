# Changelog

Semua perubahan terhadap FluidNC asli dicatat di sini. Dasar kode: FluidNC (GPL-3.0).

## Belum dirilis (2026-10-22)

Rebranding: identitas firmware diubah dari FluidNC menjadi ESP32 CNC.

### Ditambah / diubah

| File | Perubahan | Alasan |
|---|---|---|
| `FluidNC/src/Config.h` | Menambah `#define FW_BRAND "ESP32 CNC"` | Satu tempat untuk nama merek. Nama berspasi tidak aman dikirim lewat build flag, jadi dipindah ke kode |
| `platformio.ini` | Baris `-DFW_BRAND=...` di `[env:wifi]` dihapus | Menghindari dua definisi `FW_BRAND` yang bertabrakan |
| `FluidNC/src/Main.cpp` | Baris log boot memakai macro `FW_BRAND` (sebelumnya tertulis sebagai teks `"FW_BRAND "`) | Pesan boot sebelumnya menampilkan kata `FW_BRAND` secara harfiah |
| `FluidNC/src/SettingsDefinitions.cpp` | Pesan startup (`Start/Message`) memakai `FW_BRAND` | Baris `Grbl 0.0 [ESP32 CNC ...]` menampilkan nama baru |
| `FluidNC/src/WebUI/WifiConfig.cpp` | Nama default WiFi access point diganti ke `CNC ESP32` | Nama WiFi yang terlihat pengguna |
| `FluidNC/src/WebUI/NetSettings.cpp` | Hostname default diganti ke `esp32-cnc` | Nama perangkat di jaringan (hostname tidak boleh berspasi) |
| `FluidNC/src/BTConfig.h` | Nama Bluetooth default diganti ke `ESP32 CNC` | Nama perangkat Bluetooth (berlaku untuk build `bt`) |
| `FluidNC/data/index.html.gz` | Judul WebUI menjadi `ESP32 CNC` (teks `ESP3D for` dihapus), dua teks nama merek diganti. Ukuran 111.625 menjadi 112.386 byte | Judul sesuai merek baru, dan nama di WebUI cocok dengan `FW_BRAND` |

### Ketergantungan firmware dan WebUI

WebUI mengirim `$SS` lalu mencari baris log yang diawali `[MSG:INFO: <nama merek>`. Karena itu nama di `index.html.gz` harus sama persis dengan `FW_BRAND`. Kalau berbeda, kotak "Config File Error" tidak muncul. Saat nama diganti, firmware dan WebUI diubah bersamaan.

### Dirapikan

- `platformio.ini`: satu baris komentar duplikat (`;WiFi=...#canWrite`) di `wifi_deps` dihapus. Tidak memengaruhi build.
- `lib/AsyncTCP`, `lib/ESPAsyncWebServer`, `lib/JsonStreamingParser`: disalin ke folder `lib/` (commit `31cf0828`). Di `lib/ESPAsyncWebServer/library.json`, blok `"dependencies"` dihapus supaya PlatformIO memakai salinan lokal. File ini dimodifikasi dari aslinya (LGPL-3.0).

### Sengaja tidak diubah

- `FWTarget` bernilai `"FluidNC"` di `NetConfig.cpp` dan `WifiConfig.cpp`. Dibaca WebUI untuk mengenali jenis firmware.
- Nama fungsi JavaScript `setupFluidNC` di WebUI.
- Teks tampilan WebUI yang masih menyebut "FluidNC" (tab "FluidNC", subjudul "Fix it or FluidNC will not work", dan lainnya; total 21 kemunculan kata di file, sebagian besar belum dipilah). Direncanakan untuk tahap berikutnya.

### Hasil pengujian

- Build `pio run -e wifi`: SUCCESS.
- Upload ke ESP32 (COM12): log boot menampilkan `Grbl 0.0 [ESP32 CNC v0.0.0-baseline (ezi-rebrand-39e63b01) (esp32-wifi) ...]`, dan WiFi access point tampil sebagai `CNC ESP32`.
- WebUI di `http://192.168.0.1`: judul `ESP32 CNC`, dan kotak "Config File Error" muncul karena board belum punya `config.yaml` (bukti nama di WebUI cocok dengan firmware).
- Pesan `Cannot open configuration file: config.yaml` dan `Configuration is invalid` pada board baru adalah perilaku normal FluidNC sebelum ada file konfigurasi mesin.
- Hostname: perintah `$Hostname` di serial monitor menampilkan `esp32-cnc`.

### Belum diuji di board

- Nama Bluetooth `ESP32 CNC` (hanya berlaku untuk build `bt`, belum dibangun).

## Riwayat sebelumnya

Tahap pertama memakai nama `EziCNC` (nama firmware, nama Bluetooth, hostname `ezicnc`) lewat flag `-DFW_BRAND` di `platformio.ini`. Digantikan oleh perubahan di atas.