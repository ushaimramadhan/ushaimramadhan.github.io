---
title: "Membangun Blog Pribadi Gratis & Cepat dengan Hugo + GitHub Pages"
date: 2025-12-07T13:00:00+07:00
draft: false
tags: ["Hugo", "GitHub Pages", "Tutorial", "Web Development"]
categories: ["Documentation"]
cover:
  image: "" # Nanti bisa diisi link gambar kalau ada
  alt: "Hugo logo"
  caption: "Static Site Generator super cepat"
---

Sebagai mahasiswa IT, memiliki dokumentasi pribadi itu penting. Awalnya saya bingung mau pakai apa: Wordpress? Blogger? Atau coding manual HTML/CSS?

Akhirnya pilihan saya jatuh pada **Hugo** dan **GitHub Pages**. Kenapa? Karena gratis, super cepat, aman (karena statis), dan terlihat "geeky" karena kita mengelolanya lewat terminal dan Git.

Di artikel ini, saya akan berbagi langkah-langkah bagaimana website ini dibuat, mulai dari instalasi hingga troubleshooting error saat deployment.

## Apa itu Hugo?

Singkatnya, Hugo adalah _Static Site Generator_ (SSG) yang ditulis menggunakan bahasa Go (Golang). Bedanya dengan Wordpress, Hugo tidak butuh database. Dia mengubah tulisan Markdown kita menjadi file HTML statis dalam hitungan milidetik.

## Langkah 1: Instalasi & Setup

Hal pertama yang saya lakukan adalah menginstall Hugo di laptop windows melalui terminal.

```bash
winget install Hugo.Hugo.Extended
```

Kemudian saya membuat situs baru dengan membuka terminal di folder tempat saya menyimpan projek, dengan perintah:

```bash
hugo new site my-blog
cd my-blog
```

Untuk tampilan, saya memilih tema PaperMod karena tampilannya yang minimalis, bersih, dan sangat populer di kalangan developer.

Selanjutnya saya inisialisasi Git pada folder projek ini.

```bash
git init
```

Dilanjutkan dengan mendownload tema (PaperMod).

```bash
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

Lalu kita konfigurasi dasar pada file hugo.taml dengan menggunakan teks editor (VS Code atau Notepad). Berikut isinya:

```bash
baseURL = 'https://ushaimramadhan.github.io/' # ini bisa Anda isi dengan username github masing-masing
languageCode = 'id-ID'
title = 'Ushaim Ramadhan' # bisa disesuaikan dengan kemauan Anda
theme = 'PaperMod'

[params]
defaultTheme = 'auto'
author = 'Nama Anda'
ShowReadingTime = true
ShowShareButtons = true
```

## Langkah 2: Tes Lokal (Localhost)

Sebelum online, kita cek dulu di laptop. Jalankan server lokal:

```bash
hugo server -D # -D artinya menampilkan juga yang statusnya masih 'Draft'
```

Lalu buka browser dan akses alamat yang muncul di terminal, biasanya:

```bash
http://localhost:1313/
```

Jika Anda melihat halaman putih dengan tulisan "my-blog" (atau judul yang Anda buat), berarti BERHASIL! Kerangka blog sudah jadi!

## Langkah 3: Membuat isi

Jangan matikan terminal server tadi. Buka tab terminal baru. Lalu buat file untuk menyimpan isi.

```bash
hugo new posts/otomatisasi-anki-python.md # Sesuaikan namanya dengan kemauan Anda
```

Buka file `content/posts/otomatisasi-anki-python.md` di editor. Anda akan melihat "Front Matter" di bagian atas (di antara tanda +++ atau ---). Ubah draft: true menjadi draft: false.

Lalu mulailah menulis isi konten Anda di bawahnya menggunakan format Markdown.

Contoh isi:

```bash
---
title: "Otomatisasi Kartu Anki dengan Python dan Ollama"
date: 2023-12-06T12:00:00+07:00
draft: false
---

## Latar Belakang
Saya sering merasa malas input vocab satu per satu...

## Solusi
Menggunakan script Python sederhana...
```

Selanjutnya cek browser (localhost), artikelnya harusnya otomatis muncul! Selamat Anda sudah berhasil membuat website!

## Langkah 4: Hosting Gratis di GitHub Pages

Ini bagian paling menarik. Kita tidak perlu beli hosting mahal. Cukup upload kode ke GitHub, dan fitur GitHub Pages akan menayangkannya secara gratis.

Saya menggunakan GitHub Actions untuk otomatisasi. Jadi, setiap kali saya menulis artikel di laptop dan melakukan git push, GitHub akan otomatis:

- Mendeteksi perubahan.

- Membangun (build) web menggunakan Hugo.

- Menerbitkannya ke publik.

## Tantangan: Mengatasi Error Deployment

Tentu saja, tidak ada coding yang mulus tanpa error. Saat pertama kali mencoba deploy, saya mengalami kegagalan di GitHub Actions.

Masalah: Workflow GitHub gagal dengan status merah. Ternyata penyebabnya adalah perbedaan versi Hugo. Di file konfigurasi hugo.toml (workflow), versi yang tertulis adalah 0.128.0, sedangkan di laptop saya menggunakan versi terbaru (misal 0.139.0).

Solusi: Saya harus menyamakan versinya. Saya mengedit file .github/workflows/hugo.yaml dan mengubah bagian HUGO_VERSION agar sesuai dengan versi laptop.

```YAML
env:
  HUGO_VERSION: 0.139.0 # Sesuaikan dengan versi lokal
```

Setelah di-commit ulang, indikator GitHub Actions akhirnya berubah menjadi Hijau (Success)!

## Kesimpulan

Membuat blog dengan Hugo mungkin terlihat rumit di awal karena banyak bermain dengan Terminal. Tapi sekali setup selesai, proses menulisnya sangat nyaman. Kita hanya perlu fokus pada konten (Markdown) tanpa pusing memikirkan database atau loading yang lambat.

Bagi teman-teman mahasiswa IT, saya sangat menyarankan metode ini. Selain punya blog keren, kita juga jadi belajar skill Git dan CI/CD secara tidak langsung.

Tertarik mencoba? Silakan cek dokumentasi resmi Hugo untuk info lengkap. [kunjungi situs resmi Hugo](https://gohugo.io)
