---
title: "Cara Saya Menghafal Vocab: Otomatisasi Anki dengan Python & Ollama CLI"
date: 2025-12-06T13:00:00+07:00
draft: false
tags: ["Python", "PowerShell", "Ollama", "Anki", "Automation"]
categories: ["Ngoprek"]
---

Sebagai mahasiswa IT, saya sering menemukan kosakata teknis baru. Masalahnya, memasukkan kosakata tersebut ke **Anki** satu per satu sangat membosankan. Saya harus mencari arti, mencari contoh kalimat umum, dan mencari konteks IT-nya secara manual.

Solusi saya? **Otomatisasi.**

Saya membuat sistem sederhana di mana saya cukup mengetik perintah `vocab` di terminal, memasukkan kata yang ingin dipelajari, dan biarkan AI yang mengurus sisanya hingga masuk ke Anki.

Berikut adalah dokumentasi cara saya membangun alat ini menggunakan **Python**, **Ollama (Local LLM)**, dan **PowerShell**.

## Arsitektur Sistem

Alurnya sederhana namun _powerful_:

1.  **Terminal:** User mengetik `vocab` (alias PowerShell).
2.  **Python Script:** Menerima input kata.
3.  **Ollama (AI):** Mencari definisi dan contoh kalimat (Umum & IT).
4.  **AnkiConnect:** Memasukkan hasil generate AI langsung ke database Anki.

---

## 1. Otak Sistem: Custom Model Ollama

Saya tidak menggunakan model standar mentah-mentah. Saya membuat custom model bernama `ankibot` (berbasis Llama 3.1) menggunakan `Modelfile`. Tujuannya agar output AI selalu konsisten dan langsung berformat HTML yang rapi untuk Anki.

Berikut isi `Modelfile` saya:

```dockerfile
FROM llama3.1
SYSTEM "You are an English teacher assistant meant to help create Anki flashcards. When I give you a word, you MUST respond with exactly this format: 1. Indonesian Meaning, 2. A simple daily sentence (English), 3. An IT/Cybersecurity related sentence (English). Keep it concise."
```

Perintah untuk membuat modelnya: ollama create ankibot -f Modelfile.

## 2. Jembatan Logika: Python Script

Script ini bertugas menjadi perantara. Ia meminta jawaban ke Ollama, lalu "melempar" jawaban itu ke Anki melalui plugin AnkiConnect (running di port 8765).

Berikut potongan kode inti dari anki_auto.py:

```python
import requests
import json

# Konfigurasi
OLLAMA_MODEL = "ankibot"
ANKI_DECK_NAME = "My Vocab"

def generate_definition(word):
    print(f"Sedang meminta Ollama mendefinisikan: '{word}'...")

    # Prompt meminta format HTML agar rapi di kartu Anki
    prompt = f"""
    Explain the word "{word}" for a student.
    Format output exactly like this in HTML:
    <b>Meaning:</b> [Indonesian Meaning]<br><br>
    <b>Daily Sentence:</b> <i>[English Sentence]</i><br><br>
    <b>IT/Cybersecurity Context:</b> <i>[Sentence related to tech]</i>
    """

    # Request ke Ollama Local
    url = "http://localhost:11434/api/generate"
    data = {"model": OLLAMA_MODEL, "prompt": prompt, "stream": False}
    response = requests.post(url, json=data)
    return response.json()['response']

def add_to_anki(word, content):
    # Request ke AnkiConnect
    url = "http://localhost:8765"
    note = {
        "deckName": ANKI_DECK_NAME,
        "modelName": "Basic",
        "fields": { "Front": word, "Back": content },
        "options": { "allowDuplicate": False },
        "tags": ["ollama_generated"]
    }
    payload = { "action": "addNote", "version": 6, "params": { "note": note } }
    requests.post(url, json=payload)

# Loop Utama
if __name__ == "__main__":
    print("--- ANKI AUTO GENERATOR ---")
    while True:
        target_word = input("\nMasukkan kata bahasa Inggris: ")
        if not target_word: break

        definition = generate_definition(target_word)
        if definition:
            add_to_anki(target_word, definition)
            print(f"✅ Berhasil! Kartu '{target_word}' masuk ke Anki.")
```

## Shortcut: PowerShell Function

Agar saya tidak perlu membuka folder proyek secara manual setiap kali ingin belajar, saya membuat fungsi alias di PowerShell profile saya.

```python
function vocab {
    # Pindah ke folder script
    Push-Location "D:\_DATA\02_Dokumen\Pribadi\Setup-Otomatisasi"

    # Jalankan script
    python anki_auto.py

    # Kembali ke folder asal setelah selesai
    Pop-Location
}
```

## Hasil Akhir

Sekarang, setiap kali saya menemukan kata asing saat membaca artikel cybersecurity, saya cukup buka terminal, ketik vocab, masukkan katanya, dan selesai.

Kartu Anki saya sekarang terisi otomatis dengan format:

    Depan: Kata (misal: Phishing)

    Belakang: Arti Bahasa Indonesia, Contoh Kalimat Sehari-hari, dan Contoh Kalimat Konteks IT.

Sangat efisien untuk mahasiswa IT yang sibuk!
