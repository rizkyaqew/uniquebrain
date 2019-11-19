# UniqueBrain - Online Plagiarism Scan Request

Untuk memeriksa plagiarisme dengan metode online, UniqueBrain server memerlukan parameter dalam format **JSON** atau **XML** dalam struktur sebagai berikut ini:

```JSON
{
  "session_id": "e4222dfa-5b2a-4fb2-83cf-774b623eca78",
  "account_id": "123456789",
  "user_name": "Rizky",
  "university": "Unjani",
  "threshold": 0.75,
  "min_match": 2.0,
  "skip_pages": 
  [
    4,
    6,
    7
  ],
  "scan_online": true,
  "scan_locals": true,
  "scan_scraps": true,
  "auto_report": true,
  "hook_start": "https://yoursite.com/hook/scan/start",
  "hook_works": "https://yoursite.com/hook/scan/works",
  "hook_finish": "https://yoursite.com/hook/scan/finish",
  "hook_error": "https://yoursite.com/hook/scan/error",
  "hook_cancel": "https://yoursite.com/hook/scan/cancel",
  "send_mails": 
  [
    "name@domain.com"
  ],
  "file_name": "mydocument.txt",
  "title": "Contoh Dokument Pemeriksaan Plagiarisme",
  "published": "2019-04-03T00:00:00",
  "year": 2019,
  "file_url": "https://unjani.ac.id/repos/sample-scan.txt",
  "file_raw": "VGhpcyBpcyBqdXN0IHRoZSBleGFtcGxlIHBsYWluIHRleHQgZG9jdW1lbnQgY29udGVudHMu",
  "context": "Dokumen_Pribadi_123"
}
```

Nama | Default | Type | Deskripsi
-------------- | ------- | ---- | ---------
`session_id` | - | **GUID** | ID pemeriksaan yang dimana nanti digunakan untuk mengambil hasil pemeriksaan
`account_id` | - | **STRING** | ID dari akun yang mengajukan pemeriksaan plagiarisme
`user_name` | - | **STRING** | Nama user dari akun yang mengajukan pemeriksaan plagiarisme
`university` | `NULL` | **STRING** | Nama universitas pengguna akun, `NULL` jika tidak ada
`threshold` | `0.75` | **FLOAT** | Nilai threshold minimal `plagiarisme kalimat`, range `0.0` sampai `1.0`
`min_match` | `2.0` | **FLOAT** | Nilai minimal plagiarisme dokumen yang akan di-ikut sertakan dalam response
`skip_pages` | `NULL` | **ARRAY INT** | Berisikan nomor halaman yang tidak perlu diperiksa plagiarismenya
`scan_online` | `true` | **BOOLEAN** | Jika **true** maka server akan memeriksa ke internet
`scan_locals` | `true` | **BOOLEAN** | Jika **true** maka server akan memeriksa ke lokal dokumen dalam **Elasticsearch**
`scan_scraps` | `false` | **BOOLEAN** | (`OBSOLETE`) Jika **true** maka server akan melakukan web scrapping. Penting: **fitur ini akan dihapus pada versi selanjutnya !**
`auto_report` | `false` | **BOOLEAN** | Jika **true** maka report plagiarisme dalam format PDF akan di-ikut sertakan dalam response JSON dalam format `BASE-64`
`hook_start` | `NULL` | **URL** | Alamat URL yang akan menerima `POST` request dari server **saat server akan melakukan pemeriksaan dokumen secara online**
`hook_works` | `NULL` | **URL** | Alamat URL yang akan menerima `POST` request dari server **dengan konten informasi dan progress pemeriksaan**
`hook_finish` | `NULL` | **URL** | Alamat URL yang akan menerima `POST` request dari server **ketika server baru saja selesai memeriksa dokumen**
`hook_error` | `NULL` | **URL** | Alamat URL yang akan menerima `POST` request dari server **ketika terjadi kesalahan (`error`) dalam pemeriksaan**. Informasi yang akan dikirim adalah detail tentang `error` tersebut.
`hook_cancel` | `NULL` | **URL** | Alamat URL yang akan menerima `POST` request dari server yang menawarkan **pembatalan** pemeriksaan saat proses berlangsung. Server akan mengirim setiap 5 detik sekali ke alamat ini. Jika `response status` dari alamat ini adalah **200** (`OK`) maka server akan menghentikan segera proses pemeriksaan. Jika `response status` tidak **200** (misal **201**) maka server akan meneruskan pemeriksaan
`send_mails` | `NULL` | **URL** | List dari alamat email yang akan menerima laporan hasil pemeriksaan plagiarisme secara online jika pemeriksaan selesai tanpa error
`file_name` | `NULL` | **STRING** | Nama file dokumen yang dikirim ke server untuk diperiksa. Sebaiknya parameter ini di-isikan supaya server dapat membedakan file yang diperiksa dengan file lainnya
`title` | - | **STRING** | Judul dokumen yang akan diperiksa, **tidak boleh** `NULL`
`published` | `NULL` | **DATE** | Tanggal dimana dokumen yang diperiksa dipublikasikan
`year` | - | **INT** | Tahun dimana dokumen yang diperiksa dipublikasikan, **tidak boleh** `NULL`
`file_url` | `NULL` | **URL** | URL dimana dokumen file yang akan diperiksa dapat di-download oleh server, sehingg anda tidak perlu mengikutsertakan dokumen file dalam format `BASE-64` kedalam JSON parameter ini. **Catatan**: **Jika** `file_raw` **tidak NULL maka** `file_url` **akan diabaikan (ignored)**
`file_raw` | `NULL` | **BASE-64** | Raw data dari dokumen file yang akan diperiksa oleh server. Jika `NULL` maka server akan menggunakan `file_url`. Jika kedua-duanya `NULL` server akan mengembalikan status `Error 400`. **Penting**: Ukuran maksimal file yang dapat diperiksa oleh server adalah **10 MB** !

