# forensics-samples

## Tentang

forensics-samples adalah sekumpulan berkas yang berguna untuk membantu mempelajari atau menguji
alat dan teknik forensik. Berkas-berkas ini adalah contoh gambar, sistem berkas, dan
kemungkinan artefak lain sebagai dump memori (belum tersedia).

forensics-samples berguna untuk mahasiswa dan pengujian CI. Tujuan utama dari
karya ini adalah menyediakan sekumpulan berkas yang terstandarisasi untuk menghindari pemborosan waktu dalam beberapa tugas
saat mempelajari tentang forensik atau alat pengujian.

## Konten

forensics-samples memiliki beberapa berkas di dalam direktori "original-files". Ada
beberapa citra sistem berkas yang tersedia (saat ini: btrfs, exFAT, ext2, ext4,
NTFS, dan vfat (FAT32)). Di dalam setiap citra sistem berkas, semua berkas dari
"original-files" disalin dan, setelah ini, semua direktori yang diakhiri dengan
"2" pada namanya dihapus. Dimungkinkan untuk menggunakan alat untuk menganalisis file dan metadatanya atau pengukir untuk memulihkan file yang dihapus. 
Lihat di bawah konten "file-asli":
original-files/
    ├── audio1
    │   ├── debian.mp3
    │   ├── debian.ogg
    │   └── debian.wav
    ├── audio2
    │   ├── deleted.mp3
    │   ├── deleted.ogg
    │   └── deleted.wav
    ├── movie1
    │   └── VID_20191220_170832.mp4
    ├── movie2
    │   ├── movie-hello.avi
    │   ├── movie-hello.mp4
    │   ├── movie-hello.mpeg
    │   └── movie-hello.ogg
    ├── pic1
    │   ├── debian_logo.jpg
    │   ├── debian_logo.png
    │   ├── debian.png
    │   ├── debian.ppm
    │   ├── debian.xcf
    │   ├── empty.jpg
    │   ├── IMG_1054.JPG
    │   ├── IMG-20191006-WA0002.jpg
    │   └── IMG_20200827_231612.jpg
    ├── pic2
    │   ├── d-debian.jpg
    │   ├── d-debian.png
    │   ├── d-debian.ppm
    │   ├── d-debian.xcf
    │   ├── IMG_20191224_234846.jpg
    │   ├── IMG_20200124_231153.jpg
    │   └── IMG_20200608_111614.jpg
    ├── text1
    │   ├── a-text.docx
    │   ├── a-text.odt
    │   ├── a-text-pass-A5d.pdf
    │   ├── a-text-pass-peanuts.pdf
    │   └── a-text.pdf
    └── text2
        ├── d-text.docx
        ├── d-text.odt
        ├── d-text.pdf
        └── test.sh
Ada juga berkas yang disebut fs.multiple.xz. Berkas ini memiliki beberapa partisi dan sistem berkas. Lihat struktur di bawah ini:
                   Sector  Filesystem
    Partition 1      2048  btrfs
    Partition 2    227328  ext4
    Partition 3    309248  exfat
    Partition 4    391168  ntfs

Setiap sektor memiliki 512 byte. Setiap sistem berkas memiliki dua berkas: logo_debian.jpg dan
test.txt. Berkas asli berada di direktori original-multiple.

## Cara penggunaan

Cukup unduh semua repositori atau citra sistem berkas yang terisolasi dan Anda akan senang.
Anda dapat mempelajari/menguji alat-alat seperti foremost, magicrescue, scalpel, exifprobe,
ext4magic, extundelete, ext3grep, sleuthkit, disktype, afflib-tools,
metacam, dll.

Semua citra sistem berkas memiliki satu partisi yang dimulai pada sektor 2048. Untuk
memasangnya, Anda dapat menggunakan:
    # unxz fs.<name>
    # mount -o ro,offset=1048576 fs.<name> /mnt

PS: 1048576 sama dengan 2048 (sektor awal) * 512 (ukuran sektor).

File a-text-pass-A5d.pdf dan a-text-pass-peanuts.pdf dilindungi oleh kata sandi "A5d" dan "peanuts". File ini dapat digunakan untuk menguji cracker PDF.

## Langkah-langkah untuk membuat gambar

Ini adalah topik informasi. Setiap gambar dibuat dengan mengikuti langkah-langkah berikut
(contoh untuk ext4):

    $ dcfldd if=/dev/zero of=fs.ext4 bs=1M count=50
    # fdisk fs.ext4  (to create a single disk partition, starting in sector 2048)
    # losetup -fo $[2048*512] fs.ext4  (to create /dev/loop0 starting in 2048)
    # mkfs.ext4 -m .001 /dev/loop0
    # mount /dev/loop0 /mnt
    # ./generate-fs.sh /mnt/
    # umount /mnt
    # losetup -d /dev/loop0
    $ xz fs.ext4

Catatan: BTRFS memerlukan gambar berukuran 150 MB.

## Lisensi

Hak Cipta 2019-2020 Joao Eriberto Mota Filho <eriberto@eriberto.pro.br>

Untuk semua dokumen, gambar, dan film di dalam "original-files" dan "original-multiple", lisensinya adalah CC-BY-SA-4.0 (Creative Commons
Attribution-ShareAlike 4.0 International), kecuali untuk debian_logo.jpg dan
debian_logo.png, yang diambil dari Proyek Debian, Hak Cipta 1999 Software in the
Public Interest, Inc., dan di bawah LGPL-3+ atau CC BY-SA 3.0. Lihat detailnya di https://www.debian.org/logos/. Untuk semua berkas lainnya (termasuk "original-files/text2/test.sh") dan citra sistem berkas, lisensinya adalah MIT.
