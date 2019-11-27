#UniqueBrain - Format Input/Output Tugas

### Input Pemeriksaan

* Metode ini adalah berupa pemeriksaan dalam folder host, dimana **UniqueBrain** server akan mendownload semua file berformat **pdf** dari sebuah folder yang dipublish via **FTP**, berikut adalah input parameter yang **valid**:

```json
{
   "id": "1234567",
   "dosen": "Rizky",
   "email": "email@dosen.com",
   "judul": "Bunga Matahari",
   "threshold": 0.75,
   "min_match": 2,
   "folder" : "/unjani/.../folder/periksa",
   "report":
   {
      "logo_kiri": "PNG|URL|NULL",
      "logo_kanan": "PNG|URL|NULL",
      "upload_html": "/unjani/../folder/hasil/index.html",
      "upload_pdf": "/unjani/../folder/hasil/hasil.pdf"
   }
}
```
Berikut adalah penjelasan dari parameter diatas:

| Parameter | Default | Type | Deskripsi |
| --------- | --------| ---- | --------- |
| `id` | `NOT NULL` | **STRING** | Identitas (ID) tugas yang akan diperiksa, harus unik |
| `dosen` | `NOT NULL` | **STRING** | Nama dosen yang akan diperiksa |
| `email` | `NULL` | **STRING** | Alamat email dosen dimana laporan pemeriksaan akan dikirim, jika `NULL` maka tidak akan dikirim |
| `judul` | `NOT NULL` | **STRING** | Nama judul tugas yang dibuat oleh dosen |
| `threshold` | **0.75** | **DOUBLE** | Threshold kesamaan kalimat hingga diduga sebagai plagiarisme, angka dari **0.01** hingga **1.0** |
| `min_match` | **2** | **DOUBLE** | Persentase (%) minimal kesamaan antara dokumen tugas hingga dokumen tersebut diduga plagiat, angka dari **0.1** hingga **100** |
| `folder` | `NOT NULL` | **STRING** | Lokasi relative folder di dalam hosting, dimana semua file berformat **PDF** akan diperiksa. |
| `report` | `NULL` | **OBJECT** | Berisikan parameter pembuatan laporan pemeriksaan plagiat tugas |
| `report.logo_kiri` | `NULL` | **STRING** | Disikan dengan alamat `URL` atau `BASE-64` string dari file **PNG** yang dimana akan dijadikan sebagai logo header sebelah **KIRI ATAS**, jika `NULL` maka akan menggunakan header default |
| `report.logo_kanan` | `NULL` | **STRING** | Disikan dengan alamat `URL` atau `BASE-64` string dari file **PNG** yang dimana akan dijadikan sebagai logo header sebelah **KANAN ATAS**, jika `NULL` maka akan menggunakan header default |
| `report.upload_html` | `NULL` | **STRING** | Alamat relatif folder di-dalam hosting dimana **UniqueBrain** server akan mengupload laporan pemeriksaan dalam folder **HTML** ke folder tersebut |
| `report.upload_html` | `NULL` | **STRING** | Alamat relatif folder di-dalam hosting dimana **UniqueBrain** server akan mengupload laporan pemeriksaan dalam folder **PDF** ke folder tersebut |

**PENTING** !! 
1. `folder` tidak boleh diawali dengan **public_html** karena **UniqueBrain** akan secara default menggunakan folder **public_html** sebagai **root** dari semua folder
2. `folder` harus diawali dengan tanda backslash `/`
3. Misalkan nama relative folder adalah `/unjani/file_gue` maka **UniqueBrain** akan membaca sebagai `/home/{USER}/public_html/unjani/file_gue`
4. Struktur file yang akan diperiksa oleh **UniqueBrain** harus berformat sebagai berikut:
`[{NIM/Kelompok}] {nama_file}.pdf` misalnya: `[Kelompok 2] Tugas.pdf`

### Output Pemeriksaan

**UniqueBrain** akan merespon request pemeriksaan dengan konten berformat `JSON` sebagai berikut:

#### Response ketika pemeriksaan selesai

