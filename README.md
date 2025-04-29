# Extrak-.bin
Untuk mengekstrak file .deb di Linux tanpa menginstalnya, kamu bisa menggunakan perintah dpkg-deb atau alat lain seperti ar. Berikut beberapa cara:

1. Menggunakan dpkg-deb

dpkg-deb -x nama_file.deb nama_folder_tujuan

Contoh:

dpkg-deb -x paketku.deb ./output

Ini akan mengekstrak isi paket ke folder ./output.

2. Menggunakan ar dan tar

File .deb sebenarnya adalah file arsip standar Unix. Kamu bisa mengekstraknya seperti ini:

ar x nama_file.deb

Ini akan menghasilkan tiga file utama:

control.tar.gz (informasi kontrol/paket)

data.tar.gz atau data.tar.xz (isi file sesungguhnya)

debian-binary (versi format .deb)


Setelah itu, ekstrak data.tar.*:

tar -xf data.tar.gz

Atau jika xz:

tar -xf data.tar.xz

Ini akan mengekstrak file aplikasi ke direktori saat ini.

Perlu bantuan mengekstrak bagian kontrol juga?

