# Tax Assistant RAG System

**Proyek ini membangun pipeline end-to-end untuk asisten pajak berbasis Retrieval-Augmented Generation (RAG) menggunakan dokumen perpajakan Indonesia. Sistem ini mengonversi PDF ke Markdown, melakukan preprocessing, chunking, embedding, dan menyimpan hasilnya ke vector database untuk mendukung pencarian cerdas dan jawaban berbasis LLM.**

## Daftar Isi

- [Fitur Utama](#fitur-utama)
- [Arsitektur Sistem](#arsitektur-sistem)
- [Alur Kerja](#alur-kerja)
- [Struktur Proyek](#struktur-proyek)
- [Instalasi & Persiapan Lingkungan](#instalasi--persiapan-lingkungan)
- [Cara Menjalankan](#cara-menjalankan)
- [Catatan Penting](#catatan-penting)

## Fitur Utama

- Konversi PDF ke Markdown dengan `marker-pdf` untuk ekstraksi dokumen hukum perpajakan.
- Preprocessing & normalisasi istilah serta struktur dokumen.
- Chunking adaptif: header-based, hybrid, dan semantic.
- Embedding multibahasa menggunakan model `intfloat/multilingual-e5-large`.
- Penyimpanan dan pencarian chunk menggunakan ChromaDB.
- Evaluasi kualitas chunk dan retrieval otomatis.
- Integrasi RAG siap pakai dengan LLM (HuggingFace, Azure AI, dsb).

## Arsitektur Sistem

```
PDF → Markdown → Preprocessing → Chunking → Embedding → Vector DB → RAG/LLM
```

- **PDF Converter:** marker-pdf
- **Preprocessing:** Regex, normalisasi istilah pajak
- **Chunking:** MarkdownHeaderTextSplitter, RecursiveCharacterTextSplitter
- **Embedding:** SentenceTransformers
- **Vector DB:** ChromaDB
- **RAG Framework:** LangChain, Transformers, Azure AI Inference (opsional)
- **Evaluasi:** Ragas

## Alur Kerja

1. **Konversi PDF ke Markdown**
   - Ekstrak isi dokumen hukum ke format Markdown terstruktur.
2. **Preprocessing**
   - Normalisasi istilah, penandaan pasal/ayat, dan pembersihan teks.
3. **Chunking**
   - Bagi dokumen menjadi chunk berdasarkan struktur hukum (BAB, Pasal, Ayat, dst).
   - Mendukung strategi hybrid (header + recursive).
4. **Ekstraksi Metadata**
   - Ambil metadata penting: nomor UU, tahun, judul, daftar BAB.
5. **Embedding**
   - Ubah setiap chunk menjadi vektor embedding multibahasa.
6. **Penyimpanan ke Vector DB**
   - Simpan chunk, metadata, dan embedding ke ChromaDB.
7. **Evaluasi Kualitas**
   - Validasi chunk (ukuran, coverage, context preservation).
   - Uji retrieval dengan query pajak umum.
8. **Integrasi RAG**
   - Siap dihubungkan ke pipeline LLM untuk Q&A berbasis dokumen.

## Instalasi & Persiapan Lingkungan

1. **Buat virtual environment (sangat disarankan):**
   ```bash
   python -m venv venv
   source venv/bin/activate  # Linux/Mac
   # atau venv\Scripts\activate  # Windows
   ```

2. **Install dependensi:**
   ```bash
   pip install -r requirements.txt
   ```
   
## Cara Menjalankan

1. **Siapkan file PDF dokumen perpajakan.**
2. **Jalankan notebook `tax-assistant-preprocessing.ipynb`:**
   - Konversi PDF ke Markdown.
   - Lakukan preprocessing, chunking, dan simpan ke vector DB.
3. **Jalankan notebook `tax-rag-architecture.ipynb`:**
   - Tambahkan github marketplace api key, berikut template jika menggunakan kaggle notebook 
   ```
    user_secrets = UserSecretsClient()
    github_key = user_secrets.get_secret("GITHUB_KEY")
   ``` 
   - Lakukan retrieval, evaluasi, dan integrasi dengan LLM.
