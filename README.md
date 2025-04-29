# Extrak-.bin
Untuk mengekstrak file .deb di Linux tanpa menginstalnya, kamu bisa menggunakan perintah dpkg-deb atau alat lain seperti ar. Berikut beberapa cara:

1. Menggunakan dpkg-deb
<pre><code>
dpkg-deb -x nama_file.deb nama_folder_tujuan
</code></pre>
Contoh:
<pre><code>
dpkg-deb -x paketku.deb ./output
</code></pre>
Ini akan mengekstrak isi paket ke folder ./output.

2. Menggunakan ar dan tar

File .deb sebenarnya adalah file arsip standar Unix. Kamu bisa mengekstraknya seperti ini:
<pre><code>
ar x nama_file.deb
</code></pre>
Ini akan menghasilkan tiga file utama:

control.tar.gz (informasi kontrol/paket)

data.tar.gz atau data.tar.xz (isi file sesungguhnya)

debian-binary (versi format .deb)


Setelah itu, ekstrak data.tar.*:
<pre><code>
tar -xf data.tar.gz
</code></pre>
Atau jika xz:
<pre><code>
tar -xf data.tar.xz
</code></pre>
Ini akan mengekstrak file aplikasi ke direktori saat ini.

Perlu bantuan mengekstrak bagian kontrol juga?

Sebelumnya instal dulu
<pre><code>apt update
apt install binutils</code></pre>
buat nama: edit_and_repack_deb.sh

masukan kode berikut
<pre><code>#!/bin/bash

# Nama file .deb
DEB_FILE="R01FInject.deb"
WORKDIR="extract_deb"
OUTPUT_DEB="R01FInject_modified.deb"

# Step 1: Cek file .deb
if [ ! -f "$DEB_FILE" ]; then
    echo "File $DEB_FILE tidak ditemukan!"
    exit 1
fi

# Step 2: Bersihkan dan buat folder kerja
rm -rf "$WORKDIR"
mkdir "$WORKDIR"
cd "$WORKDIR" || exit 1

# Step 3: Ekstrak isi .deb
ar x "../$DEB_FILE"

# Step 4: Ekstrak control.tar.*
mkdir control
if [ -f control.tar.xz ]; then
    tar -xf control.tar.xz -C control
elif [ -f control.tar.gz ]; then
    tar -xzf control.tar.gz -C control
else
    echo "File control.tar.* tidak ditemukan!"
    exit 1
fi

# Step 5: Ekstrak data.tar.*
mkdir data
if [ -f data.tar.xz ]; then
    tar -xf data.tar.xz -C data
elif [ -f data.tar.gz ]; then
    tar -xzf data.tar.gz -C data
else
    echo "File data.tar.* tidak ditemukan!"
    exit 1
fi

echo ""
echo "=== Ekstraksi selesai ==="
echo "Folder 'control/' dan 'data/' sudah tersedia untuk diedit."
echo ""

# Step 6: Pause untuk edit manual
read -p "Tekan ENTER setelah selesai mengedit..."

# Step 7: Packing ulang control.tar.gz dan data.tar.gz
echo "Mempacking ulang..."

# Buang file .tar.gz lama
rm -f control.tar.* data.tar.*

# Repack control
tar --owner=0 --group=0 -czf control.tar.gz -C control .

# Repack data
tar --owner=0 --group=0 -czf data.tar.gz -C data .

# Step 8: Buat file .deb baru
rm -f "../$OUTPUT_DEB"
ar rcs "../$OUTPUT_DEB" debian-binary control.tar.gz data.tar.gz

cd ..

echo ""
echo "=== Repack selesai ==="
echo "File baru: $OUTPUT_DEB"
echo ""</code></pre>
jalankan perintah
<pre><code>./edit_and_repack_deb.sh</code></pre>