```json
{
   "id": "123456",
   "judul_tugas": "Membuat Tahu dan Tempe",
   "nama_dosen": "Mr. Acep",
   "tanggal_pemeriksaan": "2019/11/27",
   "waktu_pemeriksaan_dimulai": "15:30",
   "waktu_pemeriksaan_berakhir": "15:45",
   "banyak_dokumen_diperiksa": 10,
   "banyak_dokumen_terduga": 5,
   "nilai_plagiat_tertinggi": 85,
   "nilai_plagiat_terendah": 23,
   "url_laporan_html": "https://ubchecker.com/.../index.html",
   "url_laporan_pdf": "https://ubchecker.com/.../hasil.pdf",
   "detail_pemeriksaan":
   [
      {
         "nama_pemilik_dokumen": "Tn. Andri",
         "skor_plagiarisme_maks": 25.5,
         "skor_plagiarisme_min": 7.2,
         "jumlah_kalimat_terduga": 13,
         "jumlah_kalimat_keseluruhan": 123,
         "memiliki_relasi_sebanyak": 3,
         "detail_relasi_plagiarisme":
         [
            {
                "relasi_dengan": "Mr. Sinta",
                "besar_plagiat": 25.5,
                "jumlah_dugaan": 13,
                "rata_rata_kesamaan": 99,
                "detail_kesamaan_kalimat":
                [
                   {
                      "nomor_urutan": 1,
                      "kalimat_saya": "Saya suka makan bakso",
                      "kalimat_relasi": "Saya suka makan baso",
                      "besar_plagiat": 98.2
                   },
                   {
                      "nomor_urutan": 2,
                      "kalimat_saya": "Saya mengajarkan cara pembuatan nasi bungkus",
                      "kalimat_relasi": "Aku mengajarkan cara pembautan nasi bungkus",
                      "besar_plagiat": 93.2
                   }
                   //, dan seterusnya
                ]
            },
            {
                "relasi_dengan": "Mrs. Anis",
                "besar_plagiat": 18.5,
                "jumlah_dugaan": 9,
                "rata_rata_kesamaan": 92,
                "detail_kesamaan_kalimat":
                [
                   {
                      "nomor_urutan": 1,
                      "kalimat_saya": "Saya suka makan bakso",
                      "kalimat_relasi": "Saya suka makan padang",
                      "besar_plagiat": 90.2
                   },
                   {
                      "nomor_urutan": 2,
                      "kalimat_saya": "Saya mengajarkan cara pembuatan nasi bungkus",
                      "kalimat_relasi": "Aku mengajarkan cara pembuatan bungkus nasi",
                      "besar_plagiat": 85.2
                   }
                   //, dan seterusnya
                ]
            }
         ]
      }
      // , informasi plagiat per-orang seterusnya...      
   ]
}
```
 
Keterangan dan deskripsi dari `property` di dalam struktur `JSON` diatas:

| Property | Null | Type | Keterangan |
| -------- | ------- | ---- | ---------- |
| `id` | `-` | **STRING** | Sama dengan properti `id` yang dikirimkan saat request pemeriksaan tugas |
| `judul_tugas` | `-` | **STRING** | Sama dengan properti `judul` yang dikirimkan saat request pemeriksaan tugas |
| `nama_dosen` | `-` | **STRING** | Sama dengan properti `doseon` yang dikirimkan saat request pemeriksaan tugas |
| `tanggal_pemeriksaan` | `-` | **STRING** | Tanggal dimana tugas ini diperiksa, format: `YYYY/MM/DD` |
| `waktu_pemeriksaan_dimulai` | `-`| **STRING** | Waktu dimana tugas ini mulai diperika, format: `HH:MM` |
| `waktu_pemeriksaan_berakhir` | `-`| **STRING** | Waktu dimana tugas ini mulai selesai diperiksa, format: `HH:MM` |
| `banyak_dokumen_diperiksa` | `-` | **INTEGER** | Jumlah dokumen yang diterima oleh **UniqueBrain** untuk diperiksa |
| `banyak_dokumen_terduga` | `-` | **INTEGER** | Jumlah akumulasi dari semua dokumen yang terduga plagiat |
| `url_laporan_html` | `NULL` | **URL** | Alamat URL lengkap dimana **UniqueBrain** mengupload laporan pemeriksaan tugas ini dan dapat dilihat secara online dalam format **HTML**. Jika parameter `report.upload_html` berupa `NULL` maka `url_laporan_html` akan `NULL` dan tidak ada laporan yang diunggah |
| `url_laporan_pdf` | `NULL` | **URL** | Alamat URL lengkap dimana **UniqueBrain** mengupload laporan pemeriksaan tugas ini dan dapat dilihat secara online dalam format **PDF**. Jika parameter `report.upload_pdf` berupa `NULL` maka `url_laporan_pdf` akan `NULL` dan tidak ada laporan yang diunggah |
| `detail_pemeriksaan` | *maybe* | **ARRAY** | Berisikan detail hasil pemeriksaan, lengkap dengan kesamaan kalimat dan kesamaan diatara dokumen. Jika **TIDAK ADA PLAGIARISME** maka `detail_pemeriksaan` akan `NULL`. |

### Informasi Plagiarisme Dokumen

(Link)(###informasi-plagiarisme-dokumen)

 "nama_pemilik_dokumen": "Tn. Andri",
         "skor_plagiarisme_maks": 25.5,
         "skor_plagiarisme_min": 7.2,
         "jumlah_kalimat_terduga": 13,
         "jumlah_kalimat_keseluruhan": 123,
         "memiliki_relasi_sebanyak": 3,
