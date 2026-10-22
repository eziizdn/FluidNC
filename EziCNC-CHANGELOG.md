# Changelog EziCNC

Semua perubahan terhadap FluidNC asli dicatat di sini. Dasar kode: FluidNC (GPL-3.0).

## v0.0.0-baseline (2026-10-22)

Rebranding tahap pertama: identitas firmware diubah dari FluidNC menjadi EziCNC.

### Ditambah / diubah

| File | Perubahan | Alasan |
|---|---|---|
| `platformio.ini` | Menambah `-DFW_BRAND=\"EziCNC\"` di `[env:wifi]` | Menentukan nama merek firmware saat kompilasi |
| `FluidNC/src/Main.cpp` | Baris log boot memakai macro `FW_BRAND` (sebelumnya tertulis sebagai teks `"FW_BRAND "`) | Pesan boot sebelumnya menampilkan kata `FW_BRAND` secara harfiah, bukan nama merek |
| `FluidNC/src/SettingsDefinitions.cpp` | Pesan startup (`Start/Message`) memakai `FW_BRAND` | Baris `Grbl 0.0 [EziCNC ...]` menampilkan nama baru |
| `FluidNC/src/WebUI/WifiConfig.cpp` | Nama default WiFi access point diganti ke `CNC ESP32` | Nama WiFi yang terlihat pengguna |
| `FluidNC/src/WebUI/NetSettings.cpp` | Hostname default diganti ke `ezicnc` | Nama perangkat di jaringan |
| `FluidNC/src/BTConfig.h` | Nama Bluetooth default diganti ke `EziCNC` | Nama perangkat Bluetooth (berlaku untuk build `bt`) |

### Dirapikan

- `platformio.ini`: satu baris komentar duplikat (`;WiFi=...#canWrite`) di `wifi_deps` dihapus. Tidak memengaruhi build.
- `lib/AsyncTCP`, `lib/ESPAsyncWebServer`, `lib/JsonStreamingParser`: disalin ke folder `lib/` (commit `31cf0828`). Di `lib/ESPAsyncWebServer/library.json`, blok `"dependencies"` dihapus supaya PlatformIO memakai salinan lokal.

### Sengaja tidak diubah

- `FWTarget` bernilai `"FluidNC"` di `NetConfig.cpp` dan `WifiConfig.cpp`. Dibaca WebUI untuk mengenali jenis firmware. Diubah hanya jika WebUI ikut disesuaikan.

### Hasil pengujian

- Build `pio run -e wifi`: SUCCESS (build dari nol, sekitar 4 menit). Flash 94,2% (1852801 dari 1966080 byte), RAM 21,1%.
- Upload ke ESP32 (COM12): firmware berjalan. Log boot menampilkan `EziCNC v0.0.0-baseline (...)`, dan WiFi access point tampil dengan nama baru.
- Pesan `Cannot open configuration file: config.yaml` dan `Configuration is invalid` pada board baru adalah perilaku normal FluidNC sebelum ada file konfigurasi mesin.
