# EziCNC

EziCNC adalah **fork dari [FluidNC](https://github.com/bdring/FluidNC)**, firmware kontrol CNC untuk ESP32 yang dibuat oleh Mitch Bradley dan kontributor FluidNC. Proyek ini dibuat sebagai tugas rebranding firmware: dasarnya tetap FluidNC, dengan identitas dan tampilan yang disesuaikan menjadi milik kami.

Repositori ini: https://github.com/eziizdn/FluidNC (branch `ezi-rebrand`)

## Apa yang berbeda dari FluidNC asli

| Bagian | FluidNC asli | EziCNC |
|---|---|---|
| Nama firmware (versi dan pesan boot) | FluidNC | EziCNC |
| Nama WiFi access point bawaan | FluidNC | CNC ESP32 |
| Hostname bawaan | fluidnc | ezicnc |
| Nama Bluetooth bawaan | FluidNC | EziCNC |

Daftar lengkap perubahan, file yang diubah, dan alasannya ada di [EziCNC-CHANGELOG.md](EziCNC-CHANGELOG.md).

Bagian yang sengaja **tidak** diubah: penanda `FWTarget` ("FluidNC") di `NetConfig.cpp` dan `WifiConfig.cpp`. Penanda ini dibaca WebUI untuk mengenali jenis firmware, jadi mengubahnya tanpa menyesuaikan WebUI dapat merusak fitur.

## Cara build

Proyek memakai [PlatformIO](https://platformio.org/).

```
pio run -e wifi
```

Untuk upload ke board ESP32 lewat USB:

```
pio run -e wifi -t upload
pio device monitor -b 115200
```

Setelah board menyala, baris boot akan menampilkan nama dan versi:

```
Grbl 0.0 [EziCNC v0.0.0-baseline (...) (esp32-wifi) '$' for help]
```

## Lisensi dan kredit

FluidNC dirilis di bawah **GNU General Public License v3.0**. Karena EziCNC adalah karya turunan, proyek ini juga berlisensi GPL-3.0. File `LICENSE` dari FluidNC tetap disertakan dan tidak diubah.

Seluruh kredit untuk kode dasar firmware ini milik pembuat dan kontributor FluidNC asli. Kontribusi EziCNC terbatas pada modifikasi yang tercatat di changelog.
