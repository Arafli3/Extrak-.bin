# Extrak-.bin
upload file ke vps /root/R01FInject.deb

Install
<pre><code>apt update
apt install binutils</code></pre>

buat nama di /root/: repack-deb-ar.sh

masukan kode berikut
<pre><code>#!/bin/bash

# Pastikan file .deb diberikan
if [ -z "$1" ]; then
  echo "Contoh penggunaan: ./repack-deb.sh paketmu.deb"
  exit 1
fi

DEB_FILE="$1"
BASENAME=$(basename "$DEB_FILE" .deb)
WORK_DIR="deb-ar-work"
REPACKED_FILE="repacked-${BASENAME}.deb"

# Bersihkan folder kerja
rm -rf "$WORK_DIR"
mkdir -p "$WORK_DIR"

# Ekstrak .deb menggunakan ar
cd "$WORK_DIR"
ar x ../"$DEB_FILE"
if [ $? -ne 0 ]; then
  echo "Gagal mengekstrak .deb menggunakan ar"
  exit 1
fi

# Ekstrak control dan data archive
mkdir control data
tar -xf control.tar.* -C control
tar -xf data.tar.* -C data

echo "[+] File diekstrak ke $WORK_DIR/control dan $WORK_DIR/data"
echo "[+] Silakan edit file di dalam folder tersebut."
echo "Tekan ENTER jika sudah selesai mengedit..."
read

# Rekompres control dan data
rm control.tar.* data.tar.*

tar -czf control.tar.gz -C control .
tar -czf data.tar.gz -C data .

# Buat ulang .deb dengan ar
echo "2.0" > debian-binary
ar rcs "../$REPACKED_FILE" debian-binary control.tar.gz data.tar.gz

cd ..
echo "[✓] Berhasil membuat $REPACKED_FILE dengan debian-binary"
</code></pre>
jalankan perintah
<pre><code>./repack-deb-ar.sh R01FInject.deb</code></pre>
