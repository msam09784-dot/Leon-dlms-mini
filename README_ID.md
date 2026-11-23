# LEON AUDIO DLMS Android BLE — v0.2.0

Project aplikasi Android native menggunakan Kotlin + Jetpack Compose. Merek aplikasi: **LEON AUDIO** (by Leon Production).

## Isi aplikasi
- HOME: status koneksi, ringkasan DSP 2 IN / 4 OUT, signal flow, dan master mute.
- INPUT: IN1/IN2, gain, dan delay.
- OUTPUT: OUT1–OUT4, gain, phase reverse, mute, HPF, LPF, dan delay.
- DSP → EQ: 9-band EQ, pemilihan Input/Output, frequency, gain, dan Q.
- DSP → X-OVER: Linkwitz-Riley, Butterworth, Bessel; slope 12/24/36/48 dB/oct; HPF/LPF.
- DSP → DELAY, COMP, MATRIX, RTA (tampilan demo), dan SYSTEM.
- PRESET: 5 slot P1–P5 dengan SAVE / LOAD / RESET.
- BLE scanner/GATT client dengan UUID di `BLE_PROTOCOL.md`.

## Membuat APK langsung dari HP Android (AndroidIDE)
1. Pasang AndroidIDE dari sumber tepercaya yang tercantum di situs/dokumentasi resminya.
2. Siapkan minimal 4 GB ruang kosong dan koneksi Wi-Fi. Dokumentasi AndroidIDE menyarankan RAM bebas sekitar 1,5–2 GB.
3. Buka AndroidIDE dan selesaikan setup build tools. Pada terminal AndroidIDE, dokumentasi menyediakan perintah `idesetup -c` untuk memasang JDK dan Android SDK.
4. Ekstrak ZIP project ini ke penyimpanan HP, misalnya folder `Download/LEON_AUDIO_DLMS_Android_BLE`.
5. Di AndroidIDE pilih **Open existing project**, lalu pilih folder `LEON_AUDIO_DLMS_Android_BLE` (folder yang berisi `settings.gradle.kts`, bukan folder `app`).
6. Tunggu Gradle Sync selesai. Pastikan HP tersambung internet karena Gradle mungkin perlu mengunduh plugin dan library.
7. Pilih **Build / Run** untuk membangun dan memasang aplikasi. Untuk file APK, gunakan menu build APK yang tersedia di versi AndroidIDE yang dipasang.
8. Setelah APK berhasil dibuat, biasanya file debug berada di `app/build/outputs/apk/debug/app-debug.apk`. Lokasi akhir bisa berbeda menurut versi IDE.
9. Buka APK tersebut, izinkan pemasangan dari sumber ini bila Android meminta, lalu pasang.

## Catatan kompatibilitas AndroidIDE
Project memakai Android Gradle Plugin 8.7.3, Kotlin 2.0.21, Compose, dan JDK 17. AndroidIDE mendokumentasikan dukungan proyek Android Gradle Plugin 7.2.0 atau lebih baru. Namun, AndroidIDE pernah diarsipkan di GitHub dan rilis yang tersedia bersifat beta, jadi keberhasilan build pada HP tidak dijamin. Bila muncul error Gradle/SDK, kirim screenshot error untuk diperiksa.

## Status fungsi BLE — penting
Firmware referensi `LEON_AUDIO_DLMS_MINI_v0.9.4_BOOT_LOGO_ONLY.ino` yang disertakan **belum memiliki BLE/GATT server** yang cocok dengan aplikasi ini. Jadi APK dapat dipakai untuk melihat/menguji UI demo, tetapi belum bisa mengubah DSP/audio fisik sampai layanan BLE ditambahkan ke firmware ESP32-S3 dan setiap perintah aplikasi dihubungkan ke parameter DSP.

## Versi dan spesifikasi
- App version: 0.2.0
- Package: `com.leonaudio.dlms`
- Minimum Android: 8.0 (API 26)
- Target SDK: 35
- ZIP ini adalah **source project**, bukan APK siap pasang.
