---
title: "Cafe Sales Analysis (UAS Data Science)"
date: 2025-12-12
description: "Penerapan Data Science: Membersihkan 'Dirty Dataset' dari Kaggle hingga mencapai akurasi prediksi 91%."
github_link: "https://github.com/ushaimramadhan/cafe-sales-prediction"
---

### Tentang Proyek

Ini adalah proyek Tugas Akhir Semester (UAS) untuk mata kuliah Data Science. Saya dan satu teman saya mengolah dataset publik **"Dirty Cafe Sales"** dari Kaggle yang sengaja dibuat berantakan (banyak _missing values_ dan format yang salah).

### Tantangan & Solusi

Tantangan terbesar bukanlah pada _modeling_, melainkan pada **Data Cleaning**. Banyak kolom numerik yang terkontaminasi teks, sehingga tidak bisa langsung dihitung.

**Pendekatan Teknis:**

- **Cleaning:** Menggunakan teknik _coercing_ Pandas untuk memisahkan data valid dari _noise_.
- **Feature Engineering:** Membuat variabel baru untuk memvalidasi konsistensi total pendapatan.
- **Result:** Berhasil membangun model **Linear Regression** dengan R² Score **0.91**.

> **Lihat Kode Lengkap:**
> Seluruh proses analisis, mulai dari cleaning hingga evaluasi model, terdokumentasi rapi di Jupyter Notebook.

[Buka Repository GitHub](https://github.com/ushaimramadhan/cafe-sales-prediction)
