# RAG SimpliDOTS Technical Solution

## 📋 Penjelasan Singkat

RAG SimpliDOTS adalah solusi intelligent knowledge management system yang menggabungkan web scraping, vector storage, dan conversational AI untuk mengotomatisasi proses ekstraksi, pemrosesan, dan penyajian informasi dari artikel web dengan memanfaatkan tool **n8n**. Sistem ini dirancang khusus untuk memproses konten dari SimpliDOTS dan menyediakan interface chat yang memungkinkan pengguna berinteraksi dengan knowledge base secara natural.

Sistem ini menggunakan teknologi RAG (Retrieval-Augmented Generation) yang memungkinkan AI untuk memberikan jawaban yang akurat berdasarkan data yang telah diindeks, bukan hanya mengandalkan pengetahuan pre-training model.

## ✨ Fitur-Fitur Utama

### 🔍 **Web Scraping & Data Extraction**
- **Automated Content Extraction**: Menggunakan Firecrawl API untuk mengekstrak konten lengkap dari artikel web
- **Structured Data Processing**: Mengekstrak title, publication date, main content, dan image URLs
- **Smart Content Cleaning**: AI-powered cleaning untuk menghilangkan karakter dan simbol yang tidak perlu
- **Async Processing**: Sistem wait dan retry untuk memastikan ekstraksi yang sempurna

### 🧠 **AI-Powered Processing**
- **Content Summarization**: Otomatis membuat ringkasan artikel dalam bahasa Indonesia
- **DeepSeek Integration**: Menggunakan model DeepSeek untuk pemrosesan bahasa yang optimal
- **Intelligent Text Processing**: Pembersihan dan formatting otomatis konten

### 💾 **Vector Storage & Retrieval**
- **Azure OpenAI Embeddings**: Menggunakan text-embedding-3-small untuk representasi vektor yang akurat
- **In-Memory Vector Store**: Penyimpanan vektor yang cepat dan efisien
- **Semantic Search**: Pencarian berdasarkan makna semantik, bukan hanya keyword matching

### 💬 **Conversational Interface**
- **Chat Trigger**: Interface chat yang responsif dengan webhook support
- **Context-Aware Responses**: Memory buffer untuk menjaga konteks percakapan
- **Knowledge Base Integration**: AI Agent dengan akses ke knowledge base sebagai tool
- **Multi-turn Conversations**: Dukungan percakapan berkelanjutan dengan memori

### 📝 **Documentation & Reporting**
- **Notion Integration**: Otomatis membuat halaman Notion dengan hasil summary
- **Structured Output**: Format output yang konsisten dan terorganisir
- **Real-time Updates**: Update otomatis ke knowledge base setiap ada konten baru

## 🛠️ Setup dan Instalasi

### Prerequisites

Pastikan Anda memiliki:
- **Docker** (untuk menjalankan n8n)
- **Node.js** >= 16.0.0 (jika ingin setup manual)
- **Git** (untuk clone repository)

### Required API Keys & Credentials

