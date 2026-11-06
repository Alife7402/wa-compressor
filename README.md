# 📱 WhatsApp 30 FPS Video Compressor (100% Client-Side FFmpeg)

Website statis kompresor video khusus pemutaran **Status WhatsApp** yang 100% berjalan murni di peramban pengguna (client-side) tanpa dependensi server, siap dideploy langsung ke **GitHub Pages**.

---

## 🛠️ Solusi Masalah Teknis

1. **Bebas Masalah Durasi 00:00 & Audio Hilang**:
   - Pendekatan lama (Canvas + MediaRecorder) menyebabkan header durasi rusak di HP (`00:00` / status "Diputar Selesai") dan audio yang bisu/hilang karena restriksi AudioContext Android.
   - Solusi: Menggunakan **FFmpeg.wasm v0.11.2** untuk encoding H.264 asli dan AAC 44.1kHz dengan flag `-movflags +faststart`.
2. **Bebas Error Cross-Origin Worker**:
   - FFmpeg v0.12 memecah worker ke chunk eksternal (`814.ffmpeg.js`) yang diblokir oleh Same-Origin Policy di GitHub Pages (`Failed to construct 'Worker'`).
   - Solusi: Menggunakan **FFmpeg.wasm v0.11.2** via CDN (`https://unpkg.com/@ffmpeg/ffmpeg@0.11.2/dist/ffmpeg.min.js`) dengan core `@ffmpeg/core@0.11.0` yang berupa single-bundle worker stabil tanpa konfigurasi COOP/COEP rumit.
3. **Standar Video WhatsApp Status**:
   - **Lock Frame Rate**: 30 FPS konstan (`fps=30`).
   - **Dimensi Genap**: `scale=trunc(iw/2)*2:trunc(ih/2)*2` agar libx264 tidak gagal.
   - **Video Codec**: `-c:v libx264 -preset ultrafast -crf 22 -pix_fmt yuv420p` (ringan dan hemat RAM HP).
   - **Audio Codec**: `-c:a aac -b:a 128k -ar 44100` (suara sinkron dan jernih).
   - **FastStart Atom MOOV**: `-movflags +faststart` (metadata durasi ditaruh di awal file sehingga langsung berputar di WhatsApp tanpa loading lama).

---

## 🚀 Cara Pasang di GitHub Pages (3 Langkah Cepat)

1. Buat repository baru di [GitHub](https://github.com/new) (misal: `wa-video-compressor`), set ke **Public**.
2. Unggah file `index.html` dan `README.md` ke repository tersebut, lalu klik **Commit changes**.
3. Buka tab **Settings** &rarr; **Pages** &rarr; pilih branch **main** dan folder **/ (root)** &rarr; klik **Save**.
4. Website aktif di: `https://<username>.github.io/<repo-name>/`.

---

## 📄 Lisensi
Lisensi MIT. Bebas digunakan dan dimodifikasi.
