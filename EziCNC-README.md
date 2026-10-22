# ESP32 CNC (fork dari FluidNC)

Proyek ini adalah **fork dari [FluidNC](https://github.com/bdring/FluidNC)**, firmware kontrol CNC untuk ESP32 yang dibuat oleh Mitch Bradley dan kontributor FluidNC. Dibuat sebagai tugas rebranding firmware (PBL, Politeknik Negeri Batam): dasarnya tetap FluidNC, dengan identitas dan tampilan yang disesuaikan menjadi milik kami, dan setiap perubahan tercatat supaya bisa ditelusuri.

Repositori ini: https://github.com/eziizdn/FluidNC (branch `ezi-rebrand`)

## Apa yang berbeda dari FluidNC asli

| Bagian | FluidNC asli | Proyek ini |
|---|---|---|
| Nama firmware (versi dan pesan boot) | FluidNC | ESP32 CNC |
| Judul WebUI (pojok kiri atas) | ESP3D for FluidNC | ESP32 CNC |
| Nama WiFi access point bawaan | FluidNC | CNC ESP32 |
| Hostname bawaan | fluidnc | esp32-cnc |
| Nama Bluetooth bawaan | FluidNC | ESP32 CNC |

Daftar lengkap perubahan, file yang diubah, dan alasannya ada di [EziCNC-CHANGELOG.md](EziCNC-CHANGELOG.md).

## Aturan penting: nama di firmware dan WebUI harus sama

Nama firmware didefinisikan satu kali di `FluidNC/src/Config.h`:

```cpp
#define FW_BRAND "ESP32 CNC"
```

WebUI (`FluidNC/data/index.html.gz`) mengirim perintah `$SS`, lalu mencari baris log yang diawali `[MSG:INFO: <nama>`. Kalau nama di WebUI berbeda dengan `FW_BRAND`, kotak peringatan "Config File Error" tidak akan muncul saat file konfigurasi bermasalah. Jadi kalau nama diganti, **firmware dan WebUI harus diubah bersamaan**.

## Bagian yang sengaja tidak diubah

- Penanda `FWTarget` ("FluidNC") di `NetConfig.cpp` dan `WifiConfig.cpp`. WebUI memakainya untuk mengenali jenis firmware, jadi mengubahnya tanpa menyesuaikan WebUI dapat merusak fitur.
- Beberapa teks tampilan di WebUI yang masih bertuliskan "FluidNC", misalnya tab "FluidNC" dan subjudul "Fix it or FluidNC will not work". Ini rencana tahap berikutnya.

## Cara build

Proyek memakai [PlatformIO](https://platformio.org/). Selalu sebutkan environment-nya (`-e wifi`).

```
pio run -e wifi
```

Upload ke board ESP32 lewat USB, lalu lihat log boot:

```
pio run -e wifi -t upload
pio device monitor -b 115200
```

Baris boot akan menampilkan nama dan versi:

```
Grbl 0.0 [ESP32 CNC v0.0.0-baseline (...) (esp32-wifi) '$' for help]
```

## Mengubah WebUI

WebUI berupa satu file terkompresi (`FluidNC/data/index.html.gz`). Untuk mengeditnya: bongkar dengan `GzipStream`, ganti teks pada salinan, kompres lagi, lalu upload lewat dialog "FluidNC Local Filesystem" di WebUI. Jangan memakai `pio run -t uploadfs`, karena perintah itu meng-upload seluruh folder `data` (termasuk `config.yaml`) dan menimpa isi LittleFS.

## Lisensi dan kredit

FluidNC dirilis di bawah **GNU General Public License v3.0**. Karena proyek ini adalah karya turunan, ia juga berlisensi GPL-3.0. File `LICENSE` dari FluidNC tetap disertakan dan tidak diubah. Library `AsyncTCP` dan `ESPAsyncWebServer` yang disalin ke folder `lib/` berlisensi LGPL-3.0.

WebUI berasal dari proyek ESP3D-WebUI yang dipakai FluidNC. Seluruh kredit untuk kode dasar firmware dan WebUI milik pembuat dan kontributor aslinya. Kontribusi proyek ini terbatas pada modifikasi yang tercatat di changelog.