1. **Firecrawl API**
   - Daftar di [Firecrawl](https://firecrawl.dev)
   - Dapatkan API key untuk web scraping

2. **Azure OpenAI**
   - Setup Azure OpenAI service
   - Deploy model `text-embedding-3-small`
   - Catat endpoint dan API key

3. **DeepSeek API**
   - Daftar di [DeepSeek](https://www.deepseek.com)
   - Dapatkan API key untuk chat model

4. **Notion API** (opsional)
   - Buat Notion integration
   - Dapatkan API token dan page ID

### Langkah-langkah Setup

#### 1. Setup n8n dengan Docker

Jalankan n8n menggunakan Docker (cara termudah dan tercepat):

```bash
# Pull dan jalankan n8n container
docker run -it --rm --name n8n \
  -p 5678:5678 \
  -v n8n_data:/home/node/.n8n \
  --dns 8.8.8.8 \
  -e TZ=Asia/Jakarta \
  docker.n8n.io/n8nio/n8n
```

**Penjelasan parameter:**
- `-p 5678:5678`: Map port 5678 untuk akses web interface
- `-v n8n_data:/home/node/.n8n`: Persistent volume untuk menyimpan workflow dan settings
- `--dns 8.8.8.8`: DNS resolver untuk koneksi internet yang stabil
- `--rm`: Hapus container otomatis setelah dihentikan

**Akses n8n:**
1. Buka browser dan navigasi ke `http://localhost:5678`
2. Setup admin user pada first run
3. Login dengan credentials yang telah dibuat

#### 2. Import Workflow ke n8n melalui UI n8n

1. Buka n8n interface di `http://localhost:5678`
2. Login dengan credentials admin
3. Klik "Import from file"
4. Upload file `RAG_SimpliDOTS_Technical_Test.json`

#### 3. Konfigurasi Credentials

Dalam n8n, setup credentials berikut:

**Azure OpenAI Credentials:**
buat akun dan buat projek [disini](https://ai.azure.com/)
```
Resource Name: Your Azure AI Foundry resource name
Endpoint: YOUR_AZURE_OPENAI_ENDPOINT
API Key: YOUR_AZURE_OPENAI_KEY
API Version: 2023-05-15
```

**DeepSeek Credentials:**  
buat akun dan copy API [disini](https://platform.deepseek.com/usage)
```
Name: DeepSeek account
API Key: YOUR_DEEPSEEK_API_KEY
```

**Firecrawl Credentials:**  
buat akun dan copy API [disini](https://www.firecrawl.dev/app/api-keys)
```
Name: Firecrawl account
API Key: YOUR_FIRECRAWL_API_KEY
```

**Notion Credentials:**  
lihat dokumentasi [disini](https://developers.notion.com/docs/create-a-notion-integration#create-your-integration-in-notion)
```
Name: Notion account
Notion Internal Integration Secret: Your Secret
```

#### 4. Konfigurasi URL Target

Edit node "Extract Data":
```javascript
// Ganti URL dengan artikel yang ingin di-scrape
urls: {
  items: [
    {
      url: "https://fitur-sap.simplidots.id/smh/fitur-pada-smh-sales-management-hub/2025/beta-integrasi-sales-invoice-simplidots-x-accurate-online-17-juli-2025"
    }
  ]
}
```

#### 5. Setup Notion Page (opsional)

Jika menggunakan Notion integration:
1. Buat halaman baru di Notion
2. Copy URL halaman
3. Update node "Create a page" dengan URL tersebut

#### 6. Aktivasi Workflow

1. Aktifkan workflow di n8n
2. Test dengan mengklik "Execute workflow"
3. Verifikasi semua node berjalan dengan sukses

## 🚀 Cara Penggunaan

### Manual Execution
1. Klik tombol "Execute workflow" untuk memulai proses scraping
2. Sistem akan otomatis:
   - Mengekstrak konten dari URL yang ditentukan
   - Membersihkan dan memproses data
   - Menyimpan ke vector store
   - Membuat summary dan dokumentasi

### Chat Interface
1. Akses chat webhook URL yang tersedia
2. Mulai bertanya tentang konten yang telah diindeks
3. Sistem akan mencari informasi relevan dan memberikan jawaban contextual


## **Catatan Untuk Reviewer**

- Saat ini menggunakan manual trigger untuk memudahkan demo. Dalam implementasi yang lebih kompleks, dapat menggunakan scheduler atau event-driven architecture untuk memulai
- URL Target Limitations  
  - Workflow saat ini hard-coded untuk URL SimpliDOTS tertentu
  - Untuk test dengan URL lain, edit node "Extract Data"
- Pastikan untuk memasukan Credential yang diperlukan dalam node pada workflow. Tertera juga pada catatan dalam workflow

## **Demo**
<iframe width="560" height="315" src="https://youtu.be/4Y55WZg-bNo" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>