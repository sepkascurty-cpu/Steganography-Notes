# 📸 Digital Forensics & Steganography Notes

Catatan praktis untuk membongkar, menganalisis, dan mengekstrak data rahasia yang disembunyikan di dalam file gambar (`.jpg` atau `.png`) saat bermain CTF / Penetration Testing.

---

## 🛠️ Alur Kerja (Workflow) Analisis Foto

Ketika menemukan file gambar mencurigakan di server target, alur kerjanya adalah:
1. **Deteksi** apakah ada file asing di dalam foto (`binwalk`).
2. **Ekstrak** file tersembunyi tersebut jika ditemukan (`binwalk -e`).
3. **Ekstrak** data alternatif jika menggunakan enkripsi piksel (`steghide`).
4. **Crack Password** jika file hasil ekstraksi berupa ZIP yang terkunci (`john`).

---

## 💻 Rangkuman Perintah & Tools

### 1. Tool: Binwalk (Menerawang & Mengekstrak File)
Gunakan tool ini untuk memeriksa apakah ada file lain (seperti `.zip` atau `.txt`) yang sengaja "ditempel" di belakang kode gambar.

*   **Melihat isi tersembunyi (X-Ray):**
```bash
    binwalk nama_foto.jpg
    ```
*   **Mengekstrak file tersembunyi secara otomatis:**
```bash
    binwalk -e nama_foto.jpg
    ```
    *Catatan: Hasilnya akan otomatis masuk ke folder baru bernama `_nama_foto.extracted`.*

### 2. Tool: Steghide (Membongkar Enkripsi Piksel)
Beberapa data disembunyikan bukan dengan ditempel, melainkan disisipkan ke dalam kode warna/piksel gambar itu sendiri.

*   **Mengekstrak data tersembunyi:**
```bash
    steghide extract -sf nama_foto.jpg
    ```
    *Tips: Jika diminta passphrase/password, coba tekan **Enter** langsung (kosongkan).*

### 3. Tool: John the Ripper (Menjebol Password ZIP)
Jika hasil dari `binwalk -e` menghasilkan file `.zip` yang terkunci, kita harus membongkar password-nya lewat teknik *cracking hash*.

*   **Langkah A: Ubah file ZIP menjadi format teks Hash**
```bash
    zip2john file_rahasia.zip > hash.txt
    ```
*   **Langkah B: Jebol password menggunakan wordlist `rockyou.txt`**
```bash
    john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
    ```

---
💡 *Pastikan posisi terminal sudah berada di folder yang sama dengan file foto sebelum menjalankan perintah-perintah di at� 